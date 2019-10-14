
https://www.zhihu.com/question/26895729

简单的说，kubernetes是管理container的工具，openstack是管理VM的工具。

container可以运行在物理机上，也可以运行在VM上。所以kubernetes不是需要openstack的支持。但对于云计算来说，很多IasS都通过openstack来管理虚拟机。然后用户可以在这些虚拟机上运行docker，可以通过kubernetes进行管理。

<!-- ---- -->


OpenStack定位为 数据中心操作系统，面向基础设施资源，OpenStack不仅仅管理计算资源，还包括网络，存储资源的自动管理。

OpenStack = 计算资源管理（主要最成熟的是虚机） + 存储资源管理 + 网络资源管理

Kubernetes定位为基于容器的集群管理系统，直接面向cloudnative应用：

Kubernetes = 容器资源管理 + 集群编排

Kubernetes可以直接跑在裸机上，这个时候这些裸机上需要的存储，网络资源等都需要依赖手工配置，对于小规模数据中心或者业务稳定不变的应用没有问题，对于大规模数据中心或者应用频繁变化的应用，仍然依赖手工就非常不方便了，

所以采用OpenStack+Kubernetes 是目前相对完整的云应用解决方案栈

<!-- --- -->

现在主流的是底层用openstack 上层用k8s。当然前提是有一定规模，规模小直接裸机跑k8s就好了。规模大点的底层虚拟机，再在虚拟机上跑容器。