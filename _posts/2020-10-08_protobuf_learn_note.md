---
layout:     post
title:      protobuf学习笔记
subtitle:   protobuf学习笔记
date:       2020-08-25
author:     王鹏程
header-img: img/post-bg-ios10.jpg
catalog: true
tags:
    - C++
    - 序列化
---

# protobuf 学习笔记

> 2020-10-08 19:57:58

## 0 参考链接
- [官方文档](https://developers.google.com/protocol-buffers/docs/reference/cpp-generated)
- [深入 ProtoBuf-编码](https://www.jianshu.com/p/73c9ed3a4877)
- [深入 ProtoBuf - 序列化源码解析](https://www.jianshu.com/p/62f0238beec8)
- [Protobuf 语法指南](https://colobu.com/2015/01/07/Protobuf-language-guide/)
- [protobuff源码](https://github.com/protocolbuffers/protobuf)
- [flatbuffers源码](https://github.com/google/flatbuffers)


## 基础语法
下面是一个简单的使用示例:

```
message SearchRequest {
  required string query = 1;
  optional int32 page_number = 2;
  optional int32 result_per_page = 3;
}
```