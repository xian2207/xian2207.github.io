---
layout:     post
title:      UNIX网络编程 学习笔记 (三)
subtitle:   UNIX网络编程 学习笔记 (三) 
date:       2019-12-01
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
- [《UNIX网络编程卷一》读书笔记](https://zevvez.github.io/2017/12/13/unp/#%E7%AC%AC%E4%B8%80%E7%AB%A0-%E7%AE%80%E4%BB%8B)

> 2019-12-01 22:10:53

## 第 9 章 基本SCTP套接字编程

略

## 第 10 章 SCTP客户/服务器程序例子

略

### 第 11 章 名字与地址转换(DNS)

域名系统(Domain Name System, DNS):主要用于主机名字和IP地址之间的映射。主要使用`gethostbyname`和`gethostbyaddr`在主句名字与IPv4地址之间进行转换。`getservbyname`和`getservbyport`在服务器名字和端口号之间进行转换。

DNS中的条目称为资源记录(resource record)；相关的资源记录方式如下：

- `A`记录: 把一个主机名映射为一个32位IPv4地址
- `AAAA`记录: 把一个主机名映射为一个128位IPv6地址
- `PTR`记录: 称为指针记录(pointer record)把IP地址映射为主机名
  - 对于IPv4,将32位地址的4个字节先反转顺序,每个字节都转为换各自的十进制ASCII值后,在添加in-addr.arpa,结果字符串用于PTR查询
  - 对于IPv6, 128位地址的32个四位组先反转顺序,每个四位组都转换成相应的十六进制的ASCII值后在添加ip6.arpa
- `MX`记录: 把一个主机指定作为给定主机的“邮件交换器”
- `CNAME`: 代表”canonical name”(规范名字),常见的用法是为常用的服务指派CNAME记录

#### 11.2.2 解析器(resolver)和名字服务器(name server)

- 应用程序通过调用解析器函数库中的函数(gethostbyname,gethostbyaddr)接触DNS服务器。
- 解析器函数库中的代码通过读取系统相关配置文件(/etc/resolv.conf)确定本地name server的地址。
- 解析器使用UDP向本地name server发出查询，如果本地name server不知道答案，解析器就会使用UDP向整个因特网查询其他的名字服务器；如果答案太长超过了UDP的承载能力，自动切换到TCP。
- resolver可以看成DNS的客户端；nameserver可以看成DNS的服务器。

![客户、解析其和名字服务器的典型关系](https://wangpengcheng.github.io/img/2019-12-02-10-03-17.png)

解析器使用UDP向本地名字服务器发出查询，如果本地查找不到，就会使用UDP在整个Internet网上查询其他域名服务器。如果答案太长，超出了UDP的消息的承载能力，本地名字服务器和解析器会自动切换到TCP

#### 11.2.3 DNS 替代方法

- 静态主机文件(/etc/hosts)
- 网络信息系统(NIS)
- 轻量级目录访问协议(LDAP)

### 11.3 gethostbyname 函数

这个函数只能返回IPv4地址，在新的程序中应该使用getaddrinfo；
- 函数定义: `struct hostent * gethostbyname(const char * hostname);`
- 返回：成功则返回非空指针, 失败则NULL且设置h_errno
- 注意此函数失败之后并不会设置errno变量,而是将全局整数变量h_errno设置为在头文件<netdb.h>中定义的常值之一:
  - `HOST_NOT_FOUND`
  - `TRY_AGAIN`
  - `NO_RECOVERY`
  - `NO_DATA`(等同于`NO_ADDRESS`)
    - 此错误表示所指向的名字有效,但是没有`A`记录

hostnet关键结构体如下：
```c
struct hostent{
    char    *h_name;        /* 主机的官方名称 */
    char    **h_aliases;    /* 主机别名的指针数组 */
    int     h_addrtype;     /* 主机IP地址的类型 */
    int     h_length;       /* IPv4的地址长度 */
    char**  h_addr_list;    /* 指向IPv4的地址指针 */
}
```
![hostnet包含的结构体信息](https://img-blog.csdnimg.cn/20191026141722883.png#pic_center)

例子：
```c++
#include "unp.h"

int main(int argc, char const *argv[]) {
    char * ptr, ** pptr;
    char str[INET_ADDRSTRLEN];
    /* 定义返回的结构体 */
    struct hostent * hptr;

    while(--argc>0){
        /* 获取输入参数 */
        ptr = * ++argv;
        /* 查找对应的ip地址 */
        if((hptr = gethostbyname(ptr)) == NULL) {
            err_msg("gethostbyname error for host :%s : %s", ptr, hstrerror(h_errno));
                continue;
        }
        printf("official hostname: %s\n", hptr->h_name);
        /* 遍历输出所有别名 */
        for(pptr = hptr->h_aliases; *pptr != NULL; pptr++){
            printf("\talias: %s\n", *pptr);
        }
        /* 选择地址类型并输出 */
        switch (hptr->h_addrtype) {
            case AF_INET:
                pptr = hptr->h_addr_list;
                for (; *pptr != NULL; pptr++){
                    printf("\taddress : %s\n",
                    Inet_ntop(hptr->h_addrtype, * pptr, str, sizeof(str)));
                    break;
                }
            default:
                err_ret("unknown address type");
                break;
        }
    }
    return 0;
}
/* RUN
./hostent music.163.com
official hostname: music.163.com
        address : 59.111.160.197
        address : 59.111.160.195
        address : 223.252.199.66
*/

```

### 11.4 gethostbyaddr函数

gethostbyaddr函数试图由一个二进制的IP地址找到相应的主机名，与 gethostbyname 的行为刚好相反。(**注意：在新的程序中应该使用`getnameinfo`**)
注：此处与原书中的 gethostbyaddr函数不同，原书中的函数在库<netdb.h>，且addr参数为 const char *
```c
#include <sys/socket.h>
struct hostent * gethostbyaddr(const void * addr,socklen_t len,int type);
//if success return not NULL pointer,if error return NULL and set h_errno
```

addr 参数是一个指向IPV4地址的某个in_addr结构的指针；len参数是这个结构的大小；family参数为`AF_INET`或`AF_INET6`

按照DNS的说法，`gethostbyaddr`在`in_addr.arpa`域中向一个名字服务器查询`PTR`记录。

### 11.5 getservbyname和getservbyport函数
#### 11.5.1 getservbyname

getservbyname函数用于根据给定的名字查找相应的服务。一般服务器对应的IP地址保存在`/etc/services`中;典型内容如下：

```sh
[breap@breap ~]$ grep -e ^ftp -e ^domain /etc/services
ftp-data        20/tcp
ftp-data        20/udp
ftp             21/tcp
ftp             21/udp          fsp fspd
domain          53/tcp                          # name-domain server
domain          53/udp
ftp-data        20/sctp                 # FTP
.......
```

函数的定义和声明如下：

```c
#include <netdb.h>
struct servent * getservbyname(const char * servname,const char * protoname);
//if success return not NULL pointer,if error return NULL
struct servent
{
    char * s_name; //Official services name
    char ** s_aliases; //Alias list
    int s_port; //Port number,Network byte order
    char * s_proto; //Protocol to use
}
```
参数说明：
- `servname`:服务器参数名称；必须指定
- `protoname`:协议名称，可选参数，非空时，必须有匹配的协议；有些因特网服务既用 TCP 也用 UDP 提供，其他英特网则仅仅支持单个协议。
    - 如果`protoname`未指定而`servname`指定服务支持多个协议，那么返回哪个端口号取决于实现。
        - 通常情况下这种选择无关紧要，因为支持多个协议的服务往往使用相同的`TCP`端口号和`UDP`端口号，不过这点并无保证。
    - `servent`结构中我们关心的主要字段是端口号(网络字节序)
        - 既然端口号是以网络字节序返回的，把它存放到套接字地址结构时绝对不能调用`htons`。

函数调用示例：
```c
struct servent *sptr;
sptr=getservbyname("domain","udp"); //DNS using UDP
sptr=getservbyname("ftp","tcp"); //FTP using TCP
sptr=getservbyname("ftp",NULL); //FTP using TCP
sptr=getservbyname("ftp","udp"); //this call will fail
```
#### 11.5.2 getservbyport

getservbyport 函数用于给定端口号和可选协议查询相应服务.函数的定义如下：

```c
#include <netdb.h>
struct servent * getservbyport(int port,const char * protoname);
//if success return not NULL pointer,if error return NULL
```
`port`参数的值必须为**网络字节序**。该函数的典型调用如下：
```c
struct servent *sptr;
sptr=getservbyport(htons(53),"udp"); //DNS using UDP
sptr=getservbyport(htons(21),"tcp"); //FTP using TCP
sptr=getservbyport(htons(21),NULL); // FTP using TCP
sptr=getservbyport(htons(21),"udp"); //this call will fail
```

注意：
- **某些端口号在TCP上是一种服务，在UDP上却用于完全不同的另一种服务。**
- **端口可以复用,只要协议不同**
例如：
```sh
[breap@breap ~]$ grep 514 /etc/services
shell           514/tcp         cmd             # no passwords used
syslog          514/udp
```


使用示例如下：
```c++
#include "unp.h"

int main(int argc, char const *argv[]) {
    int sockfd, n;
    char recvline[MAXLINE + 1];
    struct sockaddr_in servaddr;
    struct in_addr ** pptr;
    struct in_addr * inetaddrp[2];
    struct in_addr inetaddr;
    struct hostent * hp;
    struct servent * sp;

    if(argc!=3){
        err_quit("usage: daytimetcpcli1 <hostname> <services>");
    }
    if((hp = gethostbyname(argv[1])) == NULL) {
        /* 确定是否为正确的ASCII地址格式 */
        if (inet_aton(argv[1], &inetaddr)==0){
            err_quit("hostname error for %s : %s", argv[1], hstrerror(h_errno));
        }else{
            inetaddrp[0] = &inetaddr;
            inetaddrp[1] = NULL;
            pptr = inetaddrp;
        }
    }else{
        pptr = (struct in_addr ** )hp->h_addr_list;
    }
    /* 获取服务 */
    if((sp = getservbyname(argv[2], "tcp")) == NULL){
        err_quit("getservbyname error for %s : %s", argv[2]);
    }
    for(; *pptr != NULL; pptr++) {
        /* 建立TCP连接 */
        sockfd = Socket(AF_INET, SOCK_STREAM, 0);
        bzero(&servaddr, sizeof(servaddr));
        servaddr.sin_family = AF_INET;
        servaddr.sin_port = sp->s_port;
        memcpy(&servaddr.sin_addr, * pptr, sizeof(struct in_addr));
        printf("trying %s \n", Sock_ntop((SA *)&servaddr, sizeof(servaddr)));
        /* 进行连接 */
        if (connect(sockfd, (SA *)&servaddr, sizeof(servaddr)) == 0){
            break;
        }
        err_ret("connect error");
        close(sockfd);
    }

    if (*pptr == NULL){
        err_quit("unable to connect");
    }
    /* 执行read */
    while((n = Read(sockfd, recvline, MAXLINE))>0){
        recvline[n] = 0; // null terminate
        Fputs(recvline, stdout);
    }
    return 0;
}
/*
[breap@breap name_test]$ ./daytimetcpcli1 127.0.0.1 daytime
trying 127.0.0.1:13
Tue Jan 16 20:06:35 2018
*/

```

### 11.6 getaddrinfo

这个函数能够处理名字到地址以及服务器到端口这两种转换。返回一个sockaddr结构而不是一个地址列表。该函数在POSIX规范中定义如下：
```c
#include <netdb.h>
int getaddrinfo(const char * hostname,const char * service,const struct addrinfo *hints,struct addrinfo **result);
//if success return 0,if error return not 0;
```
参数解析：
- hostname:主句名或者地址串(IPv4的点分十进制数串或IPv6的十六进制数串)
- service:服务名或者十进制端口号数串
- hints:期望返回的信息类型的暗示，可以为空指针，也可以指向某个addrinfo结构指针。
- result:返回结果；由result参数指向的变量已被填入一个指针，它指向的是由其中的ai_next成员串接起来的addrinfo结构链表(多个addrinfo结构返回时，这些结构的先后顺序没有保证 )。
  - 可导致返回多个`addrinfo`


