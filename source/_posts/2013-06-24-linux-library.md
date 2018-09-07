---
title: Linux下的动态链接库和so文件
tags:
  - Linux
id: 25
categories:
  - Linux
abbrlink: 9582a957
date: 2013-06-24 00:56:43
---

<div>
<div>Linux下动态链接库默认后缀是so，大部分的系统的动态链接库文件在以下目录：</div>
<div>/lib  /usr/lib  /usr/local/lib</div>
<div>

If you ever happen to want to link against installed libraries
in a given directory, LIBDIR, you must either use libtool, and
specify the full pathname of the library, or use the `-LLIBDIR'
flag during linking and do at least one of the following:
- add LIBDIR to the `LD_LIBRARY_PATH' environment variable
during execution
- add LIBDIR to the `LD_RUN_PATH' environment variable
during linking
- use the `-Wl,--rpath -Wl,LIBDIR' linker flag
- have your system administrator add LIBDIR to `/etc/ld.so.conf' and run ldconfig

</div>
<div></div>
<div>See any operating system documentation about shared libraries for
more information, such as the ld(1) and ld.so(8) manual pages.</div>
</div>