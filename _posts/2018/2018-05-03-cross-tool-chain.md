---
layout: post
title: 交叉工具链,交叉编译器(转)
date: 2018-05-03 20:20:20
categories:
- c/c++
- 工具
tags:
---

[ARM交叉编译工具链 http://www.veryarm.com/cross-tools](http://www.veryarm.com/cross-tools)  
[
交叉编译详解 一 概念篇 https://blog.csdn.net/pengfei240/article/details/52912833](https://blog.csdn.net/pengfei240/article/details/52912833)  

交叉编译通俗地讲就是在一种平台上编译出能运行在体系结构不同的另一种平台上的程序，比如在PC平台（X86 CPU）上编译出能运行在以ARM为内核的CPU平台上的程序，编译得到的程序在X86 CPU平台上是不能运行的，必须放到ARM CPU平台上才能运行，虽然两个平台用的都是Linux系统。

交叉编译工具链是一个由编译器、连接器和解释器组成的综合开发环境，交叉编译工具链主要由binutils、gcc和glibc三个部分组成。有时出于减小 libc 库大小的考虑，也可以用别的 c 库来代替 glibc，例如 uClibc、dietlibc 和 newlib。建立交叉编译工具链是一个相当复杂的过程，如果不想自己经历复杂繁琐的编译过程，网上有一些编译好的可用的交叉编译工具链可以下载，但就以学习为目的来说读者有必要学习自己制作一个交叉编译工具链（目前来看，对于初学者没有太大必要自己交叉编译一个工具链）。