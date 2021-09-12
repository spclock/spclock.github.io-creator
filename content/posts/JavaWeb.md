---
title: JavaWeb
date: 2020-01-18T21:45:54+08:00
lastmod: 2020-01-18T21:45:54+08:00
author: spc
cover: /img/food1.jpg
categories: ["java"]
tags: ["计算机网络", "爬虫"]
# showcase: true
draft: false
---

java爬虫

<!--more-->
    
# 想学会爬虫
1. 浏览器是如何工作的
2. 计算机网络
3. java的网络编程

想爬虫就要知道数据是怎么走的

当浏览器上输入网址，就会在主机host上找ip地址（域名解析DNS）
找到了，浏览器就会向这个IP地址发送请求（关心 TCP/IP的传输层 TCP协议）  tcp基于流Stream
服务器就会响应你的请求发你数据包，通过端口号来识别呢一个应用程序
如何浏览器就会分析 服务器发来的数据包

# 1.浏览器如何工作
* 在⽹络上传输的只是字节流
* HTTP协议
* HTML
* JavaScript
* CSS

## 2.相关计算机网络知识
[OSIvsTCP/IP](http://lazybear.online/posts/osi%E6%A8%A1%E5%9E%8B%E4%B8%8Etcpip%E6%A8%A1%E5%9E%8B%E7%AE%80%E4%BB%8B/)
* 应用层
  * HTTP/HTTPS
  * DNS
* 传输层
  * TCP
  * UDP 
## 3.java的网络编程
java 用什么发送请求
java 怎么样把着服务器响应（流io文件）变字符串处理，不一定要变字符串

## 同步与异步加载
* 服务器端⼀次返回所有的数据
* 服务器端返回部分数据，使⽤AJAX异步加载

## HTTP⼊⻔与详解
1. HTTP method
   1. GET
   2. POST
   3. PUT
   4. DELETE...  
get 和 post 区别：简单来说
get 数据都在header多，几乎没body
post 相关信息在header 表单或者用户数据在body
header url 可见 

2. HTTP status
   1. 1xx / 2xx / 3xx / 4xx / 5xx / 6xx 分别代表什么意思
3. **HTTP Request** 
   1. headers
      1. Accept*
      2. **Cookie**
      3. User-Agent
      4. Referer
   2. body
      1. 表单
      2. k-v对 
4. **HTTP Response**
   1. headers
      1. Content-type
      2. **Set-Cookie**
   2. body
      1. JSON
      2. HTML/XML
      3. ⼆进制（图⽚/下载⽂件）

cookie 是跟着域名走的 什么意思

你带有cookie的request 127:0:0:1 和 带有cookie的request localhost 是不同的(域名不一样)
HTTP协议是⽆状态的 只管传数据 浏览器分析数据