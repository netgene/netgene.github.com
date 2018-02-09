---
layout: post
title: 分布式事务
date: 2016-05-05 20:20:20
categories:
- c/c++
tags:
---

[聊聊分布式事务，再说说解决方案
 https://www.cnblogs.com/savorboard/p/distributed-system-transaction-consistency.html](https://www.cnblogs.com/savorboard/p/distributed-system-transaction-consistency.html)  

[如何用消息系统避免分布式事务？ http://blog.csdn.net/echineselearning/article/details/63799046](http://blog.csdn.net/echineselearning/article/details/63799046)  
[对分布式事务及两阶段提交、三阶段提交的理解 http://blog.csdn.net/zyt425916200/article/details/77062446](http://blog.csdn.net/zyt425916200/article/details/77062446)  
[保证分布式系统数据一致性的6种方案 https://weibo.com/ttarticle/p/show?id=2309403965965003062676](https://weibo.com/ttarticle/p/show?id=2309403965965003062676)  
[现在主流开源分布式系统架构都有哪些？https://www.zhihu.com/question/19832447](https://www.zhihu.com/question/19832447)  
[一个分布式服务器集群架构方案 https://www.cnblogs.com/liangxiaofeng/p/4920267.html](https://www.cnblogs.com/liangxiaofeng/p/4920267.html)  
[承载千万级并发的分布式系统架构设计思想 https://my.oschina.net/u/173426/blog/618865](https://my.oschina.net/u/173426/blog/618865)  
[网易视频云：分布式系统的三类一致性挑战与解决方案 https://zhuanlan.zhihu.com/p/24515722](https://zhuanlan.zhihu.com/p/24515722)  
[分布式系统理论 https://zhuanlan.zhihu.com/distributed](https://zhuanlan.zhihu.com/distributed)  

[现在mysql的分布式数据访问层主流方案有哪些? https://www.zhihu.com/question/22521550](https://www.zhihu.com/question/22521550)  
[MySQL 对于千万级的大表要怎么优化？https://www.zhihu.com/question/19719997/answer/81930332](https://www.zhihu.com/question/19719997/answer/81930332)  
[TiDB 如何重新定义下一代关系型数据库 http://www.techweb.com.cn/network/system/2017-06-24/2540403.shtml](http://www.techweb.com.cn/network/system/2017-06-24/2540403.shtml)  
[TiDB 架构的演进和开发哲学 https://zhuanlan.zhihu.com/p/25142743](https://zhuanlan.zhihu.com/p/25142743)  
[TiDB在摩拜单车在线数据业务的应用和实践 http://blog.csdn.net/dev_csdn/article/details/78839392](http://blog.csdn.net/dev_csdn/article/details/78839392)  

mysql数据库一般都是按照这个步骤去演化：  
- 第一优化你的sql和索引；
- 第二就做主从复制或主主复制，读写分离，可以在应用层做，效率高，也可以用三方工具，第三方工具推荐360的atlas,其它的要么效率不高，要么没人维护；
- 第三加缓存，memcached,redis；
- 第四如果以上都做了还是慢，不要想着去做切分，mysql自带分区表，先试试这个，对你的应用是透明的，无需更改代码,但是sql语句是需要针对分区表做优化的，sql条件中要带上分区条件的列，从而使查询定位到少量的分区上，否则就会扫描全部分区，另外分区表还有一些坑，在这里就不多说了；
- 第五如果以上都做了，那就先做垂直拆分，其实就是根据你模块的耦合度，将一个大的系统分为多个小的系统，也就是分布式系统；
- 第六才是水平切分，针对数据量大的表，这一步最麻烦，最能考验技术水平，要选择一个合理的sharding key,为了有好的查询效率，表结构也要改动，做一定的冗余，应用也要改，sql中尽量带sharding key，将数据定位到限定的表上去查，而不是扫描全部的表；

mysql数据库一般都是按照这个步骤去演化的，成本也是由低到高。


分布式关键字  
- 理论：FLP、CAP。
- 一致性协议：2PC、3PC/Paxos/Zab/Raft/Viewstamped Replication
- 构建一致性协议概念：多数派、Leader选举、租约、逻辑时钟等

FLP定理 - 对于一个只是依靠消息通讯的异步系统来说，如果出现了服务器异常，无论依靠什么精妙的算法，都有可能导致本次请求的各参与方处于永久不一致的状态。

可用性和一致性的问题：
- 数据库事务提交(commit transaction)、Leader选举、序列号生成等都会遇到一致性问题。

- replication totalorder/leader/paxos pacificA zab/底层日志复制协议吞吐量不够、保证消息全序的单线程回放导致从跟不上主。

两阶段提交问题：
- 

Google的分布式数据库演进：Bigtable -> Megastore -> Spanner。

Spanner被业界认为是第一个全球数据库。世界上任何一个人发了一条消息，我就能立马看到并保证数据的严格一致。Spanner的设计，核心思想就是依靠物理时钟来代替全局日志号。

分布式事务的强一致性还需要有一个地方集中分配全局更新日志号。