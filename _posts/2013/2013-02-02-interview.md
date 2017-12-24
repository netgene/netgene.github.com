---
layout: post
title: interview
date: 2013-02-02 20:20:20
categories:
- c/c++
tags:
---


### uc

#### 类对象中的内存对齐  结构体的内存对齐

一个Class对象需要占用多大的内存空间。最权威的结论是：

- 非静态成员变量总合。（not static)
- 加上编译器为了CPU计算，作出的数据对齐处理。（c语言中面试中经常会碰到内存对齐的问题)
- 加上为了支持虚函数(virtual function)，产生的额外负担。

1、空类、单一继承的空类、多重继承的空类所占空间大小为：1（字节，下同）；
2、一个类中，虚函数本身、成员函数（包括静态与非静态）和静态数据成员都是不占用类对象的存储空间的；
3、因此一个对象的大小≥所有非静态成员大小的总和；
4、当类中声明了虚函数（不管是1个还是多个），那么在实例化对象时，编译器会自动在对象里安插一个指针vPtr指向虚函数表VTable；
5、虚继承的情况：由于涉及到虚函数表和虚基表，会同时增加一个（多重虚继承下对应多个）vfPtr(virtual function table)指针指向虚函数表vfTable和一个vbPtr(virtual base pointer)指针指向虚基表vbTable，这两者所占的空间大小为：8（或8乘以多继承时父类的个数）；
6、在考虑以上内容所占空间的大小时，还要注意编译器下的“补齐”padding的影响，即编译器会插入多余的字节补齐；(请参考《c和指针》)
7、类对象的大小=各非静态数据成员（包括父类的非静态数据成员但都不包括所有的成员函数）的总和+ vfptr指针(多继承下可能不止一个)+vbptr指针(多继承下可能不止一个)+编译器额外增加的字节。

#### 虚函数 vtable 多态

函数体是内存中的一个代码段，函数名就表示该代码段的首地址，函数执行时就从这里开始。说得简单一点，就是必须要知道函数的入口地址，才能成功调用函数。

如果一个类包含了虚函数，那么在创建对象时会额外增加一张表，表中的每一项都是虚函数的入口地址。这张表就是虚函数表，也称为 vtable。 可以认为虚函数表是一个数组。 为了把对象和虚函数表关联起来，编译器会在对象中安插一个指针，指向虚函数表的起始位置。

对于单继承，不管继承层次有多深，只需要增加一个指针即可，不会随着继承层次的加深让对象背负越来越多的指针。

虚拟继承是为了解决多重继承下公共基类的多份拷贝问题。当派生类有多重继承时，虚函数表的结构会变得复杂，尤其是有虚继承时，还会增加虚基类表。

