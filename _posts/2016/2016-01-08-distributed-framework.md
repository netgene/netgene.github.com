---
layout: post
title: 分布式服务框架
date: 2016-01-08 18:28:58
categories:
- opensource
tags:
---

Zookeeper 分布式服务框架是Apache Hadoop的一个子项目，它是一个针对大型分布式系统的可靠协调系统，它主要是用来解决分布式应用中经常遇到的一些数据管理问题，可以高可靠的维护元数据。提供的功能包括：配置维护、名字服务、分布式同步、组服务等。ZooKeeper的设计目标就是封装好复杂易出错的关键服务，将简单易用的接口和性能高效、功能稳定的系统提供给用户。

Zookeeper一个分布式的服务框架，是树型的目录服务的数据存储，能做到集群管理数据 ，这里能很好的作为Dubbo服务的注册中心。

ZooKeeper所提供的服务主要是通过：数据结构Znode + 原语 + watcher机制，三个部分来实现的。paxos算法、Zab协议、选举/同步流程。

- 集群管理：利用临时节点特性，节点关联的是机器的主机名、IP地址等相关信息，集群单点故障也属于该范畴。
- 统一命名：主要利用节点的唯一性和目录节点树结构。
- 配置管理：节点关联的是配置信息。
- 分布式锁：节点关联的是要竞争的资源。

[https://zookeeper.apache.org/](https://zookeeper.apache.org/)  
[http://www.cnblogs.com/sunddenly/p/4033574.html](http://www.cnblogs.com/sunddenly/p/4033574.html)  


Dubbo远程服务调用的分布式框架 高性能透明RPC远程服务方案

![Dubbo](http://img.blog.csdn.net/20131224140734734)  

- provider 暴露服务的服务提供方 
- consumer 调用远程服务的服务消费方
- registry 服务注册与发现的注册中心
- Monitor: 统计服务的调用次调和调用时间的监控中心。
- Container: 服务运行容器

[https://www.cnblogs.com/Javame/p/3632473.html](https://www.cnblogs.com/Javame/p/3632473.html)  



远程过程调用，动态服务发现，负载均衡