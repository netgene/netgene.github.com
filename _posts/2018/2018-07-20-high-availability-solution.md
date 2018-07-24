---
layout: post
title: 高可用方案
date: 2018-07-20 18:28:58
categories:
- opensource
tags:
---

Keepalived/zookeeper/DNS  单点 并发 容灾 主从热备 负载均衡   

1、Keepalived：优点：简单，基本不需要业务层面做任何事情，就可以实现高可用，主备容灾。而且容灾的宕机时间也比较短。缺点：也是简单，因为VRRP、主备切换都没有什么复杂的逻辑，所以无法应对某些特殊场景，比如主备通信链路出问题，会导致脑裂。同时，keepalived也不容易做负载均衡。  

[keepalived实现服务高可用 https://www.cnblogs.com/clsn/p/8052649.html#auto_id_0](https://www.cnblogs.com/clsn/p/8052649.html#auto_id_0)  

2、zookeeper：优点：可以支持高可用，负载均衡。本身是个分布式的服务。缺点：跟业务结合的比较紧密。需要在业务代码中写好ZK使用的逻辑，比如注册名字。拉取名字对应的服务地址等。    

3、DNS优点：不复杂，同时与业务结合的不是很紧密，通过简单的逻辑就可以实现负载均衡。缺点：DNS容灾是更新DNS服务器需要时间，宕机时间比较长。  

从简单性来说：Keepalived最简单，DNS次之，ZK最复杂。从负载均衡能力来看，zookeeper最强，DNS次之，Keepalived最弱。从与业务的紧密程度来看：ZK最紧密，DNS次之，Keepalived基本跟业务层面没有关系。

