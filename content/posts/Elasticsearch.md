---
title: Elasticsearch
date: 2020-03-02T07:50:28+08:00
lastmod: 2020-03-02T07:50:28+08:00
author: spc
cover: /img/food7.jpg
categories: ["数据库"]
tags: ["Elasticsearch"]
# showcase: true
draft: false
---

Elasticsearch

<!--more-->

# Elasticsearch

![db_es](/posts/img/db_es.png)

Bulk (一次性发多个请求)  
```java
BulkRequest bulkRequest=new BulkRequest()
bulkRequest.add(request);
client.bulk(bulkRequest , RequestOptions.DEFAULT)
```

