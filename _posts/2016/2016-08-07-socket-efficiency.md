---
layout: post
title: socket传输效率
date: 2016-08-07 20:20:20
categories:
- 网络与安全
tags:
- socket
---

#### 传输效率

- 底层协议 tcp/udp kcp  
- MTU 多数ISP提供的拨号网络的标准值为576bytes，原因是Internet上不少路由器也为576bytes。

[kcp协议详解 https://www.cnblogs.com/yuanyifei1/p/6846310.html](https://www.cnblogs.com/yuanyifei1/p/6846310.html)

[可靠 UDP 传输 https://blog.codingnow.com/2016/03/reliable_udp.html](https://blog.codingnow.com/2016/03/reliable_udp.html)


#### 底层协议

- 滑动窗口
- 拥塞控制
- 慢启动
- 重传机制