---
title: "Mysql索引失效"
date: 2022-02-09T22:59:03+08:00
lastmod: 2022-02-09T22:59:03+08:00
author: Spc
# cover: /img/设计模式.png
categories: ["MySQL"]
tags: ["mysql索引","索引","面试"]
# showcase: true
draft: false
---

mysql索引失效

<!--more-->

# MySql索引失效





## 索引失效10种场景

**具体文章：**[聊聊索引失效的10种场景，太坑了](https://mp.weixin.qq.com/s/YZjE60kaBnxc6XlQ9GcolA)

- select * 
- like %
- 使用 or
- 不满足最左匹配原则
- 索引列上使用函数
- 索引列上使用计算
- 字段类型不同 例如：int varchar     索引int = varchar 会隐式转换
- 列对比
- not in 和 not exists
- order by的坑



> 如果使用了`or`关键字，那么它前面和后面的字段都要加索引，不然所有的索引都会失效，这是一个大坑。

> 在 MySQL 中应该对 in / not in查询的字节长度是有限制的。 当超过了长度（不确定）就不在走索引

**in/ not in** https://segmentfault.com/a/1190000023825926

> not exists    t1 表哪种情况都不会走索引，而 t2 表是有索引的情况下就会走索引。为什么会出现这种情况？



## in和exists的执行流程

对于 in 查询来说，会先执行子查询，如上边的 t2 表，然后把查询得到的结果和外表 t1 做笛卡尔积，再通过条件进行筛选（这里的条件就是指 name 是否相等），把每个符合条件的数据都加入到结果集中。



对于 exists 来说，是先查询遍历外表 t1 ，然后每次遍历时，再检查在内表是否符合匹配条件，即检查是否存在 name 相等的数据。

