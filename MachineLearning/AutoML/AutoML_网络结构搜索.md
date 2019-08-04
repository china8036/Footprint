- [网络结构搜索](#网络结构搜索)
  - [Refer Links](#refer-links)

# 网络结构搜索

网络结构也可以看作一种超参数，但是搜索空间（大于 10^10）相对非常大，同时场景更加明确，因此可以作为单独的研究课题。

现有网络结构一般都使用研究员通过经验和实验发现的效果表现优异的网络结构，但是往往不是最优结构，同时在不同的数据集上表现也会具有差异性，因此能否通过自动搜索网络结构的方式去发现更优秀的网络结构？

学术界的主要研究思路为 RL(Reinforcement Learning)/Evolution/Darts。

一直以来，网络结构的设计是一个非常需要经验且具有挑战性的工作，研究人员从设计功能更加强大和更加高效的模型两个方向进行研究。随着各类经典网络设计思想的完善，如今要手工设计出更强大的模型已经很难，而以 AutoML 为代表的技术在三年前开始被研究。Google 首次提出了自动设计网络模型的思想，利用增强学习进行最佳架构的搜索。学习方法如下，基本思想是从一个定义空间中选取网络组件，使用网络的准确率作为指导指标，使用强化学习进行学习。

![image](http://img.cdn.firejq.com/jpg/2019/7/31/9ded9dae283b90ff8db925607d3594e6.jpg)

现在 NAS 算法所用的基本结构和模块都是已有的模块，未来的方向应该是更广阔的搜索空间。

[1] Zoph B, Le Q V. Neural Architecture Search with Reinforcement Learning[J]. international conference on learning representations, 2017.
[2] Zoph B, Vasudevan V, Shlens J, et al. Learning Transferable Architectures for Scalable Image Recognition[J]. computer vision and pattern recognition, 2018: 8697-8710.
[3] Elsken T, Metzen J H, Hutter F, et al. Neural Architecture Search: A Survey[J]. Journal of Machine Learning Research, 2018, 20(55): 1-21.

## Refer Links
