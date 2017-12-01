---
layout: post
title: c++11特性实例
date: 2015-01-02 20:20:20
categories:
- c/c++
tags:
---

### 命名空间

```c++
#include <iostream>
#include <string>

using namespace std;

namespace pbase {
	class Hello {
	public:
		void print(){cout << "hello" << endl;}
	};
} //namespace pbase

int main()
{
	using namespace pbase;
	Hello h;
	h.print();
}
```

### 可变参数

```c++
#include <iostream>

template<typename T>
void print_args(T t)
{	
	std::cout << t;
}

template<typename T, typename... Rest>
void print_args(T t, Rest... rest)
{	
	print_args(t);	
	print_args(rest...);
	std::cout << std::endl;
}

int main()
{
	print_args("abc:", "123");
}
```