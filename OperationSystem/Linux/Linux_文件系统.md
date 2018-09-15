- [Linux 文件系统](#linux-%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F)
  - [1. 目录结构](#1-%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84)
  - [2. /etc](#2-etc)
  - [3. /mnt](#3-mnt)
  - [4. /proc 伪文件系统](#4-proc-%E4%BC%AA%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F)
    - [4.1. 基本概念](#41-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
    - [4.2. 子文件（夹）](#42-%E5%AD%90%E6%96%87%E4%BB%B6%EF%BC%88%E5%A4%B9%EF%BC%89)
      - [4.2.1. 内核信息模块](#421-%E5%86%85%E6%A0%B8%E4%BF%A1%E6%81%AF%E6%A8%A1%E5%9D%97)
      - [4.2.2. 进程信息模块](#422-%E8%BF%9B%E7%A8%8B%E4%BF%A1%E6%81%AF%E6%A8%A1%E5%9D%97)
  - [5. Refer Links](#5-refer-links)

# Linux 文件系统

## 1. 目录结构

## 2. /etc

## 3. /mnt

## 4. /proc 伪文件系统

### 4.1. 基本概念

Linux 内核提供了一种通过 /proc 文件系统，在运行时访问内核内部数据结构、改变内核设置的机制。

/proc 文件系统是一个伪文件系统，**它只存在内存当中，而不占用外存空间**，以文件系统的方式为访问系统内核数据的操作提供接口。

用户和应用程序可以通过 proc 得到系统的信息，并可以改变内核的某些参数。由于系统的信息，如进程，是动态改变的，所以用户或应用程序读取 proc 文件时，proc 文件系统是动态从系统内核读出所需信息并提交的。

### 4.2. 子文件（夹）

#### 4.2.1. 内核信息模块

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

  在 /proc 下还有一个很重要的目录：/proc/sys，该目录是可写的，可以通过它来访问或修改内核的参数。如果优化更改了参数，可以把它们添加到 rc.local 中，使它在系统启动时自动完成修改。

  eg:  /proc/sys/fs/file-max 文件指定了可以分配的文件句柄的最大数目，默认设置为 4096。如果用户得到的错误消息声明由于打开文件数已经达到了最大值，从而他们不能打开更多文件，则可能需要增加该值。可将这个值设置成有任意多个文件，并且能通过将一个新数字值写入该文件来更改该值。

#### 4.2.2. 进程信息模块

/proc 下还有一些以数字命名的目录，它们是进程目录。系统中当前运行的每一个进程在 /proc 下都有一个对应的目录，以进程的 PID 为目录名，它们是读取进程信息的接口。而 self 目录则是读取进程本身的信息接口，是一个 link。

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

## 5. Refer Links

TODO:

https://blog.csdn.net/vanbreaker/article/category/1285392
