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

_参考链接：_ [C++ 适配器模式](https://blog.csdn.net/liang19890820/article/details/66973296)

- 客户端可以通过目标类的结构访问它所提供的服务。但是接口不一定是客户端所期望的。
- 适配器模式，提供这样的接口来，实现用户期望的结果。
- 适配器模式定义一个包装类，包装不兼容接口的对象，给适配对象(客户端)；提供对应的接口。

### 1.2 模式定义

适配器模式(Adapter Pattern) ：将一个接口转换成客户希望的另一个接口，适配器模式使接口不兼容的那些类可以一起工作，其别名为包装器(Wrapper)。适配器模式既可以作为类结构型模式，也可以作为对象结构型模式。

**适配器模式（Adapter Pattern）是一种补救模式，将一个类的接口转换成客户希望的另外一个接口，从而使原本由于接口不兼容而不能一起工作的类可以一起工作。**

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
/* 实现请求方法 */
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
/* 关键类适配器 */
class Adaptee
{

public:
	Adaptee();
	virtual ~Adaptee();

	void specificRequest();

};
#endif // !ADAPTEE_H

```
### 1.8 优点

- 将目标类和适配者类解耦，通过引入一个适配器类来重用现有的适配者类，而无须修改原有代码。
- 增加了类的透明性和复用性，将具体的实现封装在适配者类中，对于客户端类来说是透明的，而且提高了适配者的复用性。
- 灵活性和扩展性都非常好，通过使用配置文件，可以很方便地更换适配器，也可以在不修改原有代码的基础上增加新的适配器类，完全符合“开闭原则”。
  
**类适配器模式还具有如下优点：**
由于适配器类是适配者类的子类，因此可以在适配器类中置换一些适配者的方法，使得适配器的灵活性更强。
**对象适配器模式还具有如下优点：**
一个对象适配器可以把多个不同的适配者适配到同一个目标，也就是说，同一个适配器可以把适配者类和它的子类都适配到目标接口。

### 1.9 缺点

- 类适配器：
  - 对于Java、C#等不支持多重继承的语言，一次最多只能适配一个适配者类，而且目标抽象类只能为抽象类，不能为具体类，其使用有一定的局限性，不能将一个适配者类和它的子类都适配到目标接口。
- 对象适配器：
  - 与类适配器模式相比，要想置换适配者类的方法就不容易。如果一定要置换掉适配者类的一个或多个方法，就只好先做一个适配者类的子类，将适配者类的方法置换掉，然后再把适配者类的子类当做真正的适配者进行适配，实现过程较为复杂。

### 1.10 使用环境

- 系统需要使用现有的类，而这些类的接口不符合系统的需要。
- 想要建立一个可以重复使用的类，用于与一些彼此之间没有太大关联的一些类，包括一些可能在将来引进的类一起工作。

### 1.11 模式应用

java的JDBC给出一个客户端通用的抽象接口，每个具体数据库引擎（如SQL Server、Oracle、MySQL等）的JDBC驱动软件都是一个介于JDBC接口和数据库引擎接口之间的适配器软件。

### 1.12 模式扩展

当不需要全部实现接口提供的方法时，可先设计一个抽象类实现接口，并为该接口中每个方法提供一个默认实现（空方法），那么该抽象类的子类可有选择地覆盖父类的某些方法来实现需求，它适用于一个接口不想使用其所有的方法的情况。因此也称为单接口适配器模式。

## 2 桥接模式

桥接模式将继承关系转换为关联关系，从而降低了类与类之间的耦合，减少了代码编写量。适用于对于多种属性组合的情况。

### 2.2 模式定义

桥接模式(Bridge Pattern)：将抽象部分与它的实现部分分离，使它们都可以独立地变化。它是一种对象结构型模式，又称为柄体(Handle and Body)模式或接口(Interface)模式。

### 2.3 模式结构

桥接模式包含如下角色：

- Abstraction：抽象类
- RefinedAbstraction：扩充抽象类
- Implementor：实现类接口
- ConcreteImplementor：具体实现类

![桥接模式](https://design-patterns.readthedocs.io/zh_CN/latest/_images/Bridge.jpg)

时序图

![时序图](https://design-patterns.readthedocs.io/zh_CN/latest/_images/seq_Bridge.jpg)

```c
/* main.c */
#include <iostream>
#include "ConcreteImplementorA.h"
#include "ConcreteImplementorB.h"
#include "RefinedAbstraction.h"
#include "Abstraction.h"

using namespace std;

int main(int argc, char *argv[])
{
	
	Implementor * pImp = new ConcreteImplementorA();
	Abstraction * pa = new RefinedAbstraction(pImp);
	//注意这里是对公共抽象接口的调用
	pa->operation();
	
	Abstraction * pb = new RefinedAbstraction(new ConcreteImplementorB());
	pb->operation();
	
	delete pa;
	delete pb;
	
	return 0;
}
/* RefinedAbstraction.h */

///////////////////////////////////////////////////////////
//  RefinedAbstraction.h
//  Implementation of the Class RefinedAbstraction
//  Created on:      03-十月-2014 18:12:43
//  Original author: colin
///////////////////////////////////////////////////////////

#if !defined(EA_4BA5BE7C_DED5_4236_8362_F2988921CFA7__INCLUDED_)
#define EA_4BA5BE7C_DED5_4236_8362_F2988921CFA7__INCLUDED_

#include "Abstraction.h"

class RefinedAbstraction : public Abstraction
{

public:
	RefinedAbstraction();
	RefinedAbstraction(Implementor* imp);
	virtual ~RefinedAbstraction();

	virtual void operation();

};

RefinedAbstraction::RefinedAbstraction(){

}

RefinedAbstraction::RefinedAbstraction(Implementor* imp)
	:Abstraction(imp)
{
}

RefinedAbstraction::~RefinedAbstraction(){

}

void RefinedAbstraction::operation(){
	cout << "do something else ,and then " << endl;
	m_pImp->operationImp();
}

 
#endif // !defined(EA_4BA5BE7C_DED5_4236_8362_F2988921CFA7__INCLUDED_)

///////////////////////////////////////////////////////////
//  ConcreteImplementorA.cpp
//  Implementation of the Class ConcreteImplementorA
//  Created on:      03-十月-2014 18:12:43
//  Original author: colin
///////////////////////////////////////////////////////////

#include "ConcreteImplementorA.h"
#include <iostream>
using namespace std;

ConcreteImplementorA::ConcreteImplementorA(){

}
ConcreteImplementorA::~ConcreteImplementorA(){

}
//A中定义的操作函数，与B基本相同
void ConcreteImplementorA::operationImp(){
	cout << "imp in ConcreteImplementorA style." << endl;
}

```

### 2.6 模式分析

桥接模式就是在适配器模式的基础上，更改为了多种可桥接的方法。**主要目的是将抽象部分与它的实现部分分离，使得他们都可以独立进行变化。抽象类只控制抽象部分，具体的实现部分由继承的实现类本身来完成。通过公共虚函数接口的实现或者重载，来对其进行控制。**

- 抽象化：抽象化就是忽略一些信息，把不同的实体当作同样的实体对待。在面向对象中，将对象的共同性质抽取出来形成类的过程即为抽象化的过程。
- 抽象化：抽象化就是忽略一些信息，把不同的实体当作同样的实体对待。在面向对象中，将对象的共同性质抽取出来形成类的过程即为抽象化的过程。
- 实现化：针对抽象化给出的具体实现，就是实现化，抽象化与实现化是一对互逆的概念，实现化产生的对象比抽象化更具体，是对抽象化事物的进一步具体化的产物。
- 脱耦：脱耦就是将抽象化和实现化之间的耦合解脱开，或者说是将它们之间的强关联改换成弱关联，将两个角色之间的继承关系改为关联关系。桥接模式中的所谓脱耦，就是指在一个软件系统的抽象化和实现化之间使用关联关系（组合或者聚合关系）而不是继承关系，从而使两者可以相对独立地变化，这就是桥接模式的用意。

### 2.7 实例

参考示例：[链接](https://blog.csdn.net/liang19890820/article/details/79501177)

### 2.8 优点

- 分离抽象接口及其实现部分。
- 桥接模式有时类似于多继承方案，但是多继承方案违背了类的单一职责原则（即一个类只有一个变化的原因），复用性比较差，而且多继承结构中类的个数非常庞大，桥接模式是比多继承方案更好的解决方法。
- 桥接模式提高了系统的可扩充性，在两个变化维度中任意扩展一个维度，都不需要修改原有系统。
- 实现细节对客户透明，可以对用户隐藏实现细节。

### 2.9 缺点

- 桥接模式的引入会增加系统的理解与设计难度，由于聚合关联关系建立在抽象层，要求开发者针对抽象进行设计与编程。 
- 桥接模式要求正确识别出系统中两个独立变化的维度，因此其使用范围具有一定的局限性。

### 2.10 适用环境

- 在两个具体的角色之间，建立静态的继承联系，通过桥接模式，将两个函数进行抽象化并建立相关的联系。
- 抽象化的角色之和实现化的角色可以以继承的方式独立扩展而互不影响。
- 一个类存在两个独立变化的维度，且这两个唯独都需要进行扩展。
- 使用继承来实现，抽象化角色和具体角色的独立
- 不希望使用继承。

## 3 装饰模式

对于类或者对象的行为增加，一般有两种方式：

- 继承机制:使用继承机制，直接进行函数的添加。
- 关联机制，即将一个类的对象嵌入另外一个对象中，由另外一个对象来决定是否调用嵌入对象的行为，以便扩展自己的行为。，我们称这个嵌入的对象为“装饰器”。

## 3.2 模式定义：

装饰模式(Decorator Pattern) ：动态地给一个对象增加一些额外的职责(Responsibility)，就增加对象功能来说，装饰模式比生成子类实现更为灵活。其别名也可以称为包装器(Wrapper)，与适配器模式的别名相同，但它们适用于不同的场合。根据翻译的不同，装饰模式也有人称之为“油漆工模式”，它是一种对象结构型模式。

### 3.3 模式结构

包含角色如下：

- Component: 抽象构件
- ConcreteComponent: 具体构件
- Decorator: 抽象装饰类
- ConcreteDecorator: 具体装饰类

![结构模式图](https://design-patterns.readthedocs.io/zh_CN/latest/_images/Decorator.jpg)

时序图:

![时序图](https://design-patterns.readthedocs.io/zh_CN/latest/_images/seq_Decorator.jpg)

**装饰模式中的装饰类一般拥有为抽象构建成员，通过具体构建类来进行添加操作，成为具体的装饰类，最终实现具体的类操作的扩展，装饰器一般也来自于基础的抽象构件**

### 3.7 实例

> 星巴克（Starbucks）

在星巴克购买咖啡时，可以要求在其中加入各种调味品（辅料）。调味品很多，有些不收费（例如：白砂糖、香草粉等），有些则需要额外收费（例如：奶油、摩卡、糖浆等），所以充分利用起来吧！倘若咖啡不带劲，我们想要添加奶油、摩卡和糖浆，这时，就可以利用装饰者模式思想来实现。

### 3.8 代码实现

```c
/* 所有饮料的基类实现，即抽象构建实现 */
// component.h
#ifndef COMPONENT_H
#define COMPONENT_H

#include <string>

using namespace std;

// 所有饮料的基类
class IBeverage
{
public:
    virtual string Name() = 0;  // 名称
    virtual double Cost() = 0;  // 价钱
};

#endif // COMPONENT_H

/* 由基类，创建具体构建 */
// concrete_component.h
#ifndef CONCRETE_COMPONENT_H
#define CONCRETE_COMPONENT_H

#include "component.h"

/********** 具体的饮料（咖啡）**********/

// 黑咖啡，属于混合咖啡
class HouseBlend : public IBeverage
{
public:
    string Name() {
        return "HouseBlend";
    }

    double Cost() {
        return 30.0;
    }
};

// 深度烘培咖啡豆
class DarkRoast : public IBeverage
{
public:
    string Name() {
        return "DarkRoast";
    }

    double Cost() {
        return 28.5;
    }
};

#endif // CONCRETE_COMPONENT_H

/* 以抽象构建为成员，创建装饰类 */
// decorator.h
#ifndef DECORATOR_H
#define DECORATOR_H

#include "component.h"

// 调味品
class CondimentDecorator : public IBeverage
{
public :
    CondimentDecorator(IBeverage *beverage) : m_pBeverage(beverage) {}

    string Name() {
        return m_pBeverage->Name();
    }

    double Cost() {
        return m_pBeverage->Cost();
    }

protected :
    IBeverage *m_pBeverage;
} ;

#endif // DECORATOR_H

/* 添加具体装饰 */
// concrete_decorator.h
#ifndef CONCRETE_DECORATOR_H
#define CONCRETE_DECORATOR_H

#include "decorator.h"

/********** 具体的饮料（调味品）**********/

// 奶油
class Cream : public CondimentDecorator
{
public:
    Cream(IBeverage *beverage) : CondimentDecorator(beverage) {}

    string Name() {
        return m_pBeverage->Name() + " Cream";
    }

    double Cost() {
        return m_pBeverage->Cost() + 3.5;
    }
};

// 摩卡
class Mocha : public CondimentDecorator
{
public:
    Mocha(IBeverage *beverage) : CondimentDecorator(beverage) {}

    string Name() {
        return m_pBeverage->Name() + " Mocha";
    }

    double Cost() {
        return m_pBeverage->Cost() + 2.0;
    }
};

// 糖浆
class Syrup : public CondimentDecorator
{
public:
    Syrup(IBeverage *beverage) : CondimentDecorator(beverage) {}

    string Name() {
        return m_pBeverage->Name() + " Syrup";
    }

    double Cost() {
        return m_pBeverage->Cost() + 3.0;
    }
};

#endif // CONCRETE_DECORATOR_H

/* 创建客户端并使用 */

// main.cpp
#include "concrete_component.h"
#include "concrete_decorator.h"
#include <iostream>

#ifndef SAFE_DELETE
#define SAFE_DELETE(p) { if(p){delete(p); (p)=NULL;} }
#endif

int main()
{
    /********** 黑咖啡 **********/
    IBeverage *pHouseBlend = new HouseBlend();
    cout << pHouseBlend->Name() << " : " << pHouseBlend->Cost() << endl;

    // 黑咖啡 + 奶油
    CondimentDecorator *pCream = new Cream(pHouseBlend);
    cout << pCream->Name() << " : " << pCream->Cost() << endl;

    // 黑咖啡 + 摩卡
    CondimentDecorator *pMocha = new Mocha(pHouseBlend);
    cout << pMocha->Name() << " : " << pMocha->Cost() << endl;

    // 黑咖啡 + 糖浆
    CondimentDecorator *pSyrup = new Syrup(pHouseBlend);
    cout << pSyrup->Name() << " : " << pSyrup->Cost() << endl;

    /********** 深度烘培咖啡豆 **********/
    IBeverage *pDarkRoast = new DarkRoast();
    cout << pDarkRoast->Name() << " : " << pDarkRoast->Cost() << endl;

    // 深度烘培咖啡豆 + 奶油
    CondimentDecorator *pCreamDR = new Cream(pDarkRoast);
    cout << pCreamDR->Name() << " : " << pCreamDR->Cost() << endl;

    // 深度烘培咖啡豆 + 奶油 + 摩卡
    CondimentDecorator *pCreamMocha = new Mocha(pCreamDR);
    cout << pCreamMocha->Name() << " : " << pCreamMocha->Cost() << endl;

    // 深度烘培咖啡豆 + 奶油 + 摩卡 + 糖浆
    CondimentDecorator *pCreamMochaSyrup = new Syrup(pCreamMocha);
    cout << pCreamMochaSyrup->Name() << " : " << pCreamMochaSyrup->Cost() << endl;

    SAFE_DELETE(pSyrup);
    SAFE_DELETE(pMocha);
    SAFE_DELETE(pCream);
    SAFE_DELETE(pHouseBlend);

    SAFE_DELETE(pCreamMochaSyrup);
    SAFE_DELETE(pCreamMocha);
    SAFE_DELETE(pCreamDR);
    SAFE_DELETE(pDarkRoast);

    getchar();

    return 0;
}
/*
输出如下：
HouseBlend : 30 
HouseBlend Cream : 33.5 
HouseBlend Mocha : 32 
HouseBlend Syrup : 33 
DarkRoast : 28.5 
DarkRoast Cream : 32 
DarkRoast Cream Mocha : 34 
DarkRoast Cream Mocha Syrup : 37

*/
```
如上所示，构建，通过共同具体的装饰器，添加种种不同的功能。装饰器通过，添加基类的成员变量，实现模拟继承。

### 3.8 优点

- 装饰模式提供比继承更多的灵活度
- 可以通过配置文件在运行时，选择不同的装饰器，从而实现不同的行为。
- 可以通过不同具体装饰器的排列组合，创造出不同行为的组合。为具体的基础构件，创建更加强大的对象。
- 具体构件类与具体装饰类可以独立变化，用户可以根据需要增加新的具体构件类和具体装饰类，在使用时再对其进行组合，原有代码无须改变，符合“开闭原则”。
  
### 3.9 缺点

- 使用装饰模式时，因为层层递进关系，会产生很多无用的小对象；和具体的装饰类。