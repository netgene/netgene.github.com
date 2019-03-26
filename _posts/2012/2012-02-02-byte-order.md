---
layout: post
title: 大小端字节序
date: 2012-02-02 18:28:58
categories:
- c/c++
tags:
---

[https://blog.csdn.net/u010406802/article/details/52415992](https://blog.csdn.net/u010406802/article/details/52415992)

C中，大于一个字节的数据类型在内存中表示时，不同CPU有不同的存放顺序，分为大端序和小端序。大端序即高字节存在低地址。

通常在网络间传输数据时，因为是在不同机器上进行交互，所以需要先将数据转换成网络字节序，再发往对端。网络字节序是TCP/IP中定义的数据格式，与具体CPU、OS无关，采用大端表示。

常用的转换函数有：

```c
#include <arpa/inet.h>
uint16_t htons(uint16_t hostshort); /* host to network short */

uint32_t htonl(uint32_t hostlong); /* host to networklong */

uint16_t ntohs(uint16_tnetshort); /*network to host short */

uint32_t ntohs(uint32_t netlong); /*networkto host short */
```

现在如果需要将64位的long long类型数据从本机(小端)转换为网络字节序，就没有现成的API可用了。可以通过下面两种方法进行转换：移位、联合体。