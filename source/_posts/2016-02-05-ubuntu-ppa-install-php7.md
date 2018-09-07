---
title: Ubuntu14.04使用ppa源安装PHP-7
tags:
  - Linux
  - PHP
  - Ubuntu
id: 449
categories:
  - Linux
abbrlink: bf78be32
date: 2016-02-05 14:03:12
---

在Ubuntu 14.04下安装PHP除了可以直接从官网下载源码编译安装，也可以PPA源安装。如果读者对编译安装的各种选项和配置方法不是很熟悉的话，则推荐使用这种方法快速安装。

在安装的时候，这里选择的是比较流行的一位个人作者维护的一个PPA源，具体的使用方法如下：

1.  添加源。
<pre class="lang:default decode:true">sudo LC_ALL=en_US.UTF-8 add-apt-repository ppa:ondrej/php</pre>

2.  如果有之前使用apt-get方法安装的PHP，先删除后再安装PHP7。
<pre class="lang:default decode:true">sudo apt-get update
sudo apt-get purge php5-common -y
sudo apt-get install php7.0 php7.0-fpm php7.0-mysql -y
sudo apt-get --purge autoremove -y</pre>

3.  如果使用nginx，注意以下配置和相应的用户权限。
<pre class="lang:default decode:true ">fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;</pre>

参考：

*   [How To Upgrade to PHP 7 on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-upgrade-to-php-7-on-ubuntu-14-04)
*   [How to install PHP 7?](http://askubuntu.com/questions/705880/how-to-install-php-7)