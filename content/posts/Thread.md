---
title: Thread
date: 2020-01-29T11:07:05+08:00
lastmod: 2020-01-29T11:07:09+08:00
author: spc
cover: /img/landscape3.jpg
categories: ["java"]
tags: ["多线程", "Thread"]
# showcase: true
draft: false    
---

什么是进程、线程和多线程，怎么用和有什么用，需要注意什么。不会就给我看

<!--more-->

# 进程、线程和任务概念
## 1.进程
**进程(Process)**是计算机中已`运行程序`的实体。进程本身不会运行，是线程的容器。程序本身只是指令的集合，进程才是程序(那些指令)的真正运行。若干进程有可能与同一个程序相关系，且每个进程皆可以同步(循序)或不同步(平行)的方式独立运行。进程为现今分时系统的基本运作单位。
## 2.线程
**线程(thread)**是操作系统能够进行运算调度的`最小单位`。它被包涵在进程之中。 
   * **单线程**：一条线程指的是进程中一个单一顺序的控制流。
   * **多线程**：一个进程中可以并发多个线程，每条线程并行执行不同的任务。
     在Unix System V及SunOS中也被称为轻量进程(lightweight processes)，但轻量进程更多指内核线程(kernel thread)，而把用户线程(user thread)称为线程。
### 线程状态
* **初始(NEW)**：新创建了一个线程对象，但还没有调用start()方法。
* **运行(RUNNABLE)**：Java线程中将就绪（ready）和运行中（running）两种状态笼统的称为“运行”。线程对象创建后，其他线程(比如main线程）调用了该对象的start()方法。该状态的线程位于可运行线程池中，等待被线程调度选中，获取CPU的使用权，此时处于就绪状态（ready）。就绪状态的线程在获得CPU时间片后变为运行中状态（running）。
* **阻塞(BLOCKED)**：表示线程阻塞于锁。
* **等待(WAITING)**：进入该状态的线程需要等待其他线程做出一些特定动作（通知或中断）。
* **超时等待(TIMED_WAITING)**：该状态不同于WAITING，它可以在指定的时间后自行返回。
* **终止(TERMINATED)**：表示该线程已经执行完毕。  
  [深入了解线程状态](https://blog.csdn.net/pange1991/article/details/53860651)

### 多线程
* Java中只有这么⼀种东⻄代表线程：**Thread**
* start⽅法才能并发执⾏！
* 每多开⼀个线程，就多⼀个执⾏流
* ⽅法栈(局部变量)是线程私有的
* 静态变量/类变量是被所有线程共享的
```java
//创建一个线程第一种方法
class MyThread extends Thread{
    @Override
        public void run() {
            //多线程的任务
        }
    public static void main(String[] args) {
        MyThread thread=new MyThread();
    }
}
//第二种方法
Thread thread=new Thread(任务);
//任务可以是Runnable的匿名内部类
```

## 3.任务
**任务（task）**是最抽象的，是一个一般性的术语，指由软件完成的一个活动。一个任务既可以是一个进程，也可以是一个线程。简言之，它指的是一系列共同达到某一目的的操作。
* Runnable —— Thread 不能返回值，没有异常抛出
* Callable —— Future 能返回值，有异常抛出
* FutureTask
  

# 问题
## 为什么要用多线程
因为要并发，cpu 太快了太闲了，需要合理使用时间片
java程序是同步/阻塞的

## 什么时候使用多线程
* IO密集型 `√`（例：文件和网络）  
* cpu密集型 `X` (cpu不闲了你还给这么多活干?)

## 使用多线程需要注意什么
1. 共享变量 (**安全隐患**)
2. 每一个线程里面的变量都是私有的




