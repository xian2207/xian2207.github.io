---
layout:     post
title:      学习KVM
subtitle:   KVM笔记
date:       2022-03-06
author:     Xian2207
header-img: img/post-bg-ios10.jpg
catalog: true
tags:
    - KVM
    - QEMU
    - 阅读笔记
---

# KVM学习阅读笔记
> 2021-03-06 23:30:22

_参考链接：_
- [GitPage](https://yifengyou.github.io/learn-kvm/)
- [GitHub](https://github.com/yifengyou/learn-kvm)
- [GitBook](https://yifengyou.gitbooks.io/learn-kvm/content/)

## 1. 虚拟化技术简介

- 云计算：
  - 云计算是一种通过因特网以服务的方式提供动态可伸缩的虚拟化的资源的计算模式。
  - **虚拟化!=云计算**
    - 云计算是网格计算、分布式计算、并行计算、效用计算、网络存储、虚拟化、负载均衡等传统计算机技术和网络技术发展融合的产物。通过网络把多个成本较低的计算实体整合成一个强大计算能力的完美系统。借助三种云计算模式交付到用户手中。
- 云计算模式：
  - 软件即服务(Software as a Service)
  - 平台即服务(Platform as a Service)
  - 基础设施即服务(Insfrastructure as a Service)
  - ![](https://yifengyou.github.io/learn-kvm/docs/%E8%99%9A%E6%8B%9F%E5%8C%96%E6%8A%80%E6%9C%AF%E7%AE%80%E4%BB%8B/image/1531895455977.png) 
  - ![](https://yifengyou.github.io/learn-kvm/docs/%E8%99%9A%E6%8B%9F%E5%8C%96%E6%8A%80%E6%9C%AF%E7%AE%80%E4%BB%8B/image/1531895794254.png)
- 云计算的本质：
  - 说白了就是资源充分利用，同时节省时间高效作业。由运营商提供计算服务。
  - 云端的云计算系统，实质就是大型的分布式系统。虚拟化通过一个物理平台虚拟出多个虚拟平台，而其中每个虚拟平台都可以作为独立的终端加入云端的分布式系统。
  - ![](https://yifengyou.github.io/learn-kvm/docs/%E8%99%9A%E6%8B%9F%E5%8C%96%E6%8A%80%E6%9C%AF%E7%AE%80%E4%BB%8B/image/1531988018906.png)
- 虚拟化的本质:
  - 虚拟化是对资源的逻辑抽象、隔离、再分配、管理的一个过程，通常对虚拟化的理解有广义狭义之分。广义包括平台虚拟化、应用程序虚拟化、存储虚拟化、网络虚拟化、设备虚拟化等等。狭义的虚拟化专门指计算机上模拟运行多个操作系统平台。
    - 服务虚拟化
    - 软件虚拟化
    - 框架虚拟化
    - 操作系统虚拟化
    - 硬件虚拟化
- 虚拟化的分类：
  - 支持层次的划分：
    - 软件虚拟化：将系统指令进行拦截和重新翻译
      - 纯软件翻译：QEMU
      - 动态二进制翻译：VMware
      - 注意：特权指令需要通过模拟中断进行实现。
      - 优点：成本低，部署方便。缺点：两次翻译，性能一般。
    - 硬件虚拟化：由硬件提供基础的指令拦截和翻译
      - Intel便开始推广应用Intel Virtualization Technology（Intel VT）虚拟化技术
      - 指令不需要经过两次转换，直接到达CPU，就好像虚拟机与客户机处于同一地位。
    - CPU虚拟化：软件抽象物理CPU以方便工作负载（进程线程）使用计算资源
    - 内存虚拟化：让CPU虚拟化出来的虚拟进程线程使用共享内存。
    - 网络虚拟化：本质就是，共享网络资源。虚拟机间的通信不需要流经网线，省带宽。
    - 存储虚拟化：NAS就是典例。
  - 从虚拟平台的角度划分：
    - 半虚拟化(Para Virtualization)：通过系统API的方式，将系统功能由母机提供给子机器。不需要额外的翻译，但是一般需要客户子机安装对应的模块支持。
    - 全虚拟化(Full Virtualization)(硬件辅助虚拟化)：虚拟机（VM）和硬件之间加了一个软件层–Hypervisor，或者叫做虚拟机管理程序（VMM）。VMM分为两类：
      - KVM：直接行在物理硬件之上，基于操作系统的Hypervisor
      - QEMU：在操作系统之上通过，硬件模拟进行相关操作。
  - 从虚拟化实现结构划分：
    - Hypervisor型虚拟化：本质就是硬件之上没有完整的OS，直接就是Hypervisor,典型的VMware vsphere。
    - 宿主模型虚拟化(host模型)：依赖一个操作系统实现关键驱动，如Xen。
    - 混合模型虚拟化：普通操作系统，但是有些硬件权限直接交给Hypervisor，缺点是特权操作系统提供服务的时候会出现上下文切换，开销比较大。如KVM。
- 虚拟化模型：
  - **虚拟化技术的核心是CPU虚拟化，只要处理器虚拟化技术支持“截获并重定向”，内存和IO设备的虚拟化都可以基于CPU虚拟化之上实现。**

