---
layout:     post
title:      图说设计模式 学习笔记 (三)
subtitle:   图说设计模式 学习笔记 (三) 行为型模式
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

# 行为型模式

## 1 命令模式

对象之间发送命令骑牛，不需要直到发送者是谁，只需要指定具体的接收请求者即可。

命令模式可以对发送者和接收者完全解耦，发送者与接收者之间没有直接引用关系，发送请求的对象只需要知道如何发送请求，而不必知道如何完成请求。这就是命令模式的模式动机。例如Qt的信号和槽函数，来实现命令模式

### 1.2 模式定义

命令模式（Command Pattern）是一种数据驱动的设计模式，它属于行为型模式。请求以命令的形式包裹在对象中，并传递给调用对象。调用对象寻找可以处理该命令的合适对象，并把该命令传给相应的对象，该对象执行命令。

### 1.3 模式结构

命令模式包含如下角色：

- Command: 抽象命令类；定义命令的接口，声明执行的方法。
- ConcreteCommand: 具体命令类：命令接口实现对象，通常会持有接收者，并调用接收者的功能来完成命令要执行的操作。
- Invoker: 调用者：要求命令对象执行请求，通常会持有命令对象，可以持有很多的命令对象。这个是客户端真正触发命令并要求命令执行相应操作的地方，也就是说相当于使用命令对象的入口。
- Receiver: 接收者，真正执行命令的对象。任何类都可能成为一个接收者，只要它能够实现命令要求实现的相应功能。
- Client:客户类：创建具体命令的对象，并设置其接收者;

