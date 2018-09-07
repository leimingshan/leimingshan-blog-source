---
title: MongoDB-ObjectId
tags: MongoDB
category: 算法
abbrlink: 397fc4ba
date: 2018-03-04 14:15:35
---
对于分布式唯一ID的生成，有几种比较好的思路，划分命名空间并行生成，是现在比较流行的一种方式，例如Twitter提出的非常有名的
Snowflake算法。与之类似的是，在很早之前，MongoDB的ObjectId生成算法就是类似的这种思路，在MongoDB的0.9.10(2009年8月24日发布)版本中就已经采用。

在最新的MongoDB-Java-Driver文档中，可以看到ObjectId的相关API说明。参考资料如下：
[Class ObjectId](http://mongodb.github.io/mongo-java-driver/3.8/javadoc/org/bson/types/ObjectId.html)
[Source code - ObjectId.java](https://github.com/mongodb/mongo-java-driver/blob/master/bson/src/main/org/bson/types/ObjectId.java)
<!--more-->

具体来说，12字节的MongoDB ObjectId的结构是
- a 4-byte value representing the seconds since the Unix epoch
- a 3-byte machine identifier
- a 2-byte process id
- a 3-byte counter, starting with a random value

可以看出，最小的划分粒度是 秒*进程实例，
对于单个进程来说，每秒的ID容量是最后一个字段的3个字节，24bit，即大约16777216个ID，这个容量已经可以满足大部分情况下的ID生成需求了。
