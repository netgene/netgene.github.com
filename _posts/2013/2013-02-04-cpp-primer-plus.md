---
layout: post
title: c++ Primer Plus 笔记
date: 2013-02-02 20:20:20
categories:
- c/c++
tags:
---

#### 命名空间

程序的组织

```c++
namespace abc {

}
```

#### 类型转换

old c: (int)19.9
new c: int(19.9)

4个强制类型运算符，比传统强制类型转换更严格 static_cast<>

#### new delete

```c++
int* i = new int;
delete i;

int* p = new int[10];
delete []p;

```


#### 数组

vector功能比数组强大，若是固定数组使用c++11 array更方便安全。

```c++
array<int, 5> ai = {1, 2, 3, 4, 5};
```

#### 函数和二维数组

```c++
int sum(int a[][4], int size)
{
	int total = 0;
	for(int i = 0; i < size; i++) {
		for(int j = 0; j < 4; j++) {
			total += a[i][j];
		}
	}
	return total;
}
```

#### 函数和array对象

```c++
const int n = 4;
void fill(array<string, n> *p)
{
	for(int i = 0; i < n; i++) {
		cin >> (*p)[i];
	}
}
```

#### 函数指针

#### 函数重载 函数多态

#### 函数模板

#### typedef 类型别名

#### 类  

抽象
封装和数据隐藏
多态
继承
代码的可重用性

#### 类的构造函数和析构函数

#### 拷贝构造函数
#### 虚函数
