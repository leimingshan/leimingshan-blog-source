---
title: PHP扩展相关
category: php
tags: php
abbrlink: c5803951
---
## PHP扩展的介绍
下面的内容大部分搜集与网络，或来自书籍的翻译，由我加以整理而成。   

PHP取得成功的一个重要原因就是他拥有大量的可用扩展。Web开发者无论有何种需求，这种需求很有可能在PHP发行包里找到。PHP发行包包括支持各种数据库，图形文件格式，压缩，XML技术扩展在内的众多扩展。   

有两个理由需要自己编写PHP扩展。第一个理由是：PHP需要支持一项她还未支持的技术。这通常包括包裹一些现成的C函数库，以便提供PHP接口。例如，如果一个叫FooBase的数据库已推出市场，你需要建立一个PHP扩展帮助你从PHP里调用FooBase的C函数库。这个工作可能仅由一个人完成，然后被整个PHP社区共享（如果你愿意的话）。第二个不是很普遍的理由是：你需要从性能或功能的原因考虑来编写一些商业逻辑。  
<!-- more -->
在PHP中构建扩展模块，主要有以下三种方式：

1.  External Modules：外部模块，也就是编译成共享库，用dl()函数动态加载。

    好处：  
    (1)不需要重新编译整个PHP  
    (2)PHP体积小，因为不需要编译进PHP

2.  Built-in Modules：编译进PHP  

    好处：  
    (1)不需要动态加载，模块在php脚本里面可以直接使用。  
    (2)不需要将模块编译成.so共享库，因为直接编译进PHP。

    缺点：  
    (1)对模块的每次小变动都需要重新编译整个PHP和重新部署，不适于生产环节。   
    (2)因为编译进PHP，所以PHP二进制文件较大，而且多占点内存.

3.  The Zend Engine：Zend核心里实现（参考Zend API）  


关于dl函数可以参考下面链接：  
[http://www.php.net/manual/zh/function.dl.php](http://www.php.net/manual/zh/function.dl.php)

可以不使用dl函数，而修改php.ini指定extension的加载，详细步骤如下：

1.  进入PHP源代码包的ext目录，使用扩展骨架(skeleton)构造器， 为扩展建立函数的第一步是写一个函数定义文件  test.proto，该函数定义文件定义了扩展对外提供的函数原形。该例中，定义函数只有一行函数原形

	`int hello(int a, int b)
	注意行后面没有分号`

2.  使用以下命令，生成对应的扩展目录test：

	`./ext_skel --extname=test --proto=test.proto`

3.  进入test目录，修改其中的config.m4文件

	`PHP_ARG_ENABLE(caleng_module, whether to enable test...`
	`[  --enable-test          Enable test support])`  
	`两行，删除前面的dnl。`

4.  修改test.c中的相关函数：

	`PHP_FUNCTION(hello)`

5.  编译和安装模块

	`phpize`  
	`./configure --with-php-config=/usr/local/php/bin/php-config`  
	`make & make install`

6.  在PHP中启用扩展，修改配置文件/usr/local/php/etc/php.ini，
加入 extension=test.so， 然后需要重启PHP或php-fpm。
这里也可以通过在PHP代码中通过dl命令来调用该模块

7.  测试扩展

	`<?php`  
	`echo hello(1,2);`  
	`phpinfo();`  
	`?>`
