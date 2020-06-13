---
layout:     post
title:      Java开发规范学习笔记
subtitle:   Java开发规范华山版学习笔记
date:       2020-06-12
author:     王鹏程
header-img: img/post-bg-ios10.jpg
catalog: true
tags:
    - Java
    - 编译管理
    - 读书笔记
---

# Java开发规范学习笔记

_参考链接:_
- [阿里云官网课程链接](https://edu.aliyun.com/course/417?spm=5176.10731460.0.0.280639f4xpKm4n)
- [Git 官方手册](https://github.com/chjw8016/alibaba-java-style-guide)
- [在线书籍下载地址](https://edu.aliyun.com/course/417/material/24016/download?spm=5176.10731542.0.0.4a8c52bb9jM9Ji)

## 一 编程规范

### 1.1 命名风格
1. 【强制】代码中的命名不能以下划线或者`$`符号开头或者结束
2. 代码中禁止使用拼音和英文混合方式
3. 类名强制使用驼峰风格；但是**DO/BO/DTO/VO/AO/PO/UID**等；([阿里巴巴Java开发手册中的DO、DTO、BO、AO、VO、POJO定义](https://www.cnblogs.com/EasonJim/p/7967999.html))
    - DO(Data Object):与数据库表结构一一对应，通过DAO层向上传输数据源对象。
    - DTO（ Data Transfer Object）：数据传输对象，Service或Manager向外传输的对象。
    - BO（ Business Object）：业务对象。 由Service层输出的封装业务逻辑的对象。
    - AO（ Application Object）：应用对象。 在Web层与Service层之间抽象的复用对象模型，极为贴近展示层，复用度不高。
    - VO（ View Object）：显示层对象，通常是Web向模板渲染引擎层传输的对象。
    - POJO（ Plain Ordinary Java Object）：在本手册中， POJO专指只有setter/getter/toString的简单类，包括DO/DTO/BO/VO等。
    - Query：数据查询对象，各层接收上层的查询请求。 注意超过2个参数的查询封装，禁止使用Map类来传输。
4. 方法名、参数名、成员变量、局部变量统一使用驼峰式命名方法
5. `static final`的常量命名全部大写，单词间用下划线隔开；力求语义表达完整清楚，不要嫌名字长；例如`MAX_COUNT`
6. 抽象类名使用Abstract或者Base开头；异常类命名使用Exception 结尾；测试以Test结尾
7. 数组中括号需要前置
8. POJO类中布尔类型变量都不要加is前缀。否则设置set/get函数的时候容易引起冲突。
9. 包名统一小写，自然语义之间使用`.`号进行隔开；类名统一首字母大写；可以使用复数形式
10. 避免在子类父类的成员变量之间或者不同的代码块的局部变量之间采用相同的命名，使可读性降低。
11. 在常量和变量的命名时，表示类型的名词放在词尾，以提升辨识度。
12. 如果模块、接口、类、方法使用了设计模式，在命名时需要体现；例如`OrderFactory`
13. 接口类中的方法和属性不要添加任何修饰符号。尽量不要在接口里面定义变量
14. 对于Service和DAO类，基于SOA的理念，暴露出来的服务一定是接口，内部的实现类使用Impl的后缀与接口区别
15. 枚举类名带上Enum后缀，枚举成员名需要全大写单词间使用下划线隔开
16. 各层命名约规
    - Service/DAO层方法命名规约 
        1. 获取单个对象的方法使用get做前缀
        2. 获取多个对象的方法使用list做前缀，复数形式如结尾，如：listObjects;
        3. 获取统计值的方法使用count做前缀
        4. 插入的方法用save/insert做前缀
        5. 删除的方法使用remove/delete做前缀
        6. 修改的方法使用update做前缀
    - 领域模型命名规约
        1. 数据对象：xxxDo,xxx为数据表名；如UserDO
        2. 数据传输对象:xxxDTO,xxx为业务领域相关的名称。如: userDTO
        3. 展示对象:xxxVO,xxx一般为网页名称
        4. POJO 时DO/DTO/BO/VO的统称，禁止命名使用

### 1.2 常量定义:
1. **不允许任何魔法值（即未经预先定义的常量）直接出现在代码中。**
2. **在long或者Long赋值时，数值后使用大写的L，不能是小写的l，小写容易跟数 字1混淆，造成误解。**
3. 常量复用的层次结构:常量的复用层次有5层:
    - 跨应用共享常量:放置在二方库中，通常在constant目录下
    - 应用内共享常量:放置在一方库中，通常时子模块中的constant目录下
    - 子工程内部共享常量：在当前子工程的constant目录下
    - 包内共享常量:在当前包下单独的constant目录下
    - 类共享常量:直接在类内部private static final 定义。
    - 如果变量值仅在一个固定范围内变化用enum类型来定义。 

### 1.3 代码格式
1. **如果是大括号内为空，则简洁地写成{}即可，大括号中间无需换行和空格；如果是非 空代码块则**： 
    1. 左大括号前不换行。  
    2. 左大括号后换行。
    3. 右大括号前换行。  
    4. 右大括号后还有 else 等代码则不换行；表示终止的右大括号后必须换行。
2. 左右括号和字符之间不出现空格。左大括号前需要空格；
3. 任何二目、三目运算符的左右两边都需要添加一个空格。
4. 强制使用4个空格缩进，禁止使用tab字符
5. 强制注释的双斜线与注释内容之间有且仅有一个空格
6. 在强制进行类型转换时，右括号与强制转换值之间不需要任何空格隔开。
8. 单行字符数限制不超过120个，超出需要换行；换行时遵循如下原则：
    - 第二行相对第一行缩进4个空格，从第三行开始，不再继续缩进
    - 运算符与下文一起换行
    - 方法调用的点符号与下文一起换行
    - 方法调用中的多个参数需要换行时，在逗号后进行
    - 在括号前不要环行。
9. 方法参数在定义和传入时，多个参数逗号后面必须加空格；例如：`method(args1, args2, args3)`
10. IDE中的编码方式设置为UTF-8;IDE中的文件的换行符使用Unix格式("\n")
11. 单个方法的总行数不超过80行；行数过多需要进行解耦
12. 不同逻辑、不同语义、不同业务的代码之间插入一个空行分隔开来以提升可读性。

### 1.4 OOP约规

1. **强制**;对于类的对象引用访问，直接使用类引用静态变量或者静态方法，增加编译器的解析
2. **强制**所有的复写方法必须使用@override关键字；可以使用idea的快捷方式进行代码的生成和检查
3. **强制**可变参数，一般使用在相同参数类型，相同含义时使用。避免使用Object；可变参数必须放置在参数列表的最后。
4. **强制**外部正在调用或者二方库依赖的接口，不允许修改方法签名，避免对接口调用方产生影响。接口过时必须加@Deprecated注解
5. 不能使用果实的类或者方法
6. Object的equals方法容易抛空指针异常，应该使用常量或者确定有值的对象来调用equals
    - 注意：这里的equals主要是两者的内容是否相等；而`==`更多的是比较两者的内存地址是否相同；Object类的equals方法只是简单的比较指针指向的内存地址是否相同。所以一般类的方法都会进行，地址判断和类型的双重判断进行比较。([Object 的 equals 方法容易抛出空指](https://blog.csdn.net/teavamc/article/details/90702512))
    - 应该使用"test".equals(object)而不是`object.equals("test")`。
7. **强制**所有包装类型对象值之间的比较，全部使用equals方法比较；
    - 对于Integer var=? 在-128~127范围内的赋值，会在IntegerCache.cache产生，会复用已有对象，这个区间内的Integer值可以直接使用==进行判断，但是这个区间之外的所有数据都会在堆上产生，并不会复用已有对象，**推荐使用equals方法进行判断**
8. **强制**浮点数之间的等值判断，基本数据类型不能使用==进行判断，包装数据类型不能使用equals进行判断。浮点数需要使用"尾数+阶码"的编码方式，进行判断。例如`if(Math.abs(1.0f-0.9f)<(1e-6f))`
    - 或者使用BigDecimal来定义值，再进行浮点数的运算；例如:`BigDecimal a=new BigDecimal("1.0"),b= new BigDecimal("0.9"); if(a.equals(b))`
9. **强制**定义数据对象DO类时，属性类型要与数据库字段类型相匹配。
10. **强制**为了防止精度损失，禁止使用构造方法BigDecimal(double)方式将double值转化为BigDecimal对象。推荐使用转String的方式进行构造。或者直接使用`BigDecimal.valueOf()`方法
11. 关于基本数据类型和包装类数据类型的使用标准如下:
    1. **强制** 所有的POJO类属性必须使用包装数据类型
    2. **强制** RPC方法的返回值和参数必须使用包装数据类型
    3. 所有的局部数据变量都使用基本数据类型
    - 原因:POJO类属性没有初始值时提醒使用者，在使用是必须自己显式地进行赋值，任何NPE问题，或者入库检查，都由使用者来保证。
    - 例如:数据库的拆线呢结果可能式null,因为自动拆箱，用基本数据类型接收有[NPE空值异常](https://www.zhihu.com/question/55650388/answer/145790742)风险。
12. **强制** 定义DO/DTO/VO等POJO类时，不要设定任何默认属性值。因为可能存在时间节点等问题的更新。导致值的差异化。
13. **强制** 序列化类新增属性时，请不要修改serialVersionUID字段，避免反序列失败；如果完全不兼容升级，避免反序列化混乱，那么请修改serialVersionUID值([理解serialVersionUID是什么？有什么用？如何生成？](https://www.cnblogs.com/xuxinstyle/p/11394358.html);[Intellij IDEA怎么自动生成serialVersionUID](https://jingyan.baidu.com/article/f79b7cb3d19cc49144023ea6.html))。
14. **强制** 构造方法中禁止加入任何业务逻辑，如果有初始化逻辑，请放在init方法中。
15. 禁止在POJO类中，同时存在对应属性xxx的isXxx()和getXxx()方法。
16. **强制** 禁止在POJO类中必须写toString方法。使用IDE工具中的source>generate toString时，如果继承了另外一个POJO类，注意在前面加一个super.toString();
17. 使用索引访问用String 的split方法得到的数组时，需要做最后一个分割符后有无内容的检查，否则会有抛出`IndexOutOfBoundException`的风险。
18. 类的多个构造方法，或者多个同名方法，这些方法应该按照顺序防止在一起；同时类内方法定义的顺序依次是:
    - 公有方法或者保护方法
    - 私有方法
    - getter/setter方法。
20. setter 方法中，参数名称与类变量名称一致，this.成员名 = 参数名。在getter和setter中，不要增加业务逻辑，增加排查问题的难度。
21. 循环体内部，字符串的连接方式，使用StringBuilder的append方法进行扩展。
22. final主要声明类，成员变量、方法、以及本地变量；下列情况使用final关键字:不被允许继承的类，不能修改的引用的域或者对象、不允许被覆写的放啊，如POJO的setter方法、不允许运行过程中重新赋值的局部变量
23. 使用Object的clone方法时，建议重载成深拷贝再进行。
24. 成员变量域方法访问控制从严:
    - 禁止外部创建，构造方法必须是private,并提供创建类
    - 工具类不允许有public和default方法
    - 注意protect、public、private的成员变量修饰的方法类。
    - static成员变量，一般都考虑是否为final

### 1.5 集合处理

1. **强制**关于hashCode和equal的处理，遵循如下规则:
    1. 只要覆写equals,就必须覆写hashCode;
    2. 因为Set存储的是不重复的对象，根据hashCode和equals进行判断，所以存储Set的对象必须覆写这两个方法。
    3. 如果自定义对象作为Map的键，那么必须覆写hashCode和equals
2. 强制ArrayList的subList不能转换为ArrayList,**否则会抛出ClassCastException异常**；因为其返回的是ArrayList的内部类SubList,是ArrayList的一个视图，对于SubList子列表的所有操作最终会反映到原列表上。
3. **使用Map的方法keySet()/values()/entrySet()**返回集合对象时，不可以对其进行添加元素操作，否则会抛出`UnsupportedOperationException`异常。
4. **Collections**类返回的对象，如：`emptyList()/singletoList()`等都是`immutablelist`,不可对其进行添加或者删除元素的操作。空链表不支持修改等操作。
5. 再subList场景中，因为其返回的是是父集合的一部分视图，因此原来集合的修改，均会使其发生变化。建议开辟一个新的集合去接收对象。
6. **使用集合转数组的方法，必须使用集合的toArray(T[] array)，传入的是类型完全一致，长度为0的数组，这样避免内存的浪费和重新分配**
7. **在使用Collection接口任何实现类的addAll()方法时，都要对输入的集合参数进行NPE判断**--在ArrayList.addAll方法的第一行代码即Object[] a = c.toArray();其中c为输入集合参数，如果为null，则直接抛出异常。
8. 使用工具类Arrays.asList()把数组转换成集合时，不能使用其修改集合相关的方法，它的add/remove/clear方法会抛出UnsupportedOperationException异常。
说明：asList的返回对象是一个Arrays内部类，并没有实现集合的修改方法。Arrays.asList体现的是适配器模式，只是转换接口，后台的数据仍是数组。
    ```java
    String[] str = new String[] { "you", "wu" };
    Listlist = Arrays.asList(str);
    ```
    第一种情况：list.add("yangguanbao");运行时异常。
    第二种情况：str[0]="gujin";那么list.get(0)也会随之修改。

9. 泛型通配符<?extendsT>来接收返回的数据，此写法的泛型集合不能使用add方法，而<?superT>不能使用get方法，作为接口调用赋值时易出错。
    - 因为<?extendsT>主要存储类和其子类；你不能往Plate<? extends T>中插入任何类型的对象,因为你不能保证列表实际指向的类型是什么。只能保证读取的是T及其子类；并且初始化后不能再添加，因此不能使用add方法。而`<?super T>`刚好和其相反相当于C++中的虚基类，不过这里统一指示成为Object。
    - 存储多时使用<?superT>;访问多时使用<?extendsT>
    - [<? extends T> 及<? super T> 重温](https://www.cnblogs.com/zero2max/p/11398537.html)
10. 在无泛型限制定义的集合赋值给泛型集合时，在使用元素集合时，需要进行instanceof判断，避免抛出`ClassCastException`异常
。不能直接进行赋值。
```java
List<String> generics = null;
List notGenerics = new ArrayList(10);
notGenerics.add(new Object());
notGenerics.add(new Integer(1));
generics = notGenerics;
// 此处抛出 ClassCastException 异常

String string = generics.get(0);
```
11. **不要在 foreach 循环里进行元素的 remove/add 操作。remove 元素请使用Iterator方式，如果并发操作，需要对 Iterator 对象加锁。**
```java
//正例

List<String> list= new ArrayList<>();list.add("1");
list.add("2");
Iterator<String> iterator = list.iterator();
while(iterator.hasNext()){
    Stringitem=iterator.next();
    if(删除元素的条件){
        iterator.remove();
    }
}
//反例

for (String item : list) {
    if ("1".equals(item)) {
        list.remove(item);
    }
}
// 主要还是和C++类似的迭代器失效的问题。
```

12. **在 JDK7 版本及以上，Comparator 实现类要满足如下三个条件，不然 Arrays.sort，Collections.sort 会抛 IllegalArgumentException 异常。**
    -  x，y 的比较结果和 y，x 的比较结果相反。
    -  x>y，y>z，则 x>z。
    - x=y，则 x，z 比较结果和 y，z 比较结果相同。
    ```java
    //反例：下例中没有处理相等的情况，交换两个对象判断结果并不互反，不符合第一个条件，在实际使用中可能会出现异常。
    new Comparator<Student>() {
        @Override
        public int compare(Student o1, Student o2) {
            return o1.getId() > o2.getId() ? 1 : -1;
        }
    };
    ```
13. 集合泛型定义时，在JDK7及以上，使用diamond语法或全省略。
14. 集合初始化时，指定集合初始值大小([java集合超详解](https://blog.csdn.net/feiyanaffection/article/details/81394745);[java集合框架（深入）](https://www.cnblogs.com/TestMa/p/10641367.html))。
15. 使用entrySet遍历Map类集合KV，而不是keySet方式进行遍历。
    - keySet其实是遍历了2次，一次是转为Iterator对象，另一次是从hashMap中取出key所对应的value。而entrySet只是遍历了一次就把key和value都放到了entry中，效率更高。如果是JDK8，使用Map.foreach方法。正例：values()返回的是V值集合，是一个list集合对象；keySet()返回的是K值集合，是一个Set集合对象；entrySet()返回的是K-V值组合集合。
    - [java中Map遍历的四种方式](https://www.cnblogs.com/damoblog/p/9124937.html)
16. 高度注意Map类集合K/V能不能存储null值的情况，如下表格:

|集合类|Key|Value|Super|说明|
|:--|:---:|:---:|:---:|:---:|
|`Hashtable`|不允许为null|不允许为null|Dictionary|线程安全|
|`ConcurrentHashMap`|不允许为null|不允许为null|`AbstractMap`|锁分段技术(JDK8:CAS)|
|`TreeMap`|不允许为null|允许为null|`AbstractMap`|线程不安全|
|`HashMap`|许为null|允许为null|`AbstractMap`|线程不安全|

17. 合理利用好集合的有序性(sort)和稳定性(order)，避免集合的无序性(unsort)和不稳定性(unorder)带来的负面影响。
    - 说明：有序性是指遍历的结果是按某种比较规则依次排列的。稳定性指集合每次遍历的元素次序是一定的。如：ArrayList是order/unsort；HashMap是unorder/unsort；TreeSet是order/sort。
18. 利用Set元素的唯一性，可以快速对一个集合进行去重操作，避免使用List的contains方法进行遍历、对比、去重操作。

## 1.6 并发处理

1. **获取单例对象需要保障线程安全，其中的方法也要保证线程安全**。
    - [Java-单例设计模式（懒汉与饿汉）](https://www.cnblogs.com/llstudy/p/9345686.html)
2. **创建线程或线程池时请指定有意义的线程名称，方便出错时回溯**
    ```java
        public class TimerTaskThread extends Thread {
            public TimerTaskThread(){
                super.setName("Timer TaskThread");
                ...
            }
        }
    ```
3. 线程资源必须通过线程池提供，不允许在应用中自行显式创建线程。避免创建线程的不必要开销。
    - [Java基础篇-线程与线程池](https://zhuanlan.zhihu.com/p/106803927?utm_source=wechat_session)
    - [Java中线程池ThreadPoolExecutor原理探究](http://ifeve.com/java%E4%B8%AD%E7%BA%BF%E7%A8%8B%E6%B1%A0threadpoolexecutor%E5%8E%9F%E7%90%86%E6%8E%A2%E7%A9%B6/)
    - [Java线程池详解及常用方法](https://baijiahao.baidu.com/s?id=1662039191028815781&wfr=spider&for=pc)
4. **线程池不允许使用Executors去创建，而是通过ThreadPoolExecutor的方式，这样的处理方式让写的同学更加明确线程池的运行规则，规避资源耗尽的风险**
    - 主要弊端如下:
        - FixedThreadPool和SingleThreadPool:允许的请求队列长度为Integer.MAX_VALUE，可能会堆积大量的请求，从而导致OOM。
        - CachedThreadPool和ScheduledThreadPool:允许的创建线程数量为Integer.MAX_VALUE，可能会创建大量的线程，从而导致OOM。
5. **SimpleDateFormat是线程不安全的类，一般不要定义为static变量，如果定义为static，必须加锁，或者使用DateUtils工具类。**

    ```java
    //正例：注意线程安全，使用DateUtils。亦推荐如下处理：
    private static final ThreadLocal<DateFormat> df = new ThreadLocal<DateFormat>() {
        @Override
        protected DateFormat initialValue() {
            return new SimpleDateFormat("yyyy-MM-dd");
        }
    };
    ```
    - 说明：如果是JDK8的应用，可以使用Instant代替Date，LocalDateTime代替Calendar，DateTimeFormatter代替SimpleDateFormat，官方给出的解释：simplebeautifulstrongimmutablethread-safe。
6. 必须回收自定义的ThreadLocal变量。尤其是在线程池场景下，线程经常会被复用，如果不清理自定义的ThreadLocal变量，可能会影响后续业务逻辑和造成内存泄漏等问题；尽量在代理中使用`try-finally`快进行回收。
7. **高并发时，同步调用应该去考量锁的性能损耗。能用无锁数据结构，就不要用锁；能锁区块，就不要锁整个方法体；能用对象锁，就不要用类锁。**
    - 可能使加锁的代码块工作量尽可能的小，避免在锁代码块中调用RPC方法。
    - [Java锁详解](https://blog.csdn.net/zcl_love_wx/article/details/93977947)
    - [java中的各种锁详细介绍](https://www.cnblogs.com/jyroy/p/11365935.html)
    - [Java高并发之无锁与Atomic源码分析](https://www.cnblogs.com/xdecode/p/9022525.html)
8. 多多个资源、数据库表、对象同时加锁时，需要保持一致的加锁顺序，否则会造成死锁。
9. 在使用阻塞等待获取锁的方式中，必须在 try 代码块之外，并且在加锁方法与 try 代码块之间没有任何可能抛出异常的方法调用，避免加锁成功后，在 finally 中无法解锁。 
    - 说明一：如果在 lock 方法与 try 代码块之间的方法调用抛出异常，那么无法解锁，造成其它线程无法成功获取锁。 
    - 说明二：如果 lock 方法在 try 代码块之内，可能由于其它方法抛出异常，导致在 finally 代码块中，unlock 对未加锁的对象解锁，它会调用 AQS 的 tryRelease 方法（取决于具体实现类），抛出IllegalMonitorStateException 异常。
    - 说明三：在 Lock 对象的 lock 方法实现中可能抛出 unchecked 异常，产生的后果与说明二相同。 
    ```java
    //正例： 
    Lock  lock  =  new  Xxx Lock(); 
    //  ...
    lock.lock();
    try{
        doSomething();
        doOthers();
    } finally {
        lock.unlock();
    }
    ```
10. **在使用尝试机制来获取锁的方式中，进入业务代码块之前，必须先判断当前线程是否持有锁。锁的释放规则与锁的阻塞等待方式相同。**
    - 说明：Lock 对象的 unlock 方法在执行时，它会调用 AQS 的 tryRelease 方法（取决于具体实现类），如果当前线程不持有锁，则抛出 IllegalMonitorStateException 异常。
    ```java
    Lock  lock  =  new  Xxx Lock(); 
    //  ...
    boolean  isLocked  =  lock.tryLock(); 
    if  (isLocked) {
        try{
            doSomething();
            doOthers();
        } finally {
            lock.unlock();
        }
    }
    ```
11. **并发修改同一记录时，避免更新丢失，需要加锁。要么在应用层加锁，要么在缓存加锁，要么在数据库层使用乐观锁，使用 version 作为更新依据。**
    - 说明：如果每次访问冲突概率小于 20%，推荐使用乐观锁，否则使用悲观锁。乐观锁的重试次数不得小于3 次。
12. **多线程并行处理定时任务时，Timer 运行多个 TimeTask 时，只要其中之一没有捕获抛出的异常，其它任务便会自动终止运行，如果在处理定时任务时使用**

13. 资金相关的金融敏感信息，使用悲观锁策略。
14. 使用 CountDownLatch 进行异步转同步操作，每个线程退出前必须调用 countDown方法，线程执行代码注意 catch 异常，确保 countDown 方法被执行到，避免主线程无法执行至 await 方法，直到超时才返回结果。
    - 注意，子线程抛出异常堆栈，不能在主线程try-catch到。 
15. 避免 Random 实例被多线程使用，虽然共享该实例是线程安全的，但会因竞争同一seed  导致的性能下降。
    - Random 实例包括 java.util.Random  的实例或者  Math.random()的方式。 
    - 在 JDK7 之后，可以直接使用 API ThreadLocalRandom，而在  JDK7 之前，需要编码保证每个线程持有一个实例。 
16. 在并发场景下，可以使用双重检查锁实现延迟初始化的优化问题隐患，推荐解决方案中较为简单的一种，将目标属性声明为`volatile`类型。
17. volatile解决多线程内存不可见问题，对于一写多读可以解决变量同步问题，但是如果时多写，同样无法解决线程安全问题；建议使用`AtomicInteger`等原子操作。如果是JDK8,推荐使用LongAdder。
18. HashMap 在容量不够进行 resize 时由于高并发可能出现死链，导致 CPU 飙升，在开发过程中可以使用其它数据结构或加锁来规避此风险。 
19. ThreadLocal 对象使用 static 修饰，ThreadLocal 无法解决共享对象的更新问题。 
    - 说明：这个变量是针对一个线程内所有操作共享的，所以设置为静态变量，所有此类实例共享此静态变量，也就是说在类第一次被使用时装载，只分配一块存储空间，所有此类的对象(只要是这个线程内定义的)都可以操控这个变量。 
    - [将ThreadLocal变量设置为private static的好处是啥？](https://www.zhihu.com/question/35250439)
    - [彻底理解ThreadLocal](https://www.cnblogs.com/xzwblog/p/7227509.html)

### 1.7 控制语句

