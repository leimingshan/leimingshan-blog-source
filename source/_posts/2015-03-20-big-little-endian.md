---
title: 大端 小端 字节序
id: 209
categories:
  - 开发
abbrlink: 74be35a0
date: 2015-03-20 18:10:18
tags:
---

<pre class="lang:default decode:true   ">#include &lt;stdio.h&gt;
/*
    关于字节序:
对于一个整数0x12346=5678,在内存中占4个字节
从低地址开始,每个字节分别存的是 12,34,56,78就说明是大端CPU; 用于网络编程,JAVA
从低地址开始,每个字节分别存的是 78,56,34,12就说明是小端CPU; x86架构是小端
下面判断在内存的单个字节中二进位是按什么顺序排的?
*/

//定义位域
typedef struct bit_field
{
	unsigned char b0:1;  //b0指向低地址那一端的第一位,b1、b2依次排列下去
	unsigned char b1:1;
	unsigned char b2:1;
	unsigned char b3:1;
	unsigned char b4:1;
	unsigned char b5:1;
	unsigned char b6:1;
	unsigned char b7:1;
}BIT_FIELD;

int main()
{
	int i = 0xFF00001B;	//小端存放方式  1B 00 00 FF
	BIT_FIELD * p = ( BIT_FIELD * )&amp;i;  // p指向 1B

	printf("%u ",p-&gt;b0);
	printf("%u ",p-&gt;b1);
	printf("%u ",p-&gt;b2);
	printf("%u ",p-&gt;b3);
	printf("%u ",p-&gt;b4);
	printf("%u ",p-&gt;b5);
	printf("%u ",p-&gt;b6);
	printf("%un",p-&gt;b7);

	getchar();
	return 0;
}</pre>
输出结果：1 1 0 1 1 0 0 0

在小端机器上验证得到,  在一个字节内部,  低位放在靠近低地址这个方向上。大端方式的高位靠近低地址方向存放。

[http://blog.csdn.net/ce123_zhouwei/article/details/6971544](http://blog.csdn.net/ce123_zhouwei/article/details/6971544)