---
title: Java泛型
date: 2020-02-16T10:30:04+08:00
lastmod: 2020-02-16T10:30:04+08:00
author: spc
cover: /img/landscape13.jpg
categories: ["java"]
tags: ["泛型"]
# showcase: true
draft: false
---

java是假泛型！！！

<!--more-->

# 泛型

## 泛型历史
jdk5才有泛型  
之前怎么保证类型安全？  
创建新的装饰器来保证类型安全  

因为java保证向后兼容性
* java选择假泛型
* C#选择重新新API


## 泛型擦除
* Java的泛型是假泛型，是编译期的泛型
  * 泛型信息在运⾏期完全不保留
* 编译器的警告
  * 使⽤限定符List<?>
  * 可以通过利⽤泛型擦除来绕过编译器检查
* List<String>并不是List<Object>的⼦类型
  * 类⽐String/Object, String[]/Object[]

不同泛型是不同类（全新的类型）

不是相同的类（编译上），也是相同的类（字节码上）
`List<String> list=new ArrayList<>();`
`List<Integer> list=new ArrayList<>();`
`List<Integer> list=new ArrayList<String>();` 是错的×

但在**字节码**上是没有了泛型`NEW java/util/ArrayList` :泛型的擦除 java是假泛型  
```java
//方法
public void testObject(Fun fun){}
public void testArray(Fun[] fun){}
public void testList(ArrayList<Fun> fun){}

//调用
testObject(Zi zi)
testArray(Zi[] zi)
testList(ArrayList<Zi> zi)  //←报错
// List<String>并不是List<Object>的⼦类型
//因为泛型擦除会带来不安全，如果编译过了的话

//绕过编译器检查
ArrayList<Fun> list=new ArrayList<>();
ArrayList rawList=list;
rawList.add("");
rawList.add(1);
ArrayList<Zi> ziList=(ArrayList<Zi>) list;
```

## 泛型的绑定
* ? extends 要求泛型是某种类型及其⼦类型
* ? super 要求泛型是某种类型及其⽗类型
* Collections.sort

