---
title: MySQL database character set encoding
tags:
  - MySQL
id: 492
categories:
  - 数据库
abbrlink: f528791e
date: 2016-03-08 11:28:16
---

使用MySQL的时候很可能遇到字符集编码相关的问题，尤其是涉及到数据和程序中有中文字符的时候，如果不注意，可能遇到乱码或一些其他的错误。

本文详细解释MySQL相关的字符集编码设置和排序规则相关的问题。

# MySQL Server的默认字符集配置

在默认安装MySQL的时候，MySQL Server使用的是英文字符集，服务端的默认配置一般是

<span style="text-decoration: underline;">character-set-server=latin1</span>

<span style="text-decoration: underline;">collation-server =latin1_swedish_ci</span>

注意latin1字符集是不支持中文的。第一行的character-set当然是指字符集，第二行的collation是指对应该字符集的比较和排序规则。

[charset-server参考手册](http://dev.mysql.com/doc/refman/5.7/en/charset-server.html)<!--more-->

通过MySQL命令
<pre class="lang:mysql decode:true">mysql&gt; SHOW VARIABLES LIKE 'character%';</pre>
可以查看当前服务端的默认配置。

如果在新建数据库的时候不指定character-set和collation，那么就会采用以上的服务器端默认值，所以还是推荐大家手动指定。示例如下：
<pre class="lang:mysql decode:true ">CREATE DATABASE mydb
  DEFAULT CHARACTER SET utf8
  DEFAULT COLLATE utf8_general_ci;</pre>
使用utf8和utf8_general_ci是在中英文应用环境下比较常用的一种设置，排序规则还有utf8_unicode_ci，另外还有编码utf8mb4和对应的排序规则，具体区别会在后面的文章说明。<!--more-->

# 修改已有数据库的字符编码

如果之前已经建立好了数据库，需要修改当前数据库的编码，可以使用ALTER DATABASE命令。

首先查看当前数据库的编码和排序规则；
<pre class="lang:mysql decode:true ">mysql&gt; USE mydb;
Database changed
mysql&gt; SHOW VARIABLES LIKE 'character_set_database';
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| character_set_database | utf8  |
+------------------------+-------+
1 row in set (0.00 sec)

mysql&gt; mysql&gt; SHOW VARIABLES LIKE 'collation_database';
+--------------------+-----------------+
| Variable_name      | Value           |
+--------------------+-----------------+
| collation_database | utf8_general_ci |
+--------------------+-----------------+
1 row in set (0.01 sec)

mysql&gt; 
</pre>
然后就可以根据情况修改为自己需要的编码设置了；
<pre class="lang:mysql decode:true">mysql&gt; ALTER DATABASE databasename CHARACTER SET utf8 COLLATE utf8_general_ci;</pre>
参考：

*   [数据库的字符集](https://dev.mysql.com/doc/refman/5.7/en/charset-database.html)
*   修改具体table编码的方法，[charset-unicode-upgrading参考手册](https://dev.mysql.com/doc/refman/5.7/en/charset-unicode-upgrading.html)。

# 修改MySQL的服务端配置

修改my.cnf配置文件可以修改MySQL Server的默认字符集等设置。以配置文件在/etc/my.cnf（可能根据具体安装情况不同）为例，修改以下几项即可：
<pre class="lang:default decode:true">[client]
default-character-set = utf8

[mysql]
default-character-set = utf8

[mysqld]
init-connect = 'SET NAMES utf8'
character-set-server = utf8
collation-server = utf8_unicode_ci</pre>
参考

*   配置方法：[http://stackoverflow.com/questions/3513773/change-mysql-default-character-set-to-utf-8-in-my-cnf](http://stackoverflow.com/questions/3513773/change-mysql-default-character-set-to-utf-8-in-my-cnf)
*   [SET NAMES的官方解释](https://dev.mysql.com/doc/refman/5.7/en/charset-connection.html)
&nbsp;