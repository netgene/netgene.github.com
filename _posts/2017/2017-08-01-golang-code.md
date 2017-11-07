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

## 架构模式

### 生产者与消费者