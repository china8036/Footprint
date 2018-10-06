- [JVM 性能调优工具](#jvm-性能调优工具)
  - [1. jps](#1-jps)
  - [2. jstack](#2-jstack)
  - [3. jmap](#3-jmap)
  - [4. jhat](#4-jhat)
  - [5. jstat](#5-jstat)
  - [6. hprof](#6-hprof)
  - [7. javap](#7-javap)
  - [8. 调优实例](#8-调优实例)
    - [8.1. CPU 高占用排查](#81-cpu-高占用排查)
  - [9. Refer Links](#9-refer-links)

# JVM 性能调优工具

## 1. jps

## 2. jstack

## 3. jmap

## 4. jhat

## 5. jstat

## 6. hprof

## 7. javap

## 8. 调优实例

### 8.1. CPU 高占用排查

https://my.oschina.net/shipley/blog/520062

https://blog.csdn.net/u010862794/article/details/78020231

1. 通过 top 命令查看当前 CPU 情况，找到 CPU 占用高的进程 PID
1. 获取 `top -H -p<pid>` 或 `ps -mp <pid> -o THREAD,tid,time` 获取该进程的各个线程的执行情况，找到 CPU 占用高的线程 tid（并通过 `printf "%x\n" <tid>` 转化为十六进制）
1. 通过 `jstack -l <pid> | grep <tid>` 命令获取当前进程的线程栈，并在其中找到上一步的线程 tid（十六进制）的对应代码段
1. 查看对应代码，从代码层面分析是否存在不合理逻辑

## 9. Refer Links
