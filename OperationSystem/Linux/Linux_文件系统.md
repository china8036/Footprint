- [Linux 文件系统](#linux-文件系统)
  - [1. 文件描述符](#1-文件描述符)
  - [2. 文件属性](#2-文件属性)
    - [2.1. 文件大小](#21-文件大小)
    - [2.2. 文件类型](#22-文件类型)
    - [2.3. 文件权限](#23-文件权限)
    - [2.4. 文件时间](#24-文件时间)
  - [3. 目录结构](#3-目录结构)
  - [4. /etc](#4-etc)
  - [5. /mnt](#5-mnt)
  - [6. /proc 伪文件系统](#6-proc-伪文件系统)
    - [6.1. 基本概念](#61-基本概念)
    - [6.2. 内核信息模块](#62-内核信息模块)
    - [6.3. 进程信息模块](#63-进程信息模块)
  - [7. Refer Links](#7-refer-links)

# Linux 文件系统

## 1. 文件描述符

在 UNIX 操作系统中，所有的外围设备都会被看作是文件系统中的文件，因此所有的输入 / 输出都要通过读文件 / 写文件来完成，也就是说通过一个单一的接口就可以处理外围设备和程序之间的所有通信。

文件描述符类似于 C 标准库中的文件指针或 MS-DOS 中的文件句柄，任何时候对文件的输入 / 输出都是通过文件描述符标识文件，而不是通过文件名。系统负责维护已打开文件的所有信息，二用户程序只能通过文件描述符来引用文件。

## 2. 文件属性

### 2.1. 文件大小

### 2.2. 文件类型

- b      block (buffered) special

- c      character (unbuffered) special

- d      directory

- p      named pipe (FIFO)

- f      regular file

- l      symbolic  link;  this  is never true if the -L option or the -follow option is in effect, unless the symbolic link is broken.  If you want to search for symbolic links when -L is in effect, use -xtype.

- s      socket

### 2.3. 文件权限

### 2.4. 文件时间

Linux 系统中，文件时间有以下 3 种类型，这 3 种类型是文件的标准时间属性，具有跨文件系统的特性：
- atime(Access Time): 访问时间。读取一次文件的内容，该时间便会更新。例如，对这个文件使用 less 命令或者 more 命令（ls、stat 这样的命令不会修改文件访问时间）。使用 `ls -ul` 看到的时间即为 atime。
- mtime(Modify Time): 修改时间。对文件内容修改一次便会更新该时间。例如使用 vim 等工具更改了文件内容并保存后，该时间会更新。使用 `ls -l` 看到的时间即为 mtime。
- ctime(Change Time): 改变时间。更改文件的属性便会更新该时间，比如使用 chmod 命令更改文件属性，或者执行其他命令时隐式的附带更改了文件的属性若文件大小等，该时间会更新。使用 `ls -t` 看到的时间即为 ctime。

https://link.zhihu.com/?target=http%3A//www.answers.com/Q/Why_doesn%2527t_Linux_store_file_creation_time

https://www.zhihu.com/question/21071680

在 Linux 中，文件没有创建时间：
- 如果文件没有修改过，那么创建时间就等于 mtime，即最后一次 write 时间。
- 如果文件没有被修改权限过，那么创建时间就等于 ctime，即最后一次使用 chmod 的时间。
- 如果文件没有被读取过，那么创建时间就等于 atime，即最后一个 read 的时间。

## 3. 目录结构

## 4. /etc

## 5. /mnt

## 6. /proc 伪文件系统

### 6.1. 基本概念

Linux 内核提供了一种通过 /proc 文件系统，在运行时访问内核内部数据结构、改变内核设置的机制。

/proc 文件系统是一个伪文件系统，**它只存在内存当中，而不占用外存空间**，以文件系统的方式为访问系统内核数据的操作提供接口。

**用户和应用程序可以通过 /proc 得到系统的信息，并可以改变内核的某些参数**。由于系统的信息，如进程，是动态改变的，所以用户或应用程序读取 proc 文件时，proc 文件系统是动态从系统内核读出所需信息并提交的。

### 6.2. 内核信息模块

- 只读文件（夹）

  下面列出的这些文件或子文件夹，并不是都是在你的系统中存在，这取决于你的内核配置和装载的模块：
  - /proc/buddyinfo 每个内存区中的每个 order 有多少块可用，和内存碎片问题有关
  - /proc/cmdline 启动时传递给 kernel 的参数信息
  - /proc/cpuinfo cpu 的信息
  - /proc/crypto 内核使用的所有已安装的加密密码及细节
  - /proc/devices 已经加载的设备并分类
  - /proc/dma 已注册使用的 ISA DMA 频道列表
  - /proc/execdomains Linux 内核当前支持的 execution domains
  - /proc/fb 帧缓冲设备列表，包括数量和控制它的驱动
  - /proc/filesystems 内核当前支持的文件系统类型
  - /proc/interrupts x86 架构中的每个 IRQ 中断数
  - /proc/iomem 每个物理设备当前在系统内存中的映射
  - /proc/ioports 一个设备的输入输出所使用的注册端口范围
  - /proc/kcore 代表系统的物理内存，存储为核心文件格式，里边显示的是字节数，等于 RAM 大小加上 4kb
  - /proc/kmsg 记录内核生成的信息，可以通过 /sbin/klogd 或 /bin/dmesg 来处理
  - /proc/loadavg 根据过去一段时间内 CPU 和 IO 的状态得出的负载状态，与 uptime 命令有关
  - /proc/locks 内核锁住的文件列表
  - /proc/mdstat 多硬盘，RAID 配置信息 (md=multiple disks)
  - /proc/meminfo RAM 使用的相关信息
  - /proc/misc 其他的主要设备（设备号为 10) 上注册的驱动
  - /proc/modules 所有加载到内核的模块列表
  - /proc/mounts 系统中使用的所有挂载
  - /proc/mtrr 系统使用的 Memory Type Range Registers (MTRRs)
  - /proc/partitions 分区中的块分配信息
  - /proc/pci 系统中的 PCI 设备列表
  - /proc/slabinfo 系统中所有活动的 slab 缓存信息
  - /proc/stat 所有的 CPU 活动信息
  - /proc/sysrq-trigger 使用 echo 命令来写这个文件的时候，远程 root 用户可以执行大多数的系统请求关键命令，就好像在本地终端执行一样。要写入这个文件，需要把 /proc/sys/kernel/sysrq 不能设置为 0。这个文件对 root 也是不可读的
  - /proc/uptime 系统已经运行了多久
  - /proc/swaps 交换空间的使用情况
  - /proc/version Linux 内核版本和 gcc 版本
  - /proc/bus 系统总线 (Bus) 信息，例如 pci/usb 等
  - /proc/driver 驱动信息
  - /proc/fs 文件系统信息
  - /proc/ide ide 设备信息
  - /proc/irq 中断请求设备信息
  - /proc/net 网卡设备信息
  - /proc/scsi scsi 设备信息
  - /proc/tty tty 设备信息
  - /proc/net/dev 显示网络适配器及统计信息
  - /proc/vmstat 虚拟内存统计信息
  - /proc/vmcore 内核 panic 时的内存映像
  - /proc/diskstats 取得磁盘信息
  - /proc/schedstat kernel 调度器的统计信息
  - /proc/zoneinfo 显示内存空间的统计信息，对分析虚拟内存行为很有用

- 可写文件（夹）

  在 /proc 下还有一个很重要的目录：**`/proc/sys`，该目录是可写的，可以通过它来访问或修改内核的参数**。如果优化更改了参数，可以把它们添加到 rc.local 中，使它在系统启动时自动完成修改。

  eg:  /proc/sys/fs/file-max 文件指定了可以分配的文件句柄的最大数目，默认设置为 4096。如果用户得到的错误消息声明由于打开文件数已经达到了最大值，从而他们不能打开更多文件，则可能需要增加该值。可将这个值设置成有任意多个文件，并且能通过将一个新数字值写入该文件来更改该值。

### 6.3. 进程信息模块

/proc 下还有一些以数字命名的目录，它们是进程目录。系统中当前运行的每一个进程在 `/proc` 下都有一个对应的目录，以进程的 PID 为目录名，它们是读取进程信息的接口。而 self 目录则是读取进程本身的信息接口，是一个 link。

- /proc/N pid 为 N 的进程信息
- /proc/N/cmdline 进程启动命令
- /proc/N/cwd 链接到进程当前工作目录
- /proc/N/environ 进程环境变量列表
- /proc/N/exe 链接到进程的执行命令文件
- /proc/N/fd 包含进程相关的所有的文件描述符
- /proc/N/maps 与进程相关的内存映射信息
- /proc/N/mem 指代进程持有的内存，不可读
- /proc/N/root 链接到进程的根目录
- /proc/N/stat 进程的状态
- /proc/N/statm 进程使用的内存的状态
- /proc/N/status 进程状态信息，比 stat/statm 更具可读性
- /proc/self 链接到当前正在运行的进程

## 7. Refer Links

[linux 下文件的创建时间、访问时间、修改时间和改变时间](https://blog.csdn.net/zyz511919766/article/details/14452027)

TODO:

https://blog.csdn.net/vanbreaker/article/category/1285392
