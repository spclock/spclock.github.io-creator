---
title: "HashMap"
date: 2022-02-09T23:02:04+08:00
author: Spc
# cover: /img/设计模式.png
categories: ["java"] 
tags: ["HashMap","数据结构","面试"]
# showcase: true
draft: false
---

对hashmap的认识有多少点进来看看吧

<!--more-->

# HashMap



## 底层数据结构：

jdk1.7：数组+链表 **链表是解决hash冲突**   

jdk1.8：数组+链表+红黑树 **链表过长会影响性能**

- 当链表超过 8 且数据总量超过 64 才会转红黑树。
- 将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树，以减少搜索时间。



## HashMap 的扩容方式？

Hashmap 在容量超过负载因子所定义的容量之后，就会扩容。Java 里的数组是无法自动扩容的，方法是将 Hashmap 的大小扩大为原来数组的两倍，并将原来的对象放入新的数组中。**jdk1.7使用头插法 jdk1.8使用尾插法** 





## 链表长度为8，为什么选择8 ？（泊松分布）

HashMap源码中，容器中节点分布在 hash 桶中的频率遵循**[泊松分布](https://link.zhihu.com/?target=http%3A//en.wikipedia.org/wiki/Poisson_distribution)**，按照泊松分布的计算公式计算出了桶中元素个数和概率的对照表，可以看到链表中元素个数为 8 时的概率已经非常小，再多的就更少了，所以原作者在选择链表元素个数时选择了 8，是根据概率统计而选择的。



默认加载因子是多少？为什么是 0.75 ？

默认的loadFactor是0.75，0.75是对空间和时间效率的一个平衡选择，一般不要修改，除非在时间和空间比较特殊的情况下 ：

- 如果内存空间很多而又对时间效率要求很高，可以降低负载因子Load factor的值 。
- 相反，如果内存空间紧张而对时间效率要求不高，可以增加负载因子loadFactor的值，这个值可以大于1。





## hashmap 和 hashtable 区别？

**线程安全问题**： hashtable的方法是同步，hashmap中的方法是非同步， 

**继承关系：**HashTable是基于陈旧的Dictionary类继承来的。 HashMap继承的抽象类AbstractMap实现了Map接口。

**允不允许null值：** HashTable中，key和value都不允许出现null值，否则会抛出NullPointerException异常。 HashMap中，null可以作为键，这样的键只有一个；可以有一个或多个键所对应的值为null。

**默认初始容量和扩容机制：** HashTable中的hash数组初始大小是11，增加的方式是 old*2+1。HashMap中hash数组的默认大小是16，而且一定是2的指数。

**哈希值的使用不同 ：** HashTable直接使用对象的hashCode。 HashMap重新计算hash值。

**遍历方式的内部实现上不同 ：** Hashtable、HashMap都使用了 Iterator。而由于历史原因，Hashtable还使用了Enumeration的方式 。 HashMap 实现 Iterator，支持fast-fail，Hashtable的 Iterator 遍历支持fast-fail，用 Enumeration 不支持 fast-fail

