- [推荐系统：基本架构](#推荐系统基本架构)
  - [1. 工程部分](#1-工程部分)
    - [1.1. 离线系统](#11-离线系统)
    - [1.2. 在线系统](#12-在线系统)
  - [2. 算法部分](#2-算法部分)
  - [3. 内容部分](#3-内容部分)
  - [4. Refer Links](#4-refer-links)

# 推荐系统：基本架构

随着网络信息量的大幅增长，使得用户往往无法从中获得对自己真正有用的信息，推荐系统就是解决此类问题的一个有效方案，和搜索引擎相比推荐系统通过研究用户的兴趣偏好，进行个性化计算，由系统发现用户的兴趣点，从而引导用户发现自己的信息需求。

**推荐系统的构成，主要包括了算法部分、内容部分以及工程部分**，由于在学习过程中侧重于工程部分，因此以下以工程部分为主进行阐述。

## 1. 工程部分

推荐系统的工程部分，负责将推荐算法与内容数据整合到一个系统当中，提供一个能够处理用户请求，响应推荐结果列表的系统。

推荐系统工程部分的架构，可分为在线系统和离线系统：

![image](http://img.cdn.firejq.com/jpg/2018/7/2/3a6feda26ded412d313ba463050f650c.jpg)

### 1.1. 离线系统

离线系统的主要职责是：
- 用户属性相关

  利用日志系统数据结合反作弊数据，通过 Hadoop 等分布式框架进行长期用户画像的刻画和存储、用户行为的处理和存储，为在线系统的调度层生成实时画像提供服务。

- 推荐模型相关

  利用日志系统数据获取离线特征，并通过 Kafka 等分布式计算框架进行打分模型的离线训练，为在线系统的引擎层获取召回列表提供服务。

- 内容数据相关

  根据内容数据获取正排索引和倒排索引，通过离线分发系统，为在线系统引擎层的召回阶段和打分排序阶段提供服务。

### 1.2. 在线系统

在线系统的职责主要是处理用户请求，响应个性化推荐的曝光内容列表。其基本流程如下：

![image](http://img.cdn.firejq.com/jpg/2018/7/1/85041d52ceac3e242d45fbe2b105ea07.jpg)

1. 应用后台接收到用户请求，传递到推荐系统接入层。

1. 接入层记录用户点击行为，然后将用户请求传递至调度层。

1. 调度层根据用户 id 获取存储在文件系统 / 缓存中的用户画像和用户行为，再使用用户画像和用户行为生成该请求对应的实时用户画像，再将实时用户画像传递到策略层。

1. 策略层将画像分发到多个推荐引擎当中。

1. 引擎层中分为粗排阶段和精排阶段：
    - 粗粒度排序阶段（召回）

      根据用户画像，使用协同过滤等算法召回相关度较高的 TopN ItemList。

      召回的作用在于根据用户的不同兴趣，通过倒排索引从海量 item 中选出数百个用户感兴趣的 item，减轻精排阶段压力。

    - 精粒度排序阶段（排序）

      用粗排召回的 ItemList 关联 Item 特征，并加载离线训练好的排序（预判）模型（pCTR 模型，如 LR）进行 CTR 的打分预判和排序，然后再根据排序的结果通过正排索引生成召回列表。

      排序的作用是基于用户的一些特征，对召回层的 item 再次进行打分排序，更精准地选出用户感兴趣的 item。

      - 使用机器学习模型来综合多方面的因子进行排序

        为了解决单个因素无法准确进行打分预测的问题，可以设计出一个能够平衡多种因素的排名函数，将预测评分、热门度以及更多可能的因素都作为特征，在此基础上设计一个排序的模型。

        ![image](http://img.cdn.firejq.com/jpg/2018/7/1/42e994262dfef3ea826268d6e126812e.jpg)

        很多监督学习的方法都能被用来设计排序模型。其中代表性的有 Logistic 回归、支持向量机、人工神经网络或者决策树类的算法（GBDT）。此外，有很多专门为排序设计的模型，比如 RankSVD 和 RankBoost。

1. 策略层获取到召回列表，结合预先人工配置的策略、打分排序、已曝光情况生成最终的推荐结果列表，并返回给调度层。

1. 调度层记录本次推荐的结果列表，然后最终响应用户请求。

## 2. 算法部分

推荐系统的算法部分，包括了多种推荐算法，如利用用户行为数据的 CF 算法、LFM 算法、利用社交网络数据的推荐算法等。

这些算法通过不同的方式、利用不同的数据，将用户与内容相联系起来，从而从大量信息中找到用户感兴趣的信息，一方面帮助用户发现对自己有价值的信息，另一方面让信息能够展现在对它感兴趣的用户面前，从而实现信息消费者和信息生产者的双赢。

## 3. 内容部分

推荐系统的内容部分，包含了系统中存储的被推荐内容，以及这些内容的正排索引和倒排索引，其中正排索引用于快速查找定位指定文档的内容数据，倒排索引用于根据内容特征找到相应的内容文档。

推荐系统的内容部分需要为工程部分的离线训练、用户画像刻画、引擎层内容获取提供数据。

## 4. Refer Links

[个性化推荐系统从 0 到 1](https://cloud.tencent.com/developer/article/1004920)

[蘑菇街搜索与推荐架构，从 0 到 1 再到 100](http://www.sohu.com/a/143176209_463994)

[美团 O2O 排序解决方案——线上篇](https://tech.meituan.com/meituan-search-rank.html)

[红豆 Live 推荐算法中召回和排序的应用和策略](https://mp.weixin.qq.com/s/BffCFKpgjf-MDKMH4V_Dig)

[美团 O2O 排序解决方案——线上篇](https://tech.meituan.com/meituan-search-rank.html)

[Netflix 推荐系统（第二部分）—— 排序](http://itindex.net/detail/38276-netflix-%E6%8E%A8%E8%8D%90%E7%B3%BB%E7%BB%9F-%E4%BA%8C%E9%83%A8)

[爱奇艺个性化推荐排序实践](http://www.woshipm.com/pd/847004.html)