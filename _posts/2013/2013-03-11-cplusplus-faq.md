---
layout: post
title: c++ faq
date: 2013-03-11 18:28:58
categories:
- c/c++
tags:
---

[https://www.zhihu.com/question/20184857](https://www.zhihu.com/question/20184857)  
[https://www.zhihu.com/question/34574154?sort=created](https://www.zhihu.com/question/34574154?sort=created)  
[https://www.zhihu.com/question/19794858](https://www.zhihu.com/question/19794858)  
[http://www.jizhuomi.com/software/129.html](http://www.jizhuomi.com/software/129.html)

#### c++基本

- OOP 封装、继承、多态
- 虚函数vfunc vtable
- Rule of Three:析构函数、拷贝构造函数和重载赋值函数 
- exception safe
- 指针
- 多态

- 类内存布局 虚继承的时候类内存布局 多继承的时候类的布局
- 内存
	静态数据域、栈空间、堆空间
	对指针的理解
	allocator的实现
	vector内存分配的策略
	智能指针实现原理
	内存泄漏
	RAII
	malloc与new
	vector增长模式
	
	面向对象（灵活应用virtual继承+shared_ptr可以达到java/C#的效果）
	模板（这里分两类，分别为type rich programming和meta programming，区别很大）
	函数式编程（如今有了lambda，配合<algorithm>文件，简直无敌了）
	过程式


#### c++11

#### stl

- allocator类是C++的一个模板，它提供类型化的内存分配以及对象的分配和撤销。

#### linux

- 进程内存空间 线程内存空间
- tcp/ip 状态转移 滑动窗口 慢启动 

#### 数据结构 & 算法