---
title: 线程安全or同步
date: 2020-01-30T20:17:04+08:00
lastmod: 2020-01-30T20:17:04+08:00
author: spc
cover: /img/landscape4.jpg
categories: ["java"]
tags: ["thread", "死锁","线程安全","同步"]
# showcase: true
draft: false
---

死锁排查，线程安全，生产者消费者，线程池，按自己想法调度线程。
静态和非静态同步方法锁问题。

<!--more-->

[线程安全(同步)](#%e7%ba%bf%e7%a8%8b%e5%ae%89%e5%85%a8%e5%90%8c%e6%ad%a5)  
[生产者消费者](#%e7%94%9f%e4%ba%a7%e8%80%85%e6%b6%88%e8%b4%b9%e8%80%85)  
    [wait/notify/notifyAll代码](#waitnotifynotifyall)  
    [lockcondition代码](#lockcondition)  
    [blockingqueue代码](#blockingqueue)  


# 线程安全(同步)
1. synchronirzed(① 方法函数上加synchronirzed，② 用的锁是this) 
   1. 静态同步方法锁：this.getClass()  非静态同步方法锁：实例对象
2. ReentrantLock(按synchronirzed撸出来的代码)
   1. Lock代替synchronirzed （用完记得一定要finally{lock.unlock()}）
   2. Condition代替Object监视器方法的使用
3. java.util.concurrent（JUC）
4. AtomicInteger （原子操作）
5. 类不可变 Integer/String
```java
//synchronized的二种形式
public synchronized int addAndGet(int i) {  } //形式1
public int addAndGet(int i) {  
    synchronized(this){方法体}//形式2
}
```

```java
//1. synchronirzed(用的锁是this)
public class Counter {
    private int value = 0;
    public int getValue() {
        return value;
    }
    // 加上一个整数i，并返回加之后的结果
    public synchronized int addAndGet(int i) {
        value += i;
        return value;
    }
    // 减去一个整数i，并返回减之后的结果
    public synchronized int minusAndGet(int i) {
        value -= i;
        return value;
    }
}
```
```java
//2. java.util.concurrent（JUC）
import java.util.concurrent.locks.ReentrantLock;
public class Counter {
    ReentrantLock lock = new ReentrantLock();
    private int value = 0;
    public int getValue() {
        return value;
    }
    // 加上一个整数i，并返回加之后的结果
    public int addAndGet(int i) {
        try{
            lock.lock();
            value += i;
            return value;
        }finally{
            lock.unlock();
        }
        
    }
    // 减去一个整数i，并返回减之后的结果
    public int minusAndGet(int i) {
        try{
            lock.lock();
            value -= i;
            return value;
        }finally{
            lock.unlock();
        }
    }
}
```
```java
//3. AtomicInteger （原子操作）
import java.util.concurrent.atomic.AtomicInteger;
public class Counter {
    private static AtomicInteger value = new AtomicInteger(0);
    public int getValue() {
        return value.get();
    }
    // 加上一个整数i，并返回加之后的结果
    public int addAndGet(int i) {
        return value.addAndGet(i);
    }
    // 减去一个整数i，并返回减之后的结果
    public int minusAndGet(int i) {
        return value.addAndGet(-i);
    }
}
```

# 生产者消费者
虽然已经解决了多线程公共变量问题，但都是cpu全部决定如何调度线程  
**按自己想法调度线程** 用一下几种方法：
1. wait/notify/notifyAll
2. Lock/Condition
3. BlockingQueue
4. Semaphore
5. Exchanger
6. 等等

如果出现了`IllegalMonitorStateException` **就是没带监视器，或者你用错了别的监视器**
### wait/notify/notifyAll
```java
//wait/notify/notifyAll方法
import java.util.Random;

public class ProducerConsumer1 {
    private static Integer random;

    public static void main(String[] args) throws InterruptedException {
        Object lock = new Object();
        Producer producer = new Producer(lock);
        Consumer consumer = new Consumer(lock);

        producer.start();
        consumer.start();

        producer.join();
        producer.join();
    }

    public static class Producer extends Thread {
        final Object lock;

        public Producer(Object lock) {
            this.lock = lock;
        }

        @Override
        public void run() {
            for (int i = 0; i < 10; i++) {
                synchronized (lock) {
                    while (random != null) {
                        try {
                            lock.wait();
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                    random = new Random().nextInt();
                    System.out.println("Producing " + random);
                    lock.notify();
                }
            }
        }
    }

    public static class Consumer extends Thread {
        final Object lock;

        public Consumer(Object lock) {
            this.lock = lock;
        }

        @Override
        public void run() {
            for (int i = 0; i < 10; i++) {
                synchronized (lock) {
                    while (random == null) {
                        try {
                            lock.wait();
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                    System.out.println("Consuming " + random);
                    random = null;
                    lock.notify();
                }
            }
        }
    }
}
```
### Lock/Condition
```java
//Lock/Condition
import java.util.Random;
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.ReentrantLock;

public class ProducerConsumer2 {
    private static Integer random;

    public static void main(String[] args) throws InterruptedException {
        ReentrantLock lock = new ReentrantLock();
        Condition condition = lock.newCondition();
        Producer producer = new Producer(lock, condition);
        Consumer consumer = new Consumer(lock, condition);

        producer.start();
        consumer.start();

        producer.join();
        producer.join();
    }

    public static class Producer extends Thread {
        ReentrantLock lock;
        Condition condition;

        public Producer(ReentrantLock lock, Condition condition) {
            this.lock = lock;
            this.condition = condition;
        }

        @Override
        public void run() {
            for (int i = 0; i < 10; i++) {
                try {
                    lock.lock();
                    while (random != null) {
                        try {
                            condition.await();
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                    random = new Random().nextInt();
                    System.out.println("Producing " + random);
                    condition.signal();

                } finally {
                    lock.unlock();
                }
            }

        }
    }

    public static class Consumer extends Thread {
        ReentrantLock lock;
        Condition condition;

        public Consumer(ReentrantLock lock, Condition condition) {
            this.lock = lock;
            this.condition = condition;
        }

        @Override
        public void run() {
            for (int i = 0; i < 10; i++) {
                try {
                    lock.lock();
                    while (random == null) {
                        try {
                            condition.await();
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                    System.out.println("Consuming " + random);
                    random = null;
                    condition.signal();

                } finally {
                    lock.unlock();
                }
            }
        }
    }
}

```
### BlockingQueue
```java
//BlockingQueue

import java.util.Random;
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.LinkedBlockingQueue;

public class ProducerConsumer3 {
    public static void main(String[] args) throws InterruptedException {
        BlockingQueue<Integer> queue = new LinkedBlockingQueue<>(1);
        BlockingQueue<Integer> signal = new LinkedBlockingQueue<>(1);
        Producer producer = new Producer(queue, signal);
        Consumer consumer = new Consumer(queue, signal);

        producer.start();
        consumer.start();

        producer.join();
        producer.join();
    }

    public static class Producer extends Thread {
        BlockingQueue<Integer> queue;
        BlockingQueue<Integer> signal;

        public Producer(BlockingQueue<Integer> queue, BlockingQueue<Integer> signal) {
            this.queue = queue;
            this.signal = signal;
        }

        @Override
        public void run() {
            for (int i = 0; i < 10; i++) {
                int random = new Random().nextInt();
                System.out.println("Producing " + random);
                try {
                    queue.put(random);
                    signal.take();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }

    }

    public static class Consumer extends Thread {
        BlockingQueue<Integer> queue;
        BlockingQueue<Integer> signal;

        public Consumer(BlockingQueue<Integer> queue, BlockingQueue<Integer> signal) {
            this.queue = queue;
            this.signal = signal;
        }

        @Override
        public void run() {
            for (int i = 0; i < 10; i++) {
                try {
                    System.out.println("Consuming " + queue.take());
                    signal.put(0);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```