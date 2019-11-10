---
layout:     post
title:      图说设计模式 学习笔记 (二)
subtitle:   图说设计模式 学习笔记 (二) 结构型模式
date:       2019-11-10
author:     王鹏程
header-img: img/post-bg-ios10.jpg
catalog: true
tags:
    - C/C++
    - 设计模式
    - UML
---

> 2019-11-10 21:41:49

# 图说设计模式阅读笔记

_参考链接：_ 

- [github地址](https://github.com/me115/design_patterns)
- [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)
- [设计模式C++实现笔记](https://www.jianshu.com/c/c3f6140b8315)
- [C++ 设计模式](https://blog.csdn.net/liang19890820/article/details/66974516)
- [C++设计模式代码地址](https://github.com/Waleon/DesignPatterns)
- [设计模式(可复用面向对象的软件的基础)](https://pan.baidu.com/disk/home#/all?path=%2FLearning_Note%2F%E8%AE%A1%E7%AE%97%E6%9C%BA%E5%9B%BE%E4%B9%A6%2F%E8%BD%AF%E4%BB%B6%E5%B7%A5%E7%A8%8B%E5%92%8C%E9%A1%B9%E7%9B%AE%E7%AE%A1%E7%90%86&vmode=list)
- [C++设计模式视频](https://www.bilibili.com/video/av52251106)

# 结构型模式

结构型模式(Structural Pattern)描述如何将类或者对 象结合在一起形成更大的结构，就像搭积木，可以通过 简单积木的组合形成复杂的、功能更为强大的结构。

结构型模式可以分为类结构型模式和对象结构型模式：

- 类结构型模式关心类的组合，由多个类可以组合成一个更大的系统，在类结构型模式中一般只存在继承关系和实现关系。 
- 对象结构型模式关心类与对象的组合，通过关联关系使得在一 个类中定义另一个类的实例对象，然后通过该对象调用其方法。 
  
根据“合成复用原则”，在系统中尽量使用关联关系来替代继 承关系，因此大部分结构型模式都是对象结构型模式。

结构型模式的重点在于外观模式和适配器模式，享元模式不怎么重要。

## 1. 适配器模式

- 客户端可以通过目标类的结构访问它所提供的服务。但是接口不一定是客户端所期望的。
- 适配器模式，提供这样的接口来，实现用户期望的结果。
- 适配器模式定义一个包装类，包装不兼容接口的对象，给适配对象(客户端)；提供对应的接口。

### 1.2 模式定义

适配器模式(Adapter Pattern) ：将一个接口转换成客户希望的另一个接口，适配器模式使接口不兼容的那些类可以一起工作，其别名为包装器(Wrapper)。适配器模式既可以作为类结构型模式，也可以作为对象结构型模式。

### 1.3 模式结构

适配器模式包含如下角色：

- Target：目标抽象类
- Adapter：适配器类
- Adaptee：适配者类
- Client：客户类

适配器模式有对象适配器和类适配器两种实现：

对象适配器：

![对象适配器](https://design-patterns.readthedocs.io/zh_CN/latest/_images/Adapter.jpg)

类适配器

![类适配器模式](https://design-patterns.readthedocs.io/zh_CN/latest/_images/Adapter_classModel.jpg)

![时序图](https://design-patterns.readthedocs.io/zh_CN/latest/_images/seq_Adapter.jpg)

### 1.5 代码分析

```c
/* main function */
#include <iostream>
#include "Adapter.h"
#include "Adaptee.h"
#include "Target.h"

using namespace std;

int main(int argc, char *argv[])
{
    //创建适配器
	Adaptee * adaptee  = new Adaptee();
	//获取目标
    Target * tar = new Adapter(adaptee);
	//执行方法
    tar->request();
	
	return 0;
}
/* 定义适配器 */
///////////////////////////////////////////////////////////
//  Adapter.h
//  Implementation of the Class Adapter
//  Created on:      03-十月-2014 17:32:00
//  Original author: colin
///////////////////////////////////////////////////////////

#ifndef ADAPTER_H
#define ADAPTER_H

#include "Target.h"
#include "Adaptee.h"

class Adapter : public Target
{

public:
	Adapter(Adaptee *adaptee);
	virtual ~Adapter();

	virtual void request();

private:
	Adaptee* m_pAdaptee;

};
#endif // ADAPTER_H
/* 适配器实现 */
///////////////////////////////////////////////////////////
//  Adapter.cpp
//  Implementation of the Class Adapter
//  Created on:      03-十月-2014 17:32:00
//  Original author: colin
///////////////////////////////////////////////////////////

#include "Adapter.h"

Adapter::Adapter(Adaptee * adaptee){
	m_pAdaptee =  adaptee;
}
Adapter::~Adapter(){

}
void Adapter::request(){
	m_pAdaptee->specificRequest();
}
/* 适配者方法 */

///////////////////////////////////////////////////////////
//  Adaptee.h
//  Implementation of the Class Adaptee
//  Created on:      03-十月-2014 17:32:00
//  Original author: colin
//////////////////////////////////////////////////////////D

#ifndef ADAPTEE_H
#define ADAPTEE_H

class Adaptee
{

public:
	Adaptee();
	virtual ~Adaptee();

	void specificRequest();

};
#endif // !ADAPTEE_H

```
