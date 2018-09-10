---
title: Redis常用开发应用规范
tags:
  - Redis
  - NoSQL
category: 开发
abbrlink: 1113160e
date: 2018-04-16 14:32:37
---
Redis在日常开发中可以说是非常常用的服务了，提供了灵活的数据结构、高效率、方便的lua script等多种功能。
在开发运用过程中，有一些点是需要特别注意的，也是对Redis的一种比较正确的使用方式，简单总结如下：

1. KEY的格式要规范，可读性要强，注意控制KEY的长度。
2. VALUE的大小限制。Redis限制每个String类型value大小不超过512MB，
   实际开发中，不要超过10KB，否则会对CPU和网卡造成极大负载。
   hash、list、set、zset元素个数不要超过5000。
<!--more-->
3. 选择合适的数据类型，而不是一味的用string类型。建议多用hash，有压缩算法，可以降低开销。
>>hash类型特别适合用于存储对象。在field的数量在限制的范围内以及value的长度小于指定的字节数，那么此时的hash类型是用zipmap存储的，所以会比较节省内存。可以在配置文件里面修改配置项来控制field的数量和value的字节数大小。
>>  hash-max-zipmap-entries 512 #配置字段最多512个
>>  hash-max-zipmap-value 64 #配置value最大为64字节。
>>必须满足以上两个条件，那么该key会被压缩。否则就是按照正常的hash结构来存储hash类型的key。
4. 设置过期时间，减少内存占用。
5. 禁用KEYS命令。查找效率是O(N)，很可能阻塞正常请求，而且CPU负载过大。
6. 禁用flushall，flushdb命令，过于危险。
7. 建议使用批量操作提供效率
>>原生命令：例如mget、mset。
>>非原生命令：可以使用pipeline提高效率。

参考文档：
[阿里云Redis开发规范](https://blog.csdn.net/glx490676405/article/details/79580748)