![模式结构](https://design-patterns.readthedocs.io/zh_CN/latest/_images/Command.jpg)

![时序图](https://design-patterns.readthedocs.io/zh_CN/latest/_images/seq_Command.jpg)

### 1.4 代码实现和实例

**打车队列**
使用打车队列实现一个简单的命令模式队列

command原型类，主要功能是定义命令执行的接口
```c
/* command.h */
#ifndef COMMAND_H
#define COMMAND_H

class Reciever;

// 提供执行命令的接口
class Command
{
public:
    Command(Reciever *reciever);
    virtual void execute() = 0;  // 执行命令
protected:
    Reciever *m_pReciever;
};

// 打车
class TakeCommand : public Command
{
public:
    TakeCommand(Reciever *reciever);
    void execute();
};

// 付款
class PayCommand : public Command
{
public:
    PayCommand(Reciever *reciever);
    void execute();
};

#endif // COMMAND_H
```
```c
/* command.cpp */
#include "command.h"
#include "reciever.h"
#include <iostream>

Command::Command(Reciever *reciever)
    : m_pReciever(reciever)
{

}

// 打车
TakeCommand::TakeCommand(Reciever *reciever)
    : Command(reciever)
{

}

void TakeCommand::execute()
{
    std::cout << "Take command..." << std::endl;
    m_pReciever->takeOrder();
}

// 付款
PayCommand::PayCommand(Reciever *reciever)
    : Command(reciever)
{

}

void PayCommand::execute()
{
    std::cout << "Pay command..." << std::endl;
    m_pReciever->receipt();
}
```
有了 Command，就需要有对应的 Invoker 来持有命令，然后进行触发。
```c
/* invoker.h */
#ifndef INVOKER_H
#define INVOKER_H

#include <list>
#include "command.h"

// 命令的持有者
class Invoker
{
public:
    Invoker();
    void addCmd(Command *cmd);  // 添加命令
    void deleteCmd(Command *cmd);  // 删除命令
    void notify();  // 执行命令

private:
    std::list<Command *> m_cmds;  // 命令队列
};

#endif // INVOKER_H
```

```c
/* invoker.cpp */
#include "invoker.h"

Invoker::Invoker()
{

}

void Invoker::addCmd(Command *cmd)
{
    m_cmds.push_back(cmd);
}

void Invoker::deleteCmd(Command *cmd)
{
    m_cmds.remove(cmd);
}

void Invoker::notify()
{
    std::list<Command *>::iterator it = m_cmds.begin();
    while (it != m_cmds.end()) {
        (*it)->execute();
        ++it;
    }
}
```
最后是 Reciever，也就是司机，用于执行命令（接单/收款）。

```c
/* reciever.h */
#ifndef RECIEVER_H
#define RECIEVER_H

// 司机
class Reciever
{
public:
    void takeOrder();  // 接单
    void receipt();  // 收款
};

#endif // RECIEVER_H

/* reciever.cpp */
#include "reciever.h"
#include <iostream>

// 接单
void Reciever::takeOrder()
{
    std::cout << "Take order..." << std::endl;
}

// 收款
void Reciever::receipt()
{
    std::cout << "Receipt..." << std::endl;
}

```
主要创建和接收方法：

```c
#include "invoker.h"
#include "reciever.h"
#include "command.h"

int main()
{
    Reciever *rev = new Reciever();
    Command *cmd1 = new TakeCommand(rev);
    PayCommand *cmd2 = new PayCommand(rev);
    Invoker inv;

    inv.addCmd(cmd1);
    inv.addCmd(cmd2);
    //发射信号执行函数
    inv.notify();

    delete cmd1;
    delete cmd2;
    delete rev;

    return 0;
}
/*
//输出内容如下;

Take command… 
Take order… 
Pay command… 
Receipt…

*/

```


### 1.5 优缺点

优点：

- 降低系统的耦合度。
- 新的命令可以很容易地加入到系统中。
- 可以比较容易地设计一个命令队列和宏命令（组合命令）。
- 可以方便地实现对请求的Undo和Redo。

缺点：

- 使用命令模式可能会导致某些系统有过多的具体命令类。因为针对每一个命令都需要设计一个具体命令类，因此某些系统可能需要大量具体命令类，这将影响命令模式的使用。(qt通过函数指针和宏定义来解决了这个问题)
- 

### 1.6 适用环境

- 系统需要将请求调用者和请求接收者解耦，使得调用者和接收者不直接交互。
- 系统需要在不同的时间指定请求、将请求排队和执行请求。
- 系统需要支持命令的撤销(Undo)操作和恢复(Redo)操作。
- 系统需要将一组操作组合在一起，即支持宏命令。

注意命令行的本质还是对于对象之间消息的接收和发送。

## 2. 中介者模式

在多对象的相互行为合作中，它们之间的联系和行为存在相似性，但是每个对象之间又存在独立性。系统的扩展度较低，往往需要单独为两个对象之间编写对应的接口函数；增大了系统的耦合性。

使用中介者模式来连接不同对象之间的相似，行为对系统进行松耦合。

**中介者模式（Mediator Pattern）**用一个中介对象来封装一系列的对象交互。中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。

### 2.1 模式结构
中介者模式包含如下角色：

- Mediator: 抽象中介者:为 Colleague 对象之间的通信定义接口。
- ConcreteMediator: 具体中介者:实现 Mediator 接口，并需要了解和维护各个 Colleague 对象，负责协调他们之间的通信。
- Colleague: 抽象同事类:定义与其他 Colleague 通信的接口。
- ConcreteColleague: 具体同事类:实现 Colleague 接口，并通过 Mediator 与其他 Colleague 进行沟通。

![中介者模式结构图](https://design-patterns.readthedocs.io/zh_CN/latest/_images/Mediator.jpg)

**时序图**

![时序图](https://design-patterns.readthedocs.io/zh_CN/latest/_images/seq_Mediator.jpg)

### 2.2 案例

> 房东-中介-客户

使用中介完善房东与客户之间的相似行为。

### 2.3 代码分析

**创建抽象中介者**

中介者除了要注册参与者之外，还需要将发送者的消息传递出去。

```c
// mediator.h
#pragma once

#include "colleague.h"
#include <list>

class IColleague;

// 抽象中介者
class IMediator
{
public:
    // 注册参与者
    virtual void registerColleague(IColleague* colleague) { m_colleagues.emplace_back(colleague); }
    const std::list<IColleague*>& getColleagues() const { return m_colleagues; }

    // 将发送者的消息发送给所有参与者
    virtual void distributeMessage(const IColleague* sender, const std::string& message) const = 0;

private:
    std::list<IColleague*> m_colleagues;//两边的类列表
};
```
**创建具体中介者**
具体中介者的职责是遍历所有的参与者，将发送者的消息通知到每一个人：
```c
// concrete_mediator.h
#ifndef CONCRETE_MEDIATOR_H
#define CONCRETE_MEDIATOR_H

#include "mediator.h"

// 具体中介者
class ConcreteMediator : public IMediator
{
public:
    // 将发送者的消息发送给所有参与者（但不包括发送者自己）
    virtual void distributeMessage(const IColleague* sender, const std::string& message) const override {
        for (const IColleague* c : getColleagues())
            if (c != sender)  // 不要将消息发送给自己
                c->receiveMessage(sender, message);
    }
};

#endif // CONCRETE_MEDIATOR_H
```
**创建抽象同事**
由于房东和租客均由同事类表示，所以既需要（房东）发送消息，又需要（租客）接收消息：
```c
// colleague.h
#pragma once

#include "mediator.h"
#include <string>

class IMediator;

// 抽象同事类
class IColleague
{
public:
    IColleague(const std::string& name) : m_strName (name) {}
    std::string getName() const { return m_strName; }

    // 通过中介者，将自己的消息发布出去
    virtual void sendMessage(const IMediator& mediator, const std::string& message) const = 0;
    // 接收来自发送者的消息
    virtual void receiveMessage(const IColleague* sender, const std::string& message) const = 0;

private:
    std::string m_strName;
};

```
**创建具体同事**
在内部，具体的消息发送由中介者完成:

```c
// concrete_colleague.h
#ifndef CONCRETE_COLLEAGUE_H
#define CONCRETE_COLLEAGUE_H

#include "colleague.h"
#include <iostream>

// 具体同事类
class ConcreteColleague : public IColleague
{
public:
    using IColleague::IColleague;

    // 通过中介者，将自己的消息发布出去
    virtual void sendMessage(const IMediator& mediator, const std::string& message) const override {
        mediator.distributeMessage(this, message);
    }

private:
    // 接收来自发送者的消息
    virtual void receiveMessage(const IColleague* sender, const std::string& message) const override {
        std::cout << getName() << " received the message from "
                  << sender->getName() << ": " << message << std::endl;
    }
};

#endif // CONCRETE_COLLEAGUE_H

```

**创建客户端**

找房啦！一旦房东将房子挂出去，中介便会通知所有需要租房的人：

```c
// main.cpp
#include "concrete_colleague.h"
#include "concrete_mediator.h"

#ifndef SAFE_DELETE
#define SAFE_DELETE(p) { if(p){delete(p); (p)=NULL;} }
#endif

int main()
{
    // 房东
    IColleague *landlord = new ConcreteColleague("Tom");

    // 租客
    IColleague *jerry = new ConcreteColleague("Jerry");
    IColleague *tuffy = new ConcreteColleague("Tuffy");

    // 中介者 - 添加租客
    ConcreteMediator mediator;
    mediator.registerColleague(jerry);
    mediator.registerColleague(tuffy);

    // 房东通过中介将消息发送出去
    landlord->sendMessage(mediator, "Xi'erqi, two bedroom house, 6000/month.");

    SAFE_DELETE(jerry);
    SAFE_DELETE(tuffy);

    getchar();

    return 0;
}
/*
输出如下：

Jerry received the message from Tom: Xi’erqi, two bedroom house, 6000/month. 
Tuffy received the message from Tom: Xi’erqi, two bedroom house, 6000/month.
*/
```

### 2.8 优缺点

优点：

- 简化了对象之间的交互。
- 将各同事解耦。
- 减少子类生成。
- 可以简化各同事类的设计和实现。

缺点：

- 在具体中介者类中包含了同事之间的交互细节，可能会导致具体中介者类非常复杂，使得系统难以维护。

该模式主要应用于MVC交媾中的控制器部分。Controller 作为一种中介者，它负责控制视图对象View和模型对象Model之间的交互。如在Struts中，Action就可以作为JSP页面与业务对象之间的中介者。

中介者模式也应用于GUI中，在比较复杂的界面中可能存在多个界面组件之间的互交关系。

### 2.9 模式扩展

迪米特法则：中介模式中，创建一个中介者对象，将系统中有关的对象所引用的其它对象数目减少到最少，使得一个对象与其同事之间的相互作用被取代。


### 2.10 总结
- 使用中介者将需要发送的消息通过中介者进行发送。和命令模式不同的是，**命令模式着重，信号前后的行为，不管具体对象；重点在于command**。中介者模式主要是通过中介将消息的接收者进行同一管理，重点在与接收者。
- 中介者模式，使得各个对象不需要显式地相互调用
- 使用中介者模式可以有效的隐藏中间消息传递筛选细节。

## 3 观察者模式

上述模式中，需要使用明显的调用函数，来进行两个对象之间的通信，但是能否建立一种关系，使得一个对象发生改变时，自动通知其它对象，做出反映。这种模式就是观察者模式

### 3.2 模式定义

**观察者模式(Observer Pattern)**：定义对象间的一种一对多依赖关系，使得每当一个对象状态发生改变时，其相关依赖对象皆得到通知并被自动更新。观察者模式又叫做发布-订阅（Publish/Subscribe）模式、模型-视图（Model/View）模式、源-监听器（Source/Listener）模式或从属者（Dependents）模式。

观察者模式是一种对象行为型模式。

### 3.3 模式结构
观察者模式包含如下角色：

- Subject: 目标主题：跟踪所有观察者，并提供添加和删除观察者的接口。
- ConcreteSubject: 具体目标；将有关状态存入各 ConcreteObserver 对象。当具体主题的状态发生任何更改时，通知所有观察者。
- Observer: 观察者：为所有的具体观察者定义一个接口，在得到主题的通知时进行自我更新。
- ConcreteObserver: 具体观察者；实现 Observer 所要求的更新接口，以便使本身的状态与主题的状态相协调。

类图如下：

![观察者类图](https://design-patterns.readthedocs.io/zh_CN/latest/_images/Obeserver.jpg)

时序图：

![时序图](https://design-patterns.readthedocs.io/zh_CN/latest/_images/seq_Obeserver.jpg)


### 3.4 实际案例

滴滴平台中，司机相当于观察者，滴滴相当于主题。

### 3.5 代码实现

**创建抽象主题**
提供注册、注销、通知观察者的接口

```c
// subject.h
#ifndef SUBJECT_H
#define SUBJECT_H

class IObserver;

// 抽象主题
class ISubject
{
public:
    virtual void Attach(IObserver *) = 0;  // 注册观察者
    virtual void Detach(IObserver *) = 0;  // 注销观察者
    virtual void Notify() = 0;  // 通知观察者
};

#endif // SUBJECT_H
```
**创建具体主题**
抽象主题的具体实现，用于管理所有的观察者：
```c
// concrete_subject.h
#ifndef CONCRETE_SUBJECT_H
#define CONCRETE_SUBJECT_H

#include "subject.h"
#include "observer.h"
#include <iostream>
#include <list>

using namespace std;

// 具体主题
class ConcreteSubject : public ISubject
{
public:
    ConcreteSubject() { m_fPrice = 10.0; }

    void SetPrice(float price) {
        m_fPrice = price;
    }

    void Attach(IObserver *observer) {
        m_observers.push_back(observer);
    }

    void Detach(IObserver *observer) {
        m_observers.remove(observer);
    }

    void Notify() {
        list<IObserver *>::iterator it = m_observers.begin();
        while (it != m_observers.end()) {
            (*it)->Update(m_fPrice);
            ++it;
        }
    }

private:
    list<IObserver *> m_observers;  // 观察者列表
    float m_fPrice;  // 价格
};

#endif // CONCRETE_SUBJECT_H
```
**创建抽象观察者**

提供一个 Update() 接口，用于更新价格：
```c
// observer.h
#ifndef OBSERVER_H
#define OBSERVER_H

// 抽象观察者
class IObserver
{
public:
    virtual void Update(float price) = 0;  // 更新价格
};

#endif // OBSERVER_H
```

**创建具体观察者**

抽象观察者的具体实现，当接收到通知后，调整对应的价格：
```c

// concrete_observer.h
#ifndef CONCRETE_OBSERVER_H
#define CONCRETE_OBSERVER_H

#include "observer.h"
#include <iostream>
#include <string>

using namespace std;

// 具体观察者
class ConcreteObserver : public IObserver
{
public:
    ConcreteObserver(string name) { m_strName = name; }

    void Update(float price) {
        cout << m_strName << " - price: " << price << "\n";
    }

private:
     string m_strName;  // 名字
};

#endif // CONCRETE_OBSERVER_H
```
**创建客户端**

创建主题以及对应的观察者，并添加观察者并更新价格：
```c
// main.cpp
#include "concrete_subject.h"
#include "concrete_observer.h"

#ifndef SAFE_DELETE
#define SAFE_DELETE(p) { if(p){delete(p); (p)=NULL;} }
#endif

int main()
{
    // 创建主题、观察者
    ConcreteSubject *pSubject = new ConcreteSubject();
    IObserver *pObserver1 = new ConcreteObserver("Jack Ma");
    IObserver *pObserver2 = new ConcreteObserver("Pony");

    // 注册观察者
    pSubject->Attach(pObserver1);
    pSubject->Attach(pObserver2);

    // 更改价格，并通知观察者
    pSubject->SetPrice(12.5);
    pSubject->Notify();

    // 注销观察者
    pSubject->Detach(pObserver2);
    // 再次更改状态，并通知观察者
    pSubject->SetPrice(15.0);
    pSubject->Notify();

    SAFE_DELETE(pObserver1);
    SAFE_DELETE(pObserver2);
    SAFE_DELETE(pSubject);

    getchar();

    return 0;
}
/*
输出如下：
Jack Ma - price: 12.5 
Pony - price: 12.5 
Jack Ma - price: 15

 */
```
它和中介者模式的重要区别在于，使用subject代替了中介者的connect；实现了**不同类的1对多**的消息监听和传递；重点在于**消息**。但是中介者模式是**同一类中多对多的**；重点在于对象。命令模式基本上是**一对一的且重点在命令**；**上述都是针对客户端的实现来说的**


### 3.8 优缺点

优点：

- 观察者和被观察者是抽象耦合的
- 建立一套触发机制
- 实现表示层与逻辑层的分离，并定义了稳定的消息更新传递机制
- 观察者模式支持广播通信。

缺点：

- 如果一个被观察者对象有很多的直接和间接的观察者，将所有的观察者都通知到会花费很多时间。
- 如果在观察者和观察目标之间有循环依赖的话，观察目标会触发它们之间进行循环调用，可能导致系统崩溃。
- 观察者模式没有相应的机制让观察者知道所观察的目标对象是怎么发生变化的，而仅仅只是知道观察目标发生了变化。

### 3.9 适用情况

- 一个抽象模型有两个方面，其中一个方面依赖于另一个方面。将这些方面封装在独立的对象中使它们可以各自独立地改变和复用。
- 一个对象的改变将导致其他一个或多个对象也发生改变，而不知道具体有多少对象将发生改变，可以降低对象之间的耦合度。
- 一个对象必须通知其他对象，而并不知道这些对象是谁。
- 需要在系统中创建一个触发链，A对象的行为将影响B对象，B对象的行为将影响C对象……，可以使用观察者模式创建一种链式触发机制。

### 3.10 模式应用

观察者模式在软件开发中应用非常广泛，如某电子商务网站可以在执行发送操作后给用户多个发送商品打折信息，某团队战斗游戏中某队友牺牲将给所有成员提示等等，凡是涉及到一对一或者一对多的对象交互场景都可以使用观察者模式。

### 3.11 模式扩展

MVC模式是一种架构模式，它包含三个角色：模型(Model)，视图(View)和控制器(Controller)。观察者模式可以用来实现MVC模式，观察者模式中的观察目标就是MVC模式中的模型(Model)，而观察者就是MVC中的视图(View)，控制器(Controller)充当两者之间的中介者(Mediator)。当模型层的数据发生改变时，视图层将自动改变其显示内容。


## 4 状态模式

类中存在状态成员变量，来描述对象中不同状态的变化；状态模式就是监听不同对象的状态变化的情况并，做出反映。从客户端来说，主要的状态变化和消息传递，都是直接封装在相似对象类中；不是观察者模式直接将消息类化，也不是中介者模式，使用中介者来管理链接。而是直接使用环境类，来进行状态的改变。当对象根据其内部状态改变其行为时，将使用状态设计模式。

### 4.2 模式定义

状态模式(State Pattern) ：允许一个对象在其内部状态改变时改变它的行为，对象看起来似乎修改了它的类。其别名为状态对象(Objects for States)，状态模式是一种对象行为型模式。

### 4.3 模式结构

- Context（上下文）：定义一个与 Client 交互的接口。它维护对 ConcreteState 对象的引用，可以用该对象来定义当前状态。
- State（抽象状态）：定义接口，来声明每个 ConcreteState 应该做什么。
- ConcreteState（具体状态）：为 State 中定义的方法提供实现。

![状态类图](https://design-patterns.readthedocs.io/zh_CN/latest/_images/State.jpg)

![状态模式时序图](https://design-patterns.readthedocs.io/zh_CN/latest/_images/seq_State.jpg)


### 4.4 案例分析

> 交通信号灯
> 交通信号灯的不同状态，是进行交通规则管制的关键。

可以看到，交通信号灯的状态流：红灯 -> 绿灯 -> 黄灯。不同的状态是程序的关键。

### 4.5 代码实现

#### 4.5.1 创建上下文
上下文由 TrafficLights 表示，该类有一个 IState 变量，在构造中被初始化为 RedLight（红灯）：
```c
// context.h
#ifndef CONTEXT_H
#define CONTEXT_H

#include "concrete_state.h"

// 交通信号灯
class TrafficLights
{
public:
    TrafficLights() { m_pState = new RedLight(this); }
    void SetState(IState* state) { m_pState = state; }
    void Request() { m_pState->Handle(); }

private:
    IState* m_pState;//state关键成员
};

#endif // CONTEXT_H
```
**注意： 在上下文提供的方法中，实际上使用的是 IState 的相应方法。**

#### 4.5.2 state
**创建抽象状态**

IState 有一个 Handle() 接口，用于改变状态：
```c
// state.h
#ifndef STATE_H
#define STATE_H

// 信号灯的状态
class IState
{
public:
    virtual void Handle() = 0;
};
#endif // STATE_H
```
**创建具体状态**

具体的状态有三个 - 红灯、绿灯、黄灯：
```c
// concrete_state.h
#ifndef CONCRETE_STATE_H
#define CONCRETE_STATE_H

#include "state.h"

class TrafficLights;

// 红灯
class RedLight : public IState
{
public:
    RedLight(TrafficLights* context);
    virtual void Handle() override;

private:
    TrafficLights* m_pContext;
};

// 绿灯
class GreenLight : public IState
{
public:
    GreenLight(TrafficLights* context);
    virtual void Handle() override;

private:
    TrafficLights* m_pContext;
};

// 黄灯
class YellowLight : public IState
{
public:
    YellowLight(TrafficLights* context);
    virtual void Handle() override;
private:
    TrafficLights* m_pContext;
};
#endif // CONCRETE_STATE_H
```
它们所提供的方法有对上下文对象的引用，并且能够改变它的状态：
```c
// concrete_state.cpp
#include "concrete_state.h"
#include "context.h"
#include <iostream>

// 红灯
RedLight::RedLight(TrafficLights* context) : m_pContext(context) {}

void RedLight::Handle()
{
    std::cout << "Red Light" << std::endl;
    m_pContext->SetState(new GreenLight(m_pContext));
    delete this;
}

// 绿灯
GreenLight::GreenLight(TrafficLights* context) : m_pContext(context) {}

void GreenLight::Handle()
{
    std::cout << "Green Light" << std::endl;
    m_pContext->SetState(new YellowLight(m_pContext));
    delete this;
}

// 黄灯
YellowLight::YellowLight(TrafficLights* context) : m_pContext(context) {}

void YellowLight::Handle()
{
    std::cout << "Yellow Light" << std::endl;
    m_pContext->SetState(new RedLight(m_pContext));
    delete this;
}
```
#### 4.5.3 创建客户端

```c
// main.cpp
#include "context.h"
#include <iostream>
#include <windows.h>

int main()
{
    //交通灯
    TrafficLights tl;

    enum TLState {Red, Green, Yellow};

    TLState state = Red;  // 初始状态为红灯
    int i = 0;  // 总次数
    int seconds;  // 秒数

    while (true) {
        // 表示一个完整的状态流（红灯->绿灯->黄灯）已经完成
        if (i % 3 == 0)
            std::cout << "**********" << "Session " << ((i+1)/3)+1 << "**********" << std::endl;

        // 根据当前状态来设置持续时间，红灯（6秒）、绿灯（4秒）、黄灯（2秒）
        if (state == Red) {
            seconds = 6;
            state = Green;
        } else if (state == Green) {
            seconds = 4;
            state = Yellow;
        } else if (state == Yellow) {
            seconds = 2;
            state = Red;
        }

        // 休眠
        Sleep(seconds * 1000);

        tl.Request();
        i++;
    }

    return 0;
}

/*
//输出如下：
*****Session 1***** 
Red Light 
Green Light 
Yellow Light 
*****Session 2***** 
Red Light 
Green Light 
Yellow Light 
*****Session 3***** 
Red Light 
Green Light 
Yellow Light 
*****Session n***** 
*/
```
### 4.8 优缺点

优点：

- 封装了转换规则。；枚举了可能的状态，避免了未定义行为
- 很容易添加状态来支持额外的行为。
- 在状态模式中，对象的行为是其状态中函数的结果，并且在运行时根据状态改变行为，这就消除了对 switch/case 或 if/else 条件逻辑的依赖。
- 可以提高内聚性，因为状态特定的行为被聚合到具体的类中，这些类被放在代码中的一个位置。

缺点：

- 使用状态模式，必然会增加系统中类和对象的个数。
- 由于状态模式的结构与实现较为复杂，一旦使用不当，将会导致程序结构和代码的混乱。
- 若要添加新的状态，则需要修改负责转换的源代码，否则无法转换到新增的状态，而且修改某个状态的行为也要修改源代码。

### 4.9 适用环境

- 对象的行为依赖于它的状态（属性）并且可以根据它的状态改变而改变它的相关行为。
- 代码中包含大量与对象状态有关的条件语句，这些条件语句的出现，会导致代码的可维护性和灵活性变差，不能方便地增加和删除状态，使客户类与类库之间的耦合增强。在这些条件语句中包含了对象的行为，而且这些条件对应于对象的各种状态。

### 4.10 模式应用

在游戏或者业务办理流程中，文件和人物存在多个状态；尚未办理；正在办理；正在批示；正在审核；已经完成等；这些状态非常重要，此时应该适用状态模式，以状态为中心。**总体来说，对象的状态就是程序的核心，程序需要建立单独的状态类**

## 5 策略模式

完成任务，往往根据不同的条件和要求，使用不同的策略，例如对于目标检测，使用深度相机和双目相机都能进行目标的检测和识别，但是针对不同的输入需要进行模块的设计来进行策略的修改。为了解决这些问题，可以定义一些独立的类来封装不同的算法，每一个类封装一个具体的算法，在这里，每一个封装算法的类我们都可以称之为策略(Strategy)，为了保证这些策略的一致性，一般会用一个抽象的策略类来做算法的定义，而具体每种算法则对应于一个具体策略类。

### 5.1 模式定义

**策略模式(Strategy Pattern)**：定义一系列算法，将每一个算法封装起来，并让它们可以相互替换。策略模式让算法独立于使用它的客户而变化，也称为政策模式(Policy)。

策略模式是一种对象行为型模式。

### 5.3 模式结构

- Context（环境角色）：持有一个对 Strategy 的引用，最终给客户端调用。
- Strategy（抽象策略）：定义了一个公共接口，让不同的算法以不同的方式来实现。通过这个接口，Context 可以调用不同的算法。
- ConcreteStrategy（具体策略）：实现 Strategy 定义的接口，提供具体算法的实现。
