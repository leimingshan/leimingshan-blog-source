---
title: Google Practice Round China New Grad Test 2014 解题报告
tags:
  - C++
id: 230
categories:
  - 算法
abbrlink: 1109c473
date: 2013-09-21 12:32:03
---

这次练习赛是Google针对2014校园招聘即将到来的一个上机测试进行的一次练习，一共三道题目，都不是很难，也主要是可以对系统有一个更好的了解。

先简单说一下Google CodeJam的这个系统，与我们通常的OnlineJudge主要的不同，是提交代码的方式，在我们写好解题代码后，需要下载系统随机提供的测试用例文件，利用自己的程序跑这个输入文件并得到相应的输出，然后上传自己代码和输出给系统去判断，总体来说是复杂了一些。

大家可以通过这个链接[https://code.google.com/codejam/contest/2933486/dashboard](https://code.google.com/codejam/contest/2933486/dashboard)去查看这次练习的具体题目和测试用例。

Problem A. Bad Horse<!--more-->

题目简单来看，就是一句话：Bad Horse has decided to split the league into two departments in order to separate troublesome members. 每个测试用例呢，会给你几对人的名字，每一对人都是会互相找麻烦的那种，必须分到不同组去，那么最后能不能成功分组呢？

例如（1,2）（2,3）（3,4）是可以分成（1,3）（2,4）这两个集合的，而（1,2）（2,3）（1,3）则是不可以划分成功的。我对这题使用的方法略复杂，就不贴出代码了，简单说一下基本的思想：利用两个集合A，B来保存划分，对于新出现的一对，查看各个成员在A，B里面的出现情况，保证一个在A，另一个在B，如果两个在之前出现在了同一个集合中，那么结果肯定是No，如果两个都没有出现过，则先不要忙着去划分，先进行后面的，遍历完一趟之后再处理。例如（1,2）（3,4）（1,3），1放在集合A，2放在集合B，你会发现，3和4之前都没有出现过，那怎么放呢，先进行后面的（1,3），走完一趟之后再来处理（3,4）这样的对。

Problem B. Captain Hammer

这其实是一道标准的物理题。一个飞行器，从地面，给你一个斜向上的速度，那么根据角度可以计算出其竖直向上的速度和水平方向的速度，在重力加速度的影响下其最终会落回地面，由竖直向上的速度可以知道时间，那么水平方向速度乘以时间就是其在水平方向的位移。

题目给出初始的斜向速度和最后的水平位移，求斜向角度。

代码如下：
<pre class="lang:default decode:true">#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;math.h&gt;

#define PI 3.141592653
int main()
{
    int t;
    scanf("%d", &amp;t);
    int i;
    int speed, distance;
    for (i = 0; i &lt; t; i++) {
        scanf("%d %d", &amp;speed, &amp;distance);
        double result;
        result = (double)9.8 * distance / speed / speed;
        result = asin(result) * 180.0 / PI / 2;
        printf("Case #%d: %.7fn", i+1, result);
    }
    return 0;
}</pre>
Problem C. Moist

这个题目有点插入排序的意思，本身意思很简单，解题思想也很简单，给你一系列的人名，最后要按字典序排序，如果一个人名需要插入到前面去，计数加1，比算移动的次数还简单。

代码如下：
<pre class="lang:default decode:true ">#include &lt;iostream&gt;
#include &lt;string&gt;

using namespace std;

int main()
{
    int t;
    cin &gt;&gt; t;
    for (int i = 0; i &lt; t; i++) {
        int n;
        cin &gt;&gt; n;
        cin.get();
        int count = 0;
        string last;
        getline(cin, last);

        string current;
        for (int j = 1; j &lt; n; j++) {
            getline(cin, current);
            if (current.compare(last) &lt; 0)
                count++;
            else
                last.assign(current);
        }
        cout &lt;&lt;"Case #" &lt;&lt; i+1 &lt;&lt; ": " &lt;&lt; count &lt;&lt; endl;
    }
    return 0;
}</pre>
&nbsp;