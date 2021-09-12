---
title: Java8_Stream
date: 2020-02-12T10:10:04+08:00
lastmod: 2020-02-12T10:15:04+08:00
author: spc
cover: /img/landscape9.jpg
categories: ["java"]
tags: ["java8", "Stream"]
# showcase: true
draft: false
---

精髓：数据变换，简化代码
Stream简化代码，熟练能让你用得很爽

<!--more-->

# Stream

# Stream的API-创建Stream
* Collection.stream()
* Stream.of
* String.chars()
* IntSteam.range()

# Stream的API-中间操作
* 仍然返回Stream的操作
* filter 
* map   对流操作，返回新的流（映射）
* sorted

# Stream的API-终结操作
* 返回⾮`Stream`的操作，包括void
* ⼀个流只能被消费⼀次
* forEach
* count/max/min
* findFirst/findAny
* anyMatch/noneMatch
* ⭐collect

# collector与Collectors
* collect操作是最强⼤的操作
* toSet/toList/toCollection
* joining()
* toMap()
* groupingBy()

## 并发流
* parallelStream() 
* 可以通过并发提⾼互相独⽴的操作的性能
* 在正确使⽤的前提下，可以获得近似线性的性能提升
* 但是！使⽤要⼩⼼，性能要测试，如果你不知道⾃⼰在做什么，就忘了它吧
