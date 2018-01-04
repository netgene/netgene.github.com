---
layout: post
title: 区块链
date: 2017-05-18 20:20:20
categories:
- BlockChain
---

### 比特币白皮书

[中本聪(Satoshi Nakamoto)比特币白皮书英文版 https://github.com/GammaGao/bitcoinwhitepaper/blob/master/bitcoin_en.pdf](https://github.com/GammaGao/bitcoinwhitepaper/blob/master/bitcoin_en.pdf)  
[中本聪(Satoshi Nakamoto)比特币白皮书中文版 https://github.com/GammaGao/bitcoinwhitepaper/blob/master/bitcoin_cn.pdf](https://github.com/GammaGao/bitcoinwhitepaper/blob/master/bitcoin_cn.pdf)  

### 学习笔记

![bitcoin](http://junwang.me/images/posts/2017-05-18-bitcoin.png)

#### 总结

形象描述:摇骰子摇到符合数字，负责验证生成记录交易的珠子，串到链子上。根据珠链可以算出每个钱包的金额。
- 摇骰子 - 挖矿工作量证明
- 珠子 - 区块
- 珠链 - 区块链
- 关键问题：去中心化可信最长链。

比特币设计：稀缺性[定量]、去中心[信仰]、流动性[交易]、确定性[可信]。

#### 关键字

electronic cash、peer-to-peer network、digital signature、double-spending problem、proof-of-work chain

电子钱包、二次支付、P2P网络、工作量证明、区块链、独立验证、激励系统、挖矿与共识、去中心化

### 学习要点

#### 比特币钱包 

- 私钥、公钥、比特币地址

#### P2P网络

- 原理

#### 工作量证明

- 工作量证明算法
- SHA-256 下,随机散列值以一个或多个 0 开始。那么随着 0 的数目的 上升, 找到这个解所需要的工作量将呈指数增长,而对结果进行检验则仅需要一次随机散列运算。

#### 区块链

- 区块结构、区块头哈希值、区块高度
- Merkle树归纳一个区块中的所有交易

#### 交易

#### 挖矿与共识

### 其他学习资料

![https://pic4.zhimg.com/v2-2cb0151b6f836deffe6e91d38dfe4a5e_r.jpg](https://pic4.zhimg.com/v2-2cb0151b6f836deffe6e91d38dfe4a5e_r.jpg)  

[精通比特币 http://zhibimo.com/read/wang-miao/mastering-bitcoin/](http://zhibimo.com/read/wang-miao/mastering-bitcoin/)  
[https://zhuanlan.zhihu.com/p/29807632](https://zhuanlan.zhihu.com/p/29807632)  
[https://www.zhihu.com/question/20941124/answer/20411491](https://www.zhihu.com/question/20941124/answer/20411491)  
[https://www.zhihu.com/question/39609610/answer/104664482](https://www.zhihu.com/question/39609610/answer/104664482)  
[http://blog.csdn.net/taifei/article/details/73546836](http://blog.csdn.net/taifei/article/details/73546836)  
[https://zhuanlan.zhihu.com/p/25017851](https://zhuanlan.zhihu.com/p/25017851)  

### 源码
