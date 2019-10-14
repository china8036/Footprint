- [自动特征工程](#自动特征工程)
  - [1. FeatureTools](#1-featuretools)
  - [2. TransmogrifAI](#2-transmogrifai)
  - [3. Refer Links](#3-refer-links)

# 自动特征工程

在构建机器学习模型的过程中，特征工程（提取）往往是决定模型性能的最关键一步。而往往机器学习中最耗时的部分也正是特性工程。因此，许多模型由于时间限制而过早地从实验阶段转移到生产阶段从而导致并不是最优的。

AutoML自动特征工程旨在减少算法工程师们的负担，使得工程师花费较少的时间自动的从已有数据中创建出新特征并且用于训练机器学习模型。目前有关自动特征工程的开源工具非常多。

## 1. FeatureTools

FeatureTools是一个开源的python库，它使用一种称为深度特征合成（Deep Feature Synthesis，DFS）的算法。能够从一系列有关联的表格中创建出很多特征，旨在快速推动特征生成的过程，从而有更多时间专注于机器学习模型构建的其他方面。

## 2. TransmogrifAI

TransmogrifAI是2018年Salesforce开源的AutoML库，是一个基于Scala和SparkML构建的库。可以自动完成数据清理、特征工程和模型选择，然后训练出一个高性能模型，进行进一步探索和迭代。是一个几乎全流程覆盖的机器学习自动化工具。

TransmogrifAI封装了机器学习过程的五个主要步骤：特征推断（Feature Inference），自动化特征工程（Transmogrification），自动化特征验证（Feature Validation），自动化模型选择（Model Selection），超参数优化（Hyperparameter Optimization）。

TransmogrifAI封装所有复杂操作，代码非常简单，只需短短几行就能搞定全流程建模，成功把训练模型所需的总时间从几周、几个月缩短到了几个小时。



## 3. Refer Links

TODO:

http://km.oa.com/group/18832/articles/show/386275?kmref=search&from_page=1&no=9

https://www.jiqizhixin.com/articles/2019-08-27-17?utm_source=tuicool&utm_medium=referral