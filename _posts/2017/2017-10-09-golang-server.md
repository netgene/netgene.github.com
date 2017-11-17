---
layout: post
title: Go服务端架构思考
date: 2017-10-09 20:20:20
categories:
- golang
- 语言与设计
tags:
---


#### dispatcher-worker 模式
                                       ╱  worker_goroutine1
        dispatcher_groutine —— channel ——  worker_goroutine2
                                       ╲  worker_goroutinen


