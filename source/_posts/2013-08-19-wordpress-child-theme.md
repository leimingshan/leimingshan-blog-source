---
title: wordpress 子主题
tags:
  - web
  - wordpress
id: 158
categories:
  - 网站
abbrlink: cc9a0d9c
date: 2013-08-19 19:07:15
---

本站采用的是wordpress官方发布的TwentyEleven主题，官方会发布主题更新，但是如果你对主题文件进行了修改的话，每次更新后，原来的修改就丢失了，解决问题的最好方法就是采用子主题（Child Theme）。

# Child Theme

创建一个子主题是很简单的。

在wp-content/themes/建立新的文件夹，例如建立 twentyeleven-childen。

创建一个目录，将格式编写正确的 _style.css_ 文件放进去，一个子主题就做成了！只需要对 HTML 和CSS 具有基本的了解，您就可以通过创建一个非常基本的子主题 来对一个父主题的样式和布局进行修改和扩展，而不需要对父主题的文件作任何修改。通过这样的方式，当父主题被更新的时候，您所做的修改就可以保存下来。

**因为这个原因，我们强烈推荐您使用子主题的方式来对主题进行修改。<!--more-->**

## 必需的style.css文件

_style.css_是一个子主题唯一**必须的**文件。它的头部提供的信息让WordPress辨认出子主题，并且**重写父主题中的style.css文件**。

对于任何WordPress主题，头部信息必须位于文件的顶端，唯一的区别就是子主题中的`Template:`行是必须的，因为它让WordPress知道子主题的父主题是什么。

下面是一个_style.css_文件的头部信息的示例：
<pre>/*
Theme Name:     Twenty Ten Child
Theme URI:      http: //example.com/
Description:    Child theme for the Twenty Ten theme 
Author:         Your name here
Author URI:     http: //example.com/about/
Template:       twentyten
Version:        0.1.0
*/</pre>
逐行的简单解释：

*   `Theme Name`. (**必需**) 子主题的**名称**。
*   `Theme URI`. (可选) 子主题的主页。
*   `Description`. (可选) 子主题的描述。比如：我的第一个子主题，真棒！
*   `Author URI`. (可选) 作者主页。
*   `Author`. (optional) 作者的名字。
*   `Template`. (**必需**) 父主题的目录名，区别大小写。 **注意：** 当你更改子主题名字时，要先换成别的主题。
*   `Version`. (可选) 子主题的版本。比如：0.1，1.0，等。
`*/ `这个关闭标记的后面部分，就会按照一个常规的样式表文件一样生效，你可以把你想对WordPress应用的样式规则都写在它的后面。

要注意的是，子主题的样式表会替换父主题的样式表而生效。（事实上WordPress根本就不会载入父主题的样式表。）所以，如果你想简单地改变父主题中的一些样式和结构——而不是从头开始制作新主题——你必须明确的导入父主题的样式表，然后对它进行修改。下面的例子告诉你如何使用`@import`规则完成这个。例如
<pre class="">@import url("../twentyten/style.css");</pre>

##  使用 functions.php

不像_style.css_，子主题中的_functions.php_不会覆盖父主题中对应功能，而是将新的功能加入到父主题的_functions.php_中。（其实它会在**父主题文件加载之前先载入**。）

完整的Child Theme說明可以參考[英文說明](http://codex.wordpress.org/Child_Themes)或[簡體中文說明](http://codex.wordpress.org/zh-cn:%E5%AD%90%E4%B8%BB%E9%A1%8C)。