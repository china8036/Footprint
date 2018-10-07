- [推荐系统：CTR](#推荐系统ctr)
  - [1. CTR 基本概念](#1-ctr-基本概念)
  - [2. CTR 预判模型](#2-ctr-预判模型)
    - [2.1. LR 海量高纬离散特征](#21-lr-海量高纬离散特征)
    - [2.2. GBDT 梯度提升决策树](#22-gbdt-梯度提升决策树)
    - [2.3. FM 与 FFM](#23-fm-与-ffm)
    - [2.4. GBDT+(LR, FM, FFM)](#24-gbdtlr-fm-ffm)
    - [2.5. MLR](#25-mlr)
  - [3. Refer Links](#3-refer-links)

# 推荐系统：CTR

## 1. CTR 基本概念

CTR (Click-Through-Rate) 即点击通过率 / 点阅率，指网络内容（图片 / 文字 / 视频等）的点击到达率，即实际点击次数除以展现次数（Show content）。CTR 是一种衡量网页热门程度的指标，也是推荐系统最重要的一种效果评价指标。

> CTR = 内容被点击次数 / 内容被显示次数

举例来说，若某广告显示次数为 100 次，在这 100 次中显示中被网友点击了 5 次，那该广告的 CTR 就等于 5/100 = 5%。

## 2. CTR 预判模型

无论是人工运营还是机器决策，我们都希望对某条广告或内容可能的点击率有一个预判，以便判断哪些条目应该被放在更重要的位置上。

- 直接统计历史点击率进行排序
  
  不考虑位置、时间等一系列环境因素，绝对的点击率水平是没有什么太大意义的。比方说 2 个放在网页上不同位置上的广告，统计得到前者的点击率是 2%，后者的点击率是 1%，究竟哪个广告好一些呢？其实我们得不出任何结论。

- 在不同的位置上分别统计点击率，然后分别排序

  这个思路从道理上来说无懈可击，相当于直接求解联合分布；不过，其实用价值并不高：在每个位置上分别统计，大多数广告或内容条目的数据都太少，比如说 100 次展示，产生了一次点击，这难道能得出 1% 点击率的结论么？

- 找到一些影响点击率的一些关健因素，对这些因素分别统计

  实际上已经产生了“特征”这样的建模思路了。比如说，广告位是一个因素，广告本身是一个因素，用户的性别是一个因素，在每个因素上分别统计点击率，从数据充分性上是可行的。不过这又产生了一个新的问题：我知道了男性用户的平均点击率、广告位 S 平均点击率、某广告 A 的平均点击率，那么如何评估某男性用户在广告位 S 上看到广告 A 的点击率呢？直觉的方法，是求上面三个点击率的几何平均。不过这里面有一个隐含的假设：即这三个因素是相互独立的。然而当特征多起来以后，这样的独立性假设是很难保证的。

  特征之间独立性，经常对我们的结论影响很大。比如说，中国的癌症发病率上升，到底是“中国”这个因素的原因呢？还是“平均寿命”这个因素的原因呢？显然这两个因素有一些相关性，因此简单的分别统计，往往也是行不通的。

  那么怎么办呢？这就要统计学家和计算机科学家出马，建立一个综合考虑各种特征，并根据历史数据调整出来的点击率模型，这个模型既要考虑各种特征的相关性，又要解决每个特征数据充分性的问题，并且还要能在大量的数据上自动训练优化。这就是**点击率模型**的意义。

> CTR 预估模型的公式：y = f(x)， y 的范围为 [0,1]，表示被点击的概率。 

### 2.1. LR 海量高纬离散特征

LR（logistics regression），是 CTR 预估模型的最基本的模型，也是工业界最喜爱使用的方案。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/7/1/300528e791b80946f147d4dc65ac002c.jpg)

LR 的优势在于处理离散化特征，而且模型十分简单，很容易实现分布式计算。关于 LR 的变种也有许多，比如 Google 的 FTRL，其实这些变种都可以看成：LR+ 正则化 + 特定优化方法。

LR 的缺点也很明显，特征与特征之间在模型中是独立的，对于一些存在交叉可能性的特征（比如：衣服类型与性别，这两个特征交叉很有意义），需要进行大量的人工特征工程进行交叉。虽然模型简单了，但是人工的工作却繁重了很多。而且 LR 需要将特征进行离散化，归一化，在离散化过程中也可能出现边界问题。这里便为 GBDT+LR 的方案进行了铺垫。

### 2.2. GBDT 梯度提升决策树

GBDT，即梯度提升决策树，是一种表达能力比较强的非线性模型。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/7/1/a9dc0492b72299420342496dd5f88e99.jpg)

GBDT 的优势在于处理连续值特征，比如用户历史点击率，用户历史浏览次数等连续值特征。而且由于树的分裂算法，它具有一定的组合特征的能力，模型的表达能力要比 LR 强。GBDT 对特征的数值线性变化不敏感，它会按照目标函数，自动选择最优的分裂特征和该特征的最优分裂点，而且根据特征的分裂次数，还可以得到一个特征的重要性排序。所以，使用 GBDT 减少人工特征工程的工作量和进行特征筛选。

GBDT 善于处理连续值特征，但是推荐系统的绝大多数场景中，出现的都是大规模离散化特征，如果我们需要使用 GBDT 的话，则需要将很多特征统计成连续值特征（或者 embedding），这里可能需要耗费比较多的时间。同时，因为 GBDT 模型特点，它具有很强的记忆行为，不利于挖掘长尾特征，而且 GBDT 虽然具备一定的组合特征的能力，但是组合的能力十分有限，远不能与 dnn 相比。

### 2.3. FM 与 FFM

### 2.4. GBDT+(LR, FM, FFM)

### 2.5. MLR

MLR 是由阿里团队提出的一种非线性模型。它等价于聚类 +LR 的形式。

## 3. Refer Links

[百度百科：CTR （点击通过率）](https://baike.baidu.com/item/CTR/10653699)

[关于点击率模型，你知道这三点就够了](http://www.meihua.info/a/65329)

[推荐系统中使用 ctr 排序的 f(x) 的设计 - 传统模型篇](https://zhuanlan.zhihu.com/p/32689178)