---
title: Gradle Check OS
tags:
  - Gradle
  - Java
id: 339
categories:
  - 开发
abbrlink: 52e12c32
date: 2015-05-15 21:40:53
---

最近使用Gradle来构建一个Spring Project，相比于传统的maven，Gradle确实有不少优点，但由于Gradle是基于Groovy语音，初用起来还是有许多不熟悉的地方，其实在简单的使用时，IDE会帮我们完成build.gradle中大多数的内容，我们也就可以直接完成构建构成了，再加上我的项目使用了Spring-boot，加上Gradle插件后，并不需要自己去写太多的脚本。总之，即使你没接触过maven，Gradle也是很容易使用和上手的。

今天遇到的一个问题，就是在脚本中判断你当前使用的操作系统，因为我这里要设置Tomcat的路径，在windows和Linux之间还是有很大区别的，基本方法如下：

利用ant
<pre class="lang:default decode:true ">ant.condition(property: "os", value: "windows") { os(family: "windows") }
ant.condition(property: "os", value: "unix"   ) { os(family: "unix")    }

task checkOS &lt;&lt; {
    switch(ant.properties.os){
        case 'windows':
            println 'This is windows.'
            break
        case 'unix':
            println 'This is unix.'
            break
    }
}</pre>
<pre class="lang:default decode:true ">import org.apache.tools.ant.taskdefs.condition.Os
task checkWin() &lt;&lt; {
    if (Os.isFamily(Os.FAMILY_WINDOWS)) {
        println "WINDOWS "
    }
}</pre>
利用系统属性
<pre class="lang:default decode:true ">task checkOS &lt;&lt; {
    if (System.properties['os.name'].toLowerCase().contains('windows')) {
        println "it's Windows"
    } else {
        println "it's not Windows"
    }
}</pre>
&nbsp;