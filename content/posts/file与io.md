---
title: File与io
date: 2020-01-22T11:14:25+08:00
lastmod: 2020-01-22T11:14:25+08:00
author: spc
cover: /img/default2.jpg
categories: ["java"]
tags: ["file", "io体系"]
# showcase: true
draft: false
---

Cut out summary from your post content here.

<!--more-->

# io体系
1. inputStream 字节输入流
2. outputStream 字节输出流
3. Reader 字符输入流 解码
4. Writer 字符输出流 编码
5. Buffer 缓冲区
   
⼀切⽂件的本质
* ⼀段字节流：
  * ⽂本⽂件（txt/代码/HTML等）
  * ⼆进制⽂件
* 每个程序负责解释⽂件中的字节流
mp4，html，class，avi 都是一段字节流，每个程序都有不同解析。

1. ⽂件写⼊字节流
2. 从⽹络读取字节流
3. 从其他什么破烂玩意读取字节流
如果你不是对⽂件系统⼗分熟悉的话，请永远使⽤绝对路
径！！！

# File
不要误会！不要误会！不要误会！
* File并不代表⼀个“⽂件”，它只代表⼀个“路径”
* 抽象的“⽂件”路径：⽂件或者⽂件夹
* File的常⻅⽅法
* 绝对路径与相对路径
* 读/写⽂件
* NIO （Java 7+）
* Non-blocking I/O，在Java领域，也称为New I/O（与多线程有关）
* NIO的Path - 就是旧版本的File （有互相转换的方法）

经典的IO模型基于流的（就意味着一个一个字节的传）  
优点：容易理解，方便抽象（方便抽象：能处理文件、网络和其他）  
缺点：慢！！！  

## NIO
基本数据模型：块  
NIO处理：1. 缓冲buffer 或 cache  2. 并发  




