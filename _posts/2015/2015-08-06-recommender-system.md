---
layout: post
title: 推荐系统综述[原]
date: 2015-08-06 18:28:58
categories:
- sysu
tags:
---

> 声明：未经博主允许不得转载。



# 推荐系统综述

**摘要：** 推荐系统是解决互联网信息过载的重要技术之一，文章首先介绍推荐系统的概念及推荐算法，分析各类算法的优点与不足，最后总结推荐系统需要解决的一些问题和研究方向。

**关键字：** 推荐系统；推荐算法 

## 1.概述

推荐系统(recommender system)是解决互联网信息过载(information overload)的重要技术之一，特别是在现在大数据时代。目的是为了帮助用户推荐其正在寻找或者感兴趣的内容，避免淹没在海量内容之中。推荐系统通过收集用户行为数据、建立偏好模型、生成个性化推荐。对于推荐内容的表现形式，包括文本、图像、声音、视频等。日常中常见的推荐内容主要包括新闻、音乐、电影、图书、商品等。目前推荐的载体主要为PC的Web平台和移动终端平台。

## 2.推荐系统概念

对于推荐系统的定义，被广泛认可和采用的是Resnick和Varian[1]的推荐系统的概念性描述：“它是以电子商务网站为平台，为消费者提供商品的信息和建议，协助他们决定应该购买什么产品，模拟推销人员协助消费者完成购买过程。”
推荐系统模型可如图1所示，推荐系统根据用户的隐含或显示信息对用户建模，同时对物品信息进行建模，推荐系统根据不同的推荐算法对用户兴趣和物品特征进行匹配筛选，找到用户可能感兴趣的推荐集合推荐给用户。

![pic1](/images/posts/2015-08-06-recommender-system-1.png)

图1 推荐系统模型

Adomavicius等人[2]给出了推荐系统形式化定义：设C表示用户集合，S表示所有可推荐给用户的项目集合，设效用函数u()可以计算项目s对用户c的推荐度的推荐度和产品的可用性，R是一定范围内的全序非负实数，推荐要研究的问题就是找到推荐度R最大的那些项目S*，即

![pic2](/images/posts/2015-08-06-recommender-system-2.png)

从上述的概念和定义中可以看出，推荐系统的关键是效用函数u的计算，即推荐算法的计算，下文将对推荐算法进行描述。

## 3.推荐算法

推荐算法是推荐系统的核心，在很大程序上决定了推荐系统的性能和推荐效果。目前主要的推荐算法有协同过滤推荐、基于内容的推荐、混合推荐、基于知识的推荐、基于模型的推荐算法、基于关联规则的推荐、基于图结构的推荐等。

**3.1协同过滤推荐算法(collaborative filtering recommendation)**

通过找到与该用户偏好相似的其他用户，将他们感兴趣的内容推荐给该用户[3]。协同过滤的概念是由Goldberg、Nicols、Oki以及Terry在1992年首次提出的，应用于Tapestry系统，其基本思想是通过对用户的显示输入或隐式输入的历史数据收集并统计计算，预测与此用户兴趣相似的用户，并将其相似用户感兴趣的项目推荐给此用户。
协同过滤推荐算法可以分为三个步骤：1)构建用户和项目的评价矩阵；2)通过用户项目评分计算用户相似度获取最近邻居集，目前常用的方法有余弦相似性、皮尔森相关系数；3)产生推荐集。
协同过滤推荐算法存在数据稀疏性问题、冷启动问题、可扩展问题、统一性问题、安全问题、隐私问题等。特别如数据稀疏性问题，在众多推荐系统中，用户评分过的项目往往很有限，使得用户和项目的评分矩阵极端稀疏，导致获取最近邻居集准确性下降，使得推荐质量下降。冷启动是数据稀疏性的极端情况，如新加入的用户无任何信息，无法找到最近邻居集，无法获得推荐。

**3.2基于内容的推荐(content-based recommendation)**

根据用户喜欢的项目，选择其他类似的项目作为推荐。首先，由系统隐式获取或者用户显示给出用户对项目属性的偏好，然后通过计算用户偏好和待预测项目的描述文档之间的匹配度，最后按照匹配度进行排序，想用户推荐其可能感兴趣的项目。

基于内容的推荐不需要用户历史数据，如对对象的评价等，没有关于新推荐对象的冷启动问题，没有稀疏性问题。不足是改方法受到推荐对象特征提取能力的限制较为严重，在新用户出现时有冷启动问题。

**3.3混合推荐(hybrid recommendation)**

主要是为了解决单一推荐技术的不足，可以按照不同的混合策略（如加权、切换、混合呈现、特征组合、串联、元层次混合等）将不同的推荐技术进行组合并完成推荐。各种推荐方法都有各自的优缺点，在实际应用中可以针对具体问题采用推荐策略的组合进行推荐，即所谓的组合推荐。组合推荐的目的是通过组合不同的推荐策略，达到扬长避短的目的，从而产生更符合用户需求的推荐。理论上讲可以有很多种的推荐组合方法，但目前研究和应用最多的组合推荐是把基于内容的推荐和系统过滤推荐的组合。把它们组合方法根据应用场景不同而不同，主要混合思路有两种：

