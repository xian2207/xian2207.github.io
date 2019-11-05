---
layout:     post
title:      Linux内核设计与实现 学习笔记 (五)
subtitle:   Linux内核设计与实现 学习笔记 (五)
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

> 2019-11-05 20:25:49

# Linux内核设计与实现 学习笔记 (五)

------

## 第 16 章 页高速缓存和页回写

页高速缓存(cache)是Linux内核实现的磁盘缓存。主要用来减少对磁盘的I/O操作。通过将磁盘中的数据缓存在物理内存中，把对磁盘数据的访问变为对物理内存的访问。

### 16.1 缓存手段

页缓存是由内存中的物理页面组成的对应磁盘上的物理块。read()操作时，首先检查页是否为在内存中。如果在则直接读取，称为缓存命中。不再则进行中断。


#### 16.1.1 写缓存手段

存在写缓存手段如下：

- 不缓存(nowrite):不去缓存任何写操作；直接写到磁盘
- 写透缓存(write-through cache):自动更新内存缓存；缓存数据时刻与后备存储保持同步。
- 回写(Linux采用)：程序页操作直接写入到缓存中，后端存储不会立刻更新；将高速缓存中的页标记成“脏”，并将其添加到脏页链表中。然后由一个进程(回写进程)周期性将脏页链表中的页写回到磁盘。最后清理脏页标识。

#### 16.1.2 缓存回收

Linux中的脏页回收策略是，通过选择干净页进行简单的替换。如果缓存中没有足够的干净页，内核将强制地进行回写操作，腾出尽可能多的页。页的回收策略方式如下：

- 最近最少使用(RLU)：
- 双链策略(RLU/2)：RLU链表的改进，维护两个链表：活跃链表和非活跃链表。活跃上的不会被换出，非活跃上的会被换出，基本上都是尾部加入头部移除。活跃链表数量超过非活跃链表时，将会将多余的头页面重新移回到非活跃链表中，以便能再被回收。

### 16.2 Linux 页高速缓存

#### 16.2.1 address_space对象

页高速缓存可能包含了多个不连续的物理磁盘块。它可以通过扩展inode结构体支持I/O操作。为了更好的性能，其使用address_space结构体。其与vm_are_struct的物理地址对等。文件只能有一个address_sapce可以有多个vm_area_struct即虚拟地址可以有多个，但是物理内存只能有一份。其数据结构定义如下：

```c
struct address_space {
	struct inode		*host;		/* 拥有节点 */
	struct radix_tree_root	page_tree;	/* 包含全部页面的radix树 */
	spinlock_t		tree_lock;	/* 保护页面的自旋锁 */
	unsigned long		nrpages;	/* 页面总数 */
	pgoff_t			writeback_index;/* writeback starts here */
	struct address_space_operations *a_ops;	/* methods */
	struct list_head	i_mmap;		/* 私有映射连败哦 */
	struct list_head	i_mmap_shared;	/* 共享map链表 */
	struct semaphore	i_shared_sem;	/* 保护所有的链表 */
	atomic_t		truncate_count;	/* 截断计数 */
	unsigned long		flags;		/* gfp_mask掩码和错误标志 */
	struct backing_dev_info *backing_dev_info; /* 预读信息等 */
	spinlock_t		private_lock;	/* 私有address_space锁 */
	struct list_head	private_list;	/* address_space链表 */
	struct address_space	*assoc_mapping;	/* 相关缓冲 */
};
```

其对应的操作如下：

```c
struct address_space_operations {
	int (*writepage)(struct page *page, struct writeback_control *wbc);
	int (*readpage)(struct file *, struct page *);
	int (*sync_page)(struct page *);

	/* Write back some dirty pages from this mapping. */
	int (*writepages)(struct address_space *, struct writeback_control *);

	/* Set a page dirty */
	int (*set_page_dirty)(struct page *page);

	int (*readpages)(struct file *filp, struct address_space *mapping,
			struct list_head *pages, unsigned nr_pages);

	/*
	 * ext3 requires that a successful prepare_write() call be followed
	 * by a commit_write() call - they must be balanced
	 */
	int (*prepare_write)(struct file *, struct page *, unsigned, unsigned);
	int (*commit_write)(struct file *, struct page *, unsigned, unsigned);
	/* Unfortunately this kludge is needed for FIBMAP. Don't use it */
	sector_t (*bmap)(struct address_space *, sector_t);
	int (*invalidatepage) (struct page *, unsigned long);
	int (*releasepage) (struct page *, int);
	ssize_t (*direct_IO)(int, struct kiocb *, const struct iovec *iov,
			loff_t offset, unsigned long nr_segs);
};
```

#### 16.2.3 基树

每个address_space对象都有一个唯一的基树(radix tree)来进行文件偏移量的搜索。页面高速缓存的搜索函数find_get_page()要调用函数radix_tree_lookup(),该函数会指定基树中搜索指定页面。

#### 16.2.5 以前的页散列表

2.6之前，系统通过维护一个全局散列表进行检索。存在种种缺点：如锁的争用情况严重，搜索范围大等。

### 16.3 缓冲区高速缓存

内核提供bread()函数实现从磁盘读取越高块的底层操作；通过缓存，磁盘块映射到他们相关的内存页。

### flusher线程

一下三种情况发生时,脏页被写回磁盘：

- 当空闲内存低于一个特定的阀值时
- 脏页在内存中驻留时间超过一个特定的阀值时；flusher线程后台例程会被周期性唤醒来执行这个操作。
- 用户进程调用sync()和fsync()系统调用时；内核会按照要求执行回写动作。


内核中由一群内核线程(flusher线程)执行这三种工作。当空闲内存比阀值(dirty_background_ratio)还低时，内核便会调用flusher_thread()唤醒一个或者多个flusher线程，随后flusher线程进一步调用函数bdi_writeback_all()开始将脏页写回磁盘。

系统管理员可以在`/proc/sys/vm`中设置回写相关的参数，也可以通过sysctl系统调用设置它们。pdflush相关的所有可设置变量如下：

![可设置变量](../img/2019-11-05-22-07-17.png)

#### 16.4.1 膝上型计算机模式

它是一种特殊的页回写策略：将硬盘的转动的机械行为最小化，允许硬盘尽可能长时间的停滞，以此延长电池供电。可以通过`/proc/sys/vm/laptop_mode`文件进行配置。向其中写入1开启。其会找准磁盘运转的实际，把所有其它的物理磁盘I/O、刷新脏页缓冲等统统写回到磁盘。来减少写磁盘要求的主动运行。

#### 16.4.3 避免拥塞的方法：使用多线程

页回写时可能存在拥塞。使用多个回写线程并发执行，避免一个bdflush堵塞在某个队列的处理(正在等待将请求提交给磁盘的I/O请求队列)上。这也是使用多个flusher的原因。不同的flusher线程处理不同的设备队列。

pflush线程采取了拥塞回避策略：主动尝试从那些没有拥塞的对了回写页。

## 第 17 章 设备与模块

