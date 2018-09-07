---
title: Docker-Basic
tags: Docker
abbrlink: a02e5c6b
date: 2018-01-25 15:45:10
---

鉴于生产环境的上线部署，都依赖于镜像构建、制作、部署上线运行等操作，作为一名合格的RD，当然不能只局限于在上线平台上进行一顿鼠标操作了，一定要弄懂这些基础设施内部到底在干嘛。因此，对Docker的相关学习也是很有必要的。

# 基础信息
http://dockone.io/article/783

http://merrigrove.blogspot.com/2015/10/visualizing-docker-containers-and-images.html

两篇文章分别是中文和英文原版，建议初学者多读几遍，收获非常大。
尤其是对镜像只读层和读写层的理解，非常重要，还有docker各个命令对各层的影响。
<!-- more -->
# Docker run
https://docs.docker.com/engine/reference/commandline/run/

docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

常用选项：
- --detach , -d		Run container in background and print container ID
- --tty , -t		Allocate a pseudo-TTY
- --interactive , -i		Keep STDIN open even if not attached
- --publish , -p		Publish a container’s port(s) to the host
- --volume , -v		Bind mount a volume


```
// sshd -D 将 sshd 作为前台进程运行，而不是脱离控制台成为后台守护进程。主要用于调试。
// -it 通常一起使用，可以开启一个终端进去交互模式，调试时很有用。
docker run -d -p 2222:22 tomcat:centos /usr/sbin/sshd -D

docker run -ti -v ~/Downloads:/Downloads tomcat:centos /bin/bash

docker run -d -p 8000:8080 -p 1098:1099 tomcat:centos /usr/local/sbin/tomcat.sh

docker run -it -p 8000:8080 -p 1098:1099 tomcat:centos /usr/local/sbin/tomcat.sh
```

# 其他Docker命令
## 概览
- docker version
- docker info
- docker stop $(docker ps -aq)
- docker rm $(docker ps -aq)
- docker pull
- docker login
- docerk rmi
- docker images


## 镜像类
```
# 检索image
$docker search image_name

# 下载image
$docker pull image_name

# 列出镜像列表; -a, --all=false Show all images; --no-trunc=false Don't truncate output; -q, --quiet=false Only show numeric IDs
$docker images

# 删除一个或者多个镜像; -f, --force=false Force; --no-prune=false Do not delete untagged parents
$docker rmi image_name

# 显示一个镜像的历史; --no-trunc=false Don't truncate output; -q, --quiet=false Only show numeric IDs
$docker history image_name
```

## 容器类
```
# 列出当前所有正在运行的container
$docker ps
# 列出所有的container
$docker ps -a
# 列出最近一次启动的container
$docker ps -l

# 保存对容器的修改; -a, --author="" Author; -m, --message="" Commit message  
$docker commit ID new_image_name

# 删除所有容器
$docker rm `docker ps -a -q`
  
# 删除单个容器; -f, --force=false; -l, --link=false Remove the specified link and not the underlying container; -v, --volumes=false Remove the volumes associated to the container
$docker rm Name/ID

# 停止、启动、杀死一个容器
$docker stop Name/ID
$docker start Name/ID
$docker kill Name/ID

# 从一个容器中取日志; -f, --follow=false Follow log output; -t, --timestamps=false Show timestamps
$docker logs Name/ID
  
# 列出一个容器里面被改变的文件或者目录，list列表会显示出三种事件，A 增加的，D 删除的，C 被改变的
$docker diff Name/ID
  
# 显示一个运行的容器里面的进程信息
$docker top Name/ID

# 从容器里面拷贝文件/目录到本地一个路径  
$docker cp Name:/container_path to_path
$docker cp ID:/container_path to_path

# 重启一个正在运行的容器; -t, --time=10 Number of seconds to try to stop for before killing the container, Default=10
$docker restart Name/ID

# 附加到一个运行的容器上面; --no-stdin=false Do not attach stdin; --sig-proxy=true Proxify all received signal to the process  
$docker attach ID
```

# Dockerfile
to be continued
