---
title: Gradle执行外部命令
tags:
  - Gradle
id: 345
categories:
  - Java
abbrlink: 8d22131f
date: 2015-05-19 20:16:13
---

在Gradle的官方网站，学习的话首要看的是User Guide，其次就是[DSL Reference](http://gradle.org/docs/current/dsl/index.html)，我觉得后者其实更加重要，前者的内容比较基础，示例也比较精简，所以推荐到DSL里去看示例。

例如关于任务类型Copy和Exec的介绍。
<pre class="lang:default decode:true ">task stopTomcat(type:Exec) {
  workingDir '../tomcat/bin'

  //on windows:
  commandLine 'cmd', '/c', 'stop.bat'

  //on linux
  commandLine './stop.sh'

  //store the output instead of printing to the console:
  standardOutput = new ByteArrayOutputStream()

  //extension method stopTomcat.output() can be used to obtain the output:
  ext.output = {
    return standardOutput.toString()
  }
}
</pre>
这个示例就很好的介绍了在Gradle中如何执行外部命令。

&nbsp;