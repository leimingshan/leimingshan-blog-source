---
title: NoSQL 数据库简介
tags:
  - NoSQL
id: 196
categories:
  - 数据库
abbrlink: 3bc6c284
date: 2013-07-31 18:26:13
---

NoSQL 数据库的概念是相对于传统的关系型数据库（RDBMS-Relational Database Management System）而言，在近年来获得了极大的发展，被许多人认为是下一代的数据库。NoSQL数据库一般关注以下几点：
> being **non-relational, distributed, open-source** and **horizontally scalable**
> 
> **非关系型，分布式，开源，水平伸缩性。**
在大数据和实时web应用方面，NoSQL技术已经很好的找到了自己的位置，并显示出了相对于传统关系型数据库的巨大优势。NoSQL在英文也常常被认为是“Not Only SQL”（不仅仅是SQL），用来强调NoSQL并不是SQL技术的对立面，它也允许类SQL查询语言的使用。<!--more-->

# 历史：

NoSQL一词最早是由Carlo Strozzi用来命名他开发的一个轻量级开源的关系型数据库，这个数据没有提供标准的SQL接口，所以就简单叫做NoSQL，他认为，现在的NoSQL运动，主要是不使用关系模型，应该叫做NoREL（非关系型）更为合适。

在2009年初，Eric Evans再次提出这个概念，那时Johan Oskarsson想组织一个会议来讨论开源的分布式数据库。这个名字用来标记那些越来越多的非关系型、分布式数据存储、并且不尝试提供ACID（原子性、一致性、隔离性和持久性）的数据库。
> **关系型数据库最大特点就是事务的一致性**：传统的关系型数据库读写操作都是事务的，具有ACID（原子性Atomicity、一致性Consistency、隔离性Isolation、持久性Durability）的特点，C就是一致性（Consistency），这个特点是关系型数据库的灵魂（其他三个AID都是为其服务的），这个特性使得关系型数据库可以用于几乎所有对一致性有要求的系统中，如典型的银行系统。
自2009年初开始，NoSQL运动发展迅猛，现在已经有了大约150种NoSQL数据库，你可以在这个网站[http://nosql-database.org/](http://nosql-database.org/)查看其中一些比较有代表性的NoSQL数据库。

# 分类

大多数人比较认同的分类方法是根据其数据模型进行分类：

*   **Column**: [HBase](http://en.wikipedia.org/wiki/HBase "HBase"), [Accumulo](http://en.wikipedia.org/wiki/Accumulo "Accumulo")
*   **Document**: [MongoDB](http://en.wikipedia.org/wiki/MongoDB "MongoDB"), [Couchbase](http://en.wikipedia.org/wiki/Couchbase "Couchbase")
*   **Key-value** : [Dynamo](http://en.wikipedia.org/wiki/Dynamo_(storage_system) "Dynamo (storage system)"), [Riak](http://en.wikipedia.org/wiki/Riak "Riak"), [Redis](http://en.wikipedia.org/wiki/Redis "Redis"), [Cache](http://en.wikipedia.org/wiki/MemcacheDB "MemcacheDB"), [Project Voldemort](http://en.wikipedia.org/wiki/Project_Voldemort "Project Voldemort")
*   **Graph**: [Neo4J](http://en.wikipedia.org/wiki/Neo4J "Neo4J"), [Allegro](http://en.wikipedia.org/wiki/AllegroGraph "AllegroGraph"), [Virtuoso](http://en.wikipedia.org/wiki/Virtuoso "Virtuoso")
初学者建议学习和使用一下MongoDB和Redis。

参考阅读：

1.  更加详细的NoSQL介绍：[http://en.wikipedia.org/wiki/NoSQL](http://en.wikipedia.org/wiki/NoSQL)
2.  要了解和学习更多的NoSQL数据库，请看：[http://nosql-database.org/](http://nosql-database.org/)
3.  NoSQL相关博客：[http://nosql.mypopescu.com/](http://nosql.mypopescu.com/)
4.  NoSQL相关博客：[http://blog.nosqlfan.com/](http://blog.nosqlfan.com/)
5.  redis设计与实现：[http://www.redisbook.com/en/latest/](http://www.redisbook.com/en/latest/)