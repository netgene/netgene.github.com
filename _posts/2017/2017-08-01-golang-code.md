---
layout: post
title: Go编码方案小集
date: 2017-09-05 20:20:20
categories:
- golang
- 语言与设计
tags:
---

## 服务进程基础结构

### 信号拦截

{% highlight golang %}
func main() {
    logger.Info("program start.PID[", os.Getpid(), "]")

    //signals
    sigs := make(chan os.Signal, 1)
    signal.Notify(sigs, syscall.SIGINT, syscall.SIGTERM)
    go func() {
        sig := <- sigs;
        logger.Info("catch sig:", sig)
        exit = true
    }()

    //mainloop

    //mygod! think goroutines close.
    //if main goroutine exit, all goroutines will exit.
    for !exit {
        time.Sleep(1 * time.Second)
    }

    logger.Info("program exit.")
}
{% endhighlight %}

### 类守护进程

用supervisor来监控。

### goroutine退出思路



## 架构模式

### 生产者与消费者

## 网络编程

### protobuf

根据protobuf消息名，动态实例化protobuf消息。根据 message name 获取到 message type, 然后利用 golang 反射实例化消息结构。

[golang中使用消息名称创建protobuf消息](https://www.cnblogs.com/wertyd/p/7749875.html)

[golang二进制协议接口映射](https://segmentfault.com/a/1190000008471015)

golang种有一个reflect包，可以对类型进行反射，动态生成相应结构体，具体做法就是，将protobuf消息结构通过interface类型和消息id注册到一个自定义map中，在map中保存结构体的类型。然后从底层网络获取到原始二进制协议数据后，通过消息id在map中找到对应的类型信息并动态创建出结构体类型来解析二进制数据。

获取 goprotobuf 提供的 Protobuf 编译器插件 protoc-gen-go（被放置于 $GOPATH/bin 下，$GOPATH/bin 应该被加入 PATH 环境变量，以便 protoc 能够找到 protoc-gen-go）
```
go get github.com/golang/protobuf/protoc-gen-go

cd github.com/golang/protobuf/protoc-gen-go

go build

go install
```

获取 goprotobuf 提供的支持库，包含诸如编码（marshaling）、解码（unmarshaling）等功能
```
go get github.com/golang/protobuf/proto
cd github.com/golang/protobuf/proto
go build
go install
```

{% highlight golang%}
package trademsg;

message Req{
  required string   SID = 1;
  required int32    UID = 2;
}
{% endhighlight %}

```
protoc --go_out=. trade.proto
```

{% highlight golang%}
package main

import (
    "github.com/golang/protobuf/proto"
    "trademsg"
    "fmt"
)

func main() {
    req := &trademsg.Req{
        SID: proto.String("sid"),
        UID: proto.Int32(2),
    }

    data, _ := proto.Marshal(req)

    dumpreq := &trademsg.Req{}
    proto.Unmarshal(data, dumpreq)
    fmt.Println(dumpreq.GetSID())
}
{% endhighlight %}

### socket 

[Socket](https://studygolang.com/articles/207)

[Socket Server](http://blog.csdn.net/ahlxt123/article/details/47726783)

## 缓存

beego是基于八大独立的模块(cache,context,config,httplibs,logs,orm,session,toolbox)之上构建的，是一个高度解耦的框架。当初设计beego的时候就是考虑功能模块化，用户即使不适用beego的http逻辑，也是可以在使用这些独立模块，例如你可以使用cache模块来做你的缓存逻辑，使用日志模块来记录你的操作信息，使用config模块来解析你各种格式的文件。

memcached作者Brad Fitzpatrick用Go语言重新实现了memcached，新的项目名称为groupcache，现已在Google多个生产环境中投入使用。

[golang的一个基于内存的key-value 缓存](http://blog.csdn.net/u010471121/article/details/53099401)

[golang开发缓存组件](http://blog.csdn.net/oaa608868/article/details/53489478)

缓存组件的设计其实挺简单的，主要思路或者设计点如下：
- 全局struct对象：用来做缓存（基于该struct实现增删改查基本操作）
- 定时gc功能（其实就是定时删除struct对象中过期的缓存对）：刚好用上golang的ticker外加channel控制实现
- 支持缓存写文件及从文件读缓存：其实就是将这里的key-value数据通过gob模块进行一次编解码操作
- 并发读写：上锁（golang支持读写锁，一般使用时在被操作的struct对象里面声明相应的锁，即sync.RWMutex，操作之前先上锁，之后解锁即可）