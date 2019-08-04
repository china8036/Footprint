- [AutoML 基础 (Automated Machine Learning)](#automl-基础-automated-machine-learning)
  - [Refer Links](#refer-links)

# AutoML 基础 (Automated Machine Learning)

近年来，机器学习取得了巨大的成功，特别是深度学习已经在许多领域取得重大进展，例如计算机视觉，语音处理和游戏。然而，机器学习的应用需要大量的人工干预。工程师需要做特征工程，模型选择，超参数优化，神经网络架构调整等方面的工作。这些工作都对算法的性能有很大的影响，于是机器学习工程师也被戏称为调参工程师。

传统的 ML 流程仍依赖于人力，但并非所有企业都有资源来投资经验丰富的数据科学团队，AutoML 可能正是这种困境的一个答案。**自动机器学习（AutoML）以自动化的方式完成特征工程，模型选择，超参数优化，神经网络架构调整等相关工作，不仅极大的降低了机器学习的使用门槛，而且减轻了“调参工程师”的工作负担，给机器学习算法提供最佳的决策方案**。AutoML 是近年来各大机器学习平台发展的重要模块之一，AutoML 使机器学习真正意义上成为可能，即使对于在该领域没有专业知识的人也是如此。AutoML 倾向于尽可能多地自动化 ML 管道中步骤，在只需最少人力的情况下仍保持模型的性能。

传统的机器学习模型包括以下过程：

![image](http://img.cdn.firejq.com/jpg/2019/7/14/03746c40648760f8dce3ebd2c4878d2c.jpg)

从摄取数据到预处理、优化，然后预测结果，每个步骤都由人来控制和执行。AutoML 主要关注两个主要方面：数据采集和预测。中间发生的所有其他步骤都可以轻松实现自动化，同时提供经过优化并准备好进行预测的模型。

优点：
- 通过自动执行的重复性任务来提高工作效率。这使得数据科学家能够更多地关注问题而不是模型。
- 自动化 ML 管道还有助于避免可能因手动引入的错误。
- 最后，AutoML 是向机器学习民主化迈出的一步，它使所有人都能使用 ML 的功能。


广义上来讲AutoML包括超参数优化、自动特征工程、自动模型选择、神经网络结构搜索、元学习等，几乎覆盖机器学习的每一个流程：
- 神经网络结构搜索 (Neural Architecture Search, NAS)
- 元学习（Meta Learning），即对模型各部分组件的学习，如自动搜索最佳的激活函数（activation function）、自动生成数据增强函数（data augmentation）等。
- 超参数优化 (Hyperparameter Optimization)：相比 NAS 和 Meta-Learning，超参调优是工业界模型中更急需的功能，可以看到谷歌的云计算平台 GCP、亚马逊的 SageMaker 以及微软的 NNI 里都整合了超参调优功能。
- 机器学习管线的自动化：包括数据预处理，特征工程 (Feature engineering)，模型选择 (Model selection)，以及结果分析的自动化。

## Refer Links

[如何评价谷歌 AUTOML(LEARNING TO LEARN)？](https://www.zhihu.com/question/59986054)

[机智AutoML：大规模并行实验调度及超参调优](https://zhuanlan.zhihu.com/p/72964306)

TODO:

https://www.automl.org/book/