---
layout:     post
title:      Linux内核设计与实现 学习笔记 (四)
subtitle:   Linux内核设计与实现 学习笔记 (四)
date:       2019-10-24
author:     王鹏程
header-img: img/post-bg-ios10.jpg
catalog: true
tags:
    - C/C++
    - Linux
    - 操作系统
    - 程序设计
---

> 2019-11-02 20:25:49

# Linux内核设计与实现 学习笔记 (四)

------

## CPU内存管理总结

### 1. x86/x64处理器体系结构寻址模式

_参考链接:_ 

- [X86/X64处理器体系结构及寻址模式](https://blog.csdn.net/liuyez123/article/details/51096914)
- [x64内核内存空间结构](https://blog.csdn.net/farmwang/article/details/52016027)

#### 1.1 处理器体系结构

CPU处理器的主要结构如下：

- 控制单元：控制单元是整个CPU的指挥控制中心，由指令寄存器IR(Instruction Register)、指令译码器ID(Instruction Decoder)和操作控制器OC(Operation Controller)等，对协调整个电脑有序工作极为重要。
- 运算单元：是运算器的核心。可以执行算术运算(包括加减乘数等基本运算及其附加运算)和逻辑运算(包括移位、逻辑测试或两个值比较)。相对控制单元而言，运算器接受控制单元的命令而进行动作，即运算单元所进行的全部操作都是由控制单元发出的控制信号来指挥的，所以它是执行部件。 
- 存储单元：包括CPU片内缓存和寄存器组，是CPU中暂时存放数据的地方，里面保存着那些等待处理的数据，或已经处理过的数据，CPU访问寄存器所用的时间要比访问内存的时间短。采用寄存器，可以减少CPU访问内存的次数，从而提高了CPU的工作速度。


![cpu工作流程](https://img-blog.csdn.net/20160408141333557)

##### 1.1.2 寄存器结构

在之前的文章[C/C++ 的一个符号操作问题](https://wangpengcheng.github.io/2019/05/12/c_peator_problem/)中也介绍过，寄存器的类别和基本结构.其主要结构如下图所示：

![通用寄存器结构](https://img-blog.csdn.net/20160408141505840)

专用寄存器：

![专用寄存器](https://img-blog.csdn.net/20160408141556445)

以上所列出的一些通用寄存器（注：其中RSP为专用寄存器，之所以把它放在通用寄存器组中只是为了方便记忆整个模型），除了数据位宽度不同之外，并无多大差别：

- RAX(累加器)：RAX如果是8/16/32位寻址，则只改变该寄存器的一部分。累加器用于乘法、除法及一些调整指令，同时也可以保存存储单元的偏移地址。
- RBX(基址)：用于保存存储单元的偏移地址，同时也能寻址存储器数据，作为偏移地址访问数据时默认使用数据段基址DS作为段前缀。
- RCX(计数)：可保存访问存储单元的偏移地址，或在串指令(REP/REPE/REPNE)以及移位、循环和LOOP/LOOPD指令中用作计数器。
- RDX(数据)：可使用RDX/EDX/DX/DH/DL寻址，同时作为通用寄存器也用于保存乘法形成的部分结果或者除法之前的部分被除数，也可用于寻址存储单元。
- RBP(基指针)：可用RBP/EBP/BP寻址，同时作为偏移地址访问存储单元时默认使用堆栈段基址SS作为段前缀。
- RDI(目的变址)：可用RDI/EDI/DI寻址，常用于在串指令中寻址目的数据串。
- RSI(源变址)：如RDI一样，RSI也可作为通用寄存器使用，通常为串指令寻址源数据串。

段寄存器CS、DS、ES、SS、FS、GS以及RSP为专用寄存器，以下是这些寄存器的概要描述：

- RSP(堆栈指针)：RSP寻址称为堆栈的存储区，通过该指针存取堆栈数据。用作16位寄存器时使用SP，如果是32位则为ESP。
- CS(代码段)：代码段寄存器存放程序所使用的代码在存储器中的基地址。 • DS(数据段)：存放数据段的基地址
- ES(附加段)：该段寄存器通常在串指令(LODS/STOS/MOVS/INS/OUTS)中使用，主要用于在存储器中将数据进行成块转移。
- SS(堆栈段)：为堆栈定义一个存储区域。主要用来存放过程调用所需参数、本地局部变量以及处理器状态等。
- FS与GS：这两个段寄存器是386~Core2中新增的段寄存器，以允许程序访问附加的存储器段。可以将其视为“通用的段寄存器”，通过将段的基地址存入这两个寄存器中可以实现自定义的寻址操作，从而增加了编程的灵活性。

**每一个寄存器都有一个”可见”部分和一个”隐藏”部分。(这个隐藏部分有时也指一个”描述符缓存”(descriptor cache)或者”阴影寄存器”(shadow register))。当一个段选择器被加载到段寄存器的可见部分，处理器也会自动把基址，段界限，和段描述符中的访问控制信息加载到段寄存器的隐藏部分。把信息缓存在段寄存器(可见和隐藏部分)允许处理器不经过额外的总线循环(bus cycles)去段描述符总读取基址和界限来转换地址。当描述符表发生了更改，软件有义务重新加载段寄存器。如果不这样做，段寄存器中使用的老段描述符还是会继续使用。**

##### 1.1.3 地址总线
_参考链接：_ [CPU的设计原理，数据总线和地址总线](https://blog.csdn.net/wxywxywxy110/article/details/78765314);


RIP寻址代码段中当前执行指令的下一条指令，当处理器工作在实模式下时使用16位的IP寄存器，当工作于保护模式时则使用32位的EIP。指令指针可由转移指令或调用指令修改。需要注意的是，**在64位模式中由于处理器包含40位地址总线，所以总共可以寻址240=1TB的内存。** 具体的地址总线数目需要看CPU的设计，linux操作系统最多支持48根地址总线。

![地址总线](https://img-blog.csdn.net/20160408142656043)

EFLAGS(program status and control) register主要用于提供程序的状态及进行相应的控制，在64-bit模式下，EFLGAS寄存器被扩展为64位的RFLGAS寄存器，高32位被保留，而低32位则与EFLAGS寄存器相同。

#### 1.2 处理器工作及寻址模式

对于实际的内存条而言，处理器把它当做8位一个字节的序列来管理和存取(**内存对齐的原因**)，每一个内存字节都有一个对应的地址，我们叫它物理地址，用地址可以表示的长度叫做寻址空间。而CPU是如何去访问内存单元里的数据的方式就叫做寻址。

- **实模式**：即“段地址”+“段内偏移量”，它们分别由段寄存器+通用寄存器，保存，即基址+变址的方式进行寻址。例如，ES=0x1000，DI=0xFFFF，那么这个数据ES:DI在内存里的绝对物理地址就是：`AD(Absolute Address)=(ES)*(0x10)+(DI)=0x1FFFF`。早期低位计算机中使用。但是因为物理内存的短缺，使用将使用空闲的物理地址重复映射(用户空间的原型？？).现代计算机为了兼容性，基本都运行在这个实模式下，但是数据容易被病毒木马等进行修改。
  ![实模式](https://img-blog.csdn.net/20160408143416803)
- **保护模式(IA-32模式)**:硬件CPU引入的地址保护的概念。并使用段页描述方式。增加了GDTR (global descriptor talbe register)指向全局段描述符数组（表）；LDTR (localdescriptor table register)执行局部段描述符数组（表）而6个段寄存器，CS/DS/SS/ES包括后来的FS/GS，其内容不在用作基址，而是用作索引去段描述符数组中查找对应的段描述符。段描述符占8个字节，其定义以及其中各个标志位的定义如下： 
- ![段页寄存器](https://img-blog.csdn.net/20160408143805110)
- ![段描述符](https://img-blog.csdn.net/20160408143932532)
下表为Linux内核对段描述符的典型设置方式：

|段|基地址|G|界限|S|TYPE|DPL|D|P|
|:---|:---|:---|:---|:---|:---|:---:|:---:|:---:|
|用户代码段|0x0000|0000|1|0xF|FFFF|1|10(0x1010)|3|1|
|用户数据段|0x0000|0000|1|0xF|FFFF|1|2(0x0010)|3|1|
|内核代码段|0x0000|0000|1|0xF|FFFF|1|10(0x1010)|0|1|
|内核数据段|0x0000|0000|1|0xF|FFFF|1|2(0x0010)|0|1|

上述设置分别与Linux内核中的宏__USER_CS，__USER_DS，__KERNEL_CS，__KERNEL_DS相对应。
- **长模式(IA-32e模式)**：在长模式下，处理器完全执行64位指令，使用64位地址空间（物理内存的寻址能力却没有被完全扩展到64位，因为目前的众多CPU在其寿命期限之内都没有机会见识到如此巨大的内存）和64操作数。因此，为了降低制造成本，目前的CPU被限制在略少于64位寻址。

### 2. Linux的内核空间

_参考链接：_ 

- [Linux的内核空间（低端内存、高端内存）](Linux的内核空间（低端内存、高端内存）)
- [linux 用户空间与内核空间——高端内存详解](https://blog.csdn.net/Tommy_wxie/article/details/17122923)
- [Linux x86_64线性地址空间布局](https://blog.csdn.net/junmuzi/article/details/18056115)
- [Linux内核在x86_64 CPU中地址映射](http://ilinuxkernel.com/?p=1303)
> ps: 参考链接最好都看一下

32 位Linux的内核空间，为了兼容性，牺牲了很对，对于逻辑的地址空间，32位对应的4GB,因此在当初的操作系统内核设计时，为了将一部分的逻辑地址空间留给用户空间，因此内核只给自己留了1GB的内核逻辑地址空间。

![用户空间与内核空间](https://img-blog.csdn.net/20180718214320564?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNzMw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

但是这1GB的逻辑内核，当物理内存大于1GB时，便会存在问题。因此除了一部分的直接映射方式之外。Linux内核中，提供了高端内存区（`ZONE_HIGHMEM`）来随意映射所有可用的物理内存(ps：**直接映射只是，建立起了映射管理关系，并没有对物理内存进行读写**)

![逻辑地址与物理页框](https://img-blog.csdn.net/20180719195131930?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNzMw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

添加高端映射之后：

![高端映射](https://img-blog.csdn.net/20180719222943732?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNzMw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

高端映射可以由vmalloc进行分配和管理

![vmalloc映射区](https://img-blog.csdn.net/20180719200554200?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNzMw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

64位操作系统采用了4层地址映射（[x86 64位内存管理](https://blog.csdn.net/liminyu/article/details/12519729)）

![4层地址映射](https://img-blog.csdn.net/20131009181225203?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGltaW55dQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

其逻辑地址空间分布如下：

![64逻辑地址空间](https://img-blog.csdn.net/20131009181344968?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGltaW55dQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

注意：64为操作系统更改了`ZONE_HIGHMEM`的错误，因此在其中没有ZONE_HIGHMEM。目前的x86_64架构CPU都遵循AMD的Canonical Form,即只有虚拟地址的最低48位才会在地址转换时被使用, 且任何虚拟地址的48位至63位必须与47位一致, 也就是说总的虚拟地址空间为256TB。

那么在64位架构下，如何分配虚拟地址空间的呢？
0000000000000000 – 00007fffffffffff(128TB)为用户空间,
ffff800000000000 – ffffffffffffffff(128TB)为内核空间。

而且内核空间中有很多空洞, 越过第一个空洞后, ffff880000000000 – ffffc7ffffffffff(64TB)才是直接映射物理内存的区域, 也就是说默认的PAGE_OFFSET为ffff880000000000.

**既然现在内核直接映射的物理内存区域有64TB， 而且一般情况下，极少有计算机的内存能达到64TB（别说64TB了，1TB内存的也很少很少），所以整个内核虚拟地址空间都能够一一映射到计算机的物理内存上，因此，不再需要 ZONE_HIGHMEM这个分区了，现在对物理内存的划分，只有ZONE_DMA， ZONE_NORMAL**

64位的实际逻辑内存空间布局如下(最左边为当前使用的48为逻辑地址空间)：

![64位的虚拟地址布局](https://76tcqw.dm1.livefilestore.com/y2pC1npSpLE4XpfjVA4ZeLk-4yfjDjeKMWFpps88Ah6COnUYBI93UW_2AWrRfij83dS8Q5KIa7GbLZlrmLOAZNjfNwnu43DkQMjjXXfIEQWbR0/x86mem.png?psid=1)

详细内存布局如下：

![linux X86 64位内存布局图](https://img-blog.csdn.net/20131119194552578?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveW91bmdlcl9jaGluYQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

具体信息可以在Linux的`/proc/meminfo`中看到。


## 3. linux NUMA技术

_参考技术：_ 

- [linux NUMA技术](https://blog.csdn.net/wu7244582/article/details/52807117)
- [Linux内核学习笔记：SMP、UMA、NUMA](https://blog.csdn.net/tiangwan2011/article/details/7298785)
- [服务器体系(SMP, NUMA, MPP)与共享存储器架构(UMA和NUMA)](https://blog.csdn.net/gatieme/article/details/52098615)
- [服务器三大体系 SMP、NUMA、MPP 之详解](https://blog.csdn.net/ZhipingXi/article/details/78133096)

NUMA（Non-Uniform Memory Access Architecture）即非一致性内存访问技术。NUMA系统有多个Node通过高速互连的网络联系起来的系统。而Node则是由一组cpu和本地内存组成。不同的Node有不同的物理内存，由于Node访问本地内存和访问其它节点的内存的速度是不一致的，为了解决非一致性访问内存对性能的影响，有一些工具可以使用。包括 numactl，autonuma， HPE-AIX等。

## 第 13 章 虚拟文件系统

_参考链接：_

- [Linux 的虚拟文件系统(强烈推荐)](https://www.cnblogs.com/feng9exe/p/8383950.html)
- [Linux虚拟文件系统解析](https://www.cnblogs.com/1441760655-Jansert/p/10800025.html)
- [详解 Linux 中的虚拟文件系统](https://linux.cn/article-10884-1.html?pr)
- [Linux的虚拟文件系统](https://blog.csdn.net/qq_38410730/article/details/81368186)


> ps:第一篇文章总结的非常好，不做过多叙述

FS对于具体文件系统来说，相当于一个管理者，提供统一的文件系统接口。对于内核其他子系统以及在操作系统之上的用户程序而言，所有的文件系统都是一样的。实际上，要支持新的文件系统，主要需要完成的工作就是编写这些接口函数。虚拟文件系统主要有以下几个作用：

- （1）对具体文件系统的数据结构进行抽象，以统一的数据结构进行管理；
- （2）接收用户层的调用，例如write、read、open等；
- （3）支持多种具体文件系统之间的相互访问；
- （4）接收内核其他子系统的操作请求，特别是内存管理子系统.

![图1 Linux中文件系统的逻辑关系示意图](https://img2018.cnblogs.com/blog/1478048/201905/1478048-20190501155530434-1213531988.png)

图1 Linux中文件系统的逻辑关系示意图。

![虚拟文件系统](../img/2019-11-02-21-34-36.png)

**写操作时的系统调用情况**
![写操作调用](../img/2019-11-02-21-35-34.png)

### 13.4 VFS 对象及其数据结构

从本质上讲，文件系统是特殊的数据分层存储结构，它包含文件、目录和相关的控制信息。为了描述 这个结构，
VFS采用面向对象的设计思路。将其分为不同的对象，并提供了对象的操作方式：

- **文件** 一组在逻辑上具有完整意义的信息项的系列。在Linux中，除了普通文件，其他诸如目录、设备、套接字等 也以文件被对待。总之，“一切皆文件”。
- **目录** 目录好比一个文件夹，用来容纳相关文件。因为目录可以包含子目录，所以目录是可以层层嵌套，形成 文件路径。在Linux中，目录也是以一种特殊文件被对待的，所以用于文件的操作同样也可以用在目录上。
- **目录项** 在一个文件路径中，路径中的每一部分都被称为目录项；如路径/home/source/helloworld.c中，目录 /, home, source和文件 helloworld.c都是一个目录项。
- **索引节点** 用于存储文件的元数据的一个数据结构。文件的元数据，也就是文件的相关信息，和文件本身是两个不同 的概念。它包含的是诸如文件的大小、拥有者、创建时间、磁盘位置等和文件相关的信息。
- **超级块** 用于存储文件系统的控制信息的数据结构。描述文件系统的状态、文件系统类型、大小、区块数、索引节 点数等，存放于磁盘的特定扇区中。

概念在磁盘中的位置关系图如下:

![文件系统关系](http://www.ibm.com/developerworks/cn/linux/l-cn-vfs/4.jpg)

### 13.5 超级块对象

描述整个文件系统的信息。其只存在于内存中。定义在`include/fs/fs.h`中。主要域含义如下：

```c
struct super_block
{
    struct list_head s_list;                /*指向超级块链表的指针*/
    /************描述具体文件系统的整体信息的域*****************/
    kdev_t s_dev; /* 包含该具体文件系统的块设备标识符。例如，对于 /dev/hda1，其设备标识符为 0x301*/
    unsigned long s_blocksize; /*该具体文件系统中数据块的大小，以字节为单位 */
    unsigned char s_blocksize_bits; /*块大小的值占用的位数，例如，如果块大小为 1024 字节，则该值为 10*/
    unsigned long long s_maxbytes; /* 文件的最大长度 */
    unsigned long s_flags; /* 安装标志*/
    unsigned long s_magic; /*魔数，即该具体文件系统区别于其他文件系统的一个标志*/
    
    /**************用于管理超级块的域******************/
    struct list_head s_list; /*指向超级块链表的指针*/
    struct semaphore s_lock /*锁标志位，若置该位，则其他进程不能对该超级块操作*/
    struct rw_semaphore s_umount /*对超级块读写时进行同步*/
    unsigned char s_dirt; /*脏位，若置该位，表明该超级块已被修改*/
    struct dentry *s_root; /*指向该具体文件系统安装目录的目录项*/
    int s_count; /*对超级块的使用计数*/
    atomic_t s_active;
    struct list_head s_dirty; /*已修改的索引节点形成的链表 */
    struct list_head s_locked_inodes;/* 要进行同步的索引节点形成的链表*/
    struct list_head s_files
    
    /***********和具体文件系统相联系的域*************************/
    struct file_system_type *s_type; /*指向文件系统的
    file_system_type 数据结构的指针 */
    struct super_operations *s_op; /*指向某个特定的具体文件系统的用于超级块操作的函数集合 */
    struct dquot_operations *dq_op; /* 指向某个特定的具体文件系统用于限额操作的函数集合 */
    u; /*一个共用体，其成员是各种文件系统的 fsname_sb_info 数据结构 */
    
};

```
其对应的超级块操作如下：

```c
struct super_operations {
    /* 给定的超级块下创建和初始化一个新的索引节点对象 */
   	struct inode *(*alloc_inode)(struct super_block *sb);
	/* 释放给定的索引节点 */
    void (*destroy_inode)(struct inode *);
    /* 当索引节点被修改时，执行函数更新日志文件系统 */
   	void (*dirty_inode) (struct inode *);
    /* 将给定的索引几点写入磁盘， wait表示是否需要同步操作*/
	int (*write_inode) (struct inode *, int wait);
    /* 最后一个索引节点的引用被释放后，VFS会调用该函数。进行删除节点 */
	void (*drop_inode) (struct inode *);
    /* 删除指定节点 */
	void (*delete_inode) (struct inode *);
	/* 卸载文件系统时，由VFS调用，用来释放超级块。调用者必须一直持有s_lock锁 */
    void (*put_super) (struct super_block *);
    /* 用给定的超级块更新磁盘上的超级快，VFS通过该函数对内存中的超级快和磁盘中的超级快进行同步，调用者必须一直持有s_lock锁 */
	void (*write_super) (struct super_block *);
    /* 文件系统的数据元与磁盘上的文件系统同步。wait指定是否同步 */
	int (*sync_fs)(struct super_block *sb, int wait);
    /*  */
	int (*freeze_fs) (struct super_block *);
	int (*unfreeze_fs) (struct super_block *);
    /* 获取文件系统状态，指定文件系统相关的统计信息放在statfs中 */
	int (*statfs) (struct dentry *, struct kstatfs *);
    /* 指定新的安装选项重新安装文件系统时调用，调用者必须一直持有s_lock锁 */
	int (*remount_fs) (struct super_block *, int *, char *);
    /* 释放索引节点，并清空包含相关数据的所有页面 */
	void (*clear_inode) (struct inode *);
    /* 中断安装操作，该函数被网络文件系统使用，如NFS */
	void (*umount_begin) (struct super_block *);
 
	int (*show_options)(struct seq_file *, struct vfsmount *);
	int (*show_stats)(struct seq_file *, struct vfsmount *);
#ifdef CONFIG_QUOTA
	ssize_t (*quota_read)(struct super_block *, int, char *, size_t, loff_t);
	ssize_t (*quota_write)(struct super_block *, int, const char *, size_t, loff_t);
#endif
	int (*bdev_try_to_free_page)(struct super_block*, struct page*, gfp_t);
};
```
调用方式：`sb->s_op->write_super(sb)`;sb是指向文件系统超级块的指针，从该指针进入超级块对应的函数表，并从中获得`write_super()`函数。相当于C++中的this指针。

#### VFS的dentry结构
结构dentry是实际文件系统的目录项在虚拟文件系统VFS中的对应物，实质上就是前面所讲的目录缓冲区。当系统访问一个具体文件时，会根据这个文件在磁盘上的目录在内存中创建一个dentry结构，它除了要存放文件目录信息之外，还要存放有关文件路径的一些动态信息。

例如：它建立了文件名与节点inode之间的联系，保存了目录项与其父、子目录之间的关系。之所以建立这样的一个文件目录的对应物，是为了同一个目录被再次访问时，其相关信息就可以直接由dentry获得，而不必再次重复访问磁盘。

结构dentry在文件include/linux/dcache.h中定义如下：

```c
struct dentry {
	atomic_t d_count;
	unsigned int d_flags;		/* 记录目录项被引用次数的计数器 */
	spinlock_t d_lock;		/* 目录项的标志 */
	int d_mounted;
	struct inode *d_inode;		/* 与文件名相对应的inode */
 
	struct hlist_node d_hash;	/* 目录项形成的散列表 */
	struct dentry *d_parent;	/* 指向父目录项的指针 */
	struct qstr d_name;        //包含文件名的结构
 
	struct list_head d_lru;		/* 已经没有用户使用的目录项的链表 */
	union {
		struct list_head d_child;	/* 父目录的子目录项形成的链表 */
	 	struct rcu_head d_rcu;
	} d_u;
	struct list_head d_subdirs;	/* i节点别名的链表 */
	struct list_head d_alias;	/* inode alias list */
	unsigned long d_time;		/* used by d_revalidate */
	const struct dentry_operations *d_op;        //指向dentry操作函数集的指针
	struct super_block *d_sb;	/* 目录树的超级块，即本目录项所在目录树的根 */
	void *d_fsdata;			/* 文件系统的特定数据 */
 
	unsigned char d_iname[DNAME_INLINE_LEN_MIN];	/* 短文件名 */
};
```
结构中的域d_name是qstr类型的结构。该结构的定义如下：
```c
struct qstr {
	unsigned int hash;            //文件名
	unsigned int len;            //文件名长度
	const unsigned char *name;        //散列值
};

```
内核通过维护一个散列表dentry_hashtable来管理dentry。系统一旦在内存中建立一个dentry，该dentry结构就会被链入散列表的某个队列中。

结构中的域d_count记录了本dentry被访问的次数。当某个dentry的d_count的值为0时，就意味着这个dentry结构已经没有用户使用了，这时这个dentry会被d_lru链入一个由系统维护的另一个队列dentry_unused中。由于内核从不删除曾经建立的dentry，于是，同一个目录被再次访问时，其相关信息就可以直接由dentry获得，而不必重复访问存储文件系统的外存。

另外，当本目录项有父目录节点时，其dentry结构通过域d_child挂入父目录项的子目录d_subdirs队列中，同时使指针d_parent指向父目录的dentry结构。若本目录有子目录，则将它们挂接在域d_subdirs指向的队列中。

dentry操作如下：

```c
struct dentry_operations {
	int (*d_revalidate)(struct dentry *, struct nameidata *);        //判断目录项是否有效
	int (*d_hash) (struct dentry *, struct qstr *);            //生成一个散列值
	int (*d_compare) (struct dentry *, struct qstr *, struct qstr *);        //比较两个文件名
	int (*d_delete)(struct dentry *);        //删除count为0的dentry
	void (*d_release)(struct dentry *);        //释放一个dentry对象
	void (*d_iput)(struct dentry *, struct inode *);        //丢弃目录项对应的inode
	char *(*d_dname)(struct dentry *, char *, int);
};
```

### 13.7 索引节点对象

文件系统处理文件所需要的所有信息都放在称为索引节点的数据结构中，索引节点对于文件是唯一的。具体文件系统的索引节点是存储在磁盘上的，是一种静态结构，要使用它，必须调入内存，填写VFS的索引节点，因此，也称VFS索引节点为动态节点。VFS索引节点的数据结构inode在/include/fs/fs.h/中定义如下：

```c
struct inode
{
    /**********描述索引节点高速缓存管理的域****************/
    struct list_head i_hash; ／*指向哈希链表的指针*/
    struct list_head i_list; /*指向索引节点链表的指针*/
    struct list_head i_dentry;/*指向目录项链表的指针*/
    struct list_head i_dirty_buffers;
    struct list_head i_dirty_data_buffers;
    
    /**********描述文件信息的域****************/
    unsigned long i_ino; /*索引节点号*/
    kdev_t i_dev; /*设备标识号 */
    umode_t i_mode; /*文件的类型与访问权限 */
    nlink_t i_nlink; /*与该节点建立链接的文件数 */
    uid_t i_uid; /*文件拥有者标识号*/
    gid_t i_gid; /*文件拥有者所在组的标识号*/
    kdev_t i_rdev; /*实际设备标识号*/
    off_t i_size; /*文件的大小（以字节为单位）*/
    unsigned long i_blksize; /*块大小*/
    unsigned long i_blocks; /*该文件所占块数*/
    time_t i_atime; /*文件的最后访问时间*/
    time_t i_mtime; /*文件的最后修改时间*/
    time_t i_ctime; /*节点的修改时间*/
    unsigned long i_version; /*版本号*/
    struct semaphore i_zombie; /*僵死索引节点的信号量*/
    
    /***********用于索引节点操作的域*****************/
    struct inode_operations *i_op; /*索引节点的操作*/
    struct super_block *i_sb; /*指向该文件系统超级块的指针 */
    atomic_t i_count; /*当前使用该节点的进程数。计数为 0，表明该节点可丢弃或被重新使用 */
    struct file_operations *i_fop; /*指向文件操作的指针 */
    unsigned char i_lock; /*该节点是否被锁定，用于同步操作中*/
    struct semaphore i_sem; /*指向用于同步操作的信号量结构*/
    wait_queue_head_t *i_wait; /*指向索引节点等待队列的指针*/
    unsigned char i_dirt; /*表明该节点是否被修改过，若已被修改，则应当将该节点写回磁盘*/
    struct file_lock *i_flock; /*指向文件加锁链表的指针*/
    struct dquot *i_dquot[MAXQUOTAS]; /*索引节点的磁盘限额*/
    /************用于分页机制的域**********************************/
    struct address_space *i_mapping; ／* 把所有可交换的页面管理起来*/
    struct address_space i_data;
    
    /**********以下几个域应当是联合体****************************************/
    struct list_head i_devices; /*设备文件形成的链表*/
    struct pipe_inode_info i_pipe; /*指向管道文件*/
    struct block_device *i_bdev; /*指向块设备文件的指针*/
    struct char_device *i_cdev; /*指向字符设备文件的指针*/
    
    /*************************其他域***************************************/
    unsigned long i_dnotify_mask; /* Directory notify events */
    struct dnotify_struct *i_dnotify; /* for directory notifications */
    unsigned long i_state; /*索引节点的状态标志*/
    unsigned int i_flags; /*文件系统的安装标志*/
    unsigned char i_sock; /*如果是套接字文件则为真*/
    atomic_t i_writecount; /*写进程的引用计数*/
    unsigned int i_attr_flags; /*文件创建标志*/
    __u32 i_generation /*为以后的开发保留*/
    
    /*************************各个具体文件系统的索引节点********************/
    union； /*类似于超级块的一个共用体，其成员是各种具体文件系统的 fsname_inode_info 数据结构 */
}
```
与VFS的超级块一样，VFS的i节点也是既包含虚拟文件系统的通用管理系统，又包含具体文件系统i节点的私有信息。其中，结构的指针generic_ip就指向了具体文件系统i节点的私有信息，该信息也被保存在内存的一个结构中。

例如：当虚拟文件系统与Ext2文件系统对接时，指针generic_ip就指向Ext2文件系统的i节点信息结构ext2_inode_info。即系统在安装Ext2系统时，会在内存中创建一个ext2_inode_info结构实例，并将磁盘文件系统Ext2的i节点的部分私有信息提取到该结构。

结构struct ext2_inode_info的代码如下：
```c
struct ext2_inode_info {
	__le32	i_data[15];            //数据库块指针数组
	__u32	i_flags;            //文件打开方式
	__u32	i_faddr;
	__u8	i_frag_no;            //第一个碎片号
	__u8	i_frag_size;            //碎片大小
	__u16	i_state;
	__u32	i_file_acl;            //文件访问控制链表
	__u32	i_dir_acl;
	__u32	i_dtime;            //文件删除时间
 
	__u32	i_block_group;            //文件所在块组号
	struct ext2_block_alloc_info *i_block_alloc_info;
 
	__u32	i_dir_start_lookup;
#ifdef CONFIG_EXT2_FS_XATTR
	struct rw_semaphore xattr_sem;
#endif
#ifdef CONFIG_EXT2_FS_POSIX_ACL
	struct posix_acl	*i_acl;
	struct posix_acl	*i_default_acl;
#endif
	rwlock_t i_meta_lock;
 
	struct mutex truncate_mutex;
	struct inode	vfs_inode;
	struct list_head i_orphan;	/* unlinked but open inodes */
};
```
当虚拟文件与Ext2文件系统相关联时，VFS的索引节点如下图所示：
![对应关系](https://img-blog.csdn.net/201808032038100?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNzMw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

每个文件都有一个inode，每个inode有一个索引节点号i_ino;inode中有两个设备号，i_dev和i_rdev。除特殊文件外，每个节点都存储在某个设备上，这就是 i_dev。如果索引节点所代表的并不是常规文件，而是某个设备，那就还得有个设备号，这就是 i_rdev;每个索引节点都会复制磁盘索引节点包含的一些数据，比如文件占用的磁盘块数。属于"正在使 用"或"脏"链表的索引节点对象也同时存放在一个称为inode_hashtable链表中。

其操作调用方式如下：`i->i_op->truncate(i)`

```c
struct inode_operations {
    /* 创建一个新的索引节点 */
	int (*create) (struct inode *,struct dentry *,int, struct nameidata *);    
    /* 在特定目录中寻找索引节点，该索引节点要对应于denrty中给出的文件名 */
	struct dentry * (*lookup) (struct inode *,struct dentry *, struct nameidata *);        
    /* 创建一个硬链接，dentry参数指定硬链接名，链接对象是dir中的old_dir */
	int (*link) (struct dentry *,struct inode *,struct dentry *);
    /* 删除一个硬链接 */    
	int (*unlink) (struct inode *,struct dentry *);        
	/* 创建一个软链接 */
    int (*symlink) (struct inode *,struct dentry *,const char *);
	/* 创建一个文件夹 */
    int (*mkdir) (struct inode *,struct dentry *,int);
	int (*rmdir) (struct inode *,struct dentry *);
	int (*mknod) (struct inode *,struct dentry *,int,dev_t);
	int (*rename) (struct inode *, struct dentry *,
			struct inode *, struct dentry *);
	int (*readlink) (struct dentry *, char __user *,int);
	void * (*follow_link) (struct dentry *, struct nameidata *);
	void (*put_link) (struct dentry *, struct nameidata *, void *);
	void (*truncate) (struct inode *);
	int (*permission) (struct inode *, int);
	int (*setattr) (struct dentry *, struct iattr *);
	int (*getattr) (struct vfsmount *mnt, struct dentry *, struct kstat *);
	int (*setxattr) (struct dentry *, const char *,const void *,size_t,int);
	ssize_t (*getxattr) (struct dentry *, const char *, void *, size_t);
	ssize_t (*listxattr) (struct dentry *, char *, size_t);
	int (*removexattr) (struct dentry *, const char *);
	void (*truncate_range)(struct inode *, loff_t, loff_t);
	long (*fallocate)(struct inode *inode, int mode, loff_t offset,
			  loff_t len);
	int (*fiemap)(struct inode *, struct fiemap_extent_info *, u64 start,
		      u64 len);
};
```

