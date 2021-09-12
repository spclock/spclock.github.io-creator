---
title: SQL
date: 2020-01-23T09:13:59+08:00
lastmod: 2020-01-23T09:13:59+08:00
author: spc
cover: /img/landscape2.jpg
categories: ["java"]
tags: ["数据库", "sql"]
# showcase: true
draft: false
---

有关数据库和sql的语法知识

<!--more-->

# 数据库
# SQL语法
* databases
  * use database_name
  * create table table_name
  * alter table table_name
  * drop tabel table_name
* tables
  * insert into
  * delete
  * **select**
    * limit 从第几个,取多少个
    * join (inner默认、left、right....)
    * order by 排序 （默认desc降序）
    * group by 分组 
  * update  


用join的时候要注意 多嵌套 join 搞清楚哪一个和哪一个的交集  
## 使⽤JDBC访问数据库
* Java Database Connection
  * 连接串url、⽤户名user、密码passage
* Statement
* PrepareStatement - 防SQL注⼊的 （AST 树的型式）
  * SQL注⼊是因为SQL没有验证传⼊的参数
  * 导致攻击者可以通过精⼼设计的参数使得拼装出的SQL达
到他的⽬的
* ResultSet
* 用完一定要释放close()
  
# Docker
docker就是一个微型的linux  
本地安装数据库会很麻烦（更新什么的）特别是windows
* 百分百兼容、百分百⽆残留、百分百统⼀、⽅便
* 现在我们启动的数据库的数据是不持久化的
  * 除⾮在启动容器的时候使⽤-v参数

提速下镜像 docker cn reistry-mirror  
因为docker是数据存内存，因此你要映射  
映射端口，映射文件路径

