---
layout:     post
title:      UNIX网络编程 学习笔记 (一)
subtitle:   UNIX网络编程 学习笔记 (一) 
date:       2019-11-10
author:     王鹏程
header-img: img/post-bg-ios10.jpg
catalog: true
tags:
    - C/C++
    - UNIX
    - 网络编程
---

# UNIX网络编程 学习笔记
_参考链接：_

- [《Unix网络编程》卷1 初级](https://blog.csdn.net/zzxiaozhao/article/details/102637708)
- [《Unix网络编程》卷1 中级](https://blog.csdn.net/zzxiaozhao/article/details/102662861)

## I/O复用：select和poll函数

进程可能在监听socket时还可能存在I/O的相关操作，会造成socket处理程序的阻塞。因此进程需要一种预先告知内核的能力，使得内核一旦发现进程指定的一个或多个I/O条件就绪;就直接通知进程--I/O复用。其由select和poll两个函数支持。

I/O复用使用场景如下：
- 客户端处理互交式输入和网络socket时，必须使用I/O复用
- 一个客户端，同时处理多个套接字
- TCP服务器既要监听套接字，又要处理已连接套接字。
- 一个服务器，既处理TCP又处理UDP
- 一个服务器处理多个服务器或者多个协议。

### 6.2 I/O模型

UNIX下可用的5种I/O模型：

- 阻塞式I/O:socket默认情况下的模式，进程从用户调用到系统调用，再到读取数据，中间过程是阻塞的；只有完成之后，才会开始处理数据
- 非阻塞式I/O：高速内核在进行I/O请求时，不要将进程投入水米娜，而是返回一个错误--不进入自旋等待，而是直接返回。对于I/O频繁的类型不友好。
![非阻塞式I/O](https://wangpengcheng.github.io/img/2019-11-27-10-56-05.png)
- I/O复用:使用select或者poll，阻塞在这两个系统调用中的某一个智商，而不是阻塞在真正的I/O系统调用上。关键在于不是等待一个的I/O而是多个的I/O;类似于多个线程中使用阻塞式I/O
![I/O复用](https://wangpengcheng.github.io/img/2019-11-27-12-04-24.png)
- 信号驱动式I/O:内核在描述符就绪时，发送SIGIO信号通知我们
![信号驱动式I/O](https://wangpengcheng.github.io/img/2019-11-27-12-09-46.png)
- 异步I/O:定义内核相关操作，操作完成之后，由内核通知我们。信号驱动模式是内核通知，何时启动一个I/O操作。异步I/O是由内核通知我们I/O操作(数据复制)何时完成。
![异步I/O](https://wangpengcheng.github.io/img/2019-11-27-12-17-48.png)

#### 6.2.6 各种I/O模型的比较

![各种I/O模型的比较](https://img-blog.csdnimg.cn/20191023162011132.png#pic_center)

注意：
- 同步I/O(synchronous I/O opetation),导致请求阻塞，直到I/O操作完成。
- 异步I/O(asynchronous I/O opetation),不导致请求进程阻塞

#### 6.2.7 关于I/O复用

I/O复用采用轮询的方式处理多个描述符，当有文件准备好时，就通知进程。关注点如下：

- I/O复用的应用场合
  - 当客户处理多个描述符时（通常是交互式输入和网络套接字），必须使用I/O复用，才能即使告知用户程序套接字的情况
  - 如果一个TCP服务器既要处理监听又要处理连接套接字，一般要用I/O复用
  - 如果既要处理TCP，又要处理UDP，一般要用I/O复用
  - 如果一个服务器要处理多个服务或多个协议如inet守护进程，一般要用I/O复用
- 采用I/O复用的客户端和服务器程序

### 6.3 select函数
_参考链接：_ [linux select函数详解](https://www.cnblogs.com/alantu2018/p/8612722.html)

select集合中的任何描述符准备好读写，他就开始工作。
```c
int select(int maxfdp1,fd_set *readset, fd_set *writeset, fd_set *exceptset, const struct timeval *timeout)
```
参数解释：
- `timeval`:告知内核等待指定描述符中的任何一个就绪需要花多少时间，即最低时间，该时间后必须有返回；
  - 永远等待就设置为空置指针，仅在有描述符准备好I/O时返回。
  - 等待一段固定的时间：在有一个描述符准备号I/O时返回，但是不超过由该参数所指向的timeval结构体中指定的秒数和微秒数。一旦查过又没有符合条件的就返回0。
  - 不等待:定时器设置为0，即为轮询(poll)；不等待，直接返回。加入描述符集的描述符都会被测试，并且返回满足要求的描述符的个数。这种方法通过轮询，无阻塞地获得了多个文件描述符状态。
- `readset`、`writeset`和`exceptset`指定我们要让内核测试读、写和异常条件的描述符。使用它们可以指定select的监听的描述符集合；**一定要初始化，否则将产生不可描述的问题**使用示例如下：
```c
fd_set rset;
/* 初始化所有位--清空集合 */
FD_ZERO(&set);
/* 开启1的位--将1加入文件集合中 */
FD_SET(1,&rset);
/* 开启4的位 */
FD_SET(4,&rset);
/* 开启5的位 */
FD_SET(5,&rset);
/* 将1从集合中清除  */
FD_CLR(14,&rset);
```

计时器到时返回0，-1表示出错

#### 6.3.1 描述符就绪的条件

- 一个套接字准备好读的情况：
  - 接收缓冲区中字节数`>=`接收缓冲区**低水位标记**的当前大小（默认1，由`SO_RCVLOWAT`设置）
  - 读半部关闭(接收了`FIN`)将不阻塞并返回`0`
  - 监听套接字的已连接数不为`0`，这时`accept`通常不阻塞
  - 其上有一个套接字错误待处理，返回-1，`error`设置成具体的错误条件，可通过`SO_ERROR`套接字选项调用`getsockopt`获取并清除
- 一个套接字准备好写：
  - 以连接套接字或`UDP`套接字发送缓冲区中的可用字节数 `>=` 发送缓冲区低水位标记的当前大小(默认`2048`，可用`SO_SNDLOWAT`)
  - 写半部关闭的套接字，写操作将产生一个`SIGPIPE`信号
  - 非阻塞式`connect`的套接字已建立连接，或者`connect`以失败告终
  - 其上有一个套接字错误待处理，返回-1，`error`设置成具体的错误条件，可通过`SO_ERROR`套接字选项调用`getsockopt`获取并清除
  
![相关条件](https://img-blog.csdnimg.cn/20191023185927436.png#pic_center)

混合使用stdio和select被认为是非常容易犯错误的：
- readline缓冲区中可能有不完整的输入行
- 也可能有一个或多个完整的输入行

#### 6.3.2 select的最大描述符
由FD_SETSIZE指定，一般为256，更改时，需要重新编译内核。


### 6.6 shutdown 函数

终止网络的常用方法是调用close函数，close函数的限制可以使用shutdown 来避免close的相关问题：
```c
#include <sys/socket.h>
int shutdown(int sockfd,int howto);
```

- `close()`把描述符的引用计数减1，`shutdown`直接激发TCP的正常连接序列的终止。
- shutdown告诉对方我已近完成了数据的发送(对方仍然可以发给我）;howto相关参数如下：
  - `SHUT_RD`：关闭连接的读这一半
    - 可以把第二个参数置为`SHUT_RD`防止回环复制
    - 关闭`SO_USELOOPBACK`套接字选项也能防止回环
  - `SHUT_WR`：关闭连接的写这一半，也叫半关闭
  - `SHUT_RDWR`：连接的读半部和写半部都关闭

### 6.8 TCP回射服务器程序

使用一个client数组，来记录对应的已经建立连接的客户端的编号。当客户端连接断开口，将其对应的文件描述符从集合中删除，然后将对应的客户端编号设置为-1。

改用了Read，Write，解决这一问题；并且改用了shutdown来关闭连接而不是用close。修复了批量输入的问题

```c
void str_cli(FILE * fp, int sockfd){
  int maxfdp1, stdineof, n;
  fd_set rset;
  char buf[MAXLINE];

  stdineof = 0;
  FD_ZERO(&rset);
  for( ; ; ){
    if(stdineof == 0){ // 是一个初始化为0, 的新标志,当标志位0时,打开标准输入检测
      FD_SET(fileno(fp), &rset);
    }
    /* 将当前的socket描述符，添加到rset中 */
    FD_SET(sockfd, &rset);
    maxfdp1 = max(fileno(fp), sockfd) + 1;
    //使用Select监听函数
    Select(maxfdp1, &rset, NULL, NULL, NULL);
    if(FD_ISSET(sockfd, &rset)){
      if((n = Read(sockfd, buf, MAXLINE)) == 0){
        if(stdineof == 1)
          return ;
        else
          err_quit("str_cli: server terminated prematurely");
      }
      Write(fileno(stdout), buf, n);
    }
    if(FD_ISSET(fileno(fp), &rset)){
      if((n = Read(fileno(fp), buf, MAXLINE)) == 0){ // 表示读入EOF标志符,文件已经空了
        stdineof = 1;// 关闭标准输入 
        Shutdown(sockfd, SHUT_WR);// 关闭socked 的读操作
        FD_CLR(fileno(fp), &rset); // 从检测位中删除 输入输入检测
        continue;
      }
      Writen(sockfd, buf, n);
    }
  }
}
```
下面是使用select的服务端程序

```c
#include "unp.h"
int main(int argc, char const *argv[]) {
    /* 定义对应的文件描述符 */
    int i, maxi, maxfd, listenfd, connfd, sockfd;
    /* 定义客户端连接集合 */
    int nready, client[FD_SETSIZE];
    ssize_t n;
    /* 定义连接客户端集合 */
    fd_set rset, allset;
    /* 定义缓冲区 */
    char buf[MAXLINE];
    /* 定义客户端关键字 */
    socklen_t clilen;
    /* 定义相关地址 */
    struct sockaddr_in cliaddr, servaddr;
    /* 开始进行监听 */
    listenfd = Socket(AF_INET, SOCK_STREAM, 0);
    /* 初始化服务器地址 */
    bzero(&servaddr, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_port = htons(SERV_PORT);
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
    /* 连接监听socket和服务器地址 */
    Bind(listenfd, (SA *)&servaddr, sizeof(servaddr));
    /* 开始监听 */
    Listen(listenfd, LISTENQ);
    maxfd = listenfd;
    maxi = -1;
    /* 初始化client集合 */
    for (i = 0; i < FD_SETSIZE; i++){
        client[i] = -1;
    }
    /* 初始化监听集合 */
    FD_ZERO (&allset);
    /* 添加监听集合 */
    FD_SET(listenfd, &allset);
    for( ; ; ){
        rset = allset;
        /* 指向select */
        nready = Select(maxfd + 1, &rset, NULL, NULL, NULL);
        /* select监听函数 */
        if(FD_ISSET(listenfd, &rset)){
            clilen = sizeof(cliaddr);
            /* 获取连接描述符 */
            connfd = Accept(listenfd, (SA *)&cliaddr, &clilen);
            /* 将其加入队列中 */
            for(i = 0; i < FD_SETSIZE; i++){
                if(client[i] < 0){
                    client[i] = connfd;
                    break;
                }
            }
            /* 超过范围开始警告 */
            if(i == FD_SETSIZE)
                err_quit("too many clients");
            /* 将连接符，添加到监听集合中 */
            FD_SET(connfd, &allset);
            if(connfd > maxfd)
                maxfd= connfd;
            if(i > maxi)
                maxi = i;
            /* 没有准备好直接继续，不进行下面的链接检查和读写操作了 */
            if(--nready <= 0)
                continue;
        }
        /* 遍历所有连接 */
        for(i = 0; i <= maxi; i++){
            /* 获取保存的连接描述符 */
            if((sockfd = client[i]) < 0)
                continue;
            if(FD_ISSET(sockfd, &rset)){
                /* 如果没有读取到，将其清除 */
                if((n = Read(sockfd, buf, MAXLINE)) == 0){
                    Close(sockfd);
                    FD_CLR(sockfd, &allset);
                    client[i] = -1;
                }else{/* 否则写入内容 */
                    
                    Writen(sockfd, buf, n);
                }
                /* 状态错误，直接跳出 */
                if(--nready <= 0)
                break;
            }
        }
    }
}

```
相关的过程如下:

![相关过程](https://img-blog.csdnimg.cn/20191023192300958.png#pic_center)

**拒绝服务攻击：** 当一个服务器在处理多个客户端时，对于单个客户端的函数调用，如果阻塞了，那么可能导致服务器被挂起，拒绝为所有其它客户提供服务。

解决办法：
- 使用非阻塞式I/O
- 让每个客户由单独的控制线程提供服务。
- 对I/O操作设置超时