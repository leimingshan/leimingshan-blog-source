---
title: Gradle与SonarQube的应用
tags:
  - Gradle
  - Java
  - SonarQube
id: 486
categories:
  - Java
abbrlink: 68467f3f
date: 2016-03-04 16:19:50
---

首先是[官方文档](http://docs.sonarqube.org/display/SONAR/Analyzing+with+SonarQube+Scanner+for+Gradle)。这里使用的是新的Gradle SonarQube plugin，注意与以往的Gradle Sonar和Runner插件区分，官方不推荐使用旧插件。

[SonarQube插件说明](https://plugins.gradle.org/plugin/org.sonarqube)

Github示例可以参考[java-gradle-simple](https://github.com/SonarSource/sonar-examples/tree/master/projects/languages/java/gradle/java-gradle-simple)，注意里面build.gradle脚本的写法，以及如何执行SonarQube的Task。

&nbsp;