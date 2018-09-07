---
title: Jenkins SonarQube的搭配使用
tags:
  - Java
  - Jenkins
  - SonarQube
id: 470
categories:
  - Java
abbrlink: 7a61ee38
date: 2016-03-04 09:20:14
---

Jenkins在Ubuntu环境下的安装配置都比较简单，在安装好Java JDK之后，使用
<pre class="">sudo apt-get install jenkins</pre>
即可安装。因为没有用到后台数据库，配置过程一般就是配置端口号，以及Nginx或Apache server的代理即可。

详细方法可以参考[官方安装指南](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu)。

接下来就是与代码质量分析平台SonarQube的结合使用，前面已经说明了SonarQube的安装，然后就是利用Jenkins在进行持续集成的过程中，进行代码质量分析、代码覆盖率分析，并将相关数据和报告通知给SonarQube。

在Jenkins中的“系统管理”-“管理插件”中搜索安装SonarQube Plugin，因为我使用的是Java Gradle工程和JaCoCo测试报告，所以之前也安装了Gradle Plugin和JaCoCo Plugin，这里大家可以根据自己具体的项目选择。<!--more-->

在安装好SonarQube Plugin之后，记得在系统设置中配置SonarQube服务器的相关信息，可以参考下图进行。

[![SonarQube Plugin](http://www.leimingshan.com/wp-content/uploads/2016/03/SonarQube-Plugin-300x91.jpg)](http://www.leimingshan.com/wp-content/uploads/2016/03/SonarQube-Plugin.jpg)

另外注意配置SonarQube scanner，这里可以选择自动安装，或者选择自己安装的目录位置。

[![SonarQube Runner](http://www.leimingshan.com/wp-content/uploads/2016/03/SonarQube-Runner-300x51.jpg)](http://www.leimingshan.com/wp-content/uploads/2016/03/SonarQube-Runner.jpg)

服务器配置好之后，然后就是在具体的项目中配置构建过程，选择“增加构建步骤”中的Invoke Standalone SonarQube Analysis，参考下图。

[![SonarQube Analysis](http://www.leimingshan.com/wp-content/uploads/2016/03/SonarQube-Analysis-300x131.jpg)](http://www.leimingshan.com/wp-content/uploads/2016/03/SonarQube-Analysis.jpg)

具体的配置如下：
<pre class="lang:default decode:true "># required metadata
sonar.projectKey=pminer:MongoDB-ImportXMLProfile
sonar.projectName=MongoDB-ImportXMLProfile
sonar.projectVersion=1.0

# path to source directories (required)
sonar.sources=src/main/java
# path to test source directories (optional)
sonar.tests=src/test/java

sonar.java.binaries=build/classes

sonar.language=java

#Tells SonarQube where the unit tests execution reports are
sonar.junit.reportsPath=reports/tests

#Tells SonarQube where the unit tests code coverage report is
sonar.jacoco.reportPath=build/jacoco/test.exec

# Encoding of the source files
sonar.sourceEncoding=UTF-8</pre>
注意以上的配置要根据自己具体的项目路径配置。

这样在下次的构建中，就会之前SonarQube的分析任务，并将结果发送给SonarQube服务器，然后访问服务器平台就能看到代码的质量报告。

参考：[http://docs.sonarqube.org/display/PLUG/Code+Coverage+by+Unit+Tests+for+Java+Project](http://docs.sonarqube.org/display/PLUG/Code+Coverage+by+Unit+Tests+for+Java+Project)