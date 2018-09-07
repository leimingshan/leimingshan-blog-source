---
title: 基于Redis实现的Rate limiter (限流器)
tags:
  - Redis
  - NoSQL
category: 开发
abbrlink: '9e983597'
date: 2018-04-10 17:46:12
---
首先建议大家好好阅读一下官方文章，如何利用incr命令实现一些应用模式（Pattern）。
[INCR命令的介绍与应用](https://redis.io/commands/incr)

本文不对原文进行大段翻译，主要讲下自己的理解。

# 模式：计数器
Redis原子性自增操作，最明显的应用就是计数器了，类似Java的AtomicInteger。
可以结合EXPIRE，INCRBY，GET，SET，DECR等操作做很多很多事情。
多命令的情况下要注意事务或者使用Lua script哦。

# 模式：Rate limiter 限流器
## 限流器的应用
限流器的应用非常广泛，比如Github对外提供了非常丰富的API，但考虑到数据安全和系统资源，对匿名用户和经过认证的用户的请求API频率都是要有限制的。
可以看看Github API的[Rate limiting](https://developer.github.com/v3/#rate-limiting)。
认证的用户每小时请求次数是5000，没认证的用户每小时只能请求60次，依靠原始IP来区分未认证用户。

上面介绍了一个很典型的应用场景，如果一个系统对我提供服务，开放API的话，为了防刷和系统资源的平衡，限流器的应用是很有必要的。
调用Github API返回结果的时候，response的Header里面都会带有限流的信息，这是一个非常好的设计，大致如下：
```
curl -i https://api.github.com/users/octocat
HTTP/1.1 200 OK
Date: Mon, 01 Jul 2013 17:27:06 GMT
Status: 200 OK
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 56
X-RateLimit-Reset: 1372700873
```
我在做网关设计中也借鉴过这种设计方式，另外也参考过spring-cloud-zuul微服务网关中的一个[API限流库](https://github.com/marcosbarbero/spring-cloud-zuul-ratelimit)的代码，里面Filter的设计还是很不错的。

<!--more-->

## 提出问题
针对每个来访IP，限制每秒只能访问10次。

## 模式1：最直接的实现
KEY值的设计会决定你的解决方案。
一种是KEY是IP+当前秒数（UNIX时间戳），那么在该秒内的所有访问，都会对这个KEY执行INCR命令，这个KEY在当前秒之后就没用了其实，设置过期时间大于1秒即可。
该方案的伪码表示如下：
```
FUNCTION LIMIT_API_CALL(ip)
ts = CURRENT_UNIX_TIME()
keyname = ip+":"+ts
current = GET(keyname)
IF current != NULL AND current > 10 THEN
    ERROR "too many requests per second"
ELSE
    MULTI
        INCR(keyname,1)
        EXPIRE(keyname,10)
    EXEC
    PERFORM_API_CALL()
END
```
显而易见的，该方案的缺点是系统访问量大时，比如当前秒有10000个IP来访问，Redis中就会出现10000个KEY，虽然有Redis的过期删除，10秒过期就会导致10秒
内的所有IP访问的KEY堆积，大量占用Redis的内存。

## 模式2：IP为KEY
这种设计也很直接啊，IP为KEY，过期时间1秒，有IP访问就自增，超过1秒，该KEY就会过期，后面的访问重新生成KEY。
```
FUNCTION LIMIT_API_CALL(ip):
current = GET(ip)
IF current != NULL AND current > 10 THEN
    ERROR "too many requests per second"
ELSE
    value = INCR(ip)
    IF value == 1 THEN
        EXPIRE(ip,1)
    END
    PERFORM_API_CALL()
END
```
官网很明确的指出了这里面的竞争条件，假如多个线程访问，都进入了ELSE进行了自增，ip的值就变为2或更大，EXPIRE没有执行，这个KEY就泄露了，永远保存在Redis中，
只有后面又遇到相同IP地址的访问。
因为有IF判断语句，所以这里不能使用MULTI-EXEC事务，必须使用lua脚本，提升了设计复杂度。
```
local current
current = redis.call("incr",KEYS[1])
if tonumber(current) == 1 then
    redis.call("expire",KEYS[1],1)
end
```

## 模式3：新思路使用list
直接上lua script好了。KEYS[1]就是访问IP，ARGV[2]是超时时间的ms值，这里是1000，ARGV[1]比较随意，可以是访问时间的ms。
```
if (redis.call('exists', KEYS[1]) == 0) then 
    redis.call('rpush', KEYS[1], ARGV[1]);
    return redis.call('pexpire', KEYS[1], ARGV[2]); 
else
    return redis.call('rpushx', KEYS[1], ARGV[1]); 
end; 
```
先执行LLEN(KEY)，如果超过限制则返回，否则执行LUA脚本。

之前有个小同事在这里用了KEYS IP*的方式，类似模式1，这里大家要注意，在很多Redis的线上系统中是会禁用KEYS的，因为KEYS会造成系统CPU的使用率骤增，
会导致系统不稳定。我直接改成了这个lua script的用法，现在运行的也很不错。

这个LUA脚本解决了官网说的竞争问题，官网的伪代码如下：
```
FUNCTION LIMIT_API_CALL(ip)
current = LLEN(ip)
IF current > 10 THEN
    ERROR "too many requests per second"
ELSE
    IF EXISTS(ip) == FALSE
        MULTI
            RPUSH(ip,ip)
            EXPIRE(ip,1)
        EXEC
    ELSE
        RPUSHX(ip,ip)
    END
    PERFORM_API_CALL()
END
```
简单解释下，这里的竞争在IF EXISTS，多个线程同时判断了IF，都进入了IF，准备执行MULTI-EXEC，
当然这里只能顺序执行，一个线程执行完之后，另一个线程也执行，EXPIRE以最后执行的线程为准，由于过期时间的改变，会有略微不准确的情况。
