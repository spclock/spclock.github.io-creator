---
title: "Redis中级篇"
date: 2022-02-11T23:14:27+08:00
# lastmod: 2022-02-11T23:14:39+08:00 
author: spc
# cover: /img/food13.jpg
categories: ["数据库"]
tags: ["redis"]
# showcase: true
draft: false
---

redis持久化，淘汰策略

<!--more-->

# Redis中级篇

以下为Redis 6.2.0 版本



## 内存淘汰机制

Redis 提供 6 种数据淘汰策略：

1. **volatile-lru（least recently used）**：从已设置过期时间的数据集（server.db[i].expires）中挑选最近最少使用的数据淘汰
2. **volatile-ttl**：从已设置过期时间的数据集（server.db[i].expires）中挑选将要过期的数据淘汰
3. **volatile-random**：从已设置过期时间的数据集（server.db[i].expires）中任意选择数据淘汰
4. **allkeys-lru（least recently used）**：当内存不足以容纳新写入数据时，在键空间中，移除最近最少使用的 key（这个是最常用的）
5. **allkeys-random**：从数据集（server.db[i].dict）中任意选择数据淘汰
6. **noeviction （默认）**：禁止驱逐数据，也就是说当内存不足以容纳新写入数据时，新写入操作会报错。这个应该没人使用吧！

4.0 版本后增加以下两种：

1. **volatile-lfu（least frequently used）**：从已设置过期时间的数据集（server.db[i].expires）中挑选最不经常使用的数据淘汰
2. **allkeys-lfu（least frequently used）**：当内存不足以容纳新写入数据时，在键空间中，移除最不经常使用的 key



**redis.conf配置文件设置内存淘汰策略 ：**

```conf
# 内存淘汰策略设置
maxmemory-policy allkeys-lru

# redis内存大小设置（5Gb根据自己服务器内存来设置）
maxmemory 5368709120
```



**配置 Redis 内存大小**

我们应该为 Redis 设置多大的内存容量呢？

根据“八二原理“，即 80% 的请求访问了 20% 的数据，因此如果按照这个原理来配置，将 Redis 内存大小设置为数据总量的 20%，就有可能拦截到 80% 的请求。当然，只是有可能，对于不同的业务场景需要进行不同的配置，一般**建议把缓存容量设置为总数据量的 15% 到 30%，兼顾访问性能和内存空间开销**。

**命令行：**

> config set maxmemory 5gb    #设置内存大小
>
>  config get maxmemory        #查看 maxmemory 命令



PS:

LRU 全称是 Least Recently Used，即最近最少使用，会将最不常用的数据筛选出来，保留最近频繁使用的数据。

LRU 会把所有数据组成一个链表，链表头部称为 MRU，代表最近最常使用的数据；尾部称为 LRU代表最近最不常使用的数据；



LFU 全称 Least Frequently Used，即最不经常使用策略，它是基于数据访问次数来淘汰数据的，在 Redis 4.0 时添加进来。它在 LRU 策略基础上，为每个数据增加了一个计数器，来统计这个数据的访问次数。



LRU 算法在实现过程中使用链表管理所有缓存的数据，这会给 Redis 带来额外的开销，而且，当有数据访问时就会有链表移动操作，进而降低 Redis 的性能。

于是，Redis 对 LRU 的实现进行了一些改变：

- 记录每个 key 最近一次被访问的时间戳（由键值对数据结构 RedisObject 中的 lru 字段记录）
- 在第一次淘汰数据时，会先随机选择 N 个数据作为一个候选集合，然后淘汰 lru 值最小的。（N 可以通过 `config set maxmemory-samples 100` 命令来配置）
- 后续再淘汰数据时，会挑选数据进入候选集合，进入集合的条件是：它的 lru 小于候选集合中最小的 lru。
- 如果候选集合中数据个数达到了 maxmemory-samples，Redis 就会将 lru 值小的数据淘汰出去。

4、LFU 算法

LFU 全称 Least Frequently Used，即最不经常使用策略，它是基于数据访问次数来淘汰数据的，在 Redis 4.0 时添加进来。它在 LRU 策略基础上，为每个数据增加了一个计数器，来统计这个数据的访问次数。

前面说到，LRU 使用了 RedisObject 中的 lru 字段记录时间戳，lru 是 24bit 的，LFU 将 lru 拆分为两部分：

- ldt 值：lru 字段的前 16bit，表示数据的访问时间戳
- counter 值：lru 字段的后 8bit，表示数据的访问次数

使用 LFU 策略淘汰缓存时，会把访问次数最低的数据淘汰，如果访问次数相同，再根据访问的时间，将访问时间戳最小的淘汰。

## 持久化机制

- RDB（默认采用）
- AOF



**（snapshotting，RDB）快照**

Redis 可以通过创建快照来获得存储在内存里面的数据在某个时间点上的副本。Redis 创建快照之后，可以对快照进行备份，可以将快照复制到其他服务器从而创建具有相同数据的服务器副本（Redis 主从结构，主要用来提高 Redis 性能），还可以将快照留在原地以便重启服务器的时候使用。 



RDB配置：（在本地redis目录下的redis.conf文件中修改）

```conf
save 60 1           
#在60秒(1分钟)之后，如果至少有1个key发生变化，Redis就会自动触发BGSAVE命令创建快照。 

save 360 6         
#在360秒(6分钟)之后，如果至少有6个key发生变化，Redis就会自动触发BGSAVE命令创建快照。
```



**（append-only file，AOF）追加文件**

每有执行一条会更改Redis数据的命令，该命令写入到内存缓存 `server.aof_buf` 中，然后再根据 `appendfsync` 配置来决定何时将其同步到硬盘中的 AOF 文件。



AOF配置：（在本地redis目录下的redis.conf文件中修改）

```conf
#AOF开启
appendonly yes

#AOF 持久化方式
appendfsync always    #每次有数据修改发生时都会写入AOF文件,这样会严重降低Redis的速度
appendfsync everysec  #每秒钟同步一次，显示地将多个写命令同步到硬盘(推荐)
appendfsync no        #让操作系统决定何时进行同步
```




## redis应用场景：

set应用场景   关注模型（set交集并集差集）  电商商品筛选

抽奖需求：参与抽奖 、 查看所有抽奖者 、 开奖

参与抽奖： sadd key {userID}    

查看所有抽奖者 ：smembers key 

开奖： srandmember key [count]  集合中所有元素不变  /   spop key [count] 抽中的元素会从元素中移除    


