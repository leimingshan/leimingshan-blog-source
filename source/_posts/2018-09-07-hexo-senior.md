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


参考资料：
- http://muyunyun.cn/posts/f55182c5/
- https://blog.csdn.net/sunshine940326/article/details/70936988