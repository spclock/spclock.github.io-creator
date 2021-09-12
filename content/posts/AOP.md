---
title: AOP
date: 2020-03-08T16:04:57+08:00
lastmod: 2020-03-08T16:04:57+08:00
author: spc
cover: /img/food14.jpg
categories: ["java"]
tags: ["AOP", "Spring"]
# showcase: true
draft: false
---

AOP?怎么用？与oop对比好处？如何代理，注解处理？

<!--more-->
不管是代理/注解/动态字节码增强
1. 都是要处理原来class，创建新的class
2. 方法拦截

# AOP
* Aspect-Oriented Programming ⾯向切⾯编程
* 相对于OOP（⾯向对象编程）
* AOP是⾯向切⾯编程，关注⼀个统⼀的切⾯
  * `面向方法的切面`来编程，而不是`面向某一个方法调`用来编程
* AOP和Spring是不同的东⻄

## AOP适合于哪些场景
* 需要统⼀处理的场景
  * ⽇志
  * 缓存
  * 鉴权
* 如果⽤OOP来做需要怎么办？
  * 装饰器模式/静态代理 
    * 代理模式主要是`控制对某个特定对象访问`，而装饰模式主要是`为了给对象添加行为`。


## AOP的实现
* JDK动态代理
  * 优点：⽅便，不需要依赖任何第三⽅库
  * 缺点：功能受限，只适⽤于接⼝
* CGLIB/ByteBuddy字节码⽣成
  * 优点：强⼤，不受接⼝的限制
  * 缺点：需要引⽤额外的第三⽅类库
  * 不能增强final类/final/private⽅法

```java
//JDK动态代理
//1.interface、实现类、Proxy类
//2.Proxy.newProxyInstance
//3.invoke
//-------------------------------

//1.
public interface Server {
     int a(int i);
     int b(int i);
}

public class DataServer implements Server {
    @Override
    public int a(int i) {return i;}
    @Override
    public int  b(int i) {return i;}
    public int c(int i){return i;}
}

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
public class LogProxy implements InvocationHandler {
    //3.invoke写自己拦截后想做什么
    @Override
    public Object invoke(Object o, Method method, Object[] objects) throws Throwable {
        if(method.getName().equals("a")){
            System.out.println("a invoke");
            return 1;
        }
        return null;
    }
}

//2.
public class ProblemDemo {
static DataServer server=new DataServer();
 public static void main(String[] args) {
       System.out.println(server.getClass());
       Server dataServer = (Server) Proxy.newProxyInstance(server.getClass().getClassLoader(),
               new Class[]{Server.class},
               new LogProxy());
       System.out.println(dataServer.a(1));
       dataServer.b(1);
       System.out.println(dataServer.getClass());
    }
}

```

```java
//CGLIB (动态字节码增强)
//maven 导包
//代理和被代理类
//Enhancer设置setSuperclass、setCallback


import net.sf.cglib.proxy.Enhancer;
import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.util.Arrays;

public class ProblemDemo {


         static DataServer server=new DataServer();

        //-----代理类-----
         public static class LogInterceptor implements MethodInterceptor{

             private DataServer delegate;

             public LogInterceptor(DataServer delegate) {
                 this.delegate = delegate;
             }

             public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
                 System.out.println(method.getName()+"is invoke "+ Arrays.toString(objects));
                 Object retValue=method.invoke(delegate,objects);
                 System.out.println(method.getName()+"is finished " +retValue);
                 return retValue;
             }
         }


    public static void main(String[] args) {

        Enhancer enhancer=new Enhancer();
        enhancer.setSuperclass(DataServer.class);
        enhancer.setCallback(new LogInterceptor(server));

        DataServer dataServer= (DataServer) enhancer.create();
        dataServer.a(1);
        dataServer.b(1);
    }
}

```


## 装饰器模式
* Decorator pattern
* 动态地为⼀个对象增加功能，但是不改变其结构
* 本质上是⼀个“包装”

## AOP与Spring
* 在Spring中使⽤AOP实现Redis缓存
* Spring是如何切换JDK动态代理和CGLIB的？
  * spring.aop.proxy-target-class=true
* @Aspect声明切⾯
  * @Before
  * @After
  * @Around