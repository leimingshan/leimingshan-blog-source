---
title: JS new Date in firefox
tags:
  - JavaScript
id: 314
categories:
  - 开发
abbrlink: 618c536b
date: 2015-04-30 13:24:21
---

JavaScript的new Date()，传入格式化日期参数，在Firefox中会比较严格，例如以下代码
<pre class="lang:default decode:true">var date = new Date("2015-4-30");</pre>
在Chrome下运行成功，在Firefox下则是“Invalid Date”。

显然以下代码才是标准的格式
<pre class="lang:default decode:true ">var date = new Date("2015-04-30");</pre>
或者
<pre class="lang:default decode:true ">var date = new Date("2015/4/30");</pre>
&nbsp;