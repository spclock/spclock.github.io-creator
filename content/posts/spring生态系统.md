---
title: Spring生态系统
date: 2020-03-04T13:40:23+08:00
lastmod: 2020-03-04T13:40:23+08:00
author: spc
cover: /img/food12.jpg
categories: ["java"]
tags: ["spring"]
# showcase: true
draft: false
---

Spring生态系统：spring springMVC MyBaits (ssm)

<!--more-->

# Spring生态系统
1. spring中引入bean
2. spring+MyBatis （h2/MySQL/Postgres）
3. 模板引擎（后端渲染HTML）
   1. FreeMarker
   2. Groovy
   3. Thymeleaf （Spring 官网使用这个）
   4. Velocity
   5. JSP
4. 前后端分离和后端渲染

MVC :model  view  controller  
　　数据  视图  controller

   

## Spring生态系统中的主要子项目：
1. Spring Framework：Spring项目的核心。IoC、AOP、MVC、JDBC、事务处理等等。
2. Spring Web Flow：Web工作流引擎，建立在Spring MVC基础之上。相对较独立于Spring Framework发展。
3. Spring BlazeDS Integration：提供Spring与Flex技术集成的模块。
4. Spring Security：基于Spring的认证和安全工具，基于Acegi框架。
5. Spring Security OAuth：对OAuth和Spring的集成提供支持。通过OAuth，桌面应用可以对Web应用进行简单、标准的安全调用。
6. Spring Dynamic Modules：可以让Spring应用运行在OSGI的平台上，如Eclipse，增加了应用再部署和运行时的灵活性。
7. Spring Batch：提供构建批处理应用和自动化操作的框架。
8. Spring Integration：企业级集成模式的实现。
9. Spring AMQP：为使用基于AMQP（高级消息队列协议）的消息服务提供支持。
10. Spring .NET：在.NET环境中使用Spring。
11. Spring Android：基于Java的REST客户端。
12. Spring Mobile：基于Spring MVC构建，为移动终端的服务器应用开发提供支持。
13. Spring Social：Spring框架的扩展，帮助Spring应用更方便的使用SNS。
14. Spring Data：为Spring 应用提供使用非关系型数据的能力。

# Spring的原理和组成
封装一系列的开箱即用的组件功能：
* Spring JDBC
* SpringMVC
* Spring Security
* Spring AOP
* Spring ORM
* Spring Test


# Spring MVC
User argent --http request --- dispatcher Servlet ---- handlerMapping ---Controller  
---- ViewResolver  
---- Model ---View  


# Spring Boot
Springboot 基本上是Spring框架的扩展，它消除了设置Spring应用程序所需的xml配置，为了更快更高效的开发生态系统
SpringBoot中一些特点：
1. 创建独立的Spring应用
2. 嵌入Tomcat，Jetty，Undertow而且不需要部署。
3. 提供的 starters pom来简化Maven配置
4. 尽可能自动部署spring应用
5. 提供生产指标，健壮检查和外部化配置 

## Spring Boot 的核心配置文件
1. Application配置文件主要用于Spring Boot项目的自动化配置
2. Bootstrap配置文件应用场景：
   1. 使用Spring Cloud Config配置中心时需要在bootstrap配置文件中添加连接到配置中心的配置属性来加载外部配置中心的配置信息。
   2. 一些固定的不能被覆盖的属性
   3. 一些加密/解密的场景

## Spring Boot 自动配置原理
@EnableAutoConfiguration，@Configuration，@ConditionalOnClass
得到配置文件，根据类路径下是否有这个类去自动配置。

## SpringBoot Starters

## SpringBoot实现热部署
主要有两种方式：
* Spring Loaded
* Spring-boot-devtools
## Spring-boot-starter-parent
我们在创建一个SpringBoot项目，默认都会有<parent>
<artifactId>spring-boot-starter-parent</artifactId>主要有如下作用：
1. 定义了java编译版本（现在还是1.8）
2. 使用utf-8格式编码
3. 继承spring-boot-dependencies，这个里边定义了依赖的版本，因此我们在写依赖时才不需要写版本号。
4. 执行打包操作的配置
5. 自动化的资源过滤
6. 自动化的插件配置
7. 针对application.properties和application.yml的资源过滤，包括profile定义的不同环境的配置文件，例如application-dev.properties和application-dev.yml

## Spring中如何解决跨域问题
前端通过jsonp来解决，不过jsonp只可以发送GET请求，在RESTful风格的应用中就显得非常无助，因此我们推荐后端通过（CORS Cross-origin resource sharing）来解决跨域问题。
之前xml配置CORS，现在通过@CrossOrigin

