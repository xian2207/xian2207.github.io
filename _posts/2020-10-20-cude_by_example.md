---
layout:     post
title:      CUDA高性能编程实战
subtitle:   CUDA高性能编程实战 学习笔记
date:       2019-4-17
author:     王鹏程
header-img: img/post-bg-ios10.jpg
catalog: true
tags:
    - C/C++
    - complier
    - CUDA
    - NVCC
---

# GPU 高性能编程CUDA实战
_参考链接:_
- [CUDA C Programming Guide](https://docs.nvidia.com/cuda/archive/9.2/cuda-c-programming-guide/index.html)
- [深入GPU硬件架构及运行机制](https://www.cnblogs.com/timlly/p/11471507.html)

> 2020-10-20 15:38:58

## 第三章 CUDA C简介

### 3.3 查询设备
cuda中可以使用cudaGetDeviceProperties进行设备查询；其函数对应接口如下:
```cpp

```

#### co-issue

co-issue是为了解决SIMD运算单元无法充分利用的问题。例如下图，由于float数量的不同，ALU利用率从100%依次下降为75%、50%、25%。

![](https://img2018.cnblogs.com/blog/1617944/201909/1617944-20190906001418746-831705203.png)

为了解决着色器在低维向量的利用率低的问题，可以通过合并1D与3D或2D与2D的指令。例如下图，DP3指令用了3D数据，ADD指令只有1D数据，co-issue会自动将它们合并，在同一个ALU只需一个指令周期即可执行完。

![](https://img2018.cnblogs.com/blog/1617944/201909/1617944-20190906001427705-915501113.png)

但是，对于向量运算单元（Vector ALU），如果其中一个变量既是操作数又是存储数的情况，无法启用co-issue技术：

![](https://img2018.cnblogs.com/blog/1617944/201909/1617944-20190906001436218-1727443284.png)

于是标量指令着色器（Scalar Instruction Shader）应运而生，它可以有效地组合任何向量，开启co-issue技术，充分发挥SIMD的优势。