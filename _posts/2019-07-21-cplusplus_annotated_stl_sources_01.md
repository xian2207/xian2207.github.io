---
layout:     post
title:      STL 源码剖笔记(一)
subtitle:   STL 源码剖析笔记(一)
date:       2019-07-06
author:     王鹏程
header-img: img/post-bg-ios10.jpg
catalog: true
tags:
    - C++
    - 基础编程
    - STL源码解析
---

> 2019-7-21 16:46:53 
> 

# STL 源码剖析 --侯捷

## STL概论与版本简介

STL六大组件

- 容器(containers):vector.list.deque,set,map等数据存放的类。
- 算法(algorithms):各种排序算法;sort、search、copy、erase..等
- 迭代器(iterators)：扮演容器与算法之间的胶合剂，即泛型指针
- 功能函数(functors)：行为类似函数，可以视为算法的某种策略
- 配接器(adapters)：一种用来修饰容器(containers)或仿函数(functors)或迭代器(iterators)借口的东西
- 配置器(allocators):负责空间配置与管理

![STL六大组件之间的关系](../img/2019-07-21-17-10-18.png)


## 第二章 空间配置器

allocator是空间配置器，定义于头文件<memory> std::allocator类模板是所有标准库容器所用的默认分配器 (Allocator)，若不提供用户指定的分配器。默认分配器无状态，即任何给定的 allocator实例可交换、比较相等，且能解分配同一`allocator`类型的任何其他实例所分配的内存。

成员类型

|类型|定义|
|:------|:-----|
|value_type|T|
|pointer (C++17 中弃用)(C++20 中移除)|T*|
|const_pointer (C++17 中弃用)(C++20 中移除)|const T*|
|reference (C++17 中弃用)(C++20 中移除)|T&|
|const_reference (C++17 中弃用)(C++20 中移除)|const T&|
|size_type|std::size_t|
|difference_type|std::ptrdiff_t|
|propagate_on_container_move_assignment(C++14)|std::true_type|
|rebind (C++17 中弃用)(C++20 中移除)|template< class U > struct rebind { typedef allocator<U> other; };|
|is_always_equal(C++17)|std::true_type|

成员函数

|函数|定义|
|:---|:---|
|(构造函数)|创建新的 allocator 实例(公开成员函数)|
|(析构函数)|析构 allocator 实例(公开成员函数)|
|address(C++17 中弃用)(C++20 中移除)|获得对象的地址，即使重载了 operator&(公开成员函数)|
|allocate|分配未初始化的存储(公开成员函数)|
|deallocate|解分配存储(公开成员函数)|
|max_size(C++17 中弃用)(C++20 中移除)|返回最大的受支持分配大小(公开成员函数)|
|construct(C++17 中弃用)(C++20 中移除)|在分配的存储构造对象(公开成员函数)|
|destroy|(C++17 中弃用)(C++20 中移除)析构在已分配存储中的对象(公开成员函数)|

下面是一个`std::allocator`的简单实现
注意这里的`size_t`与操作系统相关，32位是4字节，64位时8字节；`ptrdiff_t`是两个指针相减的结果类型，是一种有符号整数类型。减法运算的值为两个指针在内存中的距离。ptrdiff_t是signed类型，用于存放同一数组中两个指针之间的差距，它可以使负数，std::ptrdiff_t.同上，使用ptrdiff_t来得到独立于平台的地址差值.

```c++
#include <new.h>
#include <stddef.h>
#include <limits.h>
#include <iostream.h>
#include <algobase.h>

template <class T>
inline T* allocate(ptrdiff_t size,T*)
{
    //设置新句柄

    set_new_handler(0);
    T* tmp=(T*)(::operator new((size_t)(size* sizeof(T))));
    if(tmp==0)
    {
        cerr<<"out of memory"<<endl;
        exit(1);
    }
    return tmp;
}
template <class T>
inline void_deallocate(T* buffer)
{
    ::operator delete(buffer);
}
template <class T1,class T2>
inline void _construct(T1* p,const T2& value)
{
    new(p) T1(value);
}
inline void _destroy(T* ptr)
{
    ptr->~T();
}


//allocator类的基本实现

template <class T>
class allocator
{
public:
    typedef T  value_type;
    typedef T* pointer;
    typedef const T* const_pointer;
    typedef T&      reference;
    typedef const T& const_reference;
    typedef size_t size_type;
    typedef ptrdiff_t size_type;
    //连接 allocator和U

    template <class U>
    struct rebind
    {
        typedef allocator<U> other;
    };
    //获取左值指针

    pointer allocate(size_type n,const void* hint=0)
    {
        //分配内存并初始化为0

        return _allocate((difference_type)n,(pointer)0);
    }
    void deallocate(pointer p,size_type n){_deallocate(p);}

    void address(reference x){return (pointer)&x;}

    const_pointer const_address(const_reference x)
    {
        return (const_pointer)&x;
    }
    size_type max_size() const
    {
        return size_type(UINT_MAX/sizeof(T));
    }


};
```
上面只是简单的allocator实现，真实情况比这个要复杂的多。
SGI标准的空间配置器，是对::operator new和::operator delete做了一层薄薄的封装。

