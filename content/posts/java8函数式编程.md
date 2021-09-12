---
title: Java8函数式编程
date: 2020-02-11T19:12:39+08:00
lastmod: 2020-02-11T19:17:39+08:00
author: spc
cover: /img/landscape8.jpg
categories: ["java"]
tags: ["java8", "函数式编程"]
# showcase: true
draft: false
---

学习java8函数式编程

<!--more-->

# Java8函数式编程
用java8的函数式编程来简化自己的代码

lambda表达式、方法引用和实例方法应用 自动转换成函数接口  

如何使用函数式编程  
简单来说就是：
* 函数的返回值类型要相同
* 参数的类型要相同

## 几种类型
* 有 → 有
   * predicate (T → boolean)
   * funtion (T → R)
* 有 → 无 Consumer (T → System.out.println("x");)
* 无 → 有 Supplier (() → new Object)


```java
public class User{
    private String name;
    public String getName() {return name;}

    public void judgeName(Predicate preadicate){}
    public static void main(String[] args){
        judgeName(user->user.getName.equals("sqc"));

        judgeName(new Predicate<User>{
            @Override
            public boolean test(User user){
                return user.getName.equals("sqc");
            }
        });
    }
}
    
```


## lambda表达式
任何只包含一个抽象方法的接口都可以被自动转换为函数接口  
lambda表达式会变成一个static方法  
完整的lambda表达式`(User user)->{//方法体}`
你不加类型(User)也行 会帮你根据上下文匹配对应类型  

## 方法引用
* 静态方法引用
* 实例方法引用

```java
//静态方法引用
    public static void xxx(String str){} 
        //引用方式 User::xxx
```
```java
//实例方法引用
    public void xxx(){} //传参数会 传本类 this
        //引用方式 User::xxx
```

## Comparator
* comparing() 比较
* reverse() 相反
* thenComparing() 再比较



