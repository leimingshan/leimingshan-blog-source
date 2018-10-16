---
title: docker attach的使用与正确退出
category: 开发
tags: docker
abbrlink: 7d17a93a
date: 2018-10-16 14:41:31
---
# Sample Dockerfile
后面的讲解，以该Dockerfile构建的镜像为例说明。
```
FROM openjdk:8-jre-alpine

MAINTAINER leimingshan

ADD xxl-sso-server-0.1.1-SNAPSHOT.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","/app.jar"]
```

xxl-sso-server-0.1.1-SNAPSHOT.jar是Spring Boot打包生成的可执行jar，内嵌tomcat容器，直接使用java -jar启动内嵌
的tomcat容器并监听8080端口，这里是最简单的一种制作Spring Boot程序镜像的方法，java -jar就是容器内的主进程，如果该
进程终止，容器也就相应退出。

另外一种常见的镜像制作方法，就是加入supervisor来管理进程，稍微重一些，适合管理容器内的多个进程，此时，java等进程是supervisord的子进程，Dockerfile中的命令也要变成
```
ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisord.conf"] 
```
supervisord也是容器内的进程命令，只要supervisord不退出，容器就不会退出。

Spring Boot相关的内容大家可以自行学习下，这里就不详述了。

# Detached vs foreground
Docker run命令运行一个容器的时候，有一个-d选项
>> -d=false: Detached mode: Run container in the background, print new container id

detached的意思是让容器在后台运行，同时与你当前终端的STDIN，STDOUT，STDERR分离，然后告诉你一个容器id。
执行该命令之后，我们只会看到一个容器id。

默认不指定-d选项的时候，即-d=false，容器在前台运行，此时容器
处于attached状态，或者说是foreground前台模式，容器内进程（一般是ENTRYPOINT指定的运行命令）的STDIN，STDOUT，STDERR
会与你当前的命令行终端连接，你就可以直接看到容器内进程执行时候的输出，而你的输入也会重定向到容器内进程。以示例镜像运行产生的容器来说，你的所有输入输出都是和java -jar进程关联的。

参考：https://docs.docker.com/engine/reference/run/#detached-vs-foreground
<!--more-->

# docker attach command
attach命令可以让你attach到一个处于detached状态的容器。
以示例来说，我们会重新连接到java -jar进程的输入输出，看到该进程打印到STDOUT和STDERR的内容，如果此时，你属于了CTRL-c，也就是SIGKILL信号会发送给容器，java进程退出，容器也相应退出。

此处往往让大家感到困惑，我也遇到了这个问题。大多数情况下，我们按CTRL-c，是想结束docker attach这个进程，并不是想结束我们正在运行的attach到的这个容器啊。我们还是想让容器继续运行的呀。

官方文档说，CTRL-p CTRL-q 按键序列可以实现这个功能啊，其实就是同时按CTRL+p+q，但我发现没用啊。
此时我们想detach容器，使容器重新回到detached状态，我这边却无法实现。

重点来了：
https://stackoverflow.com/questions/20145717/how-to-detach-from-a-docker-container

docker run -t -i → can be detached with ^P^Q and reattached with docker attach
docker run -i → cannot be detached with ^P^Q; will disrupt stdin
docker run → cannot be detached with ^P^Q; can SIGKILL client; can reattach with docker attach

参考：https://docs.docker.com/engine/reference/commandline/attach/

这里就很明显的，只有在run的时候用了-it，才可以用CTRL-p CTRL-q的按键序列进行detach。

原因是什么呢，这里就涉及到tty和docker tty的许多知识了，后面的文章再详细解释吧。

# docker logs
想看到容器的STDOUT和STDERR，并不需要attach到容器上，docker logs命令完全可以满足需求。

参考：https://docs.docker.com/engine/reference/commandline/logs/
