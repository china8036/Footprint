- [Ray: Tune](#ray-tune)
  - [1. Core Features](#1-core-features)
  - [2. API](#2-api)
  - [3. 实现分析](#3-实现分析)
  - [4. Refer Links](#4-refer-links)

# Ray: Tune

一个模型搜索和训练平台混合了顺序和并行计算。在搜索过程中，许多试验是并行评估的。超参数搜索算法按顺序检查试验结果，并做出提高并行计算的决策。为了支持广泛的训练工作量和选择算法，模型搜索框架需要满足以下要求：
- 处理不规则计算的能力：试验通常在长度和资源使用上有所不同。
- 能够处理任意用户代码和第三方库的资源需求。这包括并行性和硬件资源（如 gpu) 的使用。
- 能够根据中间试验结果做出搜索 / 调度决策。例如，遗传算法通常在训练过程中克隆或变异模型参数。执行早期停止的算法也使用中间结果来做出停止决策。

为了获得良好的用户体验，还需要具备以下功能：
- 监测和可视化试验进展和结果。
- 简单的集成和规范的实验执行。

为了满足这些需求，我们提出了面向用户和调度 api 的 Tune，并能在 Ray 分布式计算框架上实现。Ray 框架提供了底层的分布式执行和资源管理。其灵活的任务和参与者抽象允许 Tune 调度不规则的计算，并根据中间结果做出决策。

Tune 是一个超参数优化库，可以用于 PyTorch、TensorFlow， MXnet，keras 等深度学习框架

## 1. Core Features

- 多计算节点的分布式超参数的查找。
- 支持多种深度学习框架，例如：pytorch，TensorFlow。
- 结果直接可以用 tensorboard 可视化。
- 可拓展的 SOTA 算法，例如：PBT，HyperBand/ASHA。
- 整合了很多超参数优化库， 例如：Ax， HyperOpt，Bayesian Optimization。

## 2. API

Tune 提供了两个开发接口：一个用户 API，用于寻求训练模型的用户；一个调度 API，用于对改进模型搜索过程本身感兴趣的研究人员。由于这种划分，Tune 的用户可以选择许多搜索算法。相应地，scheduler API 使研究人员能够轻松地针对不同范围的工作负载，并提供一种机制使用户可以使用他们的算法。

## 3. 实现分析

## 4. Refer Links

[Ray Github](https://github.com/ray-project/ray)

[Ray Tune Docs](https://ray.readthedocs.io/en/latest/tune.html)

[Tune: A Research Platform for Distributed Model Selection and Training](https://arxiv.org/pdf/1807.05118.pdf)

<!--  -->

[Cutting edge hyperparameter tuning with Ray Tune](https://medium.com/riselab/cutting-edge-hyperparameter-tuning-with-ray-tune-be6c0447afdf)
- 译文：[用 Tune 快速进行超参数优化（Hyperparameter Tuning）](https://www.pytorchtutorial.com/use-tune-for-fast-hyperparameter-tuning/)

[Simple hyperparameter and architecture search in tensorflow with ray tune](http://louiskirsch.com/ai/ray-tune)
- 译文：[Tune：一个分布式模型选择与训练研究平台](https://blog.csdn.net/weixin_43255962/article/details/89322767)

[Ray Tune 相关 API 介绍](https://blog.csdn.net/u011254180/article/details/81175151)

https://blog.csdn.net/weixin_43255962/article/details/89307928

https://blog.csdn.net/weixin_43255962/article/details/89290174

https://blog.csdn.net/weixin_43255962/article/details/89416847

https://blog.csdn.net/weixin_43255962/article/details/89342609

https://blog.csdn.net/weixin_43255962/article/details/89012548