STL allocator将两阶段操作区分开来。内存配置操作由alloc:allocate()负责；内存释放操作由alloc::deallocate()负责；对象构造操作由::construct()负责，对象析构操作由::destroy()负责。

![合作关系图](../img/2019-07-25-13-49-35.png)

#### 2.2.3 构造和析构基本工具： `construct()`和`destory()`

下面是c++1中 stl_construct.h的部分内容

```c++

#ifndef _STL_CONSTRUCT_H
#define _STL_CONSTRUCT_H 1

#include <new>
#include <bits/move.h>
#include <ext/alloc_traits.h>

namespace std _GLIBCXX_VISIBILITY(default)
{
_GLIBCXX_BEGIN_NAMESPACE_VERSION

  /**
   * Constructs an object in existing memory by invoking an allocated
   * object's constructor with an initializer.
   */
//c++11 新添加Construct函数 

#if __cplusplus >= 201103L
  template<typename _T1, typename... _Args>
    inline void
    _Construct(_T1* __p, _Args&&... __args)
    { ::new(static_cast<void*>(__p)) _T1(std::forward<_Args>(__args)...); }
#else
  template<typename _T1, typename _T2>
    inline void
    _Construct(_T1* __p, const _T2& __value)
    {
      // _GLIBCXX_RESOLVE_LIB_DEFECTS
      // 402. wrong new expression in [some_]allocator::construct
      ::new(static_cast<void*>(__p)) _T1(__value);
    }
#endif

  /**
   * Destroy the object pointed to by a pointer type.
   */
    //接受指针，并直接调用指针类的析构函数

  template<typename _Tp>
    inline void
    _Destroy(_Tp* __pointer)
    { __pointer->~_Tp(); }
//模板函数

  template<bool>
    struct _Destroy_aux
    {
      template<typename _ForwardIterator>
        static void
        __destroy(_ForwardIterator __first, _ForwardIterator __last)
    {
      for (; __first != __last; ++__first)
        std::_Destroy(std::__addressof(*__first));
    }
    };
//特例化函数判断对象是否含有默认构造函数，如果是则什么也不做，这样可以提高效率

  template<>
    struct _Destroy_aux<true>
    {
      template<typename _ForwardIterator>
        static void
        __destroy(_ForwardIterator, _ForwardIterator) { }
    };

  /**
   * Destroy a range of objects.  If the value_type of the object has
   * a trivial destructor, the compiler should optimize all of this
   * away, otherwise the objects' destructors must be invoked.
   */

    //销毁一系列对象。如果对象的value_type是一个默认的析构函数，编译器应该优化所有这些，否则必须调用对象的析构函数。

  template<typename _ForwardIterator>
    inline void
    _Destroy(_ForwardIterator __first, _ForwardIterator __last)
    {
        //迭代器别名

      typedef typename iterator_traits<_ForwardIterator>::value_type
                       _Value_type;

      std::_Destroy_aux<__has_trivial_destructor(_Value_type)>::
    __destroy(__first, __last);
    }

  /**
   * Destroy a range of objects using the supplied allocator.  For
   * nondefault allocators we do not optimize away invocation of 
   * destroy() even if _Tp has a trivial destructor.
   */
    //使用支持的allocator销毁一系列对象，如果没有非默认的allocators不会进行销毁函数的初始化，即便，对象含有默认构造函数

  template<typename _ForwardIterator, typename _Allocator>
    void
    _Destroy(_ForwardIterator __first, _ForwardIterator __last,
         _Allocator& __alloc)
    {
      typedef __gnu_cxx::__alloc_traits<_Allocator> __traits;
      for (; __first != __last; ++__first)
    __traits::destroy(__alloc, std::__addressof(*__first));
    }

  template<typename _ForwardIterator, typename _Tp>
    inline void
    _Destroy(_ForwardIterator __first, _ForwardIterator __last,
         allocator<_Tp>&)
    {
      _Destroy(__first, __last);
    }

_GLIBCXX_END_NAMESPACE_VERSION
} // namespace std
#endif

```

上述代码中`_Destroy_aux`主要是用来检测，对象是否有自定义的析构函数，如果有就进行迭代调用。如果没有(使用默认析构函数)直接跳过，避免资源浪费。

![construct和destory示意](../img/2019-07-25-14-45-38.png)

#### 2.2.4 空间的配置与释放，std::alloc

构造空间的配置与释放，由<stl_alloc.h>负责；设计思路如下：

- 向`system heap` 请求空间
- 考虑多线程状态
- 考虑内存不足时的应变措施
- 考虑过多“小行区块”可能造成的内存碎片(fragment)问题。

考虑到小型区块可能造成的内存破碎问题，SGI设计了双层级配置器,第一级配置器直接使用`malloc()`和`free()`；第二级配置器则视情况采用不同的策略。当配置区块超过128byte时，视之为足够大，使用第一级配置器，当小于128byte时，视之为过小，采用复杂的memory pool整理方式。而不再求助于第一级适配器。使用哪一级适配器取决于`__USE_MALLOC`是否被定义。

