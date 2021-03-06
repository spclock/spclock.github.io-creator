---
title: OSI模型与TCPIP模型简介
date: 2020-01-19T16:35:45+08:00
lastmod: 2020-01-19T16:35:45+08:00
author: spc
cover: /img/landscape1.jpg
categories: ["计算机网络"]
tags: ["OSI", "TCP/IP"]
# showcase: true
draft: false
---

OSI和TCP / IP都是最受欢迎和使用最广泛的通信网络协议。它们之间的主要区别在于OSI是一种概念模型，实际上并未用于通信。它定义了如何在网络或不同体系结构中传输数据。而TCP / IP被广泛用于链路的建立和网络互动。TCP / IP还仅允许在网络层上使用连接模式，但是这两种方法都在传输层上。使用OSI模型，它可以促进无连接和面向连接的网络通信，但是面向连接的通信仅在传输层中被允许。

<!--more-->

![TCPIPvsOSI](/posts/img/TCPIPvsOSI.png)

# OSI模型
在OSI模型是一个概念模型由国际标准化组织，它允许各种通信系统通过标准的协议进行通信的发展。


**应用程序层**：这是唯一与用户数据直接交互的层。但是，应注意，软件应用程序不是应用程序层的组件；应用程序层负责软件用户赖以获取重要信息的协议和信息操纵。  
**表示层**：该层的主要任务是准备信息，以便可以使用应用程序层，也就是说，第6层使信息可供消费应用程序使用。两个通信器可以使用不同的加密方法进行通信，以便将传入的数据转换为接收设备应用程序层可以理解的语法。  
**会话层**：这是用于打开和关闭两台机器之间的通信的层。打开和关闭对应关系之间的时间段称为会话。  
**传输层**：流控制和错误控制负责此层。传输层检查接收端是否存在错误，以确保所接收数据的完整性，如果没有，则请求重新传输。   
**网络层**：网络层允许在两个不同的网络之间传输信息。网络层还确定信息到达其目的地的最佳物理路线。  
**数据链路层**：数据链路层从该层收集数据包并将其分成较小的部分。像网络层一样，互连通信中的流控制和错误控制也负责数据链路层。  
**物理层**：此层包含信息传输中涉及的物理设备，例如电线和交换机。这也是1和0的字符串，其中信息变为比特流。
 
# TCP / IP模型：
Internet协议是为网络通信建立的规则集。TCP / IP被认为是可靠的网络协议模型。它是OSI模型的精简版本。

**网络接口层**： 网络访问层是OSI模型中可用的数据链路层和物理层的组合。物理寻址是在此层中完成的，即源和目标的MAC地址已分配给数据包。因此，该层负责数据的物理传输。  
**Internet层**： Internet层用于将独立的数据包发送到目的地的网络。在此层中分配了所有连接到TCP / IP网络的机器，Web服务器，节点。  
**传输层**：它允许信息以信息图表的形式从源传送到目标主机，而没有缺陷。  
**应用程序层**：应用程序层在三层中执行OSI模型的任务：应用程序，表示层和会话层。节点和节点之间的交互是必需的，它可以处理用户界面的要求。应用层中可用的协议是TFTP，HTTP，Telnet，SSH，NTP，DNS，NFS，FTP，SNMP，DHCP，SMTP。  

 
# OSI模型与TCP / Ip模型之间的主要区别
让我们讨论OSI模型与TCP / IP模型之间的一些主要区别

* OSI知道水平方法，垂直方法称为TCP / IP方法。
* OSI是独立于协议且通用的，而TCP / IP具有支持Internet开发的常规法律。
* 与OSI模型相比，TCP / IP模型更可靠
* 包被提供给OSI传输层，但是在TCP / IP情况下不能确定。
* 在OSI模型中，表示和会话层可用，而TCP / IP模型不包含该层。
* TCP / IP实现了操作系统的功能，OSI帮助指导网络并用作参考工具
* TCP / IP提供网络级别的连接功能，而OSI网络层提供连接和无连接服务。
* 没有其他模型是TCP / IP，而OSI则尝试匹配其他模型设计，因为它是参考。
* 协议可以轻松终止，而原始规则在TCP / IP模型中，而新规则可以在OSI模型中引入。