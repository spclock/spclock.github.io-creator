---
title: JVM
date: 2020-07-15T14:06:09+08:00
lastmod: 2020-07-15T14:06:09+08:00
author: spc
cover: /img/landscape19.jpg
categories: ["Java"]
tags: ["jvm"]
# showcase: true
draft: false
---

深入了解JVM

<!--more-->

![HotSpot JVM](/posts/img/hotSpot.jpg)

• JVM是这个星球上最强⼤的VM
• Groovy/Scala/Kotlin/Jython/JRuby 

栈、堆、本地方法栈、方法区、程序计数器

栈帧
• 局部变量表
• 操作数栈 

⽅法区
• 被整个虚拟机所共享的Class信息
• 运⾏时常量池
• Java7之前：永久代（PermGen）
    • OutOfMemory: Perm gen
• Java8之后：元空间（Metaspace）
    • 元空间和Native共享内存

为什么要改，之前是和堆放在一起，容易OutofMemory


JustInTime Compiler

JLS与JVMS
• Java语⾔规范 Java Language Specification
• 定义Java语⾔的语法
• Java虚拟机规范 JVM Specification
• 定义字节码如何在JVM中运⾏

* 是不是所有的对象和数组都会在堆内存分配空间
在Java虚拟机中，对象是在Java堆中分配内存的，这是一个普遍的常识。但是，有一种特殊情况，那就是如果经过逃逸分析后发现，一个对象并没有逃逸出方法的话，那么就可能被优化成栈上分配。这样就无需在堆上分配内存，也无须进行垃圾回收了。