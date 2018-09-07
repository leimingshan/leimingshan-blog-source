---
title: SonarQube-代码质量管理平台的安装
tags:
  - Java
  - SonarQube
id: 461
categories:
  - 开发
abbrlink: 25a76de9
date: 2016-02-25 13:49:30
---

## 安装依赖

JDK，数据库（以下以MySQL为例），操作系统支持Linux和Windows（文章以Linux Ubuntu 14.04为例）。

## 数据库配置

终端进入mysql-client：
<pre class="lang:default decode:true ">mysql -u root -p</pre>
执行以下SQL语句建立数据库和相关用户：
<pre class="lang:default decode:true">CREATE DATABASE sonar CHARACTER SET utf8 COLLATE utf8_general_ci;
CREATE USER 'sonar' IDENTIFIED BY 'sonar';
GRANT ALL ON sonar.* TO 'sonar'@'%' IDENTIFIED BY 'sonar';
GRANT ALL ON sonar.* TO 'sonar'@'localhost' IDENTIFIED BY 'sonar';
FLUSH PRIVILEGES;</pre>
<!--more-->

## 下载并解压SonarQube安装包

在[SonarQube官网](http://www.sonarqube.org/downloads/)获取最新的下载地址。
<pre class="lang:default decode:true ">wget https://sonarsource.bintray.com/Distribution/sonarqube/sonarqube-5.3.zip
unzip sonarqube-5.3.zip
sudo mv sonarqube-5.3 /usr/local/sonar</pre>

## 编辑配置文件sonar.properties

编辑conf目录下的sonar.properties，主要修改数据库配置和web server配置，取消相应行的注释并编辑为对应的值。
<pre class="lang:default decode:true ">sonar.jdbc.username=sonar
sonar.jdbc.password=sonar
sonar.jdbc.url=jdbc:mysql://localhost:3306/sonar?useUnicode=true&amp;characterEncoding=utf8&amp;rewriteBatchedStatements=true&amp;useConfigs=maxPerformance
</pre>
以下的web server配置允许以下地址访问 http://127.0.0.1:9000/sonar
<pre class="lang:default decode:true ">sonar.web.host=127.0.0.1 #默认是0.0.0.0，绑定本机所有ip地址
sonar.web.context=/sonar #默认是空
sonar.web.port=9000</pre>

## 配置Service运行

参考[官方文档](http://docs.sonarqube.org/display/SONAR/Running+SonarQube+as+a+Service+on+Linux)

新建/etc/init.d/sonar文件并编辑如下。
<pre class="lang:default decode:true ">#!/bin/sh
#
# rc file for SonarQube
#
# chkconfig: 345 96 10
# description: SonarQube system (www.sonarsource.org)
#
### BEGIN INIT INFO
# Provides: sonar
# Required-Start: $network
# Required-Stop: $network
# Default-Start: 3 4 5
# Default-Stop: 0 1 2 6
# Short-Description: SonarQube system (www.sonarsource.org)
# Description: SonarQube system (www.sonarsource.org)
### END INIT INFO

/usr/bin/sonar $*</pre>
运行以下命令安装服务并运行，注意bin子目录的32位64位区别。
<pre class="lang:default decode:true">sudo ln -s $SONAR_HOME/bin/linux-x86-64/sonar.sh /usr/bin/sonar
sudo chmod 755 /etc/init.d/sonar
sudo update-rc.d sonar defaults
sudo service sonar start</pre>
&nbsp;