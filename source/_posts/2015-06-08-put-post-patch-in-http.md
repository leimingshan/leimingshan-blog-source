---
title: PUT POST PATCH in HTTP
tags:
  - develop
  - http
id: 349
categories:
  - 开发
abbrlink: 8d3903c6
date: 2015-06-08 12:47:30
---

In the [HyperText Transfer Protocol (HTTP)](http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol "Hypertext Transfer Protocol"), idempotence and [safety](http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Safe_methods "Hypertext Transfer Protocol") are the major attributes that separate [HTTP verbs](http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods "Hypertext Transfer Protocol").

Of the major HTTP verbs, GET, PUT, and DELETE are idempotent (if implemented according to the standard), but POST is not.<sup id="cite_ref-httpStd-methods_9-1" class="reference">[[9]](http://en.wikipedia.org/wiki/Idempotence#cite_note-httpStd-methods-9)</sup> These verbs represent very abstract operations in computer science: GET retrieves a resource; PUT stores content at a resource; and DELETE eliminates a resource. As in the example above, reading data usually has no side effects, so it is idempotent (in fact nullipotent). Storing a given set of content is usually idempotent, as the final value stored remains the same after each execution. And deleting something is generally idempotent, as the end result is always the absence of the thing deleted.

参考

[PUT vs POST in REST](http://stackoverflow.com/questions/630453/put-vs-post-in-rest)

[Idempotence wiki](http://en.wikipedia.org/wiki/Idempotence)