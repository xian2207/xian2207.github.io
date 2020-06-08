
---
layout:     post
title:      Maven 学习笔记
subtitle:   Maven 学习笔记
date:       2020-06-08
author:     王鹏程
header-img: img/post-bg-ios10.jpg
catalog: true
tags:
    - Java
    - 编译管理
    - 读书笔记
---

# Maven 学习笔记

_参考链接:_
- [Maven 菜鸟教程](https://www.runoob.com/maven/maven-tutorial.html)
- [Maven 官网](http://maven.apache.org/index.html)
- [Maven 官网文档](http://maven.apache.org/guides/)

## 0. java编译指令和工具参数
_参考链接：_
- [java命令大全](https://blog.csdn.net/yang889999888/article/details/72823261?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase)
- [java命令详解](https://blog.csdn.net/Trival_dreamy/article/details/86740281)
### 0.1 JDK中的常用工具
- java: java程序的基本工具，主要是创建虚拟机等过程，因此常用参数为虚拟机参数.
```bash
用法: java [-options] class [args...]
           (执行类)
   或  java [-options] -jar jarfile [args...]
           (执行 jar 文件)
其中选项包括:
    -d32          使用 32 位数据模型 (如果可用)
    -d64          使用 64 位数据模型 (如果可用)
    -server       选择 "server" VM
                  默认 VM 是 server.

    -cp <目录和 zip/jar 文件的类搜索路径>
    -classpath <目录和 zip/jar 文件的类搜索路径>
                  用 ; 分隔的目录, JAR 档案
                  和 ZIP 档案列表, 用于搜索类文件。
    -D<名称>=<值>
                  设置系统属性
    -verbose:[class|gc|jni]
                  启用详细输出
    -version      输出产品版本并退出
    -version:<值>
                  警告: 此功能已过时, 将在
                  未来发行版中删除。
                  需要指定的版本才能运行
    -showversion  输出产品版本并继续
    -jre-restrict-search | -no-jre-restrict-search
                  警告: 此功能已过时, 将在
                  未来发行版中删除。
                  在版本搜索中包括/排除用户专用 JRE
    -? -help      输出此帮助消息
    -X            输出非标准选项的帮助
    -ea[:<packagename>...|:<classname>]
    -enableassertions[:<packagename>...|:<classname>]
                  按指定的粒度启用断言
    -da[:<packagename>...|:<classname>]
    -disableassertions[:<packagename>...|:<classname>]
                  禁用具有指定粒度的断言
    -esa | -enablesystemassertions
                  启用系统断言
    -dsa | -disablesystemassertions
                  禁用系统断言
    -agentlib:<libname>[=<选项>]
                  加载本机代理库 <libname>, 例如 -agentlib:hprof
                  另请参阅 -agentlib:jdwp=help 和 -agentlib:hprof=help
    -agentpath:<pathname>[=<选项>]
                  按完整路径名加载本机代理库
    -javaagent:<jarpath>[=<选项>]
                  加载 Java 编程语言代理, 请参阅 java.lang.instrument
    -splash:<imagepath>
                  使用指定的图像显示启动屏幕
有关详细信息, 请参阅 http://www.oracle.com/technetwork/java/javase/documentation/index.html。
```

    - 参数解析：
        - `-classpath/cp`:指定jar和zip的搜索路径；其搜索路径依次为:
            - Bootstrap classes(虚拟机库路径)，
            - Extension classes: jre"lib"ext目录下的jar文件;虚拟机在搜索完Bootstrap后就搜索该目录下的jar文件。用System. getProperty("java.ext.dirs”)可得到虚拟机使用Extension搜索路径
            - User classes: 搜索顺序为当前目录、环境变量 CLASSPATH、-classpath。
        - `-D<propertyName>=value`:相当于CMake中的-D变量设置，不过可以使用System.getProperty(“propertyName”)获取其对应的值。

- javac: 编译的Java程序，生成.class文件。
```bash
用法: javac <options> <source files>
其中, 可能的选项包括:
  -g                         生成所有调试信息
  -g:none                    不生成任何调试信息
  -g:{lines,vars,source}     只生成某些调试信息
  -nowarn                    不生成任何警告
  -verbose                   输出有关编译器正在执行的操作的消息
  -deprecation               输出使用已过时的 API 的源位置
  -classpath <路径>            指定查找用户类文件和注释处理程序的位置
  -cp <路径>                   指定查找用户类文件和注释处理程序的位置
  -sourcepath <路径>           指定查找输入源文件的位置
  -bootclasspath <路径>        覆盖引导类文件的位置
  -extdirs <目录>              覆盖所安装扩展的位置
  -endorseddirs <目录>         覆盖签名的标准路径的位置
  -proc:{none,only}          控制是否执行注释处理和/或编译。
  -processor <class1>[,<class2>,<class3>...] 要运行的注释处理程序的名称; 绕过默认的搜索进程
  -processorpath <路径>        指定查找注释处理程序的位置
  -parameters                生成元数据以用于方法参数的反射
  -d <目录>                    指定放置生成的类文件的位置
  -s <目录>                    指定放置生成的源文件的位置
  -h <目录>                    指定放置生成的本机标头文件的位置
  -implicit:{none,class}     指定是否为隐式引用文件生成类文件
  -encoding <编码>             指定源文件使用的字符编码
  -source <发行版>              提供与指定发行版的源兼容性
  -target <发行版>              生成特定 VM 版本的类文件
  -profile <配置文件>            请确保使用的 API 在指定的配置文件中可用
  -version                   版本信息
  -help                      输出标准选项的提要
  -A关键字[=值]                  传递给注释处理程序的选项
  -X                         输出非标准选项的提要
  -J<标记>                     直接将 <标记> 传递给运行时系统
  -Werror                    出现警告时终止编译
  @<文件名>                     从文件读取选项和文件名
```
- javaw: 跟java命令相对的，可以运行.class文件,主要用来执行图形界面的java程序运行java命令时，会出现并保持一个console窗口,程 序中的信息可以通过System.out在console内输出，而运行javaw，开始时会出现console，当主程序调用之后，console就会 消失； javaw 大多用来运行GUI程序
- javah: C 头文件和 Stub 文件生成器。javah 从 Java 类生成 C 头文件和 C 源文件。这些文件提供了连接胶合，使 Java 和 C 代码可进行交互。
```bash
用法:
  javah [options] <classes>
其中, [options] 包括:
  -o <file>                输出文件 (只能使用 -d 或 -o 之一)
  -d <dir>                 输出目录
  -v  -verbose             启用详细输出
  -h  --help  -?           输出此消息
  -version                 输出版本信息
  -jni                     生成 JNI 样式的标头文件 (默认值)
  -force                   始终写入输出文件
  -classpath <path>        从中加载类的路径
  -cp <path>               从中加载类的路径
  -bootclasspath <path>    从中加载引导类的路径
<classes> 是使用其全限定名称指定的
(例如, java.lang.Object)。
```
- javap: 命令用于解析类文件。其输出取决于所用的选项。若没有使用选项，javap 将输出传递给它的类的 public 域及方法。javap 将其输出到标准输出设备上。
```sh
用法: javap <options> <classes>
其中, 可能的选项包括:
  -help  --help  -?        输出此用法消息
  -version                 版本信息
  -v  -verbose             输出附加信息
  -l                       输出行号和本地变量表
  -public                  仅显示公共类和成员
  -protected               显示受保护的/公共类和成员
  -package                 显示程序包/受保护的/公共类
                           和成员 (默认)
  -p  -private             显示所有类和成员
  -c                       对代码进行反汇编
  -s                       输出内部类型签名
  -sysinfo                 显示正在处理的类的
                           系统信息 (路径, 大小, 日期, MD5 散列)
  -constants               显示最终常量
  -classpath <path>        指定查找用户类文件的位置
  -cp <path>               指定查找用户类文件的位置
  -bootclasspath <path>    覆盖引导类文件的位置
```
- jdb: Java的一个命令行调试环境，既可在本地，也可在与远程的解释器的一次对话中执行。
- jconsole: J2SE监测和管理控制台---一个同JMX兼容的监测Java虚拟机的图形工具。其能够监视当地或者远程的Java虚拟机。
- jps: java虚拟机进程状态工具---在目标系统上列出装备有HotSpot Java的虚拟机启动了 Java 进程时，请使用 jps 工具来通过键入 jps 标识其 JVM。它向您提供一个正在运行的 Java 进程的列表，这些进程以其 JVM 作为 ID。
- 

### 0.2 java扩展参数
java扩展参数，主要是针对jvm的相关参数所产生的。相关参数如下:
1. -Xmixed: 设置-client模式虚拟机对使用频率高的方式进行Just-In-Time编译和执行，对其他方法使用解释方式执行。该方式是虚拟机缺省模式。
2. -Xint: 设置-client模式下运行的虚拟机以解释方式执行类的字节码，不将字节码编译为本机码。
3. -Xbootclasspath:path和-Xbootclasspath/a:path及-Xbootclasspath/p:path: 改变虚拟机装载缺省系统运行包rt.jar而从-Xbootclasspath中设定的搜索路径中装载系统运行类。除非你自己能写一个运行时，否则不会用到该参数。
    - /a:将在缺省搜索路径后加上path 中的搜索路径。
    - /p:在缺省搜索路径前先搜索path中的搜索路径。
4. -Xnoclassgc: 关闭虚拟机对class的垃圾回收功能。
5. -Xincgc: 启动增量垃圾收集器，缺省(默认)是关闭的。增量垃圾收集器能减少偶然发生的长时间的垃圾回收造成的暂停时间。但增量垃圾收集器和应用程序并发执行，因此会占用部分CPU在应用程序上的功能。
6. -Xloggc:<file>: 将虚拟机每次垃圾回收的信息写到日志文件中，文件名由file指定，文件格式是平文件，内容和-verbose:gc输出内容相同。
7. -Xbatch: 虚拟机的缺省运行方式是在后台编译类代码，然后在前台执行代码，使用-Xbatch参数将关闭虚拟机后台编译，在前台编译完成后再执行。
8. -Xms<size>: 设置虚拟机可用内存堆的初始大小，缺省单位为字节，该大小为1024的整数倍并且要大于1MB，可用k(K)或m(M)为单位来设置较大的内存数。初始堆大小为2MB。
9. -Xmx<size>: 设置虚拟机内存堆的最大可用大小，缺省单位为字节。该值必须为1024整数倍，并且要大于2MB。可用k(K)或m(M)为单位来设置较大的内存数。缺省堆最大值为64MB。
10. -Xss<size>: 设置线程栈的大小，缺省单位为字节。与-Xmx类似，也可用K或M来设置较大的值。通常操作系统分配给线程栈的缺省大小为1MB。另外也可在java中创建线程对象时设置栈的大小，构造函数原型为Thread(ThreadGroup group, Runnable target, String name, long stackSize)。
11. -Xprof: 输出CPU运行时的诊断信息。
12. -Xfuture：对类文件进行严格格式检查，以保证类代码符合类代码规范。为保持向后兼容，虚拟机缺省不进行严格的格式检查。
13. -Xrs: 减少虚拟机中操作系统的信号（singals）的使用。该参数通常用在虚拟机以后台服务方式运行时使用（如Servlet）。
14. -Xcheck:jni: 调用JNI函数时进行附加的检查，特别地虚拟机将校验传递给JNI函数参数的合法性，在本地代码中遇到非法数据时，虚拟机将报一个致命错误而终止。使用该参数后将造成性能下降

## 1. Maven基础介绍

Maven是类似于CMake的项目管理工具，不过相对于Java的语言特性，功能更加强大。

### 1.1 Maven的约定基础配置
Maven和CMake一样，存在基础的配置变量；常用参数如下：

|目录	|目的|
|:---:|:---:|
|`${basedir}`|存放pom.xml和所有的子目录|
|`${basedir}/src/main/java`|项目的java源代码|
|`${basedir}/src/main/resources`|项目的资源，比如说property文件，springmvc.xml|
|`${basedir}/src/test/java`|项目的测试类，比如说Junit代码|
|`${basedir}/src/test/resources`|测试用的资源|
|`${basedir}/src/main/webapp/WEB-INF`|web应用文件目录，web项目的信息，比如存放web.xml、本地图片、jsp视图页面|
|`${basedir}/target`|打包输出目录|
|`${basedir}/target/classes`|编译输出目录|
|`${basedir}/target/test-classes`|测试编译输出目录|
|`Test.java`|Maven只会自动运行符合该命名规则的测试类|
|`~/.m2/repository`|Maven默认的本地仓库目录位置|

同时Maven为了更好的约束文件结构；它的常用项目文件结构如下：
```bash

```

注意：Maven的Snapshot和Release
1. SNAPSHOT 版本代表不稳定、尚处于开发中的版本。
    - 常用于被依赖并且经常更新的版本声明。
2. Release 版本则代表稳定的版本。

## 2. Maven的环境配置

详细内容参考官网和[Maven 环境配置](https://www.runoob.com/maven/maven-setup.html)