---
layout: post
title: interview
date: 2013-02-02 20:20:20
categories:
- c/c++
tags:
---

---

#### trancate delete区别

truncate是DDL語言.delete是DML語言 DDL語言是自動提交的.命令完成就不可回滾.truncate的速度也比delete要快得多.  
runcate,drop是ddl, 操作立即生效,原数据不放到rollbacksegment中,不能回滚. 操作不触发trigger.   

trancate清空表数据，不可恢复。  
delete from 清空表数据，需commit，可回滚。  

#### recv接收数据 返回值

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

