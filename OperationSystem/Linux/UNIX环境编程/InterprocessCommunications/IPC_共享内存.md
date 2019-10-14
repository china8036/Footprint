- [共享内存](#共享内存)
  - [Refer Links](#refer-links)

# 共享内存

1. ftok函数生成键值

1. shmget函数创建共享内存空间

1. shmat函数获取第一个可用共享内存空间的地址

1. shmdt函数进行分离（对共享存储段操作结束时的步骤，并不是从系统中删除共享内存和结构）

1. shmctl函数进行删除共享存储空间

## Refer Links

https://blog.csdn.net/ljianhui/article/details/10253345

https://www.zhihu.com/question/29973022

https://blog.csdn.net/Al_xin/article/details/38602093

https://stackoverflow.com/questions/19518607/shared-memory-whats-the-difference-between-the-key-and-the-id

https://blog.csdn.net/qq_27664167/article/details/81277096
