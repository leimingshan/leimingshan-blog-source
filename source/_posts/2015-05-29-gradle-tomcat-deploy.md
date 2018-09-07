---
title: Gradle tomcat相关脚本
tags:
  - Gradle
  - Java
id: 364
categories:
  - Java
abbrlink: 8b16ca12
date: 2015-05-29 10:32:38
---

初用Gradle，也学习了不少东西，相比于之前的maven（虽然用的不多），但还是感受到了Gradle的许多特点和强大之处。除了常规的添加一些依赖，也尝试着自己写一些脚本。下面的跟tomcat相关的用例，主要用于部署测试环境，也借鉴了他人的一些思路，分享出来。
<pre class="lang:default decode:true ">// deploy to local tomcat server

// All extra properties must be defined through the "ext" namespace.
ext.tomcatHome = System.getenv()["CATALINA_HOME"]
ext.tomcatBin = tomcatHome + '/bin'
ext.tomcatStart = tomcatBin + '/startup'
ext.tomcatStop = tomcatBin + '/shutdown'
ext.tomcatWebapps = tomcatHome + '/webapps'

ant.condition(property: "os", value: "windows") { os(family: "windows") }
ant.condition(property: "os", value: "unix"   ) { os(family: "unix")    }

task checkTomcat &lt;&lt; {
    if (tomcatHome == null)
        throw new RuntimeException("Could not get TOMCAT home, please set CATALINA_HOME env virable first!")
    switch(ant.properties.os){
        case 'windows':
            println 'Running on windows.'
            tomcatStart += '.bat'
            tomcatStop += '.bat'
            break
        case 'unix':
            println 'Running on unix.'
            tomcatStart += '.sh'
            tomcatStop += '.sh'
            break
    }
    println "Using CATALINA_HOME: ${tomcatHome}"
    println "Using Tomcat start cmd: ${tomcatStart}"
    println "Using Tomcat stop cmd: ${tomcatStop}"
}

task deployLocal &lt;&lt; {
    println "copy war from ${buildDir}/libs into ${tomcatWebapps}"
    copy{
        from "${buildDir}/libs"
        into "${tomcatWebapps}"
        include '*.war'
    }
    //println "start tomcat !"
    //startTomcat.execute()
}

deployLocal.dependsOn checkTomcat

task startTomcat &lt;&lt; {
    exec {
        executable tomcatStart
    }
    println 'Start Tomcat server.'
    //store the output instead of printing to the console:
    standardOutput = new ByteArrayOutputStream()

    //extension method stopTomcat.output() can be used to obtain the output:
    ext.output = {
        return standardOutput.toString()
    }
    println 'Done.'
}

startTomcat.dependsOn checkTomcat

task stopTomcat &lt;&lt; {
    exec {
        executable tomcatStop
    }
    println 'Shutting down Tomcat server.'

    //store the output instead of printing to the console:
    standardOutput = new ByteArrayOutputStream()
    //extension method stopTomcat.output() can be used to obtain the output:
    ext.output = {
        return standardOutput.toString()
    }
    println 'Done.'
}

stopTomcat.dependsOn checkTomcat
</pre>
&nbsp;