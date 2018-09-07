---
title: win10-kms-openwrt
id: 409
categories:
  - 网站
abbrlink: 6a1980de
date: 2015-10-03 22:58:33
tags:
---

http://www.right.com.cn/forum/thread-174651-1-2.html

http://www.right.com.cn/forum/thread-174287-1-1.html

http://www.right.com.cn/forum/thread-159625-1-1.html

openwrt luci

具体安装位置如下：
<pre class="lang:default decode:true ">/etc/vlmcsd.ini
/usr/sbin/vlmcsd
</pre>
[vlmcsd-svn812-2015-08-30-Hotbird64](http://www.leimingshan.com/wp-content/uploads/2015/10/vlmcsd-svn812-2015-08-30-Hotbird64.7z)

Password: 2015

具体激活使用方法：
<pre class="lang:default decode:true ">slmgr.vbs /skms 192.168.1.1
slmgr.vbs /ipk XXXXX-XXXXX-XXXXX-XXXXX-XXXXX
slmgr.vbs /ato
slmgr.vbs /xpr

cd C:\Program Files\Microsoft Office\Office15
cscript ospp.vbs /sethst:192.168.1.1
cscript ospp.vbs /act
cscript ospp.vbs /dstatus</pre>
<!--more-->

<span style="color: #ff0000;">更新下载地址，</span>
<span style="color: #ff0000;">国内地址：</span><span style="color: #336699;"><u>http://download.3kkk.org:90/os/vlmcsd-svn812-2015-08-30-Hotbird64.7z</u></span><span style="color: #336699;"><u>官方地址：[http://rghost.net/6G8wYxwnX](http://rghost.net/6G8wYxwnX)
</u></span><span style="color: #ff0000;">官方站点：[http://forums.mydigitallife.info ... n-Windows-platforms](http://forums.mydigitallife.info/threads/50234-Emulated-KMS-Servers-on-non-Windows-platforms)</span>
<span style="color: #ff0000;">解压密码 2015 </span>
<span style="color: #000000;">压缩包指纹：
MD50f1a9489ccad4a77d3b8f3ee893ede91</span>
<span style="color: #000000;">SHA1f4daa5cb5f3c4f170c0d343982ce2c1a6a88376f</span>
<span style="font-size: x-large;"><span style="color: #ff0000;">支持windows 10 激活 ，office 2016亲测 可用。</span></span>
<span style="font-size: x-large;"><span style="color: #ff0000;">说明:现在下载到的一般是零售版，是不能用kms激活的，需要转换成批量授权的才行。</span></span>
<span style="font-size: x-large;"><span style="color: #ff0000;">附转换工具，就是替换几个文件，下载地址：</span></span>[<span style="font-size: x-large;">点我下载</span>](http://download.3kkk.org:90/os/office2016%20Retail%e8%bd%acVOL%e5%b9%b6%e6%bf%80%e6%b4%bb.rar)

下载附件
把附件的三个文件传到路由器上
创建目录
mkdir /opt/kms  （随意自己定位置。位置不限）
复制三个文件 vlmcsd kmsserver.pid kmsserver.ini 到 /opt/kms
给vlmcsd 添加执行权限 chmod a+x vlmcsd
启动kms服务
/opt/kms/vlmcsd -i /opt/kms/kmsserver.ini -p /opt/kms/kmsserver.pid
测试服务器
在window 下进入cmd 切换到kmsclient.exe 目录
运行
kmsclient 1688 192.168.1.1 Windows
kmsclient 1688 192.168.1.1 Office2010
kmsclient 1688 192.168.1.1 Office2013
分别测试 类似这样就是成功了.
KMS Client Emulator started successfully.
Successfully received response from KMS Server.
KMS Server PID: 55041-00096-200-625305-03-1049-7601.0000-057201.
Activation request (KMS V4.0) 1 of 1 sent.

&nbsp;

附件下载 ![](http://www.right.com.cn/forum/static/image/filetype/rar.gif) <span id="attach_103839">[路由器kms.rar](http://www.right.com.cn/forum/forum.php?mod=attachment&amp;aid=MTAzODM5fGFmZDVmYWMzfDE0NDY5OTcyMTZ8MHwxNTk2MjU%3D) _(201.85 KB, 下载次数: 505)_ </span>
补充说明一下：官方下载下来是一个5M多的压缩包，里面包含了几乎所有平台的以及源代码
binaries 目录里面是编译好的二进制软件，可以直接运行的，针对不同系统分了几个目录 ，
这几个目录很明了不用解释了。路由器系统 包括 ddwrt openwrt tomato 等都是基于Linux的系统
选择Linux目录。这里有根据cpu不同分了几个目录
arm 有一部分路由器是目前相对少一些
Intel  很熟了 x86的一般很少有成品的路由器 ，很多都是软路由，
mips  绝大数的路由器都是mips 的  bcm artheros的基本都是
ppc  sparc32 这个很少见了，不是主流的，一些特殊设备
进入mips 目录又有2个目录 big-endian  little-endian
俗称 大端 小端 ，意思是一个数据在内存地址中按什么样的顺序存储
大体意思<span style="color: #ff0000;">小端 高位 存在高地址 低位存在低地址 大端 高位存低地址 低位存高地址 </span><span style="color: #000000;">详情百度</span>
<span style="color: #000000;">不同cpu 系统 使用的方式不一样，我知道的不全不一定准确</span>
<span style="color: #000000;">至少</span><span style="color: #ff0000;"> mips 的ar的cpu是大端的 big-endian </span><span style="color: #000000;">。选择big-endian</span>
<span style="color: #333333;"><span style="font-family: arial, 宋体, sans-serif;">我们常用的X86结构是小端模式，而KEIL C51则为大端模式。很多的ARM，DSP都为小端模式。有些ARM处理器还可以由硬件来选择是大端模式还是小端模式。</span></span>
<span style="font-family: arial, 宋体, sans-serif;"><span style="color: #333333;">大小端序 还和系统有关 具体情况具体分析</span></span>
<span style="font-family: arial, 宋体, sans-serif;"><span style="color: #333333;">big-endian目录里面有分几个目录</span></span>
<span style="font-family: arial, 宋体, sans-serif;"><span style="color: #333333;">这里是根据使用的c语言运行库来区分的</span></span>
<span style="font-family: arial, 宋体, sans-serif;"><span style="color: #333333;">x86的Linux系统 一般都是用的glibc 这个库</span></span>
<span style="font-family: arial, 宋体, sans-serif;"><span style="color: #333333;">嵌入式的Linux 用的是uclibc 这个库</span></span>
<span style="font-family: arial, 宋体, sans-serif;"><span style="color: #333333;">static 是静态的意思，这里软件不依赖共享的运行库 自己用的自己带了。但是体积大了。</span></span>
<span style="font-family: arial, 宋体, sans-serif;"><span style="color: #333333;">openwrt 之类都是用的uclibc这个库</span></span>
进入uclibc这个目录 就是软件了
ar71xx 91xx的就选择vlmcsd-mips32r2-openwrt-atheros-ar7xxx-ar9xxx-uclibc这个

vlmcs 是客户端测试 vlmcs − a client for testing and/or charging KMS servers
vlmcsd 是一个完整的kms激活服务器 vlmcsd - a fully Microsoft compatible KMS server
vlmcsd 包含上面2个的功能 vlmcsdmulti - a multi-call binary containing vlmcs(1) and vlmcsd(8)
只用来做激活服务器选用vlmcsd就可以了。

激活方法及命令：
**<span style="font-size: x-large;">Windows 激活命令：</span>**
<div align="left"><span style="font-size: x-large;">CD “%SystemRoot%\SYSTEM32″</span></div>
<div align="left"><span style="font-size: x-large;">CSCRIPT /NOLOGO SLMGR.VBS /SKMS 192.168.0.xxx</span></div>
<div align="left"><span style="font-size: x-large;">CSCRIPT /NOLOGO SLMGR.VBS /ATO</span></div>
<div align="left"><span style="font-size: x-large;">CSCRIPT /NOLOGO SLMGR.VBS /XPR</span></div>
**<span style="font-size: x-large;">Office/Project/Visio 2013(2010换下安装路径) 激活命令：</span>**
<div align="left"><span style="font-size: x-large;">32位：CD “%ProgramFiles(x86)%\MICROSOFT OFFICE\OFFICE15″</span></div>
<div align="left"><span style="font-size: x-large;">64位：CD “%ProgramFiles%\MICROSOFT OFFICE\OFFICE15″</span></div>
<div align="left"><span style="font-size: x-large;">CSCRIPT OSPP.VBS /SETHST:192.168.0.xxx</span></div>
<div align="left"><span style="font-size: x-large;">CSCRIPT OSPP.VBS /ACT</span></div>
<div align="left"><span style="font-size: x-large;">CSCRIPT OSPP.VBS /DSTATUS</span></div>
<div align="left"></div>
<div align="left">
<div align="left">
<div class="quote">
> <div align="left">> 
> <div align="left">注意：本帖的目的是在你已经搭建私有kms激活服务器的情况下，使局域网内电脑可以自动发现kms服务器而进行免配置激活的。</div>> 
> <div align="left">应用前提是你已经搭好了KMS服务器！</div>> 
> <div align="left">在openwrt上搭建KMS：</div>> 
> <div align="left">http://www.right.com.cn/forum/thread-174287-1-1.html</div>> 
> <div align="left">在cubieboard、树莓派等ARM盒子搭建py-KMS的教程：</div>> 
> <div align="left">http://www.cnblogs.com/bitspace/</div>> 
> </div>
</div>
<div class="quote">
> <div align="left">结合 @[Vincent-Emiya](http://www.right.com.cn/forum/space-uid-246879.html) 的测试发现，可以使用DNS指向任意公共的KMS激活服务器实现激活局域网内的主机。这可能是有史以来最便捷的KMS激活方案了。</div>> 
> <div align="left">想象下，只要配置好路由器的DNS，然后不用架设KMS服务器，不用安装小工具，也不要执行任何命令。只要把电脑接入你的局域网，你的系统和office就可以自动激活~不要方便太多</div>> 
> <div align="left">具体方法20楼：[http://www.right.com.cn/forum/fo ... =174651&amp;pid=1111580](http://www.right.com.cn/forum/forum.php?mod=redirect&amp;goto=findpost&amp;ptid=174651&amp;pid=1111580)</div>
</div>
</div>
<div align="left"><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">相信很多人都在自己的局域网内搭建了自己的私有kms激活服务器，比如：[http://www.right.com.cn/forum/thread-174287-1-1.html](http://www.right.com.cn/forum/thread-174287-1-1.html)。</span></span></div>
<div align="left"><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">可以说py-kms与vlmcsd的适用性真的非常之广，不管你在windows，linux下甚至安卓下都可以搭建私有的kms服务。但是最后都会遇到的问题是需要在被激活主机上运行批处理命令，不免有些繁琐。</span></span></div>
<div align="left"><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">曾闻中国某高等学府批量购买企业windows许可，你的电脑只要连入校园网，不需要任何配置就可以激活系统，不免神往。查资料发现，这是通过配置DNS服务器的SRV项实现局域网内主机自动发现kms激活服务器的。</span></span></div>
<div align="left"><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">刚好我的路由器跑着openwrt系统，可以配置dnsmasq提供SRV功能，于是ssh进入路由器后台，在/etc/dnsmasq.conf中添加配置：</span></span></div>
<div class="blockcode">
<div id="code_AMR">

1.  srv-host=_vlmcs._tcp.lan,cubietruck.lan,1688,0,100
</div>
_复制代码_

</div>
<div align="left"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;"><span style="color: #000000;">其中 _vlmcs._tcp 为服务名；lan 为我的内网域名</span><span style="color: #ff0000;">(这里要改成你的内网域名，一般都是lan)</span><span style="color: #000000;">；cubietruck.lan为我的KMS服务器在内网的地址</span></span><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">(这里要改成你的内网KMS服务器地址)</span><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">；1688为kms激活服务默认端口号；0为优先级；100为权重。</span></span></div>
<div align="left"><span style="font-size: x-large;"><span style="color: #ff0000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">注意需要修改：</span>cubietruck.lan 为你的KMS主机实际所在的地址！</span></span></div>
<div align="left">
<div class="quote">
> <div align="left"><span style="color: #000000;"><span style="font-size: medium;">比如你的KMS服务器架设在路由器上，而路由器的主机名为：openwrt</span></span></div>> 
> <div align="left"><span style="font-size: medium;"><span style="color: #000000;">你的局域网域名后缀为lan（一般都是lan）</span></span></div>> 
> <div align="left"><span style="font-size: medium;"><span style="color: #000000;">那么你的路由器地址为：openwrt.lan</span></span></div>
</div>
其中路由器主机名可以在luci界面的状态页面看到，本地域名后缀可以在dns设置页面看到。

</div>
<div align="left"><span style="font-size: medium;"><span style="color: #000000;"> </span></span></div>
<div align="left"><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">然后在路由器中重启dnsmasq服务</span></span></div>
<div align="left">/etc/init.d/dnsmasq restart</div>
<div align="left"><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">切换至windows验证dns配置是否正确，打开命令提示符，运行命令：</span></span></div>
<div align="left">nslookup -type=srv _vlmcs._tcp.lan</div>
<div align="left"></div>
<div align="left"><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">其中 _vlmcs._tcp 表示kms服务类型，lan为我的局域网域名称。</span></span></div>
<div align="left"><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">看到返回信息：</span></span></div>
<div align="left">
<div class="blockcode">
<div id="code_cY4">
<pre class="lang:default decode:true">_vlmcs._tcp.lan SRV service location:

          priority       = 0

          weight         = 100

          port           = 1688

          svr hostname   = cubietruck.lan

cubietruck.lan  internet address = 192.168.1.126</pre>
</div>
</div>
</div>
<div align="left"><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">说明dns配置正确。</span></span></div>
<div align="left"><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">这时候看看我自己电脑上的office能不能成功发现kms服务器，还是在管理员权限下运行命令：</span></span></div>
<div class="blockcode">
<div id="code_f7D">
<pre class="lang:default decode:true ">CD "%ProgramFiles(x86)%\MICROSOFT OFFICE\OFFICE15"

CSCRIPT OSPP.VBS /remhst

CSCRIPT OSPP.VBS /act

CSCRIPT OSPP.VBS /dstatus</pre>
&nbsp;

</div>
</div>
<div align="left"><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">其中第一行表示清除之前设置的kms激活服务器地址，第二行手动激活，第三行显示激活状态。最终看到信息</span></span></div>
<div class="blockcode">
<div id="code_AY6">
<pre class="lang:default decode:true ">REMAINING GRACE: 180 days  (259200 minute(s) before expiring

Last 5 characters of installed product key: XTGCT

Activation Type Configuration: ALL

        KMS machine name from DNS: cubietruck.lan:1688

        Activation Interval: 120 minutes

        Renewal Interval: 10080 minutes

        KMS host caching: Enabled

---------------------------------------

---------------------------------------

---Exiting-----------------------------</pre>
&nbsp;

</div>
</div>
<div align="left"><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">其中</span></span></div>
<div align="left">
<pre class="lang:default decode:true">KMS machine name from DNS: cubietruck.lan:1688</pre>
</div>
<div align="left"><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">表示能够根据DNS自动发现局域网内的kms激活服务器为cubietruck.lan。</span></span></div>
<div align="left"><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">说明office可以完全免配置自动激活。</span></span></div>
<div align="left"><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">以后任何电脑只要连接入我的局域网，即可对其VOL版本的office以及windows进行自动激活工作。cool~</span></span></div>
<div align="left"><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">参考：</span></span></div>
<div align="left"><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">http://blog.14401.cn/post-166.html</span></span></div>
<div align="left"><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">http://www.cnblogs.com/zhuangxuqiang/archive/2009/04/28/1445113.html</span></span></div>
<div align="left"><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">https://support.microsoft.com/en-us/kb/816587</span></span></div>
<div align="left"><span style="color: #000000;"><span style="font-family: Verdana, Arial, Helvetica, sans-serif;">http://www.cnblogs.com/bitspace/</span></span></div>
</div>
&nbsp;