（1）推荐结果混合：这是一种最简单的混合方法，就是分别是用两种或多种推荐方法产生推荐结果，然后采用某种算法把推荐结果进行混合而得到最终推荐。

（2）推荐算法的混合：以某种推荐策略为框架，混合另外的推荐策略，例如协同推荐的框架内混合基于内容的推荐。
混合推荐算法在实际应用中面临很多困难，需要解决不同推荐技术的有机组合问题，并且增加了计算的复杂度。

**3.4基于知识的推荐(knowledge-based recommendation)**

基于知识的推荐不是建立在用户需要和偏好基础上，而是利用针对特定领域指定规则来进行基于规则和实例的推理。效用知识是一种关于一个对象如何满足某一特定用户的知识，因而能够解释需求和推荐的关系，用于推荐系统。效用知识在推荐系统中必须以机器可读的方式存在。

**3.5基于模型的推荐**

推荐可以看做是分类或者预测问题，基于模型的推荐算法采用统计学、机器学习、数据挖掘等方法，根据用户历史数据为用户建模，据此生成推荐。这类方法在一定程度上解决了用户项目评分矩阵的稀疏性问题。

**3.6基于关联规则的推荐**

基于关联规则的推荐根据大量的用户购买历史数据,为用户推荐有相似购买行为的人所购买过的商品[4]. 如美国连锁超市沃尔玛的“啤酒+尿布”推荐，看上去没有关联的两种商品摆放在一起销售,却能够获得意想不到的收益。Agrawal 等提出的 Apriori 算法是经典的关联规则挖掘算法。

关联规则可以发现不同商品在销售过程中的关联性。

**3.7基于图结构的推荐**

用户-项目矩阵可建模为一个二部图(bipartite graph)[5]，其中节点分别表示用户和项目，边表示用户对项目的评价。基于图结构的推荐算法通过分析二部图结构给出合理的推荐。基于图结构计算推荐结果前需要预处理数据集.例如, MovieLens 是用户对电影的评分数据集， 评分等级分为5等，需要通过预处理将评分数据转换为二部图结构。

## 4.总结

在互联网和移动互联网的迅猛发展下，随着信息过载问题的逐年升温，互联网用户对信息需求的日益膨胀，推荐系统在各个领域的数字化进程中扮演着越来越重要的角色。
	
推荐系统的研究，一方面可以帮助用户在信息过载的网络环境中找到自己兴趣的内容，另一方面可以帮助电子商务网站更好地给用户推荐商品，提高电子商务网站的销售额。而推荐算法研究一直是推荐系统研究的重点，随着移动互联网的发展，对推荐系统实时性和准确性要求不断提高，算法实现的技术难度也越来越高，需要不断完善发展。
	
稀疏性和冷启动问题是困扰推荐系统很长时间了，协同过滤算法等推荐算法都存在该问题。进行推荐时需要掌握用户的兴趣偏好等用户信息，但用户担心个人数据得不到有效保护而不愿暴露个人信息, 这是推荐系统长期存在的一个问题。既能得到用户信息而提高推荐系统性能，又能有效保护用户信息将是未来推荐系统的一个研究方向。

## 参考文献：

* [1]ResinickP,Varian H R.Recommender systems[J].Communications of the ACM,1997,40(3):56-58.
* [2] Adomavicius G,Tuzhilin A.Towards the next generation of recommender systems:A survery *f - the state-of-the-art and possible extensions.IEEE Trans.on Knowledge and Data - *ngineering,2005,17(6):734-749.[doi:10.1109/TKDE.2005.99]
* [3]马宏伟,张光卫,李鹏.协同过滤推荐算法综述[J].小型微型计算机系统,2009,30(7):1282-1288.
* [4]赵艳霞,梁昌勇.基于关联规则的推荐系统在电子商务中的应用[J].价值工程,2006,(5):82-86.
* [5]王茜,段双艳.一种改进的基于二部图网络结构的推荐算法[J]. 计算机应用研究,2013,30(3):771-774.
* [6]张少中,陈德人.面向个性化推荐的两层混合图模型[J].软件学报,2009,20(zk):123-130.
* [7]肖扬,王道平,杨岑.基于三部图网络结构的知识推荐算法[J].计算机应用研究,2015,32(2):386-390.
* [8]许海玲,吴潇,李晓东,阎保平.互联网推荐系统比较研究[J].软件学报,2009,(2):350-361.
* [9]孟祥武,胡勋,王立才,张玉洁.移动推荐系统及其应用[J].软件学报,2013,(1):91-108.
* [10]王国霞,刘贺平.个性化推荐系统综述[J].计算机工程与应用,2012,48(7):66-76.
* [11]赵良辉,熊作贞.电子商务推荐系统综述及发展研究[J].技术应用.2013,(12):57-60.
