---
title: MySQL
date: 2020-03-02T07:51:37+08:00
lastmod: 2020-03-02T07:51:37+08:00
author: spc
cover: /img/food8.jpg
categories: ["数据库"]
tags: ["MySQL"]
# showcase: true
draft: false
---

mysql相关知识

<!--more-->

# mysql
## 存储方式
* B+树  
  * b树 
  * b*树
* 好处：1.多叉树，多个子节点（节点多个记录）  so 高度低 方便io
<!-- ![MySQLB+树](/posts/img/mysqlB+树.png) -->

## 联合索引


Create index index_name on news(字段名 或者 字段名(length))  
范围查找索引用处不大，而且后面的索引是用不到的  
你建的是（a,b,c,d）索引   
最左前缀匹配  

优先级类型  
Explain select * from NEWS where created_at =xxx and modified_at =xxx  
时间的时分秒去掉，=对应索引来说性能提升是最大的  
Mysql 不擅长文本类的索引  
 

