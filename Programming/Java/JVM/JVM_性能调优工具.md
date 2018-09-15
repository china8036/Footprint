- [JVM 性能调优工具](#jvm-%E6%80%A7%E8%83%BD%E8%B0%83%E4%BC%98%E5%B7%A5%E5%85%B7)
  - [1. jps](#1-jps)
  - [2. jstack](#2-jstack)
  - [3. jmap](#3-jmap)
  - [4. jhat](#4-jhat)
  - [5. jstat](#5-jstat)
  - [6. hprof](#6-hprof)
  - [7. 调优实例](#7-%E8%B0%83%E4%BC%98%E5%AE%9E%E4%BE%8B)
    - [7.1. CPU 高占用排查](#71-cpu-%E9%AB%98%E5%8D%A0%E7%94%A8%E6%8E%92%E6%9F%A5)
  - [8. Refer Links](#8-refer-links)

# JVM 性能调优工具

## 1. jps

## 2. jstack

## 3. jmap

## 4. jhat

## 5. jstat

## 6. hprof

## 7. 调优实例

### 7.1. CPU 高占用排查

https://my.oschina.net/shipley/blog/520062

https://blog.csdn.net/u010862794/article/details/78020231

1. 通过 top 命令查看当前 CPU 情况，找到 CPU 占用高的进程 PID
1. 获取 `top -H -p<pid>` 或 `ps -mp <pid> -o THREAD,tid,time` 获取该进程的各个线程的执行情况，找到 CPU 占用高的线程 tid（并通过 `printf "%x\n" <tid>` 转化为十六进制）
1. 通过 `jstack -l <pid> | grep <tid>` 命令获取当前进程的线程栈，并在其中找到上一步的线程 tid（十六进制）的对应代码段
1. 查看对应代码，从代码层面分析是否存在不合理逻辑

## 8. Refer Links
