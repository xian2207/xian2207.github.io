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


## 1. 基础语法
下面是一个简单的使用示例:

```
message SearchRequest {
  required string query = 1;
  optional int32 page_number = 2;
  optional int32 result_per_page = 3; [default=10] //设置默认值
}
```

其中后面的数字编号表示；二进制格式中识别各个字段的，一旦开始使用就不能再改变，[1,15]之内的标识号在编码的时候会占用一个字节，之后的[16,2047]会占用两个字节

### 1.1 指定字段规则:
- required: 表示该字段必须
- optional: 可选项，表示该字段必须有0个或者1个值(不超过一个)
- repeated: 可重复值，相当于List
基本数值类型的repeated的字段并没有被尽可能地高效编码。在新的代码中，用户应该使用特殊选项[packed=true]来保证更高效的编码。如：

```
repeated int32 samples = 4 [packed=true];
```


### 1.2 基本数据类型

proto中定义的基本数据类型会对应不同语言的基本数据类型如下:

|.proto Type|Notes	|C++ Type|Java Type|Python Type|Go Type|
|:---:|:---|:---:|:---:|:---:|:---:|
|double|	|double|double|float|*float64|
|float|		|float|float|float|*float32|
|int32|Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint32 instead.|int32|int|int|*int32|
|int64|Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint64 instead.|int64|long|int/long|*int64|
|uint32|Uses variable-length encoding.|uint32|int|int/long|*uint32|
|uint64|Uses variable-length encoding.|uint64|long|int/long|	*uint64|
|sint32|Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s.|int32|int|int|*int32|
|sint64|Usesvariable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int64s.	|int64|long|int/long|*int64|
|fixed32|Always four bytes. More efficient than uint32 if values are often greater than 228.|uint32|int|int/long|*uint32|
|fixed64|Always eight bytes. More efficient than uint64 if values are often greater than 256.|uint64|long|int/long|*uint64|
|sfixed32|Always four bytes.|int32|int|int|*int32|
|sfixed64|Always eight bytes.|int64|long|int/long|*int64|
|bool||bool|boolean|bool|*bool|
|string|A string must always contain UTF-8 encoded or 7-bit ASCII text.|string|String|unicode (Python 2) or str (Python 3)|*string|
|bytes|May contain any arbitrary sequence of bytes.	|string|	ByteString|	bytes|[]byte|

