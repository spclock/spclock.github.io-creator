---
title: Spring流程
date: 2020-07-23T21:16:13+08:00
lastmod: 2020-07-23T21:16:13+08:00
author: spc
cover: /img/landscape19.jpg
categories: ["java"]
tags: ["spring"]
# showcase: true
draft: false
---

spring流程

<!--more-->

1. xml，注解Annotation，配置类，等等
2. 有一个抽象层来的读取bean（BeanDefinitionReader）
3. beanfactoryPostProcessor来扩展bean
4. 得到bean的信息
5. 反射来创建实例和BeanPostProcessor来扩展

监听器：如果在容器的不同阶段做不同的事情，那么怎么来处理？

BeanFactory：按照模板来创建bean
FactoryBean：创建独特bean