[https://www.cnblogs.com/fanzhidongyzby/archive/2013/01/14/2859064.html](https://www.cnblogs.com/fanzhidongyzby/archive/2013/01/14/2859064.html)  

#### 字符串左移三位

暴力移位、旋转法。

[http://junwang.me/c/c++/string.html](http://junwang.me/c/c++/string.html)  

---

### dj

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


---

### zjcc

#### 内存对齐

则1：结构体中第一个成员的偏移量是0，以后每个成员的位置是x的倍数；
           x = min(#pragma pack(), 该成员自身的长度)
规则2：成员对齐后，结构体自身也要对齐，按照y的倍数进行；
          y = min(#pragma pack(), 最大成员尺寸)。
其中#pragma pack() 代表编译器默认以多少字节进行对齐，通常情况下是8Byte。

[http://blog.csdn.net/vonzhoufz/article/details/32131801](http://blog.csdn.net/vonzhoufz/article/details/32131801)  

#### ++i i++
```c++
printf("%d %d", i++, i++); //error
```

#### malloc 分配

```c++
void getm(char **p, int num)
{
	*p = (char*)malloc(sizeof(char)*num);
}

char* getm1(int num)
{
	char *s = (char*)malloc(sizeof(char)*num);
	return s;
}
int main()
{
	char *str = NULL;
	getm(&str, 100);
	strcpy(str, "hello");
	printf("%s\n",str);

	free(str);
	str=NULL;
	
	char *str1 = NULL;
	str1=getm1(100);
	strcpy(str1, "hi");
	printf("%s\n",str1);
	free(str1);
	str1=NULL;
}
```

#### 实现string类
```c++
/////////////////////////////////////////////////////////////  
//  
// 自定义字符串类MyString  
//  
/////////////////////////////////////////////////////////  
////////////////////  
#include <iostream>  
#include <cstring>  
#include <malloc.h>  
using namespace std ;  
  
// 声明  
class MyString ;  
  
// 声明  
ostream& operator << ( ostream& os , const MyString& str ) ;  
istream& operator >> ( istream& is , const MyString& str ) ;  
  
// define of class  
class MyString  
{  
public:  
    // 默认构造函数  
    MyString( char* str = "" ) ;  
  
    // 拷贝构造函数  
    MyString( const MyString& str ) ;  
  
    // 重载赋值运算符  
    MyString& operator = ( const MyString& str ) ;  
  
    // 重载输入运算符  
    friend istream& operator >> ( istream& is , const MyString& str ) ;  
  
    // 重载输出运算符  
    friend ostream& operator << ( ostream& os , const MyString& str ) ;  
  
    // 重载等于运算符  
    bool operator == ( const MyString& ) ;  
  
    // 返回字符串的长度  
    int length() const { return strlen( ch )  ; }  
  
    // 重载+运算符  
    MyString& operator + ( const MyString& str ) ;  
  
    // 在字符串中查找某个字符  
    char* findChar( char* start , char* last , char ch ) ;  
  
public:  
    char ch[1000] ;  
} ;  
  
// implement of class   
  
// 默认构造函数  
MyString::MyString( char* str )   
{  
    int i = 0 ;  
    while( str[i] != '\0' )  
    {  
        ch[i] = str[i] ;  
        i++ ;  
    }  
    ch[i] = '\0' ;  
}  
  
// 拷贝构造函数  
MyString::MyString( const MyString& str )  
{  
    int i = 0 ;  
    while( str.ch[i] != '\0' )  
    {  
        ch[i] = str.ch[i] ;  
        i++ ;  
    }  
    ch[i] = '\0' ;  
}  
  
// 重载赋值运算符  
MyString& MyString::operator = ( const MyString& str )  
{  
    if( &str != this )  
    {  
        int i = 0 ;  
        while( str.ch[i] != '\0' )  
        {  
            ch[i] = str.ch[i] ;  
            i++ ;  
        }  
        ch[i] = '\0' ;  
    }  
    return *this ;  
}  
  
// 重载输出运算符  
ostream& operator << ( ostream& os , const MyString& str )  
{  
    for( int i = 0 ; i < strlen( str.ch ) ; i++ )  
    {  
        os << str.ch[i] ;   
    }  
  
    return os ;  
}  
  
// 重载输入运算符  
istream& operator >> ( istream& is , MyString& str )  
{  
    char c ;  
    int i = 0 ;  
    while( 1 )  
    {  
        is >> noskipws ;  
        is >> c ;  
        if( c == '\n' )  
        {  
            break ;  
        }  
        else  
        {  
            str.ch[i] = c ;  
            i++ ;  
        }  
    }  
    str.ch[i] = '\0' ;  
  
    return is ;  
}  
  
// 重载等于运算符  
bool MyString::operator == ( const MyString& str )  
{  
    bool is_OK = true ;  
  
    if( strlen( this->ch ) == strlen( str.ch ) )  
    {  
        for( int i = 0 ; i < strlen( ch ) ; i++ )  
        {  
            if( ch[i] != str.ch[i] )  
            {  
                is_OK = false ;  
            }  
        }  
    }  
    else  
    {  
        return false ;  
    }  
    return is_OK ;  
}  
  
// 重载+运算符  
MyString& MyString::operator + ( const MyString& str )  
{  
    int size = this->length() + str.length() ;  
  
    char* s = ( char* )malloc( sizeof( char ) * size ) ;  
  
    for( int i = 0 ; i < this->length() ; i++ )  
    {  
        s[i] = this->ch[i] ;  
    }  
  
    for( int j = 0 ; j < str.length() ; j++ )  
    {  
        s[i+j] = str.ch[j] ;  
    }  
  
    s[i+j] = '\0' ;   // 字符串结尾,必须要注意  
  
    return MyString( s ) ;  
}  
  
// 返回指向要查找的字符的指针，找不到返回NULL  
char* MyString::findChar( char* first , char* last , char ch )   
{  
    while( first != last && *first != ch )  
    {  
        ++first ;  
    }  
    if( first == last )  
    {  
        return NULL ;  
    }  
    else  
    {  
        return first ;  
    }  
}  
  
//测试程序  
int main()  
{  
    MyString  str( "hello World!" ) ;  
    cout << "str=" << str << endl ;  
    cout << &str.ch ;  
  
    cout << endl << endl  ;  
  
    MyString str1( str ) ;  
    cout << "str1= " << str1 << endl ;  
    cout << &str1.ch ;  
  
    cout << endl << endl  ;  
  
  
    MyString str2 ;  
    str2 = str ;  
    cout << "str2=" << str2 << endl ;  
    cout << &str2.ch ;  
  
    cout << endl << endl  ;  
  
  
    MyString str3 , str4 ;  
    cin >> str3 >> str4 ;  
    if( str3 == str4 )  
    {  
        cout << "str3 == str4 " << endl ;  
    }  
    else  
    {  
        cout << "str3 != str4 " << endl ;  
    }  
  
    MyString str5 = str3 + str4 ;  
  
    cout << "str5 = " << str5 << endl ;  
  
    char ch ;  
    cout << "输入要查找的字符:" ;  
    cin >> ch ;  
  
    char* pChar = str5.findChar( str5.ch , str5.ch + str5.length() , ch ) ;  
  
    if( pChar != NULL )  
    {  
        cout << "查找到的字符为:" << *pChar << endl ;  
    }  
    else  
    {  
        cout << "未找到!" << endl ;  
    }  
  
    return 0 ;  
}  
```

#### 数据库索引 数据库优化

数据库索引，是数据库管理系统中一个排序的数据结构，以协助快速查询、更新数据库表中数据。索引的实现通常使用B树及其变种B+树。
在数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据，这样就可以在这些数据结构上实现高级查找算法。这种数据结构，就是索引

创建索引可以大大提高系统的性能。
第一，通过创建唯一性索引，可以保证数据库表中每一行数据的唯一性。
第二，可以大大加快数据的检索速度，这也是创建索引的最主要的原因。
第三，可以加速表和表之间的连接，特别是在实现数据的参考完整性方面特别有意义。
第四，在使用分组和排序子句进行数据检索时，同样可以显著减少查询中分组和排序的时间。
第五，通过使用索引，可以在查询的过程中，使用优化隐藏器，提高系统的性能。 

也许会有人要问：增加索引有如此多的优点，为什么不对表中的每一个列创建一个索引呢？因为，增加索引也有许多不利的方面。
第一，创建索引和维护索引要耗费时间，这种时间随着数据量的增加而增加。
第二，索引需要占物理空间，除了数据表占数据空间之外，每一个索引还要占一定的物理空间，如果要建立聚簇索引，那么需要的空间就会更大。
第三，当对表中的数据进行增加、删除和修改的时候，索引也要动态的维护，这样就降低了数据的维护速度。

#### 线程安全 可重入函数

线性安全：一般来说，一个函数被称为线程安全的，当且仅当被多个并发线程反复调用时，它会一直产生正确的结果。

重入：即重复调用，函数被不同的流调用，有可能会出现第一次调用还没返回时就再次进入该函数开始下一次调用。
可重入：当程序被多个线程反复执行，产生的结果正确。
如果一个函数只访问自己的局部变量或参数，称为可重入函数。
不可重入：当程序被多个线程反复调用，产生的结果出错。
当函数访问一个全局的变量或者参数时，有可能因为重入而造成混乱，像这样的函数称为不可重入函数。

可重入特点
由于可重入函数多次调用不会出错，因此可重入函数不用担心数据会被破坏。可重入函数任何时候都可以被中断，一段时间后又可以运行，而相应的数据不会丢失。可重入函数只使用局部变量，即保存在CPU寄存器或者堆栈中；或者如果使用全局变量时，则要对全局变量予以保护。
•不可重入特点
如果一个函数符合以下条件之一的，则是不可重入的：
（1）调用了malloc/free函数，因为malloc函数是用全局链表来管理堆的。
（2）调用了标准I/O库函数，标准I/O库的很多实现都以不可重入的方式使用全局数据结构。
（3）可重入体内使用了静态的数据结构。

可重入函数与线程安全的区别与联系：  
（1）线程安全是在多个线程情况下引发的，而可重入函数可以在只有一个线程的情况下来说。
（2）线程安全不一定是可重入的，而可重入函数则一定是线程安全的。
（3）如果一个函数中有全局变量，那么这个函数既不是线程安全也不是可重入的。
（4）如果将对临界资源的访问加上锁，则这个函数是线程安全的，但如果这个重入函数若锁还未释放则会产生死锁，因此是不可重入的。
（5）线程安全函数能够使不同的线程访问同一块地址空间，而可重入函数要求不同的执行流对数据的操作互不影响使结果是相同的。

线程安全问题都是由全局变量和静态变量引起的

可重入函数是线程安全函数的子集

可重入的要求：不使用、不返回任何非常量的全局或者静态变量，也不调用任何不可重入函数。它可以被中断，意味着它除了使用自己栈上的变量不依赖于任何环境（包括static），这样的函数就是purecode（纯代码）可重入。可以允许有多个函数的副本在同时运行，因为它们使用的是分离的栈，不会互相干扰。但是对于不可重入的函数，由于使用了一些系统资源比如全局变量，中断向量表等，如果它被中断的话，可能会出现问题，这类函数是不能运行在多任务环境下的。

线程安全的要求是：在多个线程中同时调用一个函数时，能够得到预期的结果，多个线程的切换不会导致该接口的执行结果产生二义性。线程安全函数在执行时多个线程可以互相影响，即使输入一样输出也可能因为中途的其他线程的行为而不一样。所以函数的返回值不具有可再现性。所以也就不一定是可重入函数。

1、如果一个函数的实现使用了全局或者静态变量，那么这个函数既不是可重入的也不是线程安全的。
2、但是针对1中的情况，如果放宽条件，这个函数仍然用到了全局或者静态变量，但是在访问这些变量的时候，通过加锁来保证互斥访问，那么这个函数就可以变成线程安全的函数。但他此时仍然是不可重入的，因为通常加锁是针对不同线程的访问，对同一线程可能出现问题。
3、所以如果把函数中的全局或者静态变量都去掉，并保证在该函数中不会调用不可重入函数，那么这个函数就可以做到既是线程安全的，也是可重入的。

所以：可重入函数一般都是线程安全的，线程安全的不一定是可重入的。可重入函数要求更高，线程安全可以用锁，可重入不行（否则死锁）

#### 智能指针 shared_ptr weak_ptr

指针	简要描述
shared_ptr	允许多个指针指向同一个对象
unique_ptr	独占所指向的对象
weak_ptr	shared_ptr的弱引用

weak_ptr是一种不控制所指向对象生存期的智能指针，指向shared_ptr管理的对象，但是不影响shared_ptr的引用计数。它像shared_ptr的助手，一旦最后一个shared_ptr被销毁,对象就被释放，weak_ptr不影响这个过程。

#### 进程间线程通信 进程间线程通信

进程间通信：信号、信号量、消息队列、共享内存、管道、套接字

共享内存( shared memory ) ：共享内存就是映射一段能被其他进程所访问的内存，这段共享内存由一个进程创建，但多个进程都可以访问。共享内存是最快的 IPC 方式，它是针对其他进程间通信方式运行效率低而专门设计的。它往往与其他通信机制，如信号两，配合使用，来实现进程间的同步和通信。

线程间通信：锁、信号量、信号

两个进程间的线程级通信？？？

#### 同步消息和异步消息通信

1、同步方式

两个通信应用服务之间必须要进行同步，两个服务之间必须都是正常运行的。发送程序和接收程序都必须一直处于运行状态，并且随时做好相互通信的准备。

发送程序首先向接收程序发起一个请求，称之为发送消息，发送程序紧接着就会堵塞当前自身的进程，不与其他应用进行任何的通信以及交互，等待接收程序的响应，待发送消息得到接收程序的返回消息之后会继续向下运行，进行下一步的业务处理。

2、异步方式

两个通信应用之间可以不用同时在线等待，任何一方只需各自处理自己的业务，比如发送方发送消息以后不用登录接收方的响应，可以接着处理其他的任务。也就是说发送方和接收方都是相互独立存在的，发送方只管方，接收方只能接收，无须去等待对方的响应。

回调、会话。。。

#### 离线访问 数据库索引 载入光盘

#### 工厂模式  单例模式 懒汉模式 饿汉模式 等级模式

#### 进程、线程、协程

进程拥有自己独立的堆和栈，既不共享堆，亦不共享栈，进程由操作系统调度。 
线程拥有自己独立的栈和共享的堆，共享堆，不共享栈，线程亦由操作系统调度(标准线程是的)。 
协程和线程一样共享堆，不共享栈，协程由程序员在协程的代码里显示调度。 

---

### c++ 面向对象 抽象 封装 继承 多态

- 抽象：找出要解决的问题。
- 封装：数据封装和过程封装。接口与信息隐藏。
- 继承：解决方案的重用性。
- 多态：解决方案的灵活性。

#### 构造函数 析构函数 虚函数 纯虚函数

- 构造函数可以被重载、因为构造函数可以多个。析构函数不可以被重载，只有一个。
- 构造函数先构造基类、再构造子类；析构函数先析构子类，再析构基类。
- 当一个类被用作基类的时候，才会把析构函数写成虚函数，保证子类析构函数能够被调用，避免内存泄露。

#### 复制构造函数 赋值函数

- 深拷贝与浅拷贝 默认的复制构造函数只是简单的把两个对象的指针做赋值运算，指向同一个地址，产生两次析构释放同一块堆内存发生奔溃，属于浅拷贝。如果复制的对象引用了外部对象如堆数据，需要在复制时为外部对象也做独立复制，就是深拷贝。
- 编写继承类的复制函数有一个原则：使用基类的复制构造函数。解决基类有私有成员的赋值问题。
- 赋值函数主要是引用，把一个对象赋值给一个原有对象。如果原来对象中有内存分配，需要把内存释放，还要检查两个对象是否同一对象，是的话不做任何操作。

#### 成员函数 静态成员函数

c++空类默认的成员函数：构造函数、拷贝构造函数、析构函数、赋值运算符、取值运算符。

### c++ 泛型编程 模板 STL

### c++11 c++14 c++17特性

