---
title: Docker
date: 2020-03-02T07:52:23+08:00
lastmod: 2020-03-02T07:52:23+08:00
author: spc
cover: /img/food9.jpg
categories: ["Docker"]
tags: ["docker"]
# showcase: true
draft: false
---

Docker?

<!--more-->


# Docker
Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的镜像中，然后发布到任何流行的 Linux或Windows 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口。

Docker通常用于如下场景：
* web应用的自动化打包和发布；
* 自动化测试和持续集成、发布；
* 在服务型环境中部署和调整数据库或其他的后台应用；
* 从头编译或者扩展现有的OpenShift或Cloud Foundry平台来搭建自己的PaaS环境。
  
Docker pull <仓库><image/镜像><版本>    仓库默认是`docker 版本默认laster`  
`Docker images`

Docker container ls -a  
`Docker start <id/name>   Docker container start <id/name>`  
`Docker stop <id/name> `  
如果id前几个字符没重复就不用全写  
Docker run <image> 后面是image的参数命令      没创建证明报错用 -it 看看报什么错  
Docker run -it <image> 后面是image的参数命令    用完就删除container  
Docker run -d <image> 后面是image的参数命令  
参数命令例子：bash（进入交互命令行）  
Docker run --name <Name> -v <path>:<path> -p <post>:<post>  
 -e <环境变量名==xxx  例如：mysqlName and password> -d <image>  

Docker rm <id/name> 在此次之前要stop，那为什么rm -rf 就可以  
Docker exec <id/name>  
Docker logs <id/name>  Docker logs -f <id/name>  
Docker inspect <id/name>  

Docker tag <id/name> 127.0.0.1:5000/文件路径/文件名:版本号  
Docker pull 127.0.0.1:5000/文件路径/文件名:版本号  

如何显示出来/访问：例子：localhost:8080/文件路径/文件名/tags/list  
具体如何要自己上网查查  
 

## Docker改变了了软件世界
* 在Docker出现之前……
	* 软件在操作系统上是如何工作的？
	* 如何交付软件？
	* 如何部署软件？
* Docker出现之后……

## Docker能做什什么？
* 保证开发、测试、交付、部署的环境完全一致
* 保证资源的隔离
* 启动临时的、⽤用完即弃的环境，例如测试
* 迅速（秒级）超大规模部署和扩容

## Docker的基本概念
* 镜像 image
	* 一个预先定义好的模板⽂文件，Docker引擎可以按照这个模板
⽂文件启动⽆无数个一模一样，互不干扰的容器
* 容器 container
	* 一台虚拟的计算机，拥有独⽴的：
		* 网络
		* 文件系统
		* 进程
* 默认和宿主机不不发生任何交互
	* 意味着数据是没有持久化的！

## docker pull/images
* 下载一个指定的镜像，⽅方便便随时启动
* docker pull mysql:5.7.28 下载指定镜像
* docker images 查看本地已有的镜像

registry.cn-beijing.aliyuncs.com/dr1/hcsp:0.0.16  
镜像仓库镜像名tag

## docker run/ps
* docker run装载镜像成为一个容器器
	* 就好像从蛋糕模⼦子做出来一个蛋糕
	* 在这个容器看来，自己就是一台独⽴立的计算机
	* 每个容器有⼀一个ID，支持缩写
* docker run -it <镜像名> <镜像中要运行的命令和参数>  没填image参数默认是bash dockerfile有写得
	* 交互式命令行，当前shell中运行，Ctrl-C退出
* docker run -d <镜像名> <镜像中要运行的命令和参数>
	* daemon模式，在后台运⾏行行

## docker run 常用的参数
* --name 为容器指定一个「名字」
* --restart=always 遇到错误自动重启
* -v <本地文件>:<容器文件>
* -p <本地端口>:<容器端口>
* -e NAME=VALUE

## docker start/stop
* 启动/停止一个容器
	* 可以想象为开关机


## docker rm
* 删除⼀一个容器
	* 想象成将电脑丢掉

## docker exec
* 指定目标容器，进⼊入容器执行命令
	* docker run -it <目标容器ID> <目标命令（通常为bash）>
	* 可以「想象」成ssh
* 调试、解决问题必备命令

## docker logs
* docker logs <容器ID或容器名>
	* 查看目标容器器的输出
* docker logs -f <容器ID或容器名>

## docker inspect
* 高级命令，可以无视
	* 查看容器的详细状态

## 分层的镜像
图片自己补全

## Dockerfile
* 指定镜像如何⽣生成
* 编写第一个Dockerfile
* docker build .
* 每个镜像会有一个唯一的ID


## Docker的镜像仓库与tag
* 可以任意对镜像进行tag操作
	* 决定了未来这个镜像会被push到哪⾥
	* 决定了未来从哪⾥载镜像
* 可以方便的创建镜像仓库的私服
	* --registry-mirror
	* --insecure-registry

## Docker与K8s
图片

## Docker与Kubernetes
图片

