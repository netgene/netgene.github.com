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