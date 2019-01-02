---
layout: post
title: mysql数据库高可用方案
date: 2019-01-02 20:20:20
categories:
- 数据库
tags:
---

[MySQL数据库的高可用方案总结 http://www.cnblogs.com/panwenbin-logs/p/7872104.html](http://www.cnblogs.com/panwenbin-logs/p/7872104.html)  
1.基于共享存储的方案SAN
2.基于磁盘复制的方案 DRBD
3.基于主从复制(单点写)方案
3.1.keepalived/heartbeat
3.2.MHA
3.3.基于zookeeper的高可用
4.基于Cluster(多点写)方案
5.基于中间件proxy的方案 比如mysql自带的mysql-proxy和fabric，阿里巴巴的cobar和tddl等

[MySQL数据库的几种常见高可用方案 https://yq.aliyun.com/articles/74454](https://yq.aliyun.com/articles/74454)  

[MySQL异步复制、半同步复制详解 https://www.cnblogs.com/paul8339/p/6879329.html](https://www.cnblogs.com/paul8339/p/6879329.html)  

[实战体验几种MySQLCluster方案 https://www.cnblogs.com/labing/p/5309185.html](https://www.cnblogs.com/labing/p/5309185.html)  

[MySQL数据库的几种常见高可用方案 https://yq.aliyun.com/articles/74454](https://yq.aliyun.com/articles/74454)  

[微信高可用分布式数据库PhxSQL设计与实现 https://blog.csdn.net/u013160024/article/details/70168708](https://blog.csdn.net/u013160024/article/details/70168708)  