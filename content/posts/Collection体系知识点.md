---
title: Collection体系知识点
date: 2020-01-17T08:18:00+08:00
lastmod: 2020-01-17T08:24:00+08:00
author: spc
cover: /img/default1.jpg
categories: ["java"]
tags: ["Collection"]
# showcase: true
draft: false
---

Collection体系原理与实践

<!--more-->
[Collection体系](#collection%e4%bd%93%e7%b3%bb)

# 数组与集合的区别
### 数组
* 数组能存储基本数据类型和引用类型
* 数组的长度不可变
### 集合
* 集合是存对象的容器
* 长度可变（扩容：创建⼀个更⼤的空间，然后把原先的所有元素拷⻉过去）
* 只能存储对象，不可以存储基本数据类型


# Collection体系
![Collection体系](/posts/img/Collection体系图.jpg)

* List（有序不唯一）
  * ArrayList（用数组存）
  * LinkedList（用链表存，增删快）
* Set（无序但唯一）
  * HashSet（用哈希算法，hashcode和equals）
    * LinkHashSet(有序)
  * TreeSet（用二叉树/红黑树，comparable或者comparetor）
* Map（键值对key和value，键和值是映射关系）
  * HashMap
  * TreeMap
  
  **HashSet比ArrayList查找速度快的多**
### Collection体系提供的常用方法：
* new: new ArrayList(Collection), new ArrayList()
* R: size()/isEmpty()/contains()/for()/stream()
* C/U: add()/addAll()/retainAll()
* D: clear()/remove()/removeAll()

### Map的常用方法：
* C/U: put()/putAll()
* R:
* get()/size()
* containsKey()/containsValue()
* keySet()/values()/entrySet()
* D: remove()/clear()
  
Map实在是就是`键为Set集合`与`值为Collection集合`组合形成了Map  
因此在调用Map.KeySet()的方法中就返回了`键为Set集合`  
是同一个对象，Map的键和keySet()是关联的（例子：Set set=xxx.keySet(),改变了set对象同时也改变了Map的键，这就是**浅拷贝**）

### Collections⼯具⽅法集合
* emptySet(): 等返回⼀个⽅便的空集合
* synchronizedCollection: 将⼀个集合变成线程安全的
* unmodifiableCollection: 将⼀个集合变成不可变的（也可以
使⽤Guava的Immutable）

## 扩展知识
### HashMap
* HashMap的扩容
* HashMap的线程不安全性
* HashMap在Java 7+后的改变：链表->红⿊树
* HashMap和HashSet本质上是⼀种东⻄
* HashMap在多线程下扩容会死循环(死锁)

### Collection的其他实现
* Queue/Deque
* Vector/Stack
* LinkedList
* ConcurrentHashMap
* PriorityQueue

### Guava
* 不要重复发明轮⼦！尽量使⽤经过实战检验的类库
* Lists/Sets/Maps
* ImmutableMap/ImmutableSet
* Multiset/Multimap
* BiMap


