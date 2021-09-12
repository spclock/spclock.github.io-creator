---
title: String原理
date: 2020-02-14T08:14:37+08:00
lastmod: 2020-02-14T08:14:37+08:00
author: spc
cover: /img/landscape11.jpg
categories: ["java"]
tags: ["String", "StringBuffer","StringBuilder"]
# showcase: true
draft: false
---

String的原理

<!--more-->

# String的原理
为什么在互联网C系列不好使，很少拿c来写后端应用  
因为C和C++字符串处理很烂。  
能处理好字符串是Web服务器的基本要求
* Java
* PHP
* Python
* Ruby
* Perl

## 字符串的不可变性
* 为什么字符串是不可变的？(看源代码final)
  * 安全：线程安全，存储安全
* 缺点，每当修改的时候都需要重复创建新的对象

扩展：存储安全  
hash 约定
1. 二个对象相等必须返回hashcode是一样的
2. 在整一个java生命周期必须返回一样hashcode  

因此需要的对象是不可变

## 想修改怎么办
* StringBuilder(优先使⽤，线程不安全，速度快)
* StringBuffer(线程安全，速度相对较慢)

## 字符串与编码
* 字符 -> 字节  (编码)
* 字节 -> 字符  (解码)

## 字符集 ：Uoicode
**字符集**：相当于一个字典  
**码点(code point)**：字典里面要找的字    
存所有语言，int是最符合Uoicode存储    
因此每一个字符都要4byte，这样太浪费了  

**BOM** （字节顺序标记：ByteOrderMark） 

* 每个数字代表⼀个字符，叫做“码点”（code point）
* 最常⻅的两种编码⽅案：
* UTF-16：Java程序内部的存储⽅法
* UTF-8
  * Mac/Linux默认编码是UTF-8
  * Windows默认的中⽂编码是GBK(时间问题，windows比UTF-8早)
  * 如果没有意外，把你所有的编码⽅案都改成UTF-8

## 选择字符集
```java
//用jdk7 NIO举例子
Files.readAllLines(file.toPath(), Charset.forName("GBK"));

//例子 InputStreamReader(InputStream in, CharsetDecoder dec)
new InputStreamReader( new FileInputStream(file) , "UTF-8");
```


## 如果出现乱码
* 文件是什么编码
* 读到计算机是什么编码
* 输出是什么编码


