---
title: Jackson转换Json的严格字母序
tags: Java
category: Java
abbrlink: '4147250'
date: 2018-02-08 15:22:48
---
经常使用Jackson的同学肯定知道这个注解，
@JsonPropertyOrder(alphabetic = true)
放在类定义上，可以在该类对象序列化的时候，字段按照字母序排序。
全局的话，可以这样配置Jackson的mapper：
```
MAPPER.configure(MapperFeature.SORT_PROPERTIES_ALPHABETICALLY, true);
MAPPER.configure(SerializationFeature.ORDER_MAP_ENTRIES_BY_KEYS, true);
```

问题来了，
如果类中的字段是不确定类型的（或者说是Object类型），该类型的字段中又可以嵌套其他不确定类型，这样就不好办了，
而我要达到的需求是，所有嵌套结构内部也要按照字母序，或者叫严格的字母序。

经验证，在不使用JsonNode的情况下，MAPPER的configure可以满足大多数需求。那么，为什么要用JsonNode呢？

在数据交换的时候，例如反序列化的时候，不知道某个字段的具体类型，是否有嵌套等，用JsonNode会比Map这种好很多，Map不太好处理嵌套的情况。
也有同学提出直接使用Object也可以呀，那就会丢失太多的类型信息，内部字段和深层嵌套就更没法处理了。
使用JsonNode可以构建出树状结构，很方便的获取属性值。

```
JsonNode node = MAPPER.valueToTree(obj);
final Object obj = MAPPER.treeToValue(node, Object.class);
String json = MAPPER.writeValueAsString(obj);
```

参考文档：
https://stackoverflow.com/questions/18952571/jackson-jsonnode-to-string-with-sorted-keys#
