
https://www.zhihu.com/question/35484429

https://www.zhihu.com/question/34206138

https://www.zhihu.com/question/19606660

学习内核有四本大部头比较经典：
- linux 内核设计与实现，简称 LKD，这本书是内核调度器主要设计者编写的，作为内核入门书籍再适合不过，中文翻译尚可；
- 深入理解 linux 内核，简称 ULK，这本书好像推荐的人蛮多，但是个人不推荐，首先它的翻译真的很烂，另外对于原理的描述也相对匮乏；
- 深入理解 linux 内核架构，简称 LKA，相对 LKD 其内容更加详实，原理的描述更加细致，中文翻译尚可，作为进阶阅读，个人认为是最佳选择；
- linux 设备驱动，简称 LDD，专门描述内核模块开发。接着是一个硬件平台，强烈推荐树莓派，不仅有丰富的接口，还有官方源代码，简单地进行配置即可编译生成一个完整的 linux 镜像。

学习的路线简单描述的话，先读一遍 LKD，对内核有一个大致的印象；参照实际源代码阅读 LKA；实际开发一些玩具内核模块，在树莓派中运行。

最后，是源代码阅读工具，我使用了 **vim + ctags + taglist** 的组合，分别在源代码根目录与 arch/x86 下生成了两个 tag，使用还算顺手，win 环境下据说 source insight 不错。
