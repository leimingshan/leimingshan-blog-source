---
title: Hexo进阶设置
abbrlink: a62b0c3d
date: 2018-09-07 16:21:04
tags:
---
# 静态代码压缩
因为Blog中都是静态页面，基本都可以压缩优化，针对html，css，js，图片进行。
这里没必要用gulp去压缩，配置太繁琐，也没法自动化。
直接使用[hexo-all-minifier](https://github.com/chenzhutian/hexo-all-minifier)这个模块，
安装：
> npm install hexo-all-minifier --save

增加配置：
> all_minifier: true

搞定！

# 文章唯一链接
hexo-abbrlink

# 文章字数统计和阅读时长
[hexo-symbols-count-time](https://github.com/theme-next/hexo-symbols-count-time)
可以替代老的hexo-wordcount。

# SEO-搜索引擎收录和优化
利用插件生成sitemap，hexo自带的两个插件，百度搜索要使用单独的一个。
<!--more-->
```
npm install hexo-generator-sitemap --save     
npm install hexo-generator-baidu-sitemap --save
```
修改根目录中的_config.yml，url必须要修改成对应的，会体现在sitemap.xml里的url里。增加配置项。
```
# Sitemap
sitemap:
  path: sitemap.xml
baidusitemap:
  path: baidusitemap.xml
```
之后hexo重新生成部署，就可以看到两个站点地图的xml文件了。

## Google收录
Google站点平台：https://www.google.com/webmasters/
验证站点可以用html文件的方式，html文件放在hexo的source目录后，一定要在开头添加
```
---
layout: false
---
```
这样hexo才可以正确处理该文件。

之后提交sitemap即可，配置比较简单，不再赘述。

参考资料：
- http://muyunyun.cn/posts/f55182c5/
- https://blog.csdn.net/sunshine940326/article/details/70936988