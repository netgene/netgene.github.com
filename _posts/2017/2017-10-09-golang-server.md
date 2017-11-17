---
layout: post
title: Go服务端架构思考
date: 2017-10-09 20:20:20
categories:
- golang
- 语言与设计
tags:
---


#### dispatcher-worker 模式 类生产者消费者模式

dispatcher相当于生产者，例如处理socket连接，把解析的报文都发往channel。worker_goroutine只处理业务报文的逻辑。要注意worker_goroutine之间的竞争问题。

                                       ╱  worker_goroutine_1
        dispatcher_groutine —— channel ——  worker_goroutine_2
                                       ╲  worker_goroutine_n


#### rpc 微服务模式

同时发起n个rpc调用，通过select监控channel并设置统一的超时时间来控制。

                                        ╱  goroutine_rpc_server_1
        server_goroutine (异步超时等待) ——  goroutine_rpc_server_2
                                        ╲  goroutine_rpc_server_n
