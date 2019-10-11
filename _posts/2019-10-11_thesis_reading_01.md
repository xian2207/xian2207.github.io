---
layout:     post
title:      一种基于GPU的高性能稀疏卷积神经网络优化
subtitle:   一种基于GPU的高性能稀疏卷积神经网络优化论文阅读 
date:       2019-10-11
author:     王鹏程
header-img: img/post-bg-ios10.jpg
catalog: true
tags:
    - 卷积加速
    - 论文阅读
    - 稀疏
    - 并行
    - 优化
    - 图形处理器
---

# 论文名称(Tile)

- 作者(authors):方 程
- 文献引用(Published in):[1] 方程,邢座程,陈顼颢,张洋.一种基于GPU的高性能稀疏卷积神经网络优化[J].计算机工程与科学,2018,40(12):2103-2111.

- 文章链接:[知网](https://kns.cnki.net/KCMS/detail/detail.aspx?dbcode=CJFQ&dbname=CJFDLAST2019&filename=JSJK201812002&v=MDAzNTZWNzNQTHo3QlpiRzRIOW5Oclk5RlpvUjhlWDFMdXhZUzdEaDFUM3FUcldNMUZyQ1VSTE9lWitkdUZDam4=)
- 期刊:[计算机工程与科学](http://navi.cnki.net/KNavi/JournalDetail?pcode=CJFD&pykm=JSJK)


## 写作目的(解决了什么问题)(What is the motivation for this work (both people problem and technical problem)?)

CNN压缩之后的稀疏性导致的GPU性能的下降，提出了新的GPU卷积算法来解决这个问题



## 提出/建议的解决方案(假设、想法、设计)? (What is the proposed solution (hypothesis, idea, design)?)

采用直接稀疏卷积算法，来加速GPU处理稀疏数据:将卷积运算转换为稀疏向量与稠密向量的内积运算，



## 该解决方案（作者和您的评价）如何评价？(What is the evaluation of the solution (both author’s and yours)?)


## 文章中的疑惑或者不了解？(What do you have questions about or don’t you understand?)

## 在采用这种方法之前，您需要了解什么？(What would you need to know before you would adopt this approach?)

- CNN基本操作
- 压缩CNN的方法：
  - 基于分解：
    - [Accelerating Very Deep Convolutional Networks for Classification and Detection](https://www.semanticscholar.org/paper/Accelerating-Very-Deep-Convolutional-Networks-for-Zhang-Zou/b89d7f7439cab841934a1ede06bf6b1f593c754f)
      - 论文阅读解析:[深度卷积神经网络的加速](https://blog.csdn.net/weixin_41977410/article/details/83106099)
    - [Speeding up convolutional neural network susingfine-tuned CP-decomposition](https://arxiv.org/abs/1412.6553)
  - 基于删减(剪枝)
    - [Fast Algorithms for Convolutional Neural Networks](https://arxiv.org/abs/1509.09308v2)
    - [Dynamic Network Surgery for Efficient DNNs](http://arxiv.org/abs/1608.04493)
      - [Dynamic Network Surgery for Efficient DNNs阅读笔记](https://blog.csdn.net/zhangjunhit/article/details/53218211)
      - code:[https://github.com/yiwenguo/Dynamic-Network-Surgery](https://github.com/yiwenguo/Dynamic-Network-Surgery)
- SCNN(Sparse Convolutional Neural Networks)
- CUDA
  - cuBLAS
  - cuSPARSE

## 论文的贡献（作者和您的意见）？(What are the paper’s contributions (author’s and your opinion)?)

### 作者文章中的贡献(paper’s contributions)

1. 针对卷积层关键优化路径上完成直接稀疏卷积 算 法［１９］在 ＧＰＵ 架 构 上 的 并 行化实现，打破GPU上采用传统稠密算法的局限性，给出了一种可行且高效的GPU加速SCNN方案。
2. 采用最大限度的线程映射，充分利用GPU的硬件计算资源，防止产生稀疏结构运算对GPU资源的浪费
3. 

### 个人认为他的贡献(your opinion about paper’s contributions)




## 这项研究（作者和您的研究）的未来方向是什么？(What are future directions for this research (author’s and yours)?)



### 这项研究作者的未来研究方向(What are future directions for this research author’s)



### 这项研究你的未来研究方向(What are future directions for this research yours)