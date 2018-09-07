---
title: JavaScript的模块化编程
tags:
  - JavaScript
id: 406
categories:
  - 网站
abbrlink: 1ce136d4
date: 2015-08-31 15:32:02
---

[http://www.ruanyifeng.com/blog/2 ... ule_definition.html](http://www.ruanyifeng.com/blog/2012/10/asynchronous_module_definition.html)
为什么讲到JS的模块化编程，也是最近在应用Echarts图标库需要面对的一个问题。
可以参考Echarts引入的方法：
[http://echarts.baidu.com/doc/doc.html#](http://echarts.baidu.com/doc/doc.html#)引入ECharts
在之前的开发中，为了使用简便，直接采用第三种引入单文件的方式，比较简单，
&lt;script src="js/echarts-all.js"&gt;&lt;/script&gt;
就可以了，这样就引入了Echarts所有的图标和地图数据，这个文件是900KB，也就是说，
无论你在页面中使用几种图表，都需要加载接近1MB大小的JS库文件，效率还是比较低下的。
比较推荐的方法是参考JS的模块化编程和AMD规范，当然使用模块化编程主要不是为了解决
加载过多的问题，对于代码的规范和质量都有比较大的帮助。
Echarts本身使用了require.js，在前台开发中也可以尝试自己使用require.js。
简单介绍：[http://www.ruanyifeng.com/blog/2012/11/require_js.html](http://www.ruanyifeng.com/blog/2012/11/require_js.html)
官网：[http://requirejs.org/](http://requirejs.org/)

建议推荐一个SeaJS 的 CMD 规范，与 AMD 非常类似，在国内的影响力非常大，也非常易于使用，是国人开发的，在 github 上的更新、互动非常频繁。
[http://seajs.org](http://seajs.org/)
[https://github.com/seajs/seajs](https://github.com/seajs/seajs)

两个的区别
[http://www.zhihu.com/question/20351507/answer/14859415](http://www.zhihu.com/question/20351507/answer/14859415)

[http://segmentfault.com/a/1190000000733959](http://segmentfault.com/a/1190000000733959)