![第一与第二级适配器](../img/2019-07-25-15-07-37.png)

![包装链接](../img/2019-07-25-15-10-16.png)

#### 2.2.5 一级适配器__malloc_alloc_template剖析

```c++
//一级适配器

template <int inst>

class __malloc_alloc_template
{
private:
    //函数指针，所代表的函数将用来处理内存不足的情况
    //oom: out of memory

    static void *oom_malloc(size_t);
    static void *oom_realloc(void*,size_t);
    static void (*__malloc_alloc_oom_handler)();

public:
    static void *allocate(size_t n)
    {
        //一级配置器必须使用malloc()

        void *result=malloc(n);
        //当无法满足需求时，改用oom_malloc()

        if(0==result) result=oom_malloc(n);
        return result;
    }
    static void deallocate(void *p,size_t)
    {
        //第一级配置器直接使用free()

        free(p);
    }
    //输入旧size和新size

    static void* reallocate(void *p,size_t,size_t new_sz)
    {
        void* result=realloc(p,new_sz);
        if(0==result) result=oom_realloc(p,new_sz);
        return result; 
    }
    //指定自己的out-of-memory handler
    
    static void (* set_malloc_handler(void(*f)()))()
    {
        //获取函数指针

        void (* old)()=__malloc_alloc_oom_handler;
        __malloc_alloc_oom_handler=f;
        return(old); 
    }


};
//下面是用户提供的malloc_alloc函数

template <int inst>
void (*__malloc_alloc_template<inst>::__malloc_alloc_oom_handler)()=0;

template <int inst>
void *__malloc_alloc_template<inst>::oom_malloc(size_t n)
{
    //获取内存分配句柄函数指针

    void (*my_malloc_handler)();
    void *result;
    for(;;)
    {
        //指针指向分配函数

        my_malloc_handler=__malloc_alloc_oom_handler;
        //分配失败抛出异常

        if(0==my_malloc_handler){__THROW_BAD_ALLOC;}
        //调用处理例程，企图释放内存

        (*my_malloc_handler)();
        //再次尝试分配内存

        result=malloc();
        if(result) return(result);
    }
}
//内存调用分配不成功时，进行二次调用

template <int inst>
void * __malloc_alloc_template<inst>::oom_realloc(void *p,size_t n)
{
    void (*my_malloc_handler)();
    void *result;
    for(;;)
    {
        my_malloc_handler=__malloc_alloc_oom_handler;
        if(0==my_malloc_handler){__THROW_BAD_ALLOC;}
        //尝试调用处理例程

        (*my_malloc_handler)();
        //尝试分配内存

        result=realloc(p,n);
        if(result) return(result);
    }
}
//注意,以下参数直接将inst指定为0

typedef __malloc_alloc_template<0> malloc_alloc;

```

#### 2.2.6 第二级适配器 __default_alloc_template 剖析

因为对于操作系统而言，需要块分配一定的内存来存储块的位置信息，因此当内存过小时，单独开辟块反而是得不偿失的。因此在此时，C++使用内存池机制来对数据进行管理。由配置器负责内存的管理和回收，通常SGI配置其会将小额区块的内存需求量上调至8的倍数(例如30B-32B),并唯独16个free-list,各自管理大小分别为8,16,24,3，,40,48,56,64...128 bytes的小额区块。结构如下：
```c++
union obj{
    union obj* free_list_link;
    char client_data[1];
}
```
因为使用了union因此第一个指针直接指向了内存地址([c++ --> union介绍](https://www.cnblogs.com/jeakeven/p/5113508.html))。

![](../img/2019-07-25-16-47-51.png)

### 2.2.7 空间配置函数allocate()

__default_alloc_template拥有配置器的标准接口函数allocate()。此函数首先判断区块大小，大于128bytes就调用第一级配置器，小鱼128bytes就检查对应的free list。如果有可用区块就直接拿来用，没有就将区块的大小上调至8倍数边界，然后调用`refill()`准备为`free list`重新填充空间。

![free list调度](../img/2019-07-25-16-58-32.png)

#### 2.2.8 空间释放函数 deallocate()

对于deallocate()函数首先判断区块大小，大于128bytes就调用第一级配置器，小鱼128bytes就找出对应的free list，将区块回收。

![区块回收](../img/2019-07-25-16-58-32.png)

#### 2.2.9 重新填充 free-list

当内存池中没有可用区块的时候，就调用`refill()`准备为free-list重新分配空间。新的空间将取自内存池，缺省取得20个新节点(新区块)，但万一内存池空间不足，获得的节点数(区块数)可能小于20。

#### 2.2.10 内存池(memory pool)
_参考链接：_ [C++实现内存池](https://blog.csdn.net/u010183728/article/details/81531392);

上文中提及使用内存池中的内存，提供给free-list方便存取内存。