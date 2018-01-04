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

形象描述:摇骰子摇到符合数字，负责验证生成记录交易的贝壳，串到链子上。根据贝壳链可以算出每个钱包的金额。
- 摇骰子 - 挖矿工作量证明
- 贝壳 - 区块
- 贝壳链 - 区块链
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
- 一个长区块链的存在可以让区块链的历史不可改变，这也是比特币安全性的一个关键特征。

#### 交易

#### 挖矿与共识

### 其他学习资料

![https://pic4.zhimg.com/v2-2cb0151b6f836deffe6e91d38dfe4a5e_r.jpg](https://pic4.zhimg.com/v2-2cb0151b6f836deffe6e91d38dfe4a5e_r.jpg)  

[精通比特币 http://zhibimo.com/read/wang-miao/mastering-bitcoin/](http://zhibimo.com/read/wang-miao/mastering-bitcoin/)  
[https://www.zhihu.com/question/22039036](https://www.zhihu.com/question/22039036)  
[https://zhuanlan.zhihu.com/p/29807632](https://zhuanlan.zhihu.com/p/29807632)  
[https://www.zhihu.com/question/20941124/answer/20411491](https://www.zhihu.com/question/20941124/answer/20411491)  
[https://www.zhihu.com/question/39609610/answer/104664482](https://www.zhihu.com/question/39609610/answer/104664482)  
[http://blog.csdn.net/taifei/article/details/73546836](http://blog.csdn.net/taifei/article/details/73546836)  
[https://zhuanlan.zhihu.com/p/25017851](https://zhuanlan.zhihu.com/p/25017851)  

### 摘录
链接：https://www.zhihu.com/question/22039036/answer/20118046

> 比特币这个系统，从根本上来说是一个网络协议（题主所说的「规则」），协议规定了比特币生成规则、交易规则、数据交换格式，等等等等。网络协议只规定使用这个协议的用户的行为。系统中任何节点只要在行为上满足这个协议的要求，就可以被网络接受，而这些行为可以由任何代码实现，并不需要某个特定的代码。所以题主提到的「核心代码」，严格说来是不存在的。而您提到的「核心算法」确实有，属于在协议里规定好的行为的一部分，与任何一个客户端代码都没有关系。

> 您改出来的代码，如果发出的交易不符合当前btc协议的话，就不会被写上区块链。要写上区块链，您必须控制全网一半以上算力。这叫pow机制。

> 512加密未来二十年还是安全的，就算用你量子计算机崩坏了一个比特币，还有来特币取而代之，还有更高级的加密算法取而代之，比特币发明的是一种网络货币协议，可以生出n多分支，总之网络货币必定有一个是顶级明星。

> 比特币虽然不完美，未来也有可能被取代，但无疑是人类最伟大的发明之一，主要得益于全球的分布式网络计算，做到了不可摧毁的电子货币，完全依靠市场来证明其内在价值，和法币有本质区别。


### 源码

[https://github.com/bitcoin/bitcoin](https://github.com/bitcoin/bitcoin)  
