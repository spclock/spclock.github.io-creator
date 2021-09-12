---
title: Maven问题
date: 2020-03-03T14:17:09+08:00
lastmod: 2020-03-03T14:17:09+08:00
author: spc
cover: /img/food11.jpg
categories: ["问题"]
tags: ["maven问题"]
# showcase: true
draft: false
---

有关maven的问题

<!--more-->

# maven问题


## maven本地仓库问题
打开`IDEA -> setting` ，左上角搜索框输入：`Maven`

![maven-setting](/posts/img/maven-setting.jpg)
而一般情况下在2和3箭头指向的目录下面没有对应的settings.xml文件，我们可以新建一个settings.xml，并把下方的配置代码复制进去。需要注意的是，需要点击上图所示右下角的override。
。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">

    <pluginGroups />
    <proxies />
    <servers />

    <localRepository>D:/server/maven/repository</localRepository>

    <mirrors>
        <mirror>
            <id>alimaven</id>
            <mirrorOf>central</mirrorOf>
            <name>aliyun maven</name>
            <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
        </mirror>
        <mirror>
            <id>alimaven</id>
            <name>aliyun maven</name>
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
            <mirrorOf>central</mirrorOf>
        </mirror>
        <mirror>
            <id>central</id>
            <name>Maven Repository Switchboard</name>
            <url>http://repo1.maven.org/maven2/</url>
            <mirrorOf>central</mirrorOf>
        </mirror>
        <mirror>
            <id>repo2</id>
            <mirrorOf>central</mirrorOf>
            <name>Human Readable Name for this Mirror.</name>
            <url>http://repo2.maven.org/maven2/</url>
        </mirror>
        <mirror>
            <id>ibiblio</id>
            <mirrorOf>central</mirrorOf>
            <name>Human Readable Name for this Mirror.</name>
            <url>http://mirrors.ibiblio.org/pub/mirrors/maven2/</url>
        </mirror>
        <mirror>
            <id>jboss-public-repository-group</id>
            <mirrorOf>central</mirrorOf>
            <name>JBoss Public Repository Group</name>
            <url>http://repository.jboss.org/nexus/content/groups/public</url>
        </mirror>
        <mirror>
            <id>google-maven-central</id>
            <name>Google Maven Central</name>
            <url>https://maven-central.storage.googleapis.com
            </url>
            <mirrorOf>central</mirrorOf>
        </mirror>
        <!-- 中央仓库在中国的镜像 -->
        <mirror>
            <id>maven.net.cn</id>
            <name>oneof the central mirrors in china</name>
            <url>http://maven.net.cn/content/groups/public/</url>
            <mirrorOf>central</mirrorOf>
        </mirror>
    </mirrors>
</settings>
```
而repository则是Maven依赖的仓库，你可以根据自己的喜好进行设置

如果是linux系统，操作过程基本相同，只是settings.xml文件的存放路径不一样，不过都可以通过上面截图所示的页面中查到  