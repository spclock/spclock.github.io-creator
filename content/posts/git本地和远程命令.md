---
title: git本地和远程命令
date: 2020-02-01T20:45:56+08:00
lastmod: 2020-02-01T20:45:56+08:00
author: spc
cover: /img/landscape5.jpg
categories: ["git"]
tags: ["git命令"]
# showcase: true
draft: false
---

git的本地和远程仓库的有关命令

<!--more-->

# 学习git命令行
## git命令（本地）
用git前要配置好
```git
git config --global user.name 你的英文名
git config --global user.email 你的邮箱
git config --global push.default simple
git config --global core.quotepath false
git config --global core.editor "code --wait"
git config --global core.autocrlf input
```
注意：上面的英文名和邮箱跟 GitHub 没有关系。  
上面的设置是默认vscode，你自己也可以设置与git相关的软件
`git config --global --list 查看配置如何`

`Git init `  创建名叫.git本地仓库（注意要看当前路径，不然设错路径文件很大）  
`Git add 文件名`  (或者当前路径’ . ’) 添加准备commit的状态  
`Git status`  顾名思义看状态  


`Git commit –m `字符串（提交的理由）  
`Git commit –v`详细看修改哪些地方，然后在第一行家提交理由
打开vscode（自己之前设置打开的软件，可选的）

`Git log `查看当前.git仓库之前的版本

`Git reset --hard xxx `回退到xxx版本（之前commit过有记录版本号用git log）

`Git reflog `查看全部的版本号可以查看所有分支的所有操作记录（包括已经被删除的 commit 记录和 reset 的操作）

`Git branch `分支名 创建分支         
`Git branch –d `分支名 删除分支  
`Git checkout `分支名 切换分支

`Git merge xxx `将xxx分支合并到当前分支(很多时候回发生冲突)
 
<!-- ![冲突解决办法](/posts/img/git解决冲突.jpg) -->

## git远程命令（ssh绑定）
### 我的第一次生成钥匙（一个私钥id_res 一个公钥id_res.pub）
 ![ssh公钥私钥](/posts/img/ssh公钥私钥.jpg)

### 生成完在github上设置自己的公钥
![github设置公钥](/posts/img/github设置公钥.jpg)

### 在git bash上验证
`ssh -T git@github.com 然后github会给自己一个公钥证明自己是github官网`
 ![bash验证ssh](/posts/img/bash验证ssh.jpg)

输出是 HI 自己的github名 就绑定成功
 
 <!-- ![冲突解决办法](/posts/img/ssh验证方式.jpg) -->

### Git Push（Pull更新之后在push）
开始上传本地仓库，没有就要（看回git本地命令了）
上传命令（ssh）
`git remote add origin git@github.com:spclock/first_Demo.git`
git 远程的意思 add origin git@github.com:git账号名/开源的项目名

如何已经有origin，怎么改
`Git remote set-url origin git@github.com:git账号名/开源的项目名`
`git push -u origin master（分支的名字）`
-u 如果再有push就执行上次那一个分支（直接git push）   
如果你要换分支要重新打一次命令改参数
### Git clone:复制github上的链接
### Git Pull（更新代码）
每次开始前都要git pull更新一下代码
 <!-- ![gitpull](/posts/img/gitpull.jpg) -->
