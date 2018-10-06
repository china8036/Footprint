- [JVM 常用参数](#jvm-常用参数)
  - [Refer Links](#refer-links)

# JVM 常用参数

JVM 参数设置规则：
- `-XX:+<option>` 启用 option，例如：-XX:+PrintGCDetails 启动打印 GC 信息的选项，其中 + 号表示 true，开启的意思。
- `-XX:-<option>` 不启用 option，例如：-XX:-PrintGCDetails 关闭启动打印 GC 信息的选项，其中 - 号表示 false，关闭的意思。
- `-XX:<option>=<number>` 设定 option 的值为数字类型，可跟单位，例如 32k, 1024m, 2g。例如：`-XX:MaxPermSize=64m`。
- `-XX:<option>=<string>` 设定 option 的值为字符串，例如： -XX:HeapDumpPath="C:\Users\Daxin\Desktop\jvmgcin"。

常用参数设置：
- `-XX:+UserLWPSynchronization` 和 `-XX:+UserBoundThreads` 指定使用的线程模型。
- `-XX:-UseBasedLocking` 禁用新型锁的优化（轻量级锁和偏向锁）。
- JVM 栈设置
  - `-Xss` 限制 JVM 栈大小
    ```bash
    java -Xss128k
    ```
- 直接内存设置
  - `-XX:MaxDirectMemorySize` 限制直接内存的大小
    ```bash
    java -Xmx20M -XX:MaxDirectMemorySize=10M
    ```
- Java 堆设置
  - `-Xms`: 设置堆的初始大小。
  - `-Xmx`: 设置堆的大小上限。
  - `-Xmn`: 设置堆中年轻代的大小。
  - `-XX:MaxPermSize=n`: 设置堆中持久代大小。
  - `-XX:NewRatio=n`: 设置年轻代和年老代的比值。如：`-XX:NewRatio=3`，表示年轻代与年老代比值为 1:3，年轻代占整个年轻代年老代和的 1/4。
  - `-XX:SurvivorRatio=n`: 年轻代中 Eden 区与两个 Survivor 区的比值。注意 Survivor 区有两个，如：3，表示 Eden:Survivor=3:2，一个 Survivor 区占整个年轻代的 1/5。

## Refer Links

[JVM 参数设置规则以及参数含义](https://blog.csdn.net/Dax1n/article/details/77163540)

[java jvm 参数 -Xms -Xmx -Xmn -Xss 调优总结](https://blog.csdn.net/xiajian2010/article/details/17376157)