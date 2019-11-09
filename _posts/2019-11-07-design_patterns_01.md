---
layout:     post
title:      图说设计模式 学习笔记 (一)
subtitle:   图说设计模式 学习笔记 (一) 创建型模式
date:       2019-10-24
author:     王鹏程
header-img: img/post-bg-ios10.jpg
catalog: true
tags:
    - C/C++
    - 设计模式
    - UML
---

> 2019-11-07 10:58:49

# 图说设计模式阅读笔记

_参考链接：_ 

- [github地址](https://github.com/me115/design_patterns)
- [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)
- [设计模式C++实现笔记](https://www.jianshu.com/c/c3f6140b8315)
- [C++ 设计模式](https://blog.csdn.net/liang19890820/article/details/66974516)
- [C++设计模式代码地址](https://github.com/Waleon/DesignPatterns)

# 看懂UML类图和时序图
 
## 图说设计模式阅读笔记-创建型模式

## 简单工厂模式( Simple Factory Pattern )
========================================


### 1.1 模式动机
--------------------
考虑一个简单的软件应用场景，一个软件系统可以提供多个外观不同的按钮（如圆形按钮、矩形按钮、菱形按钮等），
这些按钮都源自同一个基类，不过在继承基类后不同的子类修改了部分属性从而使得它们可以呈现不同的外观，如果我们希望在使用这些按钮时，不需要知道这些具体按钮类的名字，只需要知道表示该按钮类的一个参数，并提供一个调用方便的方法，把该参数传入方法即可返回一个相应的按钮对象，此时，就可以使用简单工厂模式。 

### 1.2 模式定义
--------------------
简单工厂模式(Simple Factory Pattern)：又称为静态工厂方法(Static Factory Method)模式，它属于类创建型模式。在简单工厂模式中，可以根据参数的不同返回不同类的实例。简单工厂模式专门定义一个类来负责创建其他类的实例，被创建的实例通常都具有共同的父类。


### 1.3 模式结构
--------------------
简单工厂模式包含如下角色：

- Factory：工厂角色
    工厂角色负责实现创建所有实例的内部逻辑
- Product：抽象产品角色
    抽象产品角色是所创建的所有对象的父类，负责描述所有实例所共有的公共接口
- ConcreteProduct：具体产品角色
    具体产品角色是创建目标，所有创建的对象都充当这个角色的某个具体类的实例。

![_static/SimpleFactory.jpg](https://design-patterns.readthedocs.io/zh_CN/latest/_images/SimpleFactory.jpg)

### 1.4 时序图
--------------------

![时序图](https://design-patterns.readthedocs.io/zh_CN/latest/_images/seq_SimpleFactory.jpg)

### 1.5 代码分析
--------------------

```c++
///////////////////////////////////////////////////////////
//  Factory.cpp
//  Implementation of the Class Factory
//  Created on:      01-十月-2014 18:41:33
//  Original author: colin
///////////////////////////////////////////////////////////

#include "Factory.h"
#include "ConcreteProductA.h"
#include "ConcreteProductB.h"
Product* Factory::createProduct(string proname){
	if ( "A" == proname )
	{
		return new ConcreteProductA();
	}
	else if("B" == proname)
	{
		return new ConcreteProductB();
	}
	return  NULL;
}
```


### 1.6 模式分析
--------------------

- 将对象的创建和对象本身业务处理分离可以降低系统的耦合度，使得两者修改起来都相对容易。
- 在调用工厂类的工厂方法时，由于工厂方法是静态方法，使用起来很方便，可通过类名直接调用，而且只需要传入一个简单的参数即可，在实际开发中，还可以在调用时将所传入的参数保存在XML等格式的配置文件中，修改参数时无须修改任何源代码。
- 简单工厂模式最大的问题在于工厂类的职责相对过重，增加新的产品需要修改工厂类的判断逻辑，这一点与开闭原则是相违背的。
- 简单工厂模式的要点在于：当你需要什么，只需要传入一个正确的参数，就可以获取你所需要的对象，而无须知道其创建细节。

### 实例
--------------------

```c++
/* Product.h */
/* 定义产品 */

class Product
{

public:
	Product();
	virtual ~Product();
	
	virtual void Use() = 0;

};

/* 定义不同的产品 */
#include "Product.h"
#include <string>
using namespace std;

class ConcreteProductA : public Product
{

public:
	ConcreteProductA();
	virtual ~ConcreteProductA();
	virtual void Use();

};
void ConcreteProductA::Use()
{
	cout << "use productB" << endl;
}

class ConcreteProductB : public Product
{
public:
	ConcreteProductB();
	virtual ~ConcreteProductB();
	
	virtual void Use();

};
void ConcreteProductB::Use()
{
	cout << "use productB" << endl;	
}
/* 定义工厂类 */
class Factory
{

public:
	Factory();
	virtual ~Factory();

	static Product * createProduct(string proname);

};
/* 工厂类的实现函数 */
Product* Factory::createProduct(string proname){
	if ( "A" == proname )
	{
		return new ConcreteProductA();
	}
	else if("B" == proname)
	{
		return new ConcreteProductB();
	}
	return  NULL;
}

/* 使用实例 */

int main(int argc, char *argv[])
{
	Product * prod = Factory::createProduct("A");
	prod->Use();
	delete prod;
	return 0;
}

```

简单工厂模式的优点
--------------------

- 工厂类含有必要的判断逻辑，可以决定在什么时候创建哪一个产品类的实例，客户端可以免除直接创建产品对象的责任，而仅仅“消费”产品；简单工厂模式通过这种做法实现了对责任的分割，它提供了专门的工厂类用于创建对象。
- 客户端无须知道所创建的具体产品类的类名，只需要知道具体产品类所对应的参数即可，对于一些复杂的类名，通过简单工厂模式可以减少使用者的记忆量。
- 通过引入配置文件，可以在不修改任何客户端代码的情况下更换和增加新的具体产品类，在一定程度上提高了系统的灵活性。

简单工厂模式的缺点
--------------------

- 由于工厂类集中了所有产品创建逻辑，一旦不能正常工作，整个系统都要受到影响。
- 使用简单工厂模式将会增加系统中类的个数，在一定程序上增加了系统的复杂度和理解难度。
- 系统扩展困难，一旦添加新产品就不得不修改工厂逻辑，在产品类型较多时，有可能造成工厂逻辑过于复杂，不利于系统的扩展和维护。
- 简单工厂模式由于使用了静态工厂方法，造成工厂角色无法形成基于继承的等级结构。

适用环境
--------------------
在以下情况下可以使用简单工厂模式：

- 工厂类负责创建的对象比较少：由于创建的对象较少，不会造成工厂方法中的业务逻辑太过复杂。
- 客户端只知道传入工厂类的参数，对于如何创建对象不关心：客户端既不需要关心创建细节，甚至连类名都不需要记住，只需要知道类型所对应的参数。

模式应用
--------------------
1. JDK类库中广泛使用了简单工厂模式，如工具类java.text.DateFormat，它用于格式化一个本地日期或者时间。

```java

    public final static DateFormat getDateInstance();
    public final static DateFormat getDateInstance(int style);
    public final static DateFormat getDateInstance(int style,Locale 
    locale);
```
2. Java加密技术

获取不同加密算法的密钥生成器::
```java
    KeyGenerator keyGen=KeyGenerator.getInstance("DESede");
```
创建密码器::
```java
    Cipher cp=Cipher.getInstance("DESede");
```
### 1.12 总结
--------------------

- 创建型模式对类的实例化过程进行了抽象，能够将对象的创建与对象的使用过程分离。
- 简单工厂模式又称为静态工厂方法模式，它属于类创建型模式。在简单工厂模式中，可以根据参数的不同返回不同类的实例。简单工厂模式专门定义一个类来负责创建其他类的实例，被创建的实例通常都具有共同的父类。
- 简单工厂模式包含三个角色：工厂角色负责实现创建所有实例的内部逻辑；抽象产品角色是所创建的所有对象的父类，负责描述所有实例所共有的公共接口；具体产品角色是创建目标，所有创建的对象都充当这个角色的某个具体类的实例。
- 简单工厂模式的要点在于：当你需要什么，只需要传入一个正确的参数，就可以获取你所需要的对象，而无须知道其创建细节。
- 简单工厂模式最大的优点在于实现对象的创建和对象的使用分离，将对象的创建交给专门的工厂类负责，但是其最大的缺点在于工厂类不够灵活，增加新的具体产品需要修改工厂类的判断逻辑代码，而且产品较多时，工厂方法代码将会非常复杂。
- 简单工厂模式适用情况包括：工厂类负责创建的对象比较少；客户端只知道传入工厂类的参数，对于如何创建对象不关心。


## 2 工厂方法模式(Factory Method Pattern)

### 2.1 模式动机

不仅仅只是调用对应的创建函数，更多的是定义具体的相关方法和属性(根据输入请求)；现在对该系统进行修改，不再设计一个按钮工厂类来统一负责所有产品的创建，而是将具体按钮的创建过程交给专门的工厂子类去完成，我们先定义一个抽象的按钮工厂类，再定义具体的工厂类来生成圆形按钮、矩形按钮、菱形按钮等，它们实现在抽象按钮工厂类中定义的方法。这种抽象化的结果使这种结构可以在不修改具体工厂类的情况下引进新的产品，如果出现新的按钮类型，只需要为这种新类型的按钮创建一个具体的工厂类就可以获得该新按钮的实例，这一特点无疑使得工厂方法模式具有超越简单工厂模式的优越性，更加符合“开闭原则”。

### 2.2 模式定义

工厂方法模式(Factory Method Pattern)又称为工厂模式，也叫虚拟构造器(Virtual Constructor)模式或者多态工厂(Polymorphic Factory)模式，它属于类创建型模式。在工厂方法模式中，工厂父类负责定义创建产品对象的公共接口，而工厂子类则负责生成具体的产品对象，这样做的目的是将产品类的实例化操作延迟到工厂子类中完成，即通过工厂子类来确定究竟应该实例化哪一个具体产品类。

### 2.3 模式结构和时序图

- Product：抽象产品
- ConcreteProduct：具体产品
- Factory：抽象工厂
- ConcreteFactory：具体工厂

模式结构：

![模式结构](https://design-patterns.readthedocs.io/zh_CN/latest/_images/FactoryMethod.jpg)

时序图：

![时序图](https://design-patterns.readthedocs.io/zh_CN/latest/_images/seq_FactoryMethod.jpg)

### 2.4 代码分析

```c++
///////////////////////////////////////////////////////////
//  ConcreteFactory.cpp
//  Implementation of the Class ConcreteFactory
//  Created on:      02-十月-2014 10:18:58
//  Original author: colin
///////////////////////////////////////////////////////////

#include "Factory.h"
#include "ConcreteFactory.h"
#include "Product.h"
#include <iostream>
using namespace std;

Product* ConcreteFactory::factoryMethod(){

	return  new ConcreteProduct();
}

int main(int argc, char *argv[])
{
    //创建对应的工厂
	Factory * fc = new ConcreteFactory();
	//创建对应产品
    Product * prod = fc->factoryMethod();
	prod->use();
	
	delete fc;
	delete prod;
	
	return 0;
}
```

### 2.6 模式分析

工厂方法模式是简单工厂模式的进一步抽象和推广，将产品细分到各个子场中去。使用不同的厂来创建对应的产品。在工厂方法模式中，核心的工厂类不再负责所有产品的创建，而是将具体创建工作交给子类去做。这个核心类仅仅负责给出具体工厂必须实现的接口，而不负责哪一个产品类被实例化这种细节，这使得工厂方法模式可以允许系统在不修改工厂角色的情况下引进新产品。

### 2.7 实例

日志记录器

某系统日志记录器要求支持多种日志记录方式，如文件记录、数据库记录等，且用户可以根据要求动态选择日志记录方式， 现使用工厂方法模式设计该系统。

结构图：

![结构图](https://design-patterns.readthedocs.io/zh_CN/latest/_images/loger.jpg)

时序图：

![时序图](https://design-patterns.readthedocs.io/zh_CN/latest/_images/seq_loger.jpg)

### 2.8 工厂方法模式的优先

- 工厂无需关心细节，甚至无序知道具体产品的类名称。
- 工厂角色和茶农角色的多态性设计是工厂方法模式的关键。它能确定创建的产品对象；创建细节完全封装在具体工厂内部。所有的具体工厂类都具有同一抽象父类。
- 添加新产品时，只需要添加一个具体的工厂和具体产品就可以了，系统的扩展性较好，完全符合"开闭原则"。

### 2.9 工厂模式的缺点

- 添加新产品时，需要编写新的具体产品类，而且需要提供与之赌赢的工厂类，系统中类的个数将成对增加，会带来额外的编译开销。
- 由于考虑到系统的可扩展性，需要引入抽象层，在客户端代码中均使用抽象层进行定义，增加了系统的抽象性和理解难度，且在实现时可能需要用到DOM、反射等技术，增加了系统的实现难度。

### 2.10 适用环境

在以下情况下可以使用工厂方法模式：

- 一个类不知道它所需要的对象的类：在工厂方法模式中，客户端不需要知道具体产品类的类名，只需要知道所对应的工厂即可，具体的产品对象由具体工厂类创建；客户端需要知道创建具体产品的工厂类。
- 一个类通过其子类来指定创建哪个对象：在工厂方法模式中，对于抽象工厂类只需要提供一个创建产品的接口，而由其子类来确定具体要创建的对象，利用面向对象的多态性和里氏代换原则，在程序运行时，子类对象将覆盖父类对象，从而使得系统更容易扩展。
- 将创建对象的任务委托给多个工厂子类中的某一个，客户端在使用时可以无须关心是哪一个工厂子类创建产品子类，需要时再动态指定，可将具体工厂类的类名存储在配置文件或数据库中。

### 2.12 扩展模式

- 使用多个工厂方法：在抽象工厂角色中可以定义多个工厂方法，从而使具体工厂角色实现这些不同的工厂方法，这些方法可以包含不同的业务逻辑，以满足对不同的产品对象的需求。
- 产品对象的重复使用：工厂对象将已经创建过的产品保存到一个集合（如数组、List等）中，然后根据客户对产品的请求，对集合进行查询。如果有满足要求的产品对象，就直接将该产品返回客户端；如果集合中没有这样的产品对象，那么就创建一个新的满足要求的产品对象，然后将这个对象在增加到集合中，再返回给客户端。
- 多态性的丧失和模式的退化：如果工厂仅仅返回一个具体产品对象，便违背了工厂方法的用意，发生退化，此时就不再是工厂方法模式了。一般来说，工厂对象应当有一个抽象的父类型，如果工厂等级结构中只有一个具体工厂类的话，抽象工厂就可以省略，也将发生了退化。当只有一个具体工厂，在具体工厂中可以创建所有的产品对象，并且工厂方法设计为静态方法时，工厂方法模式就退化成简单工厂模式。

### 2.13 总结

- 工厂方法模式又称为工厂模式，它属于类创建型模式。产品类的是咯花操作延迟到工厂子类中完成，即通过工厂子类来确定具体实例化的类。
- 工厂方法模式包含四个角色：抽象产品是定义产品的接口，是工厂方法模式所创建对象的超类型，即产品对象的共同父类或接口；具体产品实现了抽象产品接口，某种类型的具体产品由专门的具体工厂创建，它们之间往往一一对应；抽象工厂中声明了工厂方法，用于返回一个产品，它是工厂方法模式的核心，任何在模式中创建对象的工厂类都必须实现该接口；具体工厂是抽象工厂类的子类，实现了抽象工厂中定义的工厂方法，并可由客户调用，返回一个具体产品类的实例。
- 系统可以在不修改工厂角色的情况下，引进新产品。
- 适合情况：
    - 一个类不需要直到它所需要的对象的类;
    - 一个类通过其子类来指定创建哪个对象；
    - 客户端产品在使用时无序关心是哪个工厂子类创建子产品，需要时再动态指定。

## 