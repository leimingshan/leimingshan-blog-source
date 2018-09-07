---
title: MongoDB的数据库复制
tags:
  - MongoDB
  - NoSQL
id: 404
categories:
  - 数据库
abbrlink: b176fc0
date: 2015-08-31 14:20:14
---

在MongoDB的客户端可以使用如下命令：
db.copyDatabase("abc", "abc-tmp", "localhost")
数据库文件较大的话会需要一段时间。

1，复制数据库，使用copyDatabase命令完成复制数据库，
格式:copyDatabase(fromdb,todb,fromhost[,username,password])
fromdb：源数据库名称
todb：目标数据库名称
fromhost：源数据库地址，本地和远程都可以
username：远程数据库用户名
password：远程数据密码
例子：将本地db2库复制本地，并重命名db1
&gt; db.copyDatabase("db2","db1","localhost")<!--more-->
2，刷新磁盘：将内存中尚未写入磁盘的信息写入磁盘，并锁住对数据库更新的操作，但读操作可以使用，使用runCommand命令,这个命令只能在admin库上执行
格式：db.runCommand({fsync:1,async:true})
async：是否异步执行
lock:1 锁定数据库

3，数据压缩：mongodb的存储结构采用了预分配的机制，长期不断的操作，会留下太多的的碎片，从而导致数据库系统越来越慢。
repairDatabase命令是mongodb内置的一个方法，它会扫描数据库中的所有数据，并将通过导入/导出来重新整理数据集合，将碎片清理干净
现在看压缩前和压缩后的对比数据，如下所示：
PRIMARY&gt; db.t1.storageSize()
65232896
PRIMARY&gt; db.t1.totalSize()
81470432
PRIMARY&gt; db.repairDatabase()
{ "ok" : 1 }
PRIMARY&gt; db.t1.storageSize()
65232896
PRIMARY&gt; db.t1.totalSize()
79851584

4，当然也支持复制Collection，与复制Database类似。

&nbsp;