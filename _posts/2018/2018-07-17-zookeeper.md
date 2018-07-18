---
layout: post
title: zookeeper
date: 2018-07-17 18:28:58
categories:
- opensource
tags:
---
ZooKeeper是一种集中式服务，用于维护配置信息，命名，提供分布式同步和提供组服务。所有这些类型的服务都以分布式应用程序的某种形式使用。  

Zookeeper是一个高效的分布式协调服务，可以提供配置信息管理、命名、分布式同步、集群管理、数据库切换等服务。它不适合用来存储大量信息，可以用来存储一些配置、发布与订阅等少量信息。Hadoop、Storm、消息中间件、RPC服务框架、分布式数据库同步系统，这些都是Zookeeper的应用场景。  

Zookeeper集群中节点个数一般为奇数个（>=3），若集群中Master挂掉，剩余节点个数在半数以上时，就可以推举新的主节点，继续对外提供服务。  

Zookeeper使用的数据结构为树形结构，根节点为"/"。Zookeeper集群中的节点，根据其身份特性分为leader、follower、observer。leader负责客户端writer类型的请求；follower负责客户端reader类型的请求，并参与leader选举；observer是特殊的follower，可以接收客户端reader请求，但是不会参与选举，可以用来扩容系统支撑能力，提高读取速度。  

Zookeeper是一个基于观察者模式设计的分布式服务管理框架，负责存储和管理相关数据，接收观察者的注册。一旦这些数据的状态发生变化，zookeeper就负责通知那些已经在zookeeper集群进行注册并关心这些状态发生变化的观察者，以便观察者执行相关操作。  

Zookeeper使用的是ZAB原子消息广播协议，节点之间的一致性算法为Paxos，能够保障分布式环境中数据的一致性。分布式场景下高可用是Zookeeper的特性，可以采用第三方客户端的实现，即Curator框架。  

[https://zookeeper.apache.org/](https://zookeeper.apache.org/)  

[Zookeeper的功能以及工作原理 https://www.cnblogs.com/felixzh/p/5869212.html](https://www.cnblogs.com/felixzh/p/5869212.html)  
[Zookeeper简单介绍 https://www.cnblogs.com/sunddenly/p/4033574.html](https://www.cnblogs.com/sunddenly/p/4033574.html)  
[Zookeeper简介与集群搭建 https://blog.csdn.net/qiushisoftware/article/details/79043379](https://blog.csdn.net/qiushisoftware/article/details/79043379)  
[Zookeeper 客户端源码吐血总结](https://blog.csdn.net/cnh294141800/article/details/53039482)  

![zookeeper](https://img-blog.csdn.net/20161104212932485)

上图就是对Zookeeper源码一个最好的解释:  
- Client端发送Request(封装成Packet)请求到Zookeeper 
- Zookeeper处理Request并将该请求放入Outgoing Queue(顾名思义，外出队列，就是让Zookeeper服务器处理的队列)， 
- Zookeeper端处理Outgoing Queue，并将该事件移到Pending Queue中 
- Zookeeper端消费Pending Queue，并调用finishPacket(),生成Event 
- EventThread线程消费Event事件,并且处理Watcher.