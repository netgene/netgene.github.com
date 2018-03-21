---
layout: post
title: 密码学与安全之区块链
date: 2018-03-21 20:20:20
categories:
- 密码学与安全
- BlockChain
tags:
---


### 账户设计

- UTXO
- Accountbased

### 区块链结构

默克尔树

### 共识机制

共识机制：区块链事务达成分布式共识的算法

- PoW（工作量证明） 通过与或运算，计算出一个满足规则的随机数，即获得本次记账权，发出本轮需要记录的数据，全网其它节点验证后一起存储。
- PoS（权益证明） PoW的一种升级共识机制，本质上是采用权益证明来代替PoW的算力证明，记账权由最高权益的节点获得，而不是最高算力的节点。
- DPos（股份授权证明机制） 类似于董事会投票，持币者投出一定数量的节点，代理他们进行验证和记账。 
- DAG
- PBFT：Practical Byzantine Fault Tolerance（实用拜占庭容错算法）  
- RCP
- BFT

![gs](https://upload-images.jianshu.io/upload_images/4582835-9f5ffc563328cf7f..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/661)  

![db](https://upload-images.jianshu.io/upload_images/4582835-f71d13e7e6f00f75.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)  

### 智能合约

### P2P

### 分布式

理论：FLP、CAP

- 2pc 3pc
- paxos
- raft
- zab
- Viewstamped Replication