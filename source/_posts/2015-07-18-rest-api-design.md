---
title: Rest api design
id: 391
categories:
  - 开发
abbrlink: ed4e4712
date: 2015-07-18 17:39:57
tags:
---

最近在进行一个项目，需要设计一个Server，为移动端的client提供REST风格的服务，RESTful API是现在互联网应用程序中使用比较多、也比较成熟的一种设计理念，以HTTP协议为基础，在项目之前，只是简单听说过REST这种理念，并没有很深入的接触，在api的设计过程中，需要遵循一些原则，但是要注意的是RESTful只是一种风格，并不是标准、协议或者强制性的要求。因此，在设计过程中，我也参考了一些文章和资料，学习如何去设计一套合理规范的API。

在初步的设计中，根据之前的工作经验，采用了比较传统的Spring和Hibernate，根据应用的需求，也采用了许多相关技术，例如Spring-data-jpa和Hibernate Validator等，后面会根据一些具体内容进行总结。

下面地址是一个java轻量级restful框架，里面有不少关于RESTful的介绍和文章链接，推荐阅读。

https://github.com/Dreampie/resty