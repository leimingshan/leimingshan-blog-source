---
title: MongoDB的POJO映射
tags:
  - Java
  - MongoDB
  - NoSQL
id: 402
categories:
  - 数据库
abbrlink: b327357c
date: 2015-08-31 14:12:20
---

在MongoDB数据库的Java程序开发过程中，使用了官方支持的POJO映射Morphia
[https://github.com/mongodb/morphia/wiki](https://github.com/mongodb/morphia/wiki)。

其作用，简而言之，就是完成Java Bean对象与MongoDB数据库中一条记录的映射，大家在Entity中看到的Profile就是这样一个POJO，
Profile中嵌套了一些子类，比较容易理解，Morphia的所有映射都可以通过注解来完成，非常简单易用，可以查看上面wiki文档中的说明。
另外，Morphia提供了一个基本的DAO类，org.mongodb.morphia.dao.BasicDAO&lt;T,K&gt;
详细信息可以查看链接[https://rawgit.com/wiki/mongodb/morphia/javadoc/0.108/apidocs/index.html](https://rawgit.com/wiki/mongodb/morphia/javadoc/0.108/apidocs/index.html)

该类支持基本的CRUD操作，查询也支持各种查询条件，不过在返回结果这边可定制性较弱，通常都是返回整个的POJO类的List，属于粗粒度吧。
BasicDAO从根本上都是对mongo-java-driver中一些api的封装，如果需要更全面的功能的话可以选用底层driver，当然这样就是直接跟MongoDB中的BSON打交道了，
也就用不上POJO了。