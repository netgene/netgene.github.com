---
layout: post
title: 密码学与安全
date: 2017-05-27 18:28:58
categories:
- 密码学与安全
tags:
---

### 安全性原则

保密性、完整性、可用性。

### 对称加密

替换、变换

- 凯撒加密、单码加密、同音替换加密、块替换加密、希尔加密

将明文分成N个组，然后对各个组进行加密，形成各自的密文，最后把所有的分组密文进行合并，形成最终的密文。

- DES 块加密法 密钥位56位 64位块长加密数据  3DES
- AES AES-128、AES-192、AES-256

- base64 Base64就是一种基于64个可打印字符来表示二进制数据的方法

### 非对称加密

- RSA(public key, private key) ssh openssl https 该算法基于一个简单的数论事实：将两个大质数相乘容易，但要对其乘积因式分解却十分困难，因此可以将乘积作为公开加密秘钥。

### 不可逆加密

不可逆加密算法的特征是加密过程中不需要使用密钥

- MD5 SHA-256
- SHA-1 SHA-2 SHA-3
- crc32

### 国密

国密即国家密码局认定的国产密码算法。主要有SM1，SM2，SM3，SM4。密钥长度和分组长度均为128位。

- SM1 为对称加密。其加密强度与AES相当。该算法不公开，调用该算法时，需要通过加密芯片的接口进行调用。
- SM2 基于ECC椭圆曲线。该算法已公开。由于该算法基于ECC，故其签名速度与秘钥生成速度都快于RSA。ECC 256位（SM2采用的就是ECC 256位的一种）安全强度比RSA 2048位高，但运算速度快于RSA。
- SM3 在SHA-256基础上改进实现 SM3算法采用Merkle-Damgard结构，消息分组长度为512位，摘要值长度为256位。
- SM4 类DES。无线局域网标准的分组数据算法。对称加密，密钥长度和分组长度均为128位。

![SM2](https://img-blog.csdn.net/20170312163055305?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGNuZXRiZWU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
![SM4](https://img-blog.csdn.net/20170312162913835?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGNuZXRiZWU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  

### 密钥交换协议

### 量子密钥分发协议BB84

叫BB84算是学术界的传统，因为两位作者（Charles H. Bennett， 其时任职于IBM研究中心，Gilles Brassard，其时就职于蒙特利尔大学）的姓氏首字母都是B，论文正式发表在1984年的international conference on computers, systems and signal processing会议上。 这篇论文至9月16号的引用数高达5669次。

协议的运行流程总结如下： 

0、 首先，Alice和Bob共享两个极化基(photom polorization bases) D和R；

D和R可以被理解为两台“机器”，他们都能各自生成和测量对应0，1的量子比特（quantum bit, qbit），如下图所示。

另一个关键就是，如果将D生成qbit给R进行测量，测量结果不可预测。也就是说，如果用R来测量由D生成的Qbit，测量结果的意义就和直接猜差不多。

接下来开始做密钥协商，通信首先是在量子信道上进行。 

1、 Alice选出一个0-1 bit串S（比如\underbrace{010001001\cdots 011}_{1024\bits}）

2、 Alice 逐位随机选取D或R，然后通过量子信道发送S里0，1对应生成的量子态（qbit）

3、 Bob通过量子信道收到Alice发送的信息后，随机使用两个极化基D和R来一位一位地测量量子态，逐个得到0，1；

（其后的步骤4-7都是在我们今天使用的经典通信信道上进行.而公共信道就代表此时，如果没有别的保护，攻击者完全可以窃听并修改会话内容）

4、 Bob 通过公共信道向Alice发送部分自己的测量步骤，即告诉Alice自己在每个Qbit上用的到底是D还是R来做测量；

5、 Alice 对比自己的选择和Bob的选择，然后告诉Bob他在哪些位置上用的D和R是正确的；

这些正确位置在S，即Alice先选择的串中，唯一确定了另一个0-1 bit串，不妨称之为 pms，类似TLS的pre-master secret。

6、 Bob收到Alice的回应后，随机选择若干个他在正确位上的测量结果告知Alice；

7、 Alice确认Bob的正确性。如果Bob出错，则回到1.或者终止通信，

否则Alice给Bob发确认信息，同时从pms串中剔除 Bob公布的部分，剩下的作为通讯密钥；

8、 Bob收到Alice的确认信息后，同样从pms中剔除 Bob公布的部分，剩下的作为通讯密钥。

### 比特币

SHA-256 区块头80位（包括交易树哈希+难度+nonce随机数) 生成的哈希要求前面n个为0


### 量子加密相关

- BB84
- 一次一密

### 信息

[https://zhuanlan.zhihu.com/p/22474140](https://zhuanlan.zhihu.com/p/22474140)  
[https://www.zhihu.com/question/39111535](https://www.zhihu.com/question/39111535)  
[国密 https://blog.csdn.net/hcnetbee/article/details/53692579](https://blog.csdn.net/hcnetbee/article/details/53692579)  
