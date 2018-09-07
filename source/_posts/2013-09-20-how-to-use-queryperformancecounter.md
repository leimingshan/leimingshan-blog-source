---
title: WINAPI QueryPerformanceCounter的使用
tags:
  - C++
  - windows
id: 220
categories:
  - C/C++
abbrlink: 3c688e29
date: 2013-09-20 10:39:28
---

引用自[http://stackoverflow.com/questions/1739259/how-to-use-queryperformancecounter](http://stackoverflow.com/questions/1739259/how-to-use-queryperformancecounter)

最近在做一个程序，要从Linux转移到windows下测试一下计算时间，我们知道在Linux下可以使用gettimeofday得到微妙级别的时间，使用方法先复习一下：

使用到的数据结构和函数原型如下：
<pre class="lang:default decode:true">struct timeval {
    time_t      tv_sec;     /* seconds */
    suseconds_t tv_usec;    /* microseconds */
};</pre>
<pre class="lang:default decode:true">#include&lt;sys/time.h&gt;
int gettimeofday(struct timeval *tv, struct timezone *tz);</pre>
在gettimeofday()函数中tv或者tz都可以为空。如果为空则就不返回其对应的结构体。在struct timeval中的tv_usec域我们可以得到微妙级别的时间。

函数执行成功后返回0，失败后返回-1，错误代码存于errno中。

接下来转入到Windows部分，利用高精度性能计数器来进行定时，具体方法如下：

下面两个函数是VC提供的仅供Windows 95及其后续版本使用的精确时间函数，并要求计算机从硬件上支持精确定时器。
QueryPerformanceFrequency()函数和QueryPerformanceCounter()函数的原型如下：
<pre>BOOL  QueryPerformanceFrequency(LARGE_INTEGER ＊lpFrequency);
BOOL  QueryPerformanceCounter(LARGE_INTEGER ＊lpCount);</pre>
<!--more-->数据类型ARGE_INTEGER既可以是一个8字节长的整型数，也可以是两个4字节长的整型数的联合结构，<wbr /> 其具体用法根据编译器是否支持64位而定。该类型的定义如下：
<pre class="lang:default decode:true">typedef union _LARGE_INTEGER
{
    struct
    {
        DWORD LowPart ;// 4字节整型数
        LONG  HighPart;// 4字节整型数
    };
    LONGLONG QuadPart ;// 8字节整型数
}LARGE_INTEGER ;</pre>
在进行定时之前，先调用QueryPerformanceFrequency()函数获得机器内部定时器的时钟频率，<wbr /> 然后在需要严格定时的事件发生之前和发生之后分别调用QueryPerformanceCounter()函数，利用两次获得的计数之差及时钟频率，计算出事件经历的精确时间。
<pre class="lang:default decode:true">#include &lt;windows.h&gt;

double PCFreq = 0.0;
__int64 CounterStart = 0;

void StartCounter(){
    LARGE_INTEGER li;
    if(!QueryPerformanceFrequency(&amp;li))
        cout &lt;&lt; "QueryPerformanceFrequency failed!n";

    PCFreq = double(li.QuadPart)/1000.0;

    QueryPerformanceCounter(&amp;li);
    CounterStart = li.QuadPart;}double GetCounter(){
    LARGE_INTEGER li;
    QueryPerformanceCounter(&amp;li);
    return double(li.QuadPart-CounterStart)/PCFreq;}

int main()
{
    StartCounter();
    Sleep(1000);
    cout &lt;&lt; GetCounter() &lt;&lt;"n";
    return 0;
}</pre>
使用秒为单位，则使用：PCFreq = double(li.QuadPart);

使用微秒做单位，则使用：PCFreq = double(li.QuadPart)/1000000.0。

同时摘录一些对该API的评论：

windows API计时最准的是QueryPerformanceCounter，CPU计时的其他任何时间函数都是用操作系统时间片计时，多核下Windows的时间片是15毫秒，单核10毫秒，决定了你的最高精度10-15毫秒。CPU计时必须有CPU的硬件支持，ARM版WinCE也有QueryPerformanceCounter，但他的精度和时间片计时一样。

新的x86基本都能高精度计时。sunos是最早把高精度计时（微秒级）带入通用操作系统的，因为他的硬件封闭，sparc的CPU有高精度计时的支持。

Windows的GetSystemTime()、GetTickCount()、GetSystemTimeAsFileTime()等函数的值是存放在系统的固定内存区域的，每次时钟中断时由操作系统更新一次（更新周期即系统任务调度时间片，一般为5到15毫秒）。
Windows这么处理是基于效率考虑的，每次做真正的时间计算需要耗费一定时间（读硬件时钟、计算年月日等）。Linux的gettimeofday()比较精确，每次都做准确计算，但比较低效（一次调用要耗费1万多cpu周期）