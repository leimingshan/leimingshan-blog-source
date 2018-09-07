---
title: mybatis传入类型为Long的处理
tags:
  - Java
id: 370
categories:
  - Java
abbrlink: 120944f4
date: 2015-06-16 20:28:54
---

mybatis中如果传入类型为Long，与其他一些基本类型例如int，String的处理是不一样的，参数需要统一使用#{_parameter}，而不论你传入参数的名称是什么。

例如：
<pre class="lang:default decode:true ">&lt;select id="getUser" parameterType="java.lang.Long" resultType="com.test.User"&gt;
    SELECT * from user
    &lt;if test = "accountId!=null"&gt;
        WHERE accountId = #{_parameter}
    &lt;/if&gt;  
&lt;/select&gt;</pre>
&nbsp;