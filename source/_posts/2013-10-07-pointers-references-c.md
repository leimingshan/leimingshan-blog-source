---
title: C++中指针与引用的区别-More Effective C++
tags:
  - C++
id: 242
categories:
  - C/C++
abbrlink: c47f27ac
date: 2013-10-07 23:55:04
---

指针与引用看上去完全不同（指针用操作符“*”和“-&gt;”，引用使用操作符“. ”），但是它们似乎有相同的功能。指针与引用都是让你间接引用其他对象。你如何决定在什么时候使用指针，在什么时候使用引用呢？

首先，要认识到在任何情况下都不能使用指向空值的引用。一个引用必须总是指向某些对象。因此如果你使用一个变量并让它指向一个对象，但是该变量在某些时候也可能不指向任何对象，这时你应该把变量声明为指针，因为这样你可以赋空值给该变量。相反，如果变量肯定指向一个对象，例如你的设计不允许变量为空，这时你就可以把变量声明为引用。

“但是，请等一下”，你怀疑地问，“这样的代码会产生什么样的后果？”
<pre class="lang:default decode:true">char *pc = 0;          // 设置指针为空值

char&amp; rc = *pc;        // 让引用指向空值</pre>
这是非常有害的，毫无疑问。结果将是不确定的（编译器能产生一些输出，导致任何事情都有可能发生）。应该躲开写出这样代码的人，除非他们同意改正错误。如果你担心这样的代码会出现在你的软件里，那么你最好完全避免使用引用，要不然就去让更优秀的程序员去做。我们以后将忽略一个引用指向空值的可能性。<!--more-->

因为引用肯定会指向一个对象，在C＋＋里，引用应被初始化。
<pre class="lang:default decode:true">string&amp; rs;             // 错误，引用必须被初始化
string s("xyzzy");
string&amp; rs = s;         // 正确，rs指向s</pre>
指针没有这样的限制。
<pre class="lang:default decode:true">string *ps;             // 未初始化的指针
// 合法但危险</pre>
不存在指向空值的引用这个事实意味着使用引用的代码效率比使用指针的要高。因为在使用引用之前不需要测试它的合法性。
<pre class="lang:default decode:true">void printDouble(const double&amp; rd)
{
    cout &lt;&lt; rd;         // 不需要测试rd,它
}                       // 肯定指向一个double值</pre>
相反，指针则应该总是被测试，防止其为空：
<pre class="lang:default decode:true">void printDouble(const double *pd)
{
  if (pd) {             // 检查是否为NULL
    cout &lt;&lt; *pd;
  }
}</pre>
指针与引用的另一个重要的不同是指针可以被重新赋值以指向另一个不同的对象。但是引用则总是指向在初始化时被指定的对象，以后不能改变。
<pre class="lang:default decode:true">string s1("Nancy");

string s2("Clancy");

string&amp; rs = s1;          // rs 引用 s1

string *ps = &amp;s1;         // ps 指向 s1

rs = s2;                 // rs 仍旧引用s1,

// 但是 s1的值现在是

// "Clancy"

ps = &amp;s2;               // ps 现在指向 s2;

// s1 没有改变</pre>
总的来说，在以下情况下你应该使用指针，一是你考虑到存在不指向任何对象的可能（在这种情况下，你能够设置指针为空），二是你需要能够在不同的时刻指向不同的对象（在这种情况下，你能改变指针的指向）。如果总是指向一个对象并且一旦指向一个对象后就不会改变指向，那么你应该使用引用。

还有一种情况，就是当你重载某个操作符时，你应该使用引用。最普通的例子是操作符[]。这个操作符典型的用法是返回一个目标对象，其能被赋值。
<pre class="lang:default decode:true">vector&lt;int&gt; v(10);       // 建立整形向量（vector），大小为10;

// 向量是一个在标准C库中的一个模板(见条款M35)

v[5] = 10;               // 这个被赋值的目标对象就是操作符[]返回的值

如果操作符[]返回一个指针，那么后一个语句就得这样写：

*v[5] = 10;</pre>
但是这样会使得v看上去象是一个向量指针。因此你会选择让操作符返回一个引用。（这有一个有趣的例外，参见条款M30）

当你知道你必须指向一个对象并且不想改变其指向时，或者在重载操作符并为防止不必要的语义误解时，你不应该使用指针。而在除此之外的其他情况下，则应使用指针。

&nbsp;

为了进一步加深大家对指针和引用的区别，下面我从编译的角度来阐述它们之间的区别：

程序在编译时分别将指针和引用添加到符号表上，符号表上记录的是变量名及变量所对应地址。指针变量在符号表上对应的地址值为指针变量的地址值，而引用在符号表上对应的地址值为引用对象的地址值。符号表生成后就不会再改，因此指针可以改变其指向的对象（指针变量中的值可以改），而引用对象则不能修改。

最后，总结一下指针和引用的相同点和不同点：

**相同点：**

*   都是地址的概念；指针指向一块内存，它的内容是所指内存的地址；而引用则是某块内存的别名。
**不同点：**

*   指针是一个实体，而引用仅是个别名；
*   引用只能在定义时被初始化一次，之后不可变；指针可变；引用“从一而终”，指针可以“见异思迁”；
*   引用没有const，指针有const，const的指针不可变；（具体指没有int&amp; const a这种形式，而const int&amp; a是有的，  前者指引用本身即别名不可以改变，这是当然的，所以不需要这种形式，后者指引用所指的值不可以改变）
*   引用不能为空，指针可以为空；
*   “sizeof 引用”得到的是所指向的变量(对象)的大小，而“sizeof 指针”得到的是指针本身的大小；
*   指针和引用的自增(++)运算意义不一样；
*   引用是类型安全的，而指针不是 (引用比指针多了类型检查)。