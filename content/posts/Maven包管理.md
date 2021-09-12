---
title: Maven包管理
date: 2020-01-15T09:01:48+08:00
lastmod: 2020-01-15T09:01:48+08:00
author: spc
cover: /img/food2.jpg
categories: ["java"]
tags: ["包管理", "Maven"]
# showcase: true
draft: false
---

# maven笔记

<!--more-->
### 目录
[什么是包](#%e4%bb%80%e4%b9%88%e6%98%af%e5%8c%85jar)  
[包的故事](#包的故事)  
[包管理](#包管理)  
[Maven包管理](#maven%e5%8c%85%e7%ae%a1%e7%90%86)  
[包冲突问题](#包冲突问题)  
[没有Maven前的解决办法](#%e6%b2%a1%e6%9c%89maven%e5%89%8d%e7%9a%84%e8%a7%a3%e5%86%b3%e5%8a%9e%e6%b3%95)  
[Maven解决包冲突办法](#maven%e8%a7%a3%e5%86%b3%e5%8c%85%e5%86%b2%e7%aa%81%e5%8a%9e%e6%b3%95)  
[项目缺失包解决办法](#项目缺失包解决办法)
# 什么是包（jar）
很简单就相当于zip，类多了就用jar压缩


# 包的故事
JVM是一个很**耿直**的虚拟机。为什么说JVM很耿直，因为JVM的工作被设计的很简单
  1. 执行一个类的字节码
  2. 当'1'的过程遇到新的类，就会加载它
   
**问题**
* 那么你有没有想过一个问题，那些类JVM会在哪里找，这些类终不会无中生有吧！
* 项目需要某个jar包，你有没有试过你复制了一个jar包到项目，但还是没有卵用！！！

**答案**

  这就涉及到了java编译运行时 Jvm会帮你创建一个命令，命令的其中一个参数 `-classpath 全包名XXX....`
  JVM遇到一个新的类就好去`-classpath `上找对应的jar包里面的类，找到就执行，否则报错。

**传递性依赖**：简单来说就是你用的jar包里面又用别的jar包，然后别的jar包又用另一个jar包。
![包树图](/posts/img/包树图.png)

classpath hell：全限定类名是类的唯一标识 (例子：org.apache.commons.lang3)
但多个同名类同时出现在Classpath中，就是噩梦开始（包冲突问题）

# 包管理
使用第三方类，
包管理本质就是告诉JVM如何找到所需的第三方库
以及成功解决其中的冲突问题（很困难）

# Maven包管理
* 约定优于配置 （约定能通用，配置再好可能只是个人不能很好团队开发）
* Maven远远不止是包管理工具
* Maven中央仓库
  * 按照一定约定存储包(像图书馆一样管理书籍)
  ```xml
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-engine<artifact>
        <version>5.4.2</version>
        <scope>test</scope>
    </dependency>
  ```
* Maven本地仓库
  * 默认位于~/.m2
  * 下载第三方包进行缓存
* Maven的包
  * 按照约定为所有的包编号，⽅便检索
  * groupId/artifactId/version
  * 语义化版本
  * SNAPSHOT快照版本
* 传递性依赖的⾃动管理
  * 原则：绝对不允许最终的classpath出现同名不同版本的jar包
  * 依赖冲突的解决：原则：最近的胜出
  * 依赖的scope
    * compile：生产代码的依赖
    * test：只有测试代码的依赖，例如测试框架JUnit
    * provided：编译时需要，但是运⾏时由容器提供，典型如Servlet API
    * runtime：编译时候不需要，运⾏时候需要，典型如mysql,connector等JDBC的maven本地仓库


# 包冲突问题
最先声明的原则：  
`-classpath org.apache.commons.lang3(版本1):org.apache.commons.lang3(版本2)`  
多个同名类同时出现在Classpath中，JVM在`-classpath`按顺序找，找到哪一个先就用哪一个（就会用版本1 ）
你本想用版本2（修复了版本1bug），之前导入版本1忘记删后面导入版本2，但JVM用了版本1，这样就是有bug问题。

常见的包冲突异常如下：
```
AbstractMethodError
NoClassDefFoundError
ClassNotFoundException
LinkageError
```

# 没有Maven前的解决办法
手写命令行进行编译  
不完整的简单例子：  
`javac -classpath jar包名:jar包名:jar包名`
一个一个依赖全部打上去，还要搞清楚那几个包中的类冲突并进行处理

# Maven解决包冲突办法
1. 清楚那些相同类名，看依赖树（有三种方法）
   1. idea右侧Maven的可视化视图
   2. 终端bash运行`mvn dependency:tree`
   3. idea下载Maven Helper插件查看依赖树
2. 采用某一个版本
   1. 用最近原则
   2. 强行解除依赖关系
   3. 最先原则
   
# 项目缺失包解决办法
    建议最好用google搜索 “jar包名 Maven” 搜索到的依赖加到pom.xml，刷新maven。