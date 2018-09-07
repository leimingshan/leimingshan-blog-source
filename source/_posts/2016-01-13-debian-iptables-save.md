---
title: 'Ubuntu, Debian中iptables规则保存和重启自动加载'
tags:
  - Debian
  - Ubuntu
id: 437
categories:
  - Linux
abbrlink: 7892b79a
date: 2016-01-13 22:01:31
---

在Debian中iptables命令输完之后会立刻生效，但重启之后配置就会消失，Debian提供了一个iptables-save程序快速保存配置。

通过iptables-save和iptables-restore可以让debian自动保存并在开机时自动加载iptables规则。

### 1、将iptables配置保存到/etc/iptables，这个文件名可以自己定义，与下面的配置一致即可

<pre class="lang:default decode:true">iptables-save &gt; /etc/iptables</pre>

### 2、创建并编辑自启动配置文件，内容为启动网络时恢复iptables配置

<pre class="lang:default decode:true">sudo vim /etc/network/if-pre-up.d/iptables</pre>
文件内容为：
<pre class="lang:default decode:true ">#!/bin/sh
/sbin/iptables-restore &lt; /etc/iptables</pre>
保存并退出。

之后系统每次启动时iptables就可以自动加载规则了。