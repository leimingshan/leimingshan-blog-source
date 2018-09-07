---
title: Spring mvc无法处理put参数的问题
tags:
  - Java
  - Spring
id: 354
categories:
  - Java
abbrlink: 4ec18d47
date: 2015-05-21 11:57:01
---

最近在用Spring MVC做一个Restful的Web Service，在HTTP的各类method中，除了非常常用的GET和POST之外，还有PUT、DELETE、PATCH等可以使用。这次遇到问题的是PUT方法，在REST风格中，PUT一般表示对原有资源的修改，但是在CLIENT端编程发出PUT请求后，却总是会遇到400错误，在用另外一个REST测试工具（IDEA中集成）的时候却一切正常。

分析了一下原因，发现几点：
<!-- more -->
1.  使用的测试工具在发出PUT请求时，参数是带在url尾部的，而不是放在body中，类似于/users/123?name=123&amp;gender=456这样的形式，Spring mvc在处理这种形式的PUT请求时是没有问题的。
2.  在编写Client的过程中，发送HTTP PUT请求的时候，一般会把参数放入Request Body中，以key:value的格式发送，本质上是json string的格式，此时，在Spring mvc中，通过注解@RequestParam和@ModelAttribute都不能得到请求数据，问题的原因不在Spring，即使使用原始Servlet的request.getParameter也是得不到数据的。
解决方法主要有两种：

1.  Spring mvc中处理参数时，改用注解@RequestBody，这里得到的是请求字符串，需要自己进行转换，得到对应的参数信息，Spring这里也提供了对应的转换方法，处理起来并不复杂。
2.  自己增加一个filter，来专门处理HTTP中的PUT请求，这里建议参考[该文章](http://www.360doc.com/content/14/0306/17/15700330_358288127.shtml)。关键配置部分如下：

```
<filter>
  <filter-name>httpPutFormFilter</filter-name>
  <filter-class>org.springframework.web.filter.HttpPutFormContentFilter</filter-class>
</filter>
<filter-mapping>
  <filter-name>httpPutFormFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```
