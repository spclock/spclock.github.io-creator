---
title: Spring Web应用
date: 2020-03-02T07:57:49+08:00
lastmod: 2020-03-02T07:57:49+08:00
author: spc
cover: /img/food10.jpg
categories: ["java"]
tags: ["框架", "Spring web"]
# showcase: true
draft: false
---

Cut out summary from your post content here.

<!--more-->

# Spring 
## 从零开始⼀个Spring应⽤
* pom.xml
* src/main/java/hello/Application.java
* src/main/java/hello/HelloController.java

## Web应⽤的本质
* 处理HTTP请求
  * 从HTTP请求中提取query string (查询字符串)
  * 从HTTP请求中接收payload（负载/body）中的参数
* 返回HTTP响应
  * status code
  * HTTP response header
  * HTTP response body
    * JSON
    * HTML
    * ...

Get method 是幂等 (你发多少次都能看成一次)
```java
    @RequestMapping("/search")
    public String index(@RequestParam("q")String searchWork,
                        @RequestParam(value="charset",required = false)String searchWork,) {
        return "you search work is:"+searchWork;
    }
    //RequestParam参数 你得看看源代码怎么写
```


## HTTP GET
* Query string
  * ?param1=value1&param2=value2
* 通常⽤来传递⾮敏感信息
* 使⽤@RequestParam进⾏接收


## RESTful API
* 使⽤HTTP动词来代表动作
  * GET：获取资源
  * POST：新建资源
  * PUT：更新资源
  * DELTE：删除资源
* 使⽤URL（名词）来代表资源
  * 资源⾥⾯没有动词
  * 使⽤复数来代表资源列表

## @RestController

古老的用@Controller spring 2.5版本  
* 使⽤RESTful⻛格的参数
* 使⽤@PathVariable进⾏参数提取

## @PostMapping
* 处理POST请求
* 从HTTP POST请求中提取body

![绑定](/posts/img/spring_web_http.jpg)

## ⽣成HTTP响应
* 直接操作HttpServletResponse对象
  * 原始、简单、粗暴  
* 直接返回HTML字符串
  * 原始、简单、粗暴
* 返回对象，并⾃动格式化成JSON
  * 常⽤
  * @ResponseBody
* 模板引擎渲染
  * JSP/Velocity/Freemaker

## 周边⽣态系统
* HTTPS
* 分布式部署
* 扩展功能
  * 数据库
  * Redis缓存
  * 消息队列
  * RPC（Dubbo/Spring Cloud）
  * 微服务化

获取参数
1. query string
2. RESTAPI
3. body
4. ……

这是spring mvc提供的自动参数绑定  
绑定参数@PathVariable("参数名")  

header 中的type属性能告诉 xxx body是什么东西   
requsetMapping、postMapping……  