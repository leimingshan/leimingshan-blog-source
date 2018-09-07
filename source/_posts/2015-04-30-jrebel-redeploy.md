---
title: Java热部署工具-JRebel
tags:
  - Java
id: 308
categories:
  - Java
abbrlink: 95ea9c90
date: 2015-04-30 10:43:57
---

之前看了一篇文章，讲5个比较优秀的Java项目，文章虽然比较老，但是对里面的Neo4J，Gradle和JRebel都有了不少了解。

最近开发一个Java Web项目，经常需要部署到Tomcat上进行调试，但是每次修改代码或者配置文件后，都需要重新部署并重启Tomcat容器，实在是很浪费时间的一项操作。于是决定尝试一下JRebel，实现热部署，对程序的修改可以直接反应在部署的程序上。

可惜的是，JRebel是一个收费软件，个人可以申请30天的免费试用，可以使用myJRebel，https://my.jrebel.com/，是JRebel的一个社区计划，会要求你允许分享一些使用信息，在这里可以免费申请一个key，但需要定期联网激活。

JRebel对各种IDE都有相应的插件，使用起来非常方便，具体的信息可以查看http://zeroturnaround.com/software/jrebel/learn/。选择JRebel monitor的项目，并在Tomcat server中配置使用即可。启动Tomcat后可以看到JRebel的信息，例如
<pre class="lang:default decode:true">2015-04-30 10:26:45 JRebel:  
2015-04-30 10:26:45 JRebel:  #############################################################
2015-04-30 10:26:45 JRebel:  
2015-04-30 10:26:45 JRebel:  JRebel Agent 6.1.2 (201504061000)
2015-04-30 10:26:45 JRebel:  (c) Copyright ZeroTurnaround AS, Estonia, Tartu.
2015-04-30 10:26:45 JRebel:  
2015-04-30 10:26:45 JRebel:  Over the last 6 days JRebel prevented
2015-04-30 10:26:45 JRebel:  at least 166 redeploys/restarts saving you about 6.7 hours.
2015-04-30 10:26:45 JRebel:  
2015-04-30 10:26:45 JRebel:  Licensed to Mingshan Lei (using myJRebel).
2015-04-30 10:26:45 JRebel:  
2015-04-30 10:26:45 JRebel:  
2015-04-30 10:26:45 JRebel:  #############################################################
2015-04-30 10:26:45 JRebel:</pre>
使用效果确实非常好，JRebel支持许多框架，常用的Spring、myBatis、Struts等等，可以monitor这些框架的配置文件，修改之后会在控制台显示提示信息，Reload class或者Reload SQL map等，确实可以节约许多用于部署的时间。