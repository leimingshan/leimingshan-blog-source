---
title: Elasticsearch-nested-query
tags: elasticsearch
abbrlink: 3f81b35b
date: 2018-08-27 13:23:35
---
Elasticsearch嵌套查询，具体可参考[Nested Query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-nested-query.html).

建议还是要多看多熟悉Elasticsearch的官方文档，比到处去搜强多了。

简而言之，在对ES doc的多层嵌套对象进行查询的时候，要使用Nested Query，常规查询无效。

```
{
    "query": {
        "nested" : {
            "path" : "obj",
            "query" : {
                "bool" : {
                    "must" : [
                        { "match" : {"obj.info.name" : "zhangsan"} }
                    ]
                }
            }
        }
    }
}
```
