---
title: Class and Reflect
date: 2020-02-15T23:26:25+08:00
lastmod: 2020-02-15T23:26:25+08:00
author: spc
cover: /img/landscape12.jpg
categories: ["java"]
tags: ["class", "reflect"]
# showcase: true
draft: false
---

类型与反射

<!--more-->

# Class
` RTTI（Run-Time Type Identification）运⾏时类型识别  `：不管你是当前是什么类型，总能调用`getClass()`获得真实类型

class：字节码，它相当于一份说明书，每当创建一个对象，有加载过按说明书创建对象，jvm会加载class文件。  

静态变量本质 是属于 class  

* jdk7：永久代
* jdk8：元空间

class 对象生命周期

获取class
```java
 xxx.getClass()  //1
 Class.forName("全限定类名") //2
```

# classloader
* Classloader负责从外部系统中加载⼀个类
  * 这个类对应的Java⽂件并不⼀定需要存在
  * 这个字节码并不⼀定需要存在（字节码本质是字节流，能在内存中）
  * 这是Java世界丰富多彩的应⽤的基⽯
* Classloader的双亲委派加载模型 （父类有加载，子类不加载，否则子类自己加载）
* Java语⾔规范与Java虚拟机规范
  * Java Language Specification JLS
  * Java Virtual Machine Specification JVMS
  * 这种分离提供了在JVM上运⾏其他语⾔的可能

# 反射
反射：通过jvm看到自己，看清自己有什么东西(看到自己`说明书`)。  
根据参数动态生成xx：  
创建实例： 1.有对应class 2.获取构造器 3.newInstance()  
调用方法:  1.有对应class 2.获取方法 3.invoke()  
获取属性:  1.有对应class 2.获取属性 3.get()  

```java
Class c=Class.forName("//全限定类名");
//getDeclaredConstructor：对应构造函数     getConstructor:public构造函数
Object object= c.getDeclaredConstructor().newInstance(); 

c.getMethod("方法名", 参数的class ... ).invoke(该对象Object实例,参数...);

c.getField("属性").get(该对象Object实例);
```


