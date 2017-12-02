---
layout: post
title: interview
date: 2013-02-02 20:20:20
categories:
- c/c++
tags:
---

---


---

uc

#### 类对象中的内存对齐

#### 虚函数 vtable 多态

#### 字符串左移三位

---

#### trancate delete区别

truncate是DDL語言.delete是DML語言 DDL語言是自動提交的.命令完成就不可回滾.truncate的速度也比delete要快得多.  
truncate,drop是ddl, 操作立即生效,原数据不放到rollbacksegment中,不能回滚. 操作不触发trigger.   

trancate清空表数据，不可恢复。  
delete from 清空表数据，需commit，可回滚。  

#### recv接收数据 EAGAIN、EWOULDBLOCK、EINTR

EAGAIN、EWOULDBLOCK、EINTR与非阻塞 长连接

EWOULDBLOCK用于非阻塞模式，不需要重新读或者写

EINTR指操作被中断唤醒，需要重新读/写

在Linux环境下开发经常会碰到很多错误(设置errno)，其中EAGAIN是其中比较常见的一个错误(比如用在非阻塞操作中)。
从字面上来看，是提示再试一次。这个错误经常出现在当应用程序进行一些非阻塞(non-blocking)操作(对文件或socket)的时候。例如，以 O_NONBLOCK的标志打开文件/socket/FIFO，如果你连续做read操作而没有数据可读。此时程序不会阻塞起来等待数据准备就绪返 回，read函数会返回一个错误EAGAIN，提示你的应用程序现在没有数据可读请稍后再试。

又例如，当一个系统调用(比如fork)因为没有足够的资源(比如虚拟内存)而执行失败，返回EAGAIN提示其再调用一次(也许下次就能成功)。
Linux - 非阻塞socket编程处理EAGAIN错误

在linux进行非阻塞的socket接收数据时经常出现Resource temporarily unavailable，errno代码为11(EAGAIN)，这是什么意思？
这表明你在非阻塞模式下调用了阻塞操作，在该操作没有完成就返回这个错误，这个错误不会破坏socket的同步，不用管它，下次循环接着recv就可以。 

对非阻塞socket而言，EAGAIN不是一种错误。在VxWorks和Windows上，EAGAIN的名字叫做EWOULDBLOCK。
另外，如果出现EINTR即errno为4，错误描述Interrupted system call，操作也应该继续。

最后，如果recv的返回值为0，那表明连接已经断开，我们的接收操作也应该结束。

linux下EAGAIN、WOULDBLOCK错误码都是11

#### recv分多次接收数据 ?

#### 内连接 inner join

left join(左联接) 返回包括左表中的所有记录和右表中联结字段相等的记录   
right join(右联接) 返回包括右表中的所有记录和左表中联结字段相等的记录  
inner join(等值连接) 只返回两个表中联结字段相等的行  

#### C++：构造函数和析构函数能否为虚函数？

[http://blog.csdn.net/zz460833359/article/details/48394255](http://blog.csdn.net/zz460833359/article/details/48394255)

简单回答是：构造函数不能为虚函数，而析构函数可以且常常是虚函数。  

（1） 构造函数不能为虚函数  
> 让我们来看看大牛C++之父 Bjarne Stroustrup 在《The C++ Programming Language》里是怎么说的：
> To construct an object, a constructor needs the exact type of the object it is to create. Consequently,a constructor cannot be virtual. Furthermore, a constructor is not quite an ordinary function, In particular, it interacts with memory management in ways ordinary member functions don't. Consequently, you cannot have a ponter to a constructor.
> --- From 《The C++ Progamming Language》

然而大牛就是大牛，这段话对一般人来说太难理解了。那下面就试着解释一下为什么：
这就要涉及到C++对象的构造问题了，C++对象在三个地方构建：（1）函数堆栈；（2）自由存储区，或称之为堆；（3）静态存储区。无论在那里构建，其过程都是两步：首先，分配一块内存；其次，调用构造函数。好，问题来了，如果构造函数是虚函数，那么就需要通过vtable来调用，但此时面对一块 raw memeory，到哪里去找 vtable 呢？毕竟，vtable 是在构造函数中才初始化的啊，而不是在其之前。因此构造函数不能为虚函数。
 
（2）析构函数可以是虚函数，且常常如此  
这个就好理解了，因为此时 vtable 已经初始化了；况且我们通常通过基类的指针来销毁对象，如果析构函数不为虚的话，就不能正确识别对象类型，从而不能正确销毁对象。

