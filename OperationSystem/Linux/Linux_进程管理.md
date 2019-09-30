- [Linux 进程管理](#linux-进程管理)
  - [1. 基本概念](#1-基本概念)
  - [2. 进程类型](#2-进程类型)
  - [3. 进程结构](#3-进程结构)
  - [4. 进程状态](#4-进程状态)
  - [5. Refer Links](#5-refer-links)

# Linux 进程管理

## 1. 基本概念

在 Unix/Linux 中，正常情况下子进程是通过父进程创建的，而父进程永远无法预测子进程到底什么时候结束。**当一个进程完成它的工作终止之后，它的父进程需要调用 wait() 或者 waitpid() 系统调用取得子进程的终止状态**。

- 孤儿进程

  **一个父进程退出，而它的一个或多个子进程还在运行**，那么那些子进程就称为孤儿进程。

  孤儿进程将被 init 进程（进程号 PID 为 1）所收养，并由 init 进程对它们完成状态收集工作。孤儿进程并不会有什么危害。

- 僵尸进程

  一个进程使用 fork 创建了子进程后，如果父进程没有调用 wait 或 waitpid 获取子进程的状态信息，会导致**子进程执行完毕退出后，子进程的进程描述符却仍保存在系统中**。这时子进程就称为僵尸进程。

  在每个进程退出的时候，内核会释放该进程的资源，包括打开的文件、占用的内存等。但仍会为其保留一定的信息（包括进程号、退出状态、运行时间等），直到父进程通过 wait / waitpid 来取时才完全释放。因此，如果进程不调用 wait / waitpid 的话， 那么保留的那段信息就不会释放，其进程号就会一直被占用，但是**系统所能使用的进程号是有限的，如果大量的产生僵尸进程，将因为没有可用的进程号而导致系统不能产生新的进程**。

- 用户态和内核态

  Linux 使用两级保护机制：0 级供内核使用，3 级供用户程序使用。

  - 当一个任务（进程）执行系统调用而陷入内核代码中执行时，称进程处于内核运行态（内核态）。此时处理器处于特权级最高的（0 级）内核代码中执行。当进程处于内核态时，执行的内核代码会使用当前进程的内核栈。每个进程都有自己的内核栈。

  - 当进程在执行用户自己的代码时，则称其处于用户运行态（用户态）。此时处理器在特权级最低的（3 级）用户代码中运行。当正在执行用户程序而突然被中断程序中断时，此时用户程序也可以象征性地称为处于进程的内核态。因为中断处理程序将使用当前进程的内核栈。

- 进程上下文和中断上下文

  - 进程上下文：可以看作是用户进程通过系统调用陷入内核态时，传递给内核的参数以及内核要保存的那一整套的变量和寄存器值和当时的环境等。

    一个进程的上下文可以分为三个部分：
    - 用户级上下文：正文、数据、用户堆栈以及共享存储区。
    - 寄存器上下文：通用寄存器、程序寄存器 (IP)、处理器状态寄存器 (EFLAGS)、栈指针 (ESP)。
    - 系统级上下文：进程控制块 task_struct、内存管理信息 (mm_struct、vm_area_struct、pgd、pte)、内核栈。

    当发生进程调度时，进行进程切换就是上下文切换 (context switch). 操作系统必须对上面提到的全部信息进行切换，新调度的进程才能运行。而系统调用进行的模式切换 (mode switch)。模式切换与进程切换比较起来，容易很多，而且节省时间，因为模式切换最主要的任务只是切换进程寄存器上下文的切换。

  - 中断上下文：可以看作是硬件通过触发信号，导致内核调用中断处理程序，进入内核空间时，传递的参数和内核需要保存的一些其他环境（主要是当前被打断执行的进程环境）。

    中断时，内核不代表任何进程运行，它一般只访问系统空间，而不会访问进程空间，内核在中断上下文中执行时一般不会阻塞。

- 硬中断和软中断

  从本质上来讲，中断是一种电信号，当设备有某种事件发生时，它就会产生中断，通过总线把电信号发送给中断控制器。如果中断的线是激活的，中断控制器就把电信号发送给处理器的某个特定引脚。处理器于是立即停止自己正在做的事，跳到中断处理程序的入口点，进行中断处理。
  - 硬中断：由与系统相连的外设（比如网卡、硬盘) 自动产生的。主要是用来通知操作系统系统外设状态的变化。比如当网卡收到数据包的时候，就会发出一个中断。我们通常所说的中断指的是硬中断 (hardirq)。
    - 硬中断是由外设引发的。
    - 硬中断的中断号是由中断控制器提供的。
    - 硬中断是可屏蔽的。
    - 硬中断处理程序要确保它能快速地完成任务，这样程序执行时才不会等待较长时间，属于上半部。

  - 软中断：为了满足实时系统的要求，中断处理应该是越快越好。linux 为了实现这个特点，当中断发生的时候，硬中断处理那些短时间就可以完成的工作，而将那些处理事件比较长的工作，放到中断之后来完成，也就是软中断 (softirq) 来完成。
    - 软中断是执行中断指令产生的。
    - 软中断的中断号由指令直接指出，无需使用中断控制器。
    - 软中断不可屏蔽。
    - 软中断处理硬中断未完成的工作，是一种推后执行的机制，属于下半部。

## 2. 进程类型

在 Linux 系统中存在不同类型的进程，包括用户进程、守护进程和内核进程。

- 守护进程

  守护进程是一种被设计用来进行后台运行的程序，通常用来管理一些持续运行的服务。一个守护进程可以用来监听到达某一服务的请求。例如，httpd 的守护进程就用来监听网页的请求。或者守护请求可以根据时间的推移来自己启动一些任务，如 crond 的守护进程就设计用来在某个时刻启动 cron 任务。

  虽然守护进程通常被 root 用户以服务来运行，但是守护进程也常常被非 root 用户作为服务来运行。在不同的用户下运行守护进程，会给系统在某些攻击行为下以一定程度的保护。例如，如果一个攻击者攻击并接管了由 Apache 用户运行的 httpd 守护进程的权限，这个攻击者并不能额外的获得属于其他用户（包含 root 用户）拥有的文件及守护进程的访问权限。

  操作系统经常在系统启动的时候加载守护进程，并且一直持续不断的运行直到系统关闭。守护进程可以在需要的时候启动、关闭或者被设置运行为特殊的系统运行级别，并且在一些场景下可以在运行时动态的加载配置信息（如 crond 就可以在运行时动态加载配置文件无须重启）。

- 内核进程

  内核进程只会运行在内核空间。它们和守护进行比较类似，所不同的是内核进程对内核空间的数据具有无限的访问权限，因而使得它们的功能比运行的用户空间的守护进程更为强大。

  内核进程通常不会像守护进程那么灵活。你可以通过改变守护进程的配置以及重新启动服务来改变其行为。然而如果要改变内核进程的功能就需要重新编译内核。

- 用户进程

  操作系统中的大多数进程都属于用户进程，一个用户进程由普通用户初始化并运行在用户进程空间中，除非通过一些方式给予该进程特殊的权限，否则该用户加载的普通用户进程并没有权限去访问不属于该用户的处理器或者文件系统。

## 3. 进程结构

https://www.cnblogs.com/JohnABC/p/9084750.html

系统中把所有进程控制块组织为如下图所示的双向链表：

![image](http://img.cdn.firejq.com/jpg/2019/9/7/360800c593248477381870afb92cacd5.jpg)

task_struct 结构体包含了一个进程所需的所有信息。它定义在 linux/include/linux/sched.h 文件中：
```c
struct task_struct
{
    /*
    1. state: 进程执行时，它会根据具体情况改变状态。进程状态是进程调度和对换的依据。Linux 中的进程主要有如下状态：
        1) TASK_RUNNING: 可运行
        处于这种状态的进程，只有两种状态：
            1.1) 正在运行
            正在运行的进程就是当前进程（由 current 所指向的进程)
            1.2) 正准备运行
            准备运行的进程只要得到 CPU 就可以立即投入运行，CPU 是这些进程唯一等待的系统资源，系统中有一个运行队列 (run_queue)，用来容纳所有处于可运行状态的进程，调度程序执行时，从中选择一个进程投入运行

        2) TASK_INTERRUPTIBLE: 可中断的等待状态，是针对等待某事件或其他资源的睡眠进程设置的，在内核发送信号给该进程表明事件已经发生时，进程状态变为 TASK_RUNNING，它只要调度器选中该进程即可恢复执行

        3) TASK_UNINTERRUPTIBLE: 不可中断的等待状态
        处于该状态的进程正在等待某个事件 (event) 或某个资源，它肯定位于系统中的某个等待队列 (wait_queue) 中，处于不可中断等待态的进程是因为硬件环境不能满足而等待，例如等待特定的系统资源，它任何情况下都不能被打断，只能用特定的方式来唤醒它，例如唤醒函数 wake_up() 等
　　　　　它们不能由外部信号唤醒，只能由内核亲自唤醒

        4) TASK_ZOMBIE: 僵死
        进程虽然已经终止，但由于某种原因，父进程还没有执行 wait() 系统调用，终止进程的信息也还没有回收。顾名思义，处于该状态的进程就是死进程，这种进程实际上是系统中的垃圾，必须进行相应处理以释放其占用的资源。

        5) TASK_STOPPED: 暂停
        此时的进程暂时停止运行来接受某种特殊处理。通常当进程接收到 SIGSTOP、SIGTSTP、SIGTTIN 或 SIGTTOU 信号后就处于这种状态。例如，正接受调试的进程就处于这种状态
　　　　
　　　　　6) TASK_TRACED
　　　　　从本质上来说，这属于 TASK_STOPPED 状态，用于从停止的进程中，将当前被调试的进程与常规的进程区分开来
　　　　　　
　　　　　7) TASK_DEAD
　　　　　父进程 wait 系统调用发出后，当子进程退出时，父进程负责回收子进程的全部资源，子进程进入 TASK_DEAD 状态

        8) TASK_SWAPPING: 换入 / 换出
    */
    /* -1 unrunnable, 0 runnable, >0 stopped: */
    volatile long state;

    /*
    2. stack
    进程内核栈，进程通过 alloc_thread_info 函数分配它的内核栈，通过 free_thread_info 函数释放所分配的内核栈
    */
    void *stack;

    /*
    3. usage
    进程描述符使用计数，被置为 2 时，表示进程描述符正在被使用而且其相应的进程处于活动状态
    */
    atomic_t usage;

    /*
    4. flags
    flags 是进程当前的状态标志（注意和运行状态区分)
        1) #define PF_ALIGNWARN    0x00000001: 显示内存地址未对齐警告
        2) #define PF_PTRACED    0x00000010: 标识是否是否调用了 ptrace
        3) #define PF_TRACESYS    0x00000020: 跟踪系统调用
        4) #define PF_FORKNOEXEC 0x00000040: 已经完成 fork，但还没有调用 exec
        5) #define PF_SUPERPRIV    0x00000100: 使用超级用户 (root) 权限
        6) #define PF_DUMPCORE    0x00000200: dumped core
        7) #define PF_SIGNALED    0x00000400: 此进程由于其他进程发送相关信号而被杀死
        8) #define PF_STARTING    0x00000002: 当前进程正在被创建
        9) #define PF_EXITING    0x00000004: 当前进程正在关闭
        10) #define PF_USEDFPU    0x00100000: Process used the FPU this quantum(SMP only)
        #define PF_DTRACE    0x00200000: delayed trace (used on m68k)
    */
    unsigned int flags;

    /*
    5. ptrace
    ptrace 系统调用，成员 ptrace 被设置为 0 时表示不需要被跟踪，它的可能取值如下：
    linux-2.6.38.8/include/linux/ptrace.h
        1) #define PT_PTRACED    0x00000001
        2) #define PT_DTRACE    0x00000002: delayed trace (used on m68k, i386)
        3) #define PT_TRACESYSGOOD    0x00000004
        4) #define PT_PTRACE_CAP    0x00000008: ptracer can follow suid-exec
        5) #define PT_TRACE_FORK    0x00000010
        6) #define PT_TRACE_VFORK    0x00000020
        7) #define PT_TRACE_CLONE    0x00000040
        8) #define PT_TRACE_EXEC    0x00000080
        9) #define PT_TRACE_VFORK_DONE    0x00000100
        10) #define PT_TRACE_EXIT    0x00000200
    */
    unsigned int ptrace;
    unsigned long ptrace_message;
    siginfo_t *last_siginfo;

    /*
    6. lock_depth
    用于表示获取大内核锁的次数，如果进程未获得过锁，则置为 -1
    */
    int lock_depth;

    /*
    7. oncpu
    在 SMP 上帮助实现无加锁的进程切换 (unlocked context switches)
    */
#ifdef CONFIG_SMP
#ifdef __ARCH_WANT_UNLOCKED_CTXSW
    int oncpu;
#endif

    /*
    8. 进程调度
        1) prio: 调度器考虑的优先级保存在 prio，由于在某些情况下内核需要暂时提高进程的优先级，因此需要第三个成员来表示（除了 static_prio、normal_prio 之外)，由于这些改变不是持久的，因此静态 (static_prio) 和普通 (normal_prio) 优先级不受影响
        2) static_prio: 用于保存进程的"静态优先级"，静态优先级是进程"启动"时分配的优先级，它可以用 nice、sched_setscheduler 系统调用修改，否则在进程运行期间会一直保持恒定
        3) normal_prio: 表示基于进程的"静态优先级"和"调度策略"计算出的优先级，因此，即使普通进程和实时进程具有相同的静态优先级 (static_prio)，其普通优先级 (normal_prio) 也是不同的。进程分支时 (fork)，新创建的子进程会集成普通优先级
    */
    int prio, static_prio, normal_prio;
    /*
        4) rt_priority: 表示实时进程的优先级，需要明白的是，"实时进程优先级"和"普通进程优先级"有两个独立的范畴，实时进程即使是最低优先级也高于普通进程，最低的实时优先级为 0，最高的优先级为 99，值越大，表明优先级越高
    */
    unsigned int rt_priority;
    /*
        5) sched_class: 该进程所属的调度类，目前内核中有实现以下四种：
            5.1) static const struct sched_class fair_sched_class;
            5.2) static const struct sched_class rt_sched_class;
            5.3) static const struct sched_class idle_sched_class;
            5.4) static const struct sched_class stop_sched_class;
    */
    const struct sched_class *sched_class;
    /*
        6) se: 用于普通进程的调用实体
　　调度器不限于调度进程，还可以处理更大的实体，这可以实现"组调度"，可用的 CPU 时间可以首先在一般的进程组（例如所有进程可以按所有者分组) 之间分配，接下来分配的时间在组内再次分配
　　这种一般性要求调度器不直接操作进程，而是处理"可调度实体"，一个实体有 sched_entity 的一个实例标识
　　在最简单的情况下，调度在各个进程上执行，由于调度器设计为处理可调度的实体，在调度器看来各个进程也必须也像这样的实体，因此 se 在 task_struct 中内嵌了一个 sched_entity 实例，调度器可据此操作各个 task_struct
    */
    struct sched_entity se;
    /*
        7) rt: 用于实时进程的调用实体
    */
    struct sched_rt_entity rt;

#ifdef CONFIG_PREEMPT_NOTIFIERS
    /*
    9. preempt_notifier
    preempt_notifiers 结构体链表
    */
    struct hlist_head preempt_notifiers;
#endif

     /*
     10. fpu_counter
     FPU 使用计数
     */
    unsigned char fpu_counter;

#ifdef CONFIG_BLK_DEV_IO_TRACE
    /*
    11. btrace_seq
    blktrace 是一个针对 Linux 内核中块设备 I/O 层的跟踪工具
    */
    unsigned int btrace_seq;
#endif

    /*
    12. policy
    policy 表示进程的调度策略，目前主要有以下五种：
        1) #define SCHED_NORMAL        0: 用于普通进程，它们通过完全公平调度器来处理
        2) #define SCHED_FIFO        1: 先来先服务调度，由实时调度类处理
        3) #define SCHED_RR            2: 时间片轮转调度，由实时调度类处理
        4) #define SCHED_BATCH        3: 用于非交互、CPU 使用密集的批处理进程，通过完全公平调度器来处理，调度决策对此类进程给与"冷处理"，它们绝不会抢占 CFS 调度器处理的另一个进程，因此不会干扰交互式进程，如果不打算用 nice 降低进程的静态优先级，同时又不希望该进程影响系统的交互性，最适合用该调度策略
        5) #define SCHED_IDLE        5: 可用于次要的进程，其相对权重总是最小的，也通过完全公平调度器来处理。要注意的是，SCHED_IDLE 不负责调度空闲进程，空闲进程由内核提供单独的机制来处理
    只有 root 用户能通过 sched_setscheduler() 系统调用来改变调度策略
    */
    unsigned int policy;

    /*
    13. cpus_allowed
    cpus_allowed 是一个位域，在多处理器系统上使用，用于控制进程可以在哪里处理器上运行
    */
    cpumask_t cpus_allowed;

    /*
    14. RCU 同步原语
    */
#ifdef CONFIG_TREE_PREEMPT_RCU
    int rcu_read_lock_nesting;
    char rcu_read_unlock_special;
    struct rcu_node *rcu_blocked_node;
    struct list_head rcu_node_entry;
#endif /* #ifdef CONFIG_TREE_PREEMPT_RCU */

#if defined(CONFIG_SCHEDSTATS) || defined(CONFIG_TASK_DELAY_ACCT)
    /*
    15. sched_info
    用于调度器统计进程的运行信息
    */
    struct sched_info sched_info;
#endif

    /*
    16. tasks
    通过 list_head 将当前进程的 task_struct 串联进内核的进程列表中，构建；linux 进程链表
    */
    struct list_head tasks;

    /*
    17. pushable_tasks
    limit pushing to one attempt
    */
    struct plist_node pushable_tasks;

    /*
    18. 进程地址空间
        1) mm: 指向进程所拥有的内存描述符
        2) active_mm: active_mm 指向进程运行时所使用的内存描述符
    对于普通进程而言，这两个指针变量的值相同。但是，内核线程不拥有任何内存描述符，所以它们的 mm 成员总是为 NULL。当内核线程得以运行时，它的 active_mm 成员被初始化为前一个运行进程的 active_mm 值
    */
    struct mm_struct *mm, *active_mm;

    /*
    19. exit_state
    进程退出状态码
    */
    int exit_state;

    /*
    20. 判断标志
        1) exit_code
        exit_code 用于设置进程的终止代号，这个值要么是_exit() 或 exit_group() 系统调用参数（正常终止)，要么是由内核提供的一个错误代号（异常终止)
        2) exit_signal
        exit_signal 被置为 -1 时表示是某个线程组中的一员。只有当线程组的最后一个成员终止时，才会产生一个信号，以通知线程组的领头进程的父进程
    */
    int exit_code, exit_signal;
    /*
        3) pdeath_signal
        pdeath_signal 用于判断父进程终止时发送信号
    */
    int pdeath_signal;
    /*
        4)  personality 用于处理不同的 ABI，它的可能取值如下：
            enum
            {
                PER_LINUX =        0x0000,
                PER_LINUX_32BIT =    0x0000 | ADDR_LIMIT_32BIT,
                PER_LINUX_FDPIC =    0x0000 | FDPIC_FUNCPTRS,
                PER_SVR4 =        0x0001 | STICKY_TIMEOUTS | MMAP_PAGE_ZERO,
                PER_SVR3 =        0x0002 | STICKY_TIMEOUTS | SHORT_INODE,
                PER_SCOSVR3 =        0x0003 | STICKY_TIMEOUTS |
                                 WHOLE_SECONDS | SHORT_INODE,
                PER_OSR5 =        0x0003 | STICKY_TIMEOUTS | WHOLE_SECONDS,
                PER_WYSEV386 =        0x0004 | STICKY_TIMEOUTS | SHORT_INODE,
                PER_ISCR4 =        0x0005 | STICKY_TIMEOUTS,
                PER_BSD =        0x0006,
                PER_SUNOS =        0x0006 | STICKY_TIMEOUTS,
                PER_XENIX =        0x0007 | STICKY_TIMEOUTS | SHORT_INODE,
                PER_LINUX32 =        0x0008,
                PER_LINUX32_3GB =    0x0008 | ADDR_LIMIT_3GB,
                PER_IRIX32 =        0x0009 | STICKY_TIMEOUTS,
                PER_IRIXN32 =        0x000a | STICKY_TIMEOUTS,
                PER_IRIX64 =        0x000b | STICKY_TIMEOUTS,
                PER_RISCOS =        0x000c,
                PER_SOLARIS =        0x000d | STICKY_TIMEOUTS,
                PER_UW7 =        0x000e | STICKY_TIMEOUTS | MMAP_PAGE_ZERO,
                PER_OSF4 =        0x000f,
                PER_HPUX =        0x0010,
                PER_MASK =        0x00ff,
            };
    */
    unsigned int personality;
    /*
        5) did_exec
        did_exec 用于记录进程代码是否被 execve() 函数所执行
    */
    unsigned did_exec:1;
    /*
        6) in_execve
        in_execve 用于通知 LSM 是否被 do_execve() 函数所调用
    */
    unsigned in_execve:1;
    /*
        7) in_iowait
        in_iowait 用于判断是否进行 iowait 计数
    */
    unsigned in_iowait:1;

    /*
        8) sched_reset_on_fork
        sched_reset_on_fork 用于判断是否恢复默认的优先级或调度策略
    */
    unsigned sched_reset_on_fork:1;

    /*
    21. 进程标识符 (PID)
    在 CONFIG_BASE_SMALL 配置为 0 的情况下，PID 的取值范围是 0 到 32767，即系统中的进程数最大为 32768 个
    #define PID_MAX_DEFAULT (CONFIG_BASE_SMALL ? 0x1000 : 0x8000)
    在 Linux 系统中，一个线程组中的所有线程使用和该线程组的领头线程（该组中的第一个轻量级进程) 相同的 PID，并被存放在 tgid 成员中。只有线程组的领头线程的 pid 成员才会被设置为与 tgid 相同的值。注意，getpid() 系统调用
返回的是当前进程的 tgid 值而不是 pid 值。
    */
    pid_t pid;
    pid_t tgid;

#ifdef CONFIG_CC_STACKPROTECTOR
    /*
    22. stack_canary
    防止内核堆栈溢出，在 GCC 编译内核时，需要加上 -fstack-protector 选项
    */
    unsigned long stack_canary;
#endif

     /*
     23. 表示进程亲属关系的成员
         1) real_parent: 指向其父进程，如果创建它的父进程不再存在，则指向 PID 为 1 的 init 进程
         2) parent: 指向其父进程，当它终止时，必须向它的父进程发送信号。它的值通常与 real_parent 相同
     */
    struct task_struct *real_parent;
    struct task_struct *parent;
    /*
        3) children: 表示链表的头部，链表中的所有元素都是它的子进程（子进程链表)
        4) sibling: 用于把当前进程插入到兄弟链表中（连接到父进程的子进程链表（兄弟链表))
        5) group_leader: 指向其所在进程组的领头进程
    */
    struct list_head children;
    struct list_head sibling;
    struct task_struct *group_leader;

    struct list_head ptraced;
    struct list_head ptrace_entry;
    struct bts_context *bts;

    /*
    24. pids
    PID 散列表和链表
    */
    struct pid_link pids[PIDTYPE_MAX];
    /*
    25. thread_group
    线程组中所有进程的链表
    */
    struct list_head thread_group;

    /*
    26. do_fork 函数
        1) vfork_done
        在执行 do_fork() 时，如果给定特别标志，则 vfork_done 会指向一个特殊地址
        2) set_child_tid、clear_child_tid
        如果 copy_process 函数的 clone_flags 参数的值被置为 CLONE_CHILD_SETTID 或 CLONE_CHILD_CLEARTID，则会把 child_tidptr 参数的值分别复制到 set_child_tid 和 clear_child_tid 成员。这些标志说明必须改变子
进程用户态地址空间的 child_tidptr 所指向的变量的值。
    */
    struct completion *vfork_done;
    int __user *set_child_tid;
    int __user *clear_child_tid;

    /*
    27. 记录进程的 I/O 计数（时间)
        1) utime
        用于记录进程在"用户态"下所经过的节拍数（定时器)
        2) stime
        用于记录进程在"内核态"下所经过的节拍数（定时器)
        3) utimescaled
        用于记录进程在"用户态"的运行时间，但它们以处理器的频率为刻度
        4) stimescaled
        用于记录进程在"内核态"的运行时间，但它们以处理器的频率为刻度
    */
    cputime_t utime, stime, utimescaled, stimescaled;
    /*
        5) gtime
        以节拍计数的虚拟机运行时间 (guest time)
    */
    cputime_t gtime;
    /*
        6) prev_utime、prev_stime 是先前的运行时间
    */
    cputime_t prev_utime, prev_stime;
    /*
        7) nvcsw
        自愿 (voluntary) 上下文切换计数
        8) nivcsw
        非自愿 (involuntary) 上下文切换计数
    */
    unsigned long nvcsw, nivcsw;
    /*
        9) start_time
        进程创建时间
        10) real_start_time
        进程睡眠时间，还包含了进程睡眠时间，常用于 /proc/pid/stat，
    */
    struct timespec start_time;
    struct timespec real_start_time;
    /*
        11) cputime_expires
        用来统计进程或进程组被跟踪的处理器时间，其中的三个成员对应着 cpu_timers[3] 的三个链表
    */
    struct task_cputime cputime_expires;
    struct list_head cpu_timers[3];
    #ifdef CONFIG_DETECT_HUNG_TASK
    /*
        12) last_switch_count
        nvcsw 和 nivcsw 的总和
    */
    unsigned long last_switch_count;
    #endif
    struct task_io_accounting ioac;
#if defined(CONFIG_TASK_XACCT)
    u64 acct_rss_mem1;
    u64 acct_vm_mem1;
    cputime_t acct_timexpd;
#endif

    /*
    28. 缺页统计
    */
    unsigned long min_flt, maj_flt;

    /*
    29. 进程权能
    */
    const struct cred *real_cred;
    const struct cred *cred;
    struct mutex cred_guard_mutex;
    struct cred *replacement_session_keyring;

    /*
    30. comm[TASK_COMM_LEN]
    相应的程序名
    */
    char comm[TASK_COMM_LEN];

    /*
    31. 文件
        1) fs
        用来表示进程与文件系统的联系，包括当前目录和根目录
        2) files
        表示进程当前打开的文件
    */
    int link_count, total_link_count;
    struct fs_struct *fs;
    struct files_struct *files;

#ifdef CONFIG_SYSVIPC
    /*
    32. sysvsem
    进程通信 (SYSVIPC)
    */
    struct sysv_sem sysvsem;
#endif

    /*
    33. 处理器特有数据
    */
    struct thread_struct thread;

    /*
    34. nsproxy
    命名空间
    */
    struct nsproxy *nsproxy;

    /*
    35. 信号处理
        1) signal: 指向进程的信号描述符
        2) sighand: 指向进程的信号处理程序描述符
    */
    struct signal_struct *signal;
    struct sighand_struct *sighand;
    /*
        3) blocked: 表示被阻塞信号的掩码
        4) real_blocked: 表示临时掩码
    */
    sigset_t blocked, real_blocked;
    sigset_t saved_sigmask;
    /*
        5) pending: 存放私有挂起信号的数据结构
    */
    struct sigpending pending;
    /*
        6) sas_ss_sp: 信号处理程序备用堆栈的地址
        7) sas_ss_size: 表示堆栈的大小
    */
    unsigned long sas_ss_sp;
    size_t sas_ss_size;
    /*
        8) notifier
        设备驱动程序常用 notifier 指向的函数来阻塞进程的某些信号
        9) otifier_data
        指的是 notifier 所指向的函数可能使用的数据。
        10) otifier_mask
        标识这些信号的位掩码
    */
    int (*notifier)(void *priv);
    void *notifier_data;
    sigset_t *notifier_mask;

    /*
    36. 进程审计
    */
    struct audit_context *audit_context;
#ifdef CONFIG_AUDITSYSCALL
    uid_t loginuid;
    unsigned int sessionid;
#endif

    /*
    37. secure computing
    */
    seccomp_t seccomp;

     /*
     38. 用于 copy_process 函数使用 CLONE_PARENT 标记时
     */
       u32 parent_exec_id;
       u32 self_exec_id;

     /*
     39. alloc_lock
     用于保护资源分配或释放的自旋锁
     */
    spinlock_t alloc_lock;

    /*
    40. 中断
    */
#ifdef CONFIG_GENERIC_HARDIRQS
    struct irqaction *irqaction;
#endif
#ifdef CONFIG_TRACE_IRQFLAGS
    unsigned int irq_events;
    int hardirqs_enabled;
    unsigned long hardirq_enable_ip;
    unsigned int hardirq_enable_event;
    unsigned long hardirq_disable_ip;
    unsigned int hardirq_disable_event;
    int softirqs_enabled;
    unsigned long softirq_disable_ip;
    unsigned int softirq_disable_event;
    unsigned long softirq_enable_ip;
    unsigned int softirq_enable_event;
    int hardirq_context;
    int softirq_context;
#endif

     /*
     41. pi_lock
     task_rq_lock 函数所使用的锁
     */
    spinlock_t pi_lock;

#ifdef CONFIG_RT_MUTEXES
    /*
    42. 基于 PI 协议的等待互斥锁，其中 PI 指的是 priority inheritance/9 优先级继承)
    */
    struct plist_head pi_waiters;
    struct rt_mutex_waiter *pi_blocked_on;
#endif

#ifdef CONFIG_DEBUG_MUTEXES
    /*
    43. blocked_on
    死锁检测
    */
    struct mutex_waiter *blocked_on;
#endif

/*
    44. lockdep，
*/
#ifdef CONFIG_LOCKDEP
# define MAX_LOCK_DEPTH 48UL
    u64 curr_chain_key;
    int lockdep_depth;
    unsigned int lockdep_recursion;
    struct held_lock held_locks[MAX_LOCK_DEPTH];
    gfp_t lockdep_reclaim_gfp;
#endif

     /*
     45. journal_info
     JFS 文件系统
     */
    void *journal_info;

     /*
     46. 块设备链表
     */
    struct bio *bio_list, **bio_tail;

    /*
    47. reclaim_state
    内存回收
    */
    struct reclaim_state *reclaim_state;

    /*
    48. backing_dev_info
    存放块设备 I/O 数据流量信息
    */
    struct backing_dev_info *backing_dev_info;

    /*
    49. io_context
    I/O 调度器所使用的信息
    */
    struct io_context *io_context;

    /*
    50. CPUSET 功能
    */
#ifdef CONFIG_CPUSETS
    nodemask_t mems_allowed;
    int cpuset_mem_spread_rotor;
#endif

    /*
    51. Control Groups
    */
#ifdef CONFIG_CGROUPS
    struct css_set *cgroups;
    struct list_head cg_list;
#endif

    /*
    52. robust_list
    futex 同步机制
    */
#ifdef CONFIG_FUTEX
    struct robust_list_head __user *robust_list;
#ifdef CONFIG_COMPAT
    struct compat_robust_list_head __user *compat_robust_list;
#endif
    struct list_head pi_state_list;
    struct futex_pi_state *pi_state_cache;
#endif
#ifdef CONFIG_PERF_EVENTS
    struct perf_event_context *perf_event_ctxp;
    struct mutex perf_event_mutex;
    struct list_head perf_event_list;
#endif

    /*
    53. 非一致内存访问 (NUMA  Non-Uniform Memory Access)
    */
#ifdef CONFIG_NUMA
    struct mempolicy *mempolicy;    /* Protected by alloc_lock */
    short il_next;
#endif

    /*
    54. fs_excl
    文件系统互斥资源
    */
    atomic_t fs_excl;

    /*
    55. rcu
    RCU 链表
    */
    struct rcu_head rcu;

    /*
    56. splice_pipe
    管道
    */
    struct pipe_inode_info *splice_pipe;

    /*
    57. delays
    延迟计数
    */
#ifdef    CONFIG_TASK_DELAY_ACCT
    struct task_delay_info *delays;
#endif

    /*
    58. make_it_fail
    fault injection
    */
#ifdef CONFIG_FAULT_INJECTION
    int make_it_fail;
#endif

    /*
    59. dirties
    FLoating proportions
    */
    struct prop_local_single dirties;

    /*
    60. Infrastructure for displayinglatency
    */
#ifdef CONFIG_LATENCYTOP
    int latency_record_count;
    struct latency_record latency_record[LT_SAVECOUNT];
#endif

    /*
    61. time slack values，常用于 poll 和 select 函数
    */
    unsigned long timer_slack_ns;
    unsigned long default_timer_slack_ns;

    /*
    62. scm_work_list
    socket 控制消息 (control message)
    */
    struct list_head    *scm_work_list;

    /*
    63. ftrace 跟踪器
    */
#ifdef CONFIG_FUNCTION_GRAPH_TRACER
    int curr_ret_stack;
    struct ftrace_ret_stack    *ret_stack;
    unsigned long long ftrace_timestamp;
    atomic_t trace_overrun;
    atomic_t tracing_graph_pause;
#endif
#ifdef CONFIG_TRACING
    unsigned long trace;
    unsigned long trace_recursion;
#endif
};

```

## 4. 进程状态

http://www.linfo.org/process_state.html

TODO:

https://my.oschina.net/pkjason/blog/124371

在 Linux 内核中分别为 task 的 runnability 和 exiting 定义了具体状态：
```c
/* Used in tsk->state: */
#define TASK_RUNNING			0x0000
#define TASK_INTERRUPTIBLE		0x0001
#define TASK_UNINTERRUPTIBLE		0x0002
#define __TASK_STOPPED			0x0004
#define __TASK_TRACED			0x0008
/* Used in tsk->exit_state: */
#define EXIT_DEAD			0x0010
#define EXIT_ZOMBIE			0x0020
#define EXIT_TRACE			(EXIT_ZOMBIE | EXIT_DEAD)
/* Used in tsk->state again: */
#define TASK_PARKED			0x0040
#define TASK_DEAD			0x0080
#define TASK_WAKEKILL			0x0100
#define TASK_WAKING			0x0200
#define TASK_NOLOAD			0x0400
#define TASK_NEW			0x0800
#define TASK_STATE_MAX			0x1000
```
其中：
- `TASK_RUNNING`: 可运行状态。处于这种状态的进程，有两种状态：
  - 正在运行：程序正在运行，Linux 会把一个专门用来指向当前运行任务的指针 current 指向它，以表示它是一个正在运行的进程。
  - 正准备运行：程序已被挂入运行队列 (run_queue)，一旦获得处理器使用权，即可进入运行状态（系统中有一个运行队列，用来容纳所有处于可运行状态的进程，调度程序执行时，从中选择一个进程投入运行）。

- `TASK_INTERRUPTIBLE`: 可中断的等待状态，通常称为 S 进程（Sleep）。由于进程未获得它所申请的资源而处在等待状态。一旦内核通知资源有效或者有唤醒信号，进程会立即结束等待而进入就绪状态 (TASK_RUNNING)，只要调度器选中该进程即可恢复执行。

- `TASK_UNINTERRUPTIBLE`: 不可中断的等待状态，通常称为 D 进程（Disk Sleep）。与 TASK_INTERRUPTIBLE 状态的区别在于：**处于 TASK_UNINTERRUPTIBL 状态的进程不能被异步信号量或中断所唤醒**，只有当它申请的资源有效时通过内核的 wake_up() 调用才能唤醒，而不能由外部信号唤醒。这个状态被应用在内核中某些场景中，比如当进程需要对磁盘进行读写，而此刻正在 DMA 中进行着数据到内存的拷贝，如果这时进程休眠被打断（比如强制退出信号）那么很可能会出现问题，所以这时进程就会处于不可被打断的状态下。

  TASK_UNINTERRUPTIBLE 状态存在的意义就在于，内核的某些处理流程是不能被打断的。如果响应异步信号，程序的执行流程中就会被插入一段用于处理异步信号的流程（这个插入的流程可能只存在于内核态，也可能延伸到用户态），于是原有的流程就被中断了。（参见《linux 内核异步中断浅析》） 在进程对某些硬件进行操作时（比如进程调用 read 系统调用对某个设备文件进行读操作，而 read 系统调用最终执行到对应设备驱动的代码，并与对应的物理设备进行交互），可能需要使用 TASK_UNINTERRUPTIBLE 状态对进程进行保护，以避免进程与设备交互的过程被打断，造成设备陷入不可控的状态。这种情况下的 TASK_UNINTERRUPTIBLE 状态总是非常短暂的，通过 ps 命令基本上不可能捕捉到。

  e.g.

  当 NFS 服务端关闭时，若未事先 umount 相关目录，在 NFS 客户端执行 df 就会挂住整个登录会话，按 Ctrl+C 、Ctrl+Z 都无济于事。断开连接再登录，执行 ps axf 则看到刚才的 df 进程状态位已变成了 D ，kill -9 无法杀灭。处理方式：
  - 提供 IO 资源：

    马上恢复 NFS 服务端，再度提供服务，刚才挂起的 df 进程发现了其苦苦等待的资源，便完成任务，自动消亡。

  - 重启整个系统：

    若 NFS 服务端无法恢复服务，在 reboot 之前也应将 /etc/mtab 里的相关 NFS mount 项删除，以免 reboot 过程例行调用 netfs stop 时再次发生等待资源，导致系统重启过程挂起。

- `TASK_STOPPED`: 暂停状态，暂时停止运行来接受某种特殊处理。通常当进程接收到 SIGSTOP、SIGTSTP、SIGTTIN 或 SIGTTOU 信号后就进入暂停状态；当受到一个 SIGCONT 信号时，又会恢复运行状态。这种状态主要用于程序的调试。

- `TASK_DEAD`: 终止状态，并且不具有再次执行的条件。父进程 wait 系统调用发出后，当子进程退出时，父进程负责回收子进程的全部资源，子进程进入 TASK_DEAD 状态。

- `TASK_ZOMBIE`: 僵死状态，通常称为 Z 进程（Zombie）。进程虽然已经终止，但由于某种原因，父进程还没有执行 wait() 系统调用，导致虽然进程占有的大部分资源被回收，但其 task_struct 结构（以及少数资源）仍然 保存在内存中，成为僵尸进程。处于该状态的进程实际上是系统中的垃圾，必须进行相应处理以释放其占用的资源。

  清除 ZOMBIE（僵尸）进程可以使用如下方法：
  - `kill –18 PPID` (PPID 是其父进程): 告诉父进程，该子进程已经死亡了，请收回分配给他的资源。
  - `kill –15 PID1 PID2 (PID1,PID2 是僵尸进程的父进程的其它子进程);kill –15 PPID`: 直接终止其父进程（如果其父进程不需要的话）。先看其父进程又无其他子进程，如果有，可能需要先 kill 其他子进程，也就是兄弟进程。然后再 kill 父进程。

- `TASK_TRACED`: 从本质上来说，这属于 TASK_STOPPED 状态，用于从停止的进程中，将当前被调试的进程与常规的进程区分开来。

- `TASK_SWAPPING`: 换入 / 换出状态。

NOTE：
- 通过 `kill -9` 无法杀死处于 D 状态 (TASK_UNINTERRUPTIBLE)、Z 状态 (TASK_ZOMBIE) 的进程。
  - 对于 D 状态进程：进程不响应异步信号，因此 KILL 信号对该类进程无效。
  - 对于 Z 状态进程：进程本身已经退出，只是在内存中没有销毁而已，因此自然不会响应 KILL 信号。

P.S.

linux5.3/include/linux/sched.h 中对进程状态的完整定义：
```c
/*
 * Task state bitmask. NOTE! These bits are also
 * encoded in fs/proc/array.c: get_task_state().
 *
 * We have two separate sets of flags: task->state
 * is about runnability, while task->exit_state are
 * about the task exiting. Confusing, but this way
 * modifying one set can't modify the other one by
 * mistake.
 */

/* Used in tsk->state: */
#define TASK_RUNNING			0x0000
#define TASK_INTERRUPTIBLE		0x0001
#define TASK_UNINTERRUPTIBLE		0x0002
#define __TASK_STOPPED			0x0004
#define __TASK_TRACED			0x0008
/* Used in tsk->exit_state: */
#define EXIT_DEAD			0x0010
#define EXIT_ZOMBIE			0x0020
#define EXIT_TRACE			(EXIT_ZOMBIE | EXIT_DEAD)
/* Used in tsk->state again: */
#define TASK_PARKED			0x0040
#define TASK_DEAD			0x0080
#define TASK_WAKEKILL			0x0100
#define TASK_WAKING			0x0200
#define TASK_NOLOAD			0x0400
#define TASK_NEW			0x0800
#define TASK_STATE_MAX			0x1000

/* Convenience macros for the sake of set_current_state: */
#define TASK_KILLABLE			(TASK_WAKEKILL | TASK_UNINTERRUPTIBLE)
#define TASK_STOPPED			(TASK_WAKEKILL | __TASK_STOPPED)
#define TASK_TRACED			(TASK_WAKEKILL | __TASK_TRACED)

#define TASK_IDLE			(TASK_UNINTERRUPTIBLE | TASK_NOLOAD)

/* Convenience macros for the sake of wake_up(): */
#define TASK_NORMAL			(TASK_INTERRUPTIBLE | TASK_UNINTERRUPTIBLE)

/* get_task_state(): */
#define TASK_REPORT			(TASK_RUNNING | TASK_INTERRUPTIBLE | \
					 TASK_UNINTERRUPTIBLE | __TASK_STOPPED | \
					 __TASK_TRACED | EXIT_DEAD | EXIT_ZOMBIE | \
					 TASK_PARKED)

#define task_is_traced(task)		((task->state & __TASK_TRACED) != 0)

#define task_is_stopped(task)		((task->state & __TASK_STOPPED) != 0)

#define task_is_stopped_or_traced(task)	((task->state & (__TASK_STOPPED | __TASK_TRACED)) != 0)

#define task_contributes_to_load(task)	((task->state & TASK_UNINTERRUPTIBLE) != 0 && \
					 (task->flags & PF_FROZEN) == 0 && \
					 (task->state & TASK_NOLOAD) == 0)

#ifdef CONFIG_DEBUG_ATOMIC_SLEEP

/*
 * Special states are those that do not use the normal wait-loop pattern. See
 * the comment with set_special_state().
 */
#define is_special_task_state(state)				\
	((state) & (__TASK_STOPPED | __TASK_TRACED | TASK_PARKED | TASK_DEAD))

#define __set_current_state(state_value)			\
	do {							\
		WARN_ON_ONCE(is_special_task_state(state_value));\
		current->task_state_change = _THIS_IP_;		\
		current->state = (state_value);			\
	} while (0)

#define set_current_state(state_value)				\
	do {							\
		WARN_ON_ONCE(is_special_task_state(state_value));\
		current->task_state_change = _THIS_IP_;		\
		smp_store_mb(current->state, (state_value));	\
	} while (0)

#define set_special_state(state_value)					\
	do {								\
		unsigned long flags; /* may shadow */			\
		WARN_ON_ONCE(!is_special_task_state(state_value));	\
		raw_spin_lock_irqsave(&current->pi_lock, flags);	\
		current->task_state_change = _THIS_IP_;			\
		current->state = (state_value);				\
		raw_spin_unlock_irqrestore(&current->pi_lock, flags);	\
	} while (0)
#else
/*
 * set_current_state() includes a barrier so that the write of current->state
 * is correctly serialised wrt the caller's subsequent test of whether to
 * actually sleep:
 *
 *   for (;;) {
 *	set_current_state(TASK_UNINTERRUPTIBLE);
 *	if (!need_sleep)
 *		break;
 *
 *	schedule();
 *   }
 *   __set_current_state(TASK_RUNNING);
 *
 * If the caller does not need such serialisation (because, for instance, the
 * condition test and condition change and wakeup are under the same lock) then
 * use __set_current_state().
 *
 * The above is typically ordered against the wakeup, which does:
 *
 *   need_sleep = false;
 *   wake_up_state(p, TASK_UNINTERRUPTIBLE);
 *
 * where wake_up_state() executes a full memory barrier before accessing the
 * task state.
 *
 * Wakeup will do: if (@state & p->state) p->state = TASK_RUNNING, that is,
 * once it observes the TASK_UNINTERRUPTIBLE store the waking CPU can issue a
 * TASK_RUNNING store which can collide with __set_current_state(TASK_RUNNING).
 *
 * However, with slightly different timing the wakeup TASK_RUNNING store can
 * also collide with the TASK_UNINTERRUPTIBLE store. Losing that store is not
 * a problem either because that will result in one extra go around the loop
 * and our @cond test will save the day.
 *
 * Also see the comments of try_to_wake_up().
 */
#define __set_current_state(state_value)				\
	current->state = (state_value)

#define set_current_state(state_value)					\
	smp_store_mb(current->state, (state_value))

/*
 * set_special_state() should be used for those states when the blocking task
 * can not use the regular condition based wait-loop. In that case we must
 * serialize against wakeups such that any possible in-flight TASK_RUNNING stores
 * will not collide with our state change.
 */
#define set_special_state(state_value)					\
	do {								\
		unsigned long flags; /* may shadow */			\
		raw_spin_lock_irqsave(&current->pi_lock, flags);	\
		current->state = (state_value);				\
		raw_spin_unlock_irqrestore(&current->pi_lock, flags);	\
	} while (0)

#endif

```

## 5. Refer Links

[孤儿进程与僵尸进程](https://www.cnblogs.com/Anker/p/3271773.html)

[用户空间与内核空间，进程上下文与中断上下文](https://www.cnblogs.com/Anker/p/3269106.html)

[硬中断和软中断](https://blog.csdn.net/zhangskd/article/details/21992933)

TODO:

[CPU 检测到中断信号时，怎么知道是发给哪个进程的？](https://www.zhihu.com/question/47862508)

[内核态是指一个特殊的进程，还是指进程的一种特殊状态？](https://www.zhihu.com/question/40147261)

[一个进程所能分配的最大内存是多大？这是由什么决定的？](https://www.zhihu.com/question/29468200)
