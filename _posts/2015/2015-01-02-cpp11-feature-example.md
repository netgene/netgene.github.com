---
layout: post
title: c++特性实例
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

### c++11可变参数

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

### 静态编译

```c++
//log.h
#include <iostream>
#include <string>

using namespace std;

namespace pbase {
	class Logger {
	public:
		Logger();
	private:
		string filename;
	};
} //namespace pbase
```

```c++
//log.cpp
#include "log.h"

namespace pbase {
	Logger::Logger() {
		filename = "a.log";
		cout << "log file:" << filename << endl;
	}
} //namespace pbase
```

```c++
//testlog.cpp
#include "log.h"
#include <iostream>

int main()
{
	pbase::Logger l;
	return 0;
}
```

```
# g++ -c log.cpp 
# ar crv libpbase.a log.o
# g++ -o testlog testlog.cpp libpbase.a
# ./testlog
log file:a.log
```