- [Linux 各种限制](#linux-各种限制)
- [ulimit -a](#ulimit--a)
  - [1. Linux 理论最大进程数](#1-linux-理论最大进程数)
  - [2. Linux 理论最大线程数](#2-linux-理论最大线程数)
  - [3. Linux 最大打开文件数](#3-linux-最大打开文件数)
- [cat /proc/sys/fs/file-max](#cat-procsysfsfile-max)
  - [4. Refer Links](#4-refer-links)
- [字节序](#字节序)
- [文件指针 / 句柄 & 文件描述符 & 文件路径](#文件指针--句柄--文件描述符--文件路径)

TODO: [计算机底层知识拾遗](https://blog.csdn.net/column/details/computer-os-network.html)

# Linux 各种限制

可以使用 `ulimit -a` 查看我们系统的大部分限制：
```bash
# ulimit -a
core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 118271
max locked memory       (kbytes, -l) 64
max memory size         (kbytes, -m) unlimited
open files                      (-n) 1024
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 8192
cpu time               (seconds, -t) unlimited
max user processes              (-u) 118271
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited
```

## 1. Linux 理论最大进程数

- PID 限制

  Linux 内核通过进程标识值 PID(process identification value) 来标示进程，PID 是一个 pid_t 类型的数，实际上就是 int 类型。通过查看 /proc/sys/kernel/pid_max 可得到当前系统中的 pid 上限：
  ```bash
  # cat /proc/sys/kernel/pid_max 
  32768
  ```
  在 64 位系统中 pid_max 默认为 32768，因此最大进程数不能超过 32798。

- GDT 限制
  - 每个进程都要在全局段描述表 GDT 中占据两个表项
    
    每个进程的局部段描述表 LDT 都作为一个独立的段而存在，在全局段描述表 GDT 中要有一个表项指向这个段的起始地址，并说明该段的长度以及其他一些参数。除上之外，每个进程还有一个 TSS 结构（任务状态段) 也是一样。所以，每个进程都要在全局段描述表 GDT 中占据两个表项。

  - GDT 的容量有多大呢？
    
    段寄存器中用作 GDT 表下标的位段宽度是 13 位，所以 GDT 中可以有 2^13=8192 个描述项。

    除其中固定用于系统的开销的 12 项（例如 GDT 中的第 2 项和第 3 项分别用于内核的代码段和数据段，第 4 项和第 5 项永远用于当前进程的代码段和数据段，第 1 项永远是 0 等）以外，尚有 8192-12=**8180** 个表项可供使用，所以理论上系统中最大的进程数不能超过 8180/2=**4090**。

所以系统中理论上最大的进程数是 4090。

## 2. Linux 理论最大线程数

- pthread 限制
  
  查看 /usr/include/bits/local_lim.h 可知，pthread 限制了一个进程内可以创建的 PTHREAD_KEYS 上限为 1024：
  ```
  /* This is the value this implementation supports.  */
  #define PTHREAD_KEYS_MAX    1024
  ```
  因此，一个进程内能创建的最大线程数不超过 1024。

- 内存资源限制

  Linux 中最大线程数一般受限于系统的内存资源，即线程的 stack 所占用的内存资源。用 `ulimit -s` 可以查看系统中默认的线程栈的大小限制，一般情况下，这个值是 8192KB=8MB。

  若在 32 位系统中，用户进程的内存空间大小为 2^32=3GB=3072MB，因此一个进程中可以创建的最大线程数理论上是 3072MB/8MB=384。除去 Code 段、BBS 段和 Data 段的开销，这个上限应下降到 383。再除去进程的默认主线程和管理线程，在一个进程中可以创建的最大线程数理论上是 382。

  通过减小线程栈的大小，可以突破 382 的线程数限制：
  - `ulimit -s 1024` 来减小默认的栈大小
  - 调用 pthread_create 的时候用 pthread_attr_getstacksize 设置一个较小的栈大小

  但需要注意，即使突破了 382 的限制，仍无法突破 pthread 的 1024 限制，除非更改参数设置后重新编译 C 库。

## 3. Linux 最大打开文件数

/proc/sys/fs/file-max 指定了系统范围内当前系统中单进程可打开的文件描述符数目（系统级别，kernel-level）：
```
# cat /proc/sys/fs/file-max
1504597
```
当收到“Too many open files in system”这样的错误消息时，就应该增加这个值了。

## 4. Refer Links

[linux 下进程的进程最大数、最大线程数、进程打开的文件数和 ulimit 命令修改硬件资源限制](https://blog.csdn.net/gatieme/article/details/51058797)

# 字节序

https://zh.wikipedia.org/zh-hans/%E5%AD%97%E8%8A%82%E5%BA%8F

http://www.ruanyifeng.com/blog/2016/11/byte-order.html

https://www.cnblogs.com/onepixel/p/7468343.html

https://blog.csdn.net/K346K346/article/details/79053136

字节序，顾名思义就是字节的顺序，就是大于一个字节类型的数据在内存中的存放顺序（一个字节的数据当然就无需谈顺序的问题了）。 

- 主机字节序

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/10/19/5ce1dbc0feb12be92794be63d1ed956e.jpg)

  CPU 存储数据时采用的字节顺序。不同的 CPU 设计时采用的字节序是不同的，谈到字节序的问题，必然牵涉到两大 CPU 派系。那就是 Motorola 的 PowerPC 系列 CPU 和 Intel 的 x86 与 x86_64（该指令集由 AMD 率先设计推出）系列 CPU：
  - PowerPC 系列采用 big endian 方式存储数据。
  - x86 与 x86_64 系列则采用 little endian 方式存储数据。
  平常大多数 PC 与服务器如果使用的是 Intel 与 AMD CPU，因此一般都是 little endian。

  如何具体判断本机的主机字节序呢？
  ```c
  //
  //@ret：返回 0 小端字节序，返回 1 大端字节序
  //
  int dGetHostByteOrder()
  {
      uint32_t a = 0x12345678;  
      uint8_t *p = (uint8_t *)(&a);  
      if(*p==0x78)
      {
          return 0
      }
      else
      {
          return 1;
      }
  }
  ```

- 网络字节序 -- big endian

  网络字节序，是 TCP/IP 中规定好的一种数据表示格式，它与具体的 CPU 类型、操作系统等无关，从而可以保证数据在不同主机之间传输时能够被正确解释。网络字节顺序采用 big endian 排序方式。IP 数据报头的传输从左到右，再从上到下。**Version 字段的高序字节最先被传送出去，因此 little-endian 字节序的机器（如 Intel x86）在传输和接收 IP 报文时需要进行字节序转换**。

- 字节序转换

  Linux socket 网络编程中，经常会使用下面四个 C 标准库函数进行字节序间的转换：
  ```c
  #include <arpa/inet.h>

  uint32_t htonl(uint32_t hostlong);      // 把 uint32_t 类型从主机序转换到网络序
  uint16_t htons(uint16_t hostshort);     // 把 uint16_t 类型从主机序转换到网络序
  uint32_t ntohl(uint32_t netlong);       // 把 uint32_t 类型从网络序转换到主机序
  uint16_t ntohs(uint16_t netshort);      // 把 uint16_t 类型从网络序转换到主机序
  ```
  
# 文件指针 / 句柄 & 文件描述符 & 文件路径

https://www.cnblogs.com/niocai/archive/2011/11/24/2261686.html