---
title: MongoDB--mongoimport loses connection when importing big files
tags:
  - NoSQL
id: 442
categories:
  - 数据库
abbrlink: 33725cfb
date: 2016-01-20 14:36:20
---

今天在新版本下进行MongoDB数据导入的时候，遇到了这样一个问题，例如使用如下命令：
<pre class="lang:default decode:true">mongoimport -d test -c profile users.dat</pre>
在之前使用过几次这样的导入方法都没有问题，但这次却提示：
<pre class="lang:default decode:true">2016-01-20T10:05:25.228+0100    connected to: localhost
2016-01-20T10:05:25.735+0100    error inserting documents: lost connection to server
2016-01-20T10:05:25.735+0100    Failed: lost connection to server
2016-01-20T10:05:25.735+0100    imported 0 documents</pre>
查看MongoDB的Log，发现出现异常的原因，如下：
<pre class="lang:default decode:true ">2016-01-20T11:26:39.103+0800 I -        [conn17] Assertion: 10334:BSONObj size: 33562755 (0x2002083) is invalid. Size must be between 0 and 16793600(16MB) First element: insert: "Profile"</pre>
搜索解决方案，发现这是mongo工具包在新版本下的小bug，mongorestore和mongoimport都有一样的问题，官方说明可以参考[https://jira.mongodb.org/browse/TOOLS-939](https://jira.mongodb.org/browse/TOOLS-939)。

原因就是bulk write api，原来的api中批量写入的batch size最大是32MB，现在已经变为16MB了。在导入或还原数据的时候，指定选项 --batchSize=1000，指定一个较小的值即可，默认是10000。

参考：

1.  [http://stackoverflow.com/questions/33475505/mongodb-mongoimport-loses-connection-when-importing-big-files](http://stackoverflow.com/questions/33475505/mongodb-mongoimport-loses-connection-when-importing-big-files)
2.  [https://jira.mongodb.org/browse/TOOLS-939](https://jira.mongodb.org/browse/TOOLS-939)
3.  [http://chenzhou123520.iteye.com/blog/1641319](http://chenzhou123520.iteye.com/blog/1641319)