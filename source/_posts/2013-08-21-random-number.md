---
title: 如何评价随机数生成算法
tags:
  - random number
id: 172
categories:
  - 算法
abbrlink: bc50611c
date: 2013-08-21 01:42:32
---

部分内容转自知乎 [http://www.zhihu.com/question/20222653](http://www.zhihu.com/question/20222653 "如何评价一个随机数生成算法的优劣？")，在此做一个记录和总结。

主要的评价标准可以参考德国联邦信息安全办公室给出了随机数发生器质量评判的四个标准。

四个判别随机数序列质量的准则：
> *   K1 — A sequence of random numbers with a low probability of containing identical consecutive elements.
> *   K2 — A sequence of numbers which is indistinguishable from 'true random' numbers according to specified statistical tests.
> *   K3 — It should be impossible for any attacker (for all practical purposes) to calculate, or otherwise guess, from any given sub-sequence, any previous or future values in the sequence, nor any inner state of the generator.
> *   K4 — It should be impossible, for all practical purposes, for an attacker to calculate, or guess from an inner state of the generator, any previous numbers in the sequence or any previous inner generator states.<!--more-->
翻译如下：

*   K1——出现相同连续元素的随机数序列的概率较低。
*   K2——按照特定的统计学测试，无法区分生成的随机数与真随机数。符合统计学的平均性，比如所有数字出现概率应该相同，卡方检验应该能通过，超长游程长度概略应该非常小，自相关应该只有一个尖峰，任何长度的同一数字之后别的数字出现概率应该仍然是相等的等等。
*   K3——从一段已知随机数序列计算或者猜测出随机数发生器的内部工作状态或者上一个或下一个随机数，应该是不可能的。
*   K4——从随机数发生器的工作状态猜测出随机数发生器以前的工作状态，或序列中前面的随机数，应该是不可能的。
我们一般用的随机数发生器至少要符合K1和K2，而用于加密等应用的随机数发生器则还要符合K3和K4。

有一系列的测试可以判断一个随机数生成器的优劣。NIST发布了一个测试工具包专门来做这件事情。
[http://csrc.nist.gov/groups/ST/toolkit/rng/documentation_software.html](http://csrc.nist.gov/groups/ST/toolkit/rng/documentation_software.html)

还有一些其他应用随机数计算与理论值对比的方法，例如用蒙特卡洛方法求高维数值积分（例如n维球的体积和面积），如果随机数生成器足够好，积分结果应该能很好地逼近理论值。用蒙特卡洛方法计算PI也可以是一种尝试。