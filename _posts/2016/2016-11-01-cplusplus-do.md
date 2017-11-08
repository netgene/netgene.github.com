---
layout: post
title: c++语言编程实践
date: 2016-11-01 20:20:20
categories:
- c/c++
- 语言与设计
tags:
---

## 面向对象

### 封装、继承、多态

### 函数重载和运算符重载

### 虚函数表（vtbl）和虚表指针（vptr）

当类中有虚函数的时候，编译器会为类插入一个我们看不见的数据并建立一个表。这个表就是虚函数表（vtbl），那个我们看不见的数据就是指向虚函数表的指针——虚表指针（vptr）。虚函数表就是为了保存类中的虚函数的地址。我们可以把虚函数表理解成一个数组，数组中的每个元素存放的就是类中虚函数的地址。当调用虚函数的时候，程序不是像普通函数那样直接跳到函数的代码处，而是先取出vptr即得到虚函数表的地址，根据这个来到虚函数表里，从这个表里取出该函数的地址，最后调用该函数。所以只要不同类的vptr不同，他对应的vtbl就不同，不同的vtbl装着对应类的虚函数地址，这样虚函数就可以完成它的任务了。

## 智能指针

smart_ptr: 智能指针的原理基于一个常见的习语叫做 RAII

std::auto_ptr
它基本上就像是个普通的指针： 通过地址来访问一个动态分配的对象。 std::auto_ptr 之所以被看作是智能指针，是因为它会在析构的时候调用 delete 操作符来自动释放所包含的对象。
scoped_ptr 作用域指针 scoped_array 一个作用域指针不能传递它所包含的对象的所有权到另一个作用域指针。 一旦用一个地址来初始化，这个动态分配的对象将在析构阶段释放。保证指针的绝对安全。
shared_ptr 共享指针 shared_array 智能指针 boost::shared_ptr 基本上类似于 boost::scoped_ptr。 关键不同之处在于 boost::shared_ptr 不一定要独占一个对象。 它可以和其他 boost::shared_ptr 类型的智能指针共享所有权。 在这种情况下，当引用对象的最后一个智能指针销毁后，对象才会被释放。

因为所有权可以在 boost::shared_ptr 之间共享，任何一个共享指针都可以被复制，这跟 boost::scoped_ptr 是不同的。 这样就可以在标准容器里存储智能指针了——你不能在标准容器中存储 std::auto_ptr，因为它们在拷贝的时候传递了所有权。

weak_ptr 弱指针 boost::weak_ptr 必定总是通过 boost::shared_ptr 来初始化的。一旦初始化之后，它基本上只提供一个有用的方法: lock()。此方法返回的boost::shared_ptr 与用来初始化弱指针的共享指针共享所有权。 如果这个共享指针不含有任何对象，返回的共享指针也将是空的。

## rvalue右值引用 && 语义转移 std::move

右值引用的意义通常解释为两大作用：移动语义和完美转发。
C++中右值可以被赋值给左值或者绑定到引用。类的右值是一个临时对象，如果没有被绑定到引用，在表达式结束时就会被废弃。于是我们可以在右值被废弃之前，移走它的资源进行废物利用，从而避免无意义的复制。被移走资源的右值在废弃时已经成为空壳，析构的开销也会降低。
右值中的数据可以被安全移走这一特性使得右值被用来表达移动语义。
没有右值引用的时候，为避免临时对象的多余复制，和实现函数的完美转发，使了很多古怪手段，当有了右值引用，解决相同的问题，已经简单很多了。
一句话答案：右值引用的出现是为了实现移动语义，顺便解决完美转发的问题，其意义在于扩充了值语义，帮助Modern C++可以全面地应用RAII。原因即在于C++独有的值语义：程序员通过值语义可以方便直观地控制对象生命期，让RAII用起来更自然。

## RAII资源获取即初始化

## 类型转换

类型转换有c风格的，当然还有c++风格的。c风格的转换的格式很简单（TYPE）EXPRESSION，但是c风格的类型转换有不少的缺点，有的时候用c风格的转换是不合适的，因为它可以在任意类型之间转换，比如你可以把一个指向const对象的指针转换成指向非const对象的指针，把一个指向基类对象的指针转换成指向一个派生类对象的指针，这两种转换之间的差别是巨大的，但是传统的c语言风格的类型转换没有区分这些。还有一个缺点就是，c风格的转换不容易查找，他由一个括号加上一个标识符组成，而这样的东西在c++程序里一大堆。所以c++为了克服这些缺点，引进了4新的类型转换操作符，他们是1.static_cast  2.const_cast  3.dynamic_cast  4.reinterpret_cast.

1.static_cast

最常用的类型转换符，在正常状况下的类型转换，如把int转换为float，如：int i；float f； f=（float）i；或者f=static_cast<float>(i);

2.const_cast

用于取出const属性，把const类型的指针变为非const类型的指针，如：const int *fun(int x,int y){}　　int *ptr=const_cast<int *>(fun(2.3))

3.dynamic_cast

该操作符用于运行时检查该转换是否类型安全，但只在多态类型时合法，即该类至少具有一个虚拟方法。dynamic_cast与static_cast具有相同的基本语法，dynamic_cast主要用于类层次间的上行转换和下行转换，还可以用于类之间的交叉转换。在类层次间进行上行转换时，dynamic_cast和static_cast的效果是一样的；在进行下行转换时，dynamic_cast具有类型检查的功能，比static_cast更安全。如：

{% highlight c++ %}
class C

{
　　//…C没有虚拟函数
}；
class T{
　　//…
}
int main()
{
　　dynamic_cast<T*> (new C);//错误
}
此时如改为以下则是合法的：
class C

{
public:
　　virtual void m() {};// C现在是 多态
}
{% endhighlight %}

4.reinterpret_cast

interpret是解释的意思，reinterpret即为重新解释，此标识符的意思即为数据的二进制形式重新解释，但是不改变其值。如：int i; char *ptr="hello freind!"; i=reinterpret_cast<int>(ptr);这个转换方式很少使用。

## 模板泛型

## 相关

- [c++11特性](http://junwang.me/c/c++/cpp11.html)
- [c++14特性](http://junwang.me/c/c++/cpp14.html)
- [c++17特性](http://junwang.me/c/c++/cpp17.html)
- [c++ TR1 & TR2 with boost](http://junwang.me/c/c++/cpp-tr1.html)
- [boost库](http://junwang.me/c/c++/boost.html)
- [c/c++语法糖](http://junwang.me/c/c++/cplusplus-syntax-sugar.html)
- [stl库](http://junwang.me/c/c++/stl.html)