---
title: NodeJS - How to update packages by npm
category: 开发
tags:
  - JavaScript
  - NodeJs
abbrlink: 595239dd
date: 2018-10-22 13:06:27
---
# NVM安装新版本node之后的 global packages 迁移
NVM安装新版本node之后，全局的package需要迁移，注意使用以下命令，这样就不用手动一个个安装global package了。
例如我今天安装了v8.12.0的node，之前的版本是v8.11.3，那么执行以下命令就可以了。
```
nvm install v8.12.0
nvm reinstall-packages v8.11.3  # 把在v8.11.3版本node下安装的全部global package安装到新的node下面
nvm uninstall v8.11.3
nvm install-latest-npm          # 顺便更新一下npm
```
<!--more-->
# 更新 global packages
npm update -g

参考：https://docs.npmjs.com/getting-started/updating-global-packages

# 更新 local packages
npm outdated 会列出所有可更新的 node_modules。
npm update 更新命令，只能按照package.js中标注的版本号，进行更新，所以每次都要改下package.js中的版本号为最新才能够更新，太麻烦。
那还有没有更好的办法呢，当然有，就是借助升级插件npm-check-updates

```
npm install -g npm-check-updates
ncu     # 查看全部需要更新的包和最新版本
ncu -a  # 更新全部依赖到最新版本，并覆盖package.js
```

参考：
- https://docs.npmjs.com/getting-started/updating-local-packages
- https://www.npmjs.com/package/npm-check-updates