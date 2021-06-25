---
title: Hexo upgrade
category: 开发
tags:
  - JavaScript
  - NodeJs
abbrlink: d9017f30
date: 2021-06-25 21:58:36
---
太久没回来更新的后果之一，就是整个博客的技术栈已经完全outdate了，需要进行大的升级，NodeJS要升级，Hexo和Next theme都要进行几个大版本的升级。几个简要的步骤记录下吧，用得上的同学自取哦。
<!--more-->
# Node的升级
因为之前用的nvm，就比较简单，

```
nvm ls-remote
nvm install --lts     #安装最新的lts版本吧，比较推荐
nvm ls                #查看现在安装的哪些版本
nvm uninstall v8.12.0 #找到旧的版本删除掉吧
```

# Hexo的升级
1. 全局升级hexo-cli，先hexo version查看当前版本，然后npm install -g hexo-cli，再次hexo version查看是否升级成功。如果hexo不能直接识别运行，改为npx hexo。
2. 使用npm-check，检查系统中的插件是否有升级的。
3. 使用npm-upgrade，升级系统中的相关插件。
4. npm update -g，检查升级npm本身。

```
npm install -g hexo-cli
hexo version

npm install -g npm-check
npm-check

npm install -g npm-upgrade
npm-upgrade

npm update -g
npm install -g npm

hexo clean #清理hexo数据并重新生成页面并部署
hexo g -s
hexo d
```

经过以上这些命令，整体翻新就差不多了，theme的升级，我这边是删除了原来的主题目录，重新进行了拉取，注意保留配置文件，重新配置即可，就不详述了。
