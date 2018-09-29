---
title: 自建Git服务的几个选择
tags: git
category: 开发
abbrlink: a9253739
date: 2018-09-12 15:38:51
---
两年前比较早的时候，git刚流行起来，私服方案记得只有一个，就是gitlab，gitlab自己提供累死github的公有服务器，也提供自己搭建
服务的功能，现在官方还有了docker image，可以说是非常方便了。

Gitlab的特点：
- 优点：功能很全，自带CI持续集成和Issue tracking。
- 缺点：比较重，配置要求高。建议独立部署，内存需求比较大。

最近偶尔看到了几个非常轻量的自建git服务，这里简单记录一下，以后也许会用得到：
1. Gogs。https://gogs.io/。GO语言开发，跨平台，支持docker。
2. Gitea。Gogs的社区开发维护版本。https://gitea.io/zh-cn/。
