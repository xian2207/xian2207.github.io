---
layout:     post
title:      UNIX网络编程 学习笔记 (一)
subtitle:   UNIX网络编程 学习笔记 (一) 
date:       2019-11-10
author:     王鹏程
header-img: img/post-bg-ios10.jpg
catalog: true
tags:
    - C/C++
    - UNIX
    - 网络编程
---

# UNIX网络编程 学习笔记
_参考链接：_

- [《Unix网络编程》卷1 初级](https://blog.csdn.net/zzxiaozhao/article/details/102637708)

## I/O复用：select和poll函数

进程可能在监听socket时还可能存在I/O的相关操作，会造成socket处理程序的阻塞。因此进程需要一种预先告知内核的能力，使得内核一旦发现进程指定的一个或多个I/O条件就绪;就直接通知进程--I/O复用。其由select和poll两个函数支持。

I/O复用使用场景如下：
- 客户端处理互交式输入和网络socket时，必须使用I/O复用
- 一个客户端，同时处理多个套接字
- TCP服务器既要监听套接字，又要处理已连接套接字。
- 一个服务器，既处理TCP又处理UDP
- 一个服务器处理多个服务器或者多个协议。

### 6.2 I/O模型

UNIX下可用的5种I/O模型：

- 阻塞式I/O；
- 非阻塞式I/O；
- I/O复用；
- 信号驱动式I/O；
- 异步I/O。