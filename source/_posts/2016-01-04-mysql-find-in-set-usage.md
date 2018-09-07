---
title: MySQL FIND_IN_SET Usage
tags:
  - MySQL
id: 422
categories:
  - 数据库
abbrlink: 39136b96
date: 2016-01-04 16:04:59
---

MySQL手册中FIND_IN_SET函数的语法:

FIND_IN_SET(str, strlist)

Returns a value in the range of 1 to _`N`_ if the string _`str`_ is in the string list _`strlist`_ consisting of _`N`_ substrings. A string list is a string composed of substrings separated by <span class="quote">“<span class="quote">`,`</span>”</span> characters. If the first argument is a constant string and the second is a column of type [`SET`](http://dev.mysql.com/doc/refman/5.7/en/set.html "11.4.5 The SET Type"), the [`FIND_IN_SET()`](http://dev.mysql.com/doc/refman/5.7/en/string-functions.html#function_find-in-set) function is optimized to use bit arithmetic. Returns `0` if _`str`_ is not in _`strlist`_ or if _`strlist`_ is the empty string. Returns `NULL` if either argument is `NULL`. This function does not work properly if the first argument contains a comma (<span class="quote">“<span class="quote">`,`</span>”</span>) character.

假如字符串str 在由N 子链组成的字符串列表strlist 中，则返回值的范围在 1 到 N 之间。
一个字符串列表就是一个由一些被‘,’符号分开的子链组成的字符串。如果第一个参数是一个常数字符串，而第二个是type SET列，则   FIND_IN_SET() 函数被优化，使用比特计算。
如果str不在strlist 或strlist 为空字符串，则返回值为 0 。如任意一个参数为NULL，则返回值为 NULL。这个函数在第一个参数包含一个逗号(‘,’)时将无法正常运行。<!--more-->

例如：
<pre class="lang:default decode:true ">mysql&gt; SELECT FIND_IN_SET('b','a,b,c,d');
        -&gt; 2</pre>
[`SUBSTRING_INDEX(_<code>str`_,_`delim`_,_`count`_)</code>](http://dev.mysql.com/doc/refman/5.7/en/string-functions.html#function_substring-index)

Returns the substring from string _`str`_ before _`count`_ occurrences of the delimiter _`delim`_. If _`count`_ is positive, everything to the left of the final delimiter (counting from the left) is returned. If _`count`_ is negative, everything to the right of the final delimiter (counting from the right) is returned. [`SUBSTRING_INDEX()`](http://dev.mysql.com/doc/refman/5.7/en/string-functions.html#function_substring-index) performs a case-sensitive match when searching for _`delim`_.
<pre class="lang:default decode:true ">mysql&gt; SELECT SUBSTRING_INDEX('www.mysql.com', '.', 2);
        -&gt; 'www.mysql'
mysql&gt; SELECT SUBSTRING_INDEX('www.mysql.com', '.', -2);
        -&gt; 'mysql.com'</pre>
This function is multibyte safe.

&nbsp;

MySQL中使用WHERE IN进行条件查询的时候，一般情况下，查询的结果和IN中值的顺序并不一致。

有两种方式可以对IN查询的结果进行排序。一种是ORDER BY FIND_IN_SET，另外一种是ORDER BY SUBSTRING_INDEX。