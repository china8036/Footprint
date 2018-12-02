- [JVM 性能调优工具](#jvm-性能调优工具)
    - [1. 命令行工具](#1-命令行工具)
        - [1.1. jps](#11-jps)
        - [1.2. jstat](#12-jstat)
        - [1.3. jstatd](#13-jstatd)
        - [1.4. jinfo](#14-jinfo)
        - [1.5. jmap](#15-jmap)
        - [1.6. jhat](#16-jhat)
        - [1.7. jstack](#17-jstack)
        - [1.9. hprof](#19-hprof)
        - [1.8. HSDIS](#18-hsdis)
        - [1.10. javap](#110-javap)
    - [2. 可视化工具](#2-可视化工具)
        - [2.1. JConsole](#21-jconsole)
        - [2.2. VisualVM](#22-visualvm)
    - [3. Refer Links](#3-refer-links)

# JVM 性能调优工具

## 1. 命令行工具

JDK 在 bin 目录下包含了多种命令行工具，每逢 JDK 更新版本之时，bin 目录下命令行工具的数量和功能总会不知不觉地增加和增强。这些工具被 Sun 公司作为“礼物”附赠给 JDK 的使用者，并在软件的使用说明中把他们声明为“没有技术支持并且是实验性质的”（unsupported and experimental）的产品，但事实上，这些工具都非常稳定而且功能强大，能在处理应用程序性能问题、定位故障时发挥很大的作用。

![image](http://img.cdn.firejq.com/jpg/2018/11/25/27d0acbb982201549ddb3a8cd7749e5e.jpg)

可以发现，这些命令行工具的大小大部分都稳定在 17 KB 左右，这是因为**这些命令行工具大多数是 jdk/lib/tools.jar 类库的一层包装而已**，其主要的功能代码都是在 tools.jar 类库中实现的。JDK 开发团队选择采用 Java 代码来实现这些监控工具是有特别用意的：当应用程序部署到生产环境后，无论是直接接触物理服务器还是远程 Telnet 到服务器上都可能会受到限制。**借助 tools.jar 类库里面的接口，我们可以直接在应用程序中实现功能强大的监控分析功能**。

NOTE
- tools.jar 中的类库不属于 Java 的标准 API，如果引入这个类库，就意味着用户的程序只能运行于 Sun Hotspot（或一些从 Sun 公司购买了 JDK 的源码 License 的虚拟机，如 IBM J9、BEA JRockit）上面，或者在部署程序时需要一起部署 tools.jar。
- 如果在工作中需要监控运行于 JDK 1.5 的虚拟机之上的程序，在程序启动时请添加参数 `-Dcom.sun.management.jmxremote` 开启 JMX 管理功能，否则由于部分工具都是基于 JMX，他们都将会无法使用，如果被监控程序运行于 JDK 1.6 的虚拟机之上，那 JMX 管理默认是开启的，虚拟机启动无须再添加任何参数。

### 1.1. jps

[jps (JVM Process Status Tool)](https://docs.oracle.com/javase/10/tools/jps.htm) 虚拟机进程状况工具，用于显示正在运行的所有虚拟机进程、虚拟机执行主类名称及这些进程的本地虚拟机唯一 ID (Local Virtual Machine Identifier, LVMID)。对于本地虚拟机进程来说，LVMID 与操作系统的 PID 是一致的，但如果同时启动了多个虚拟机进程，无法根据进程名称定位时，就无法使用系统 `ps` 命令而只能使用 `jps` 命令来区分了。

Usage:
```
jps [-help]
jps [-q] [-mlvV] [<hostid>]
```
其中，jps 可以通过 RMI 协议查询开启了 RMI 服务的远程虚拟机进程状态，hostid 为 RMI 注册表中注册的主机名，即 `<hostname>[:<port>]`。

Options:
- `-q`: 只输出 LVMID，省略主类的名称。
- `-m`: 输出虚拟机进程启动时传递给主类 Main() 函数的参数。
- `-l`: 输出主类的命名，如果进程执行的是 jar 包，输出 jar 路径。
- `-v`: 输出虚拟机进程启动时 JVM 参数。

### 1.2. jstat

[jstat (JVM Statistics Monitoring Tool)](https://docs.oracle.com/javase/10/tools/jstat.htm) 虚拟机统计信息监视工具，利用 JVM 内建的指令，可用于实时监控 HotSpot 虚拟机各方面的运行数据，可以显示本地或远程虚拟机进程中的类装载、内存、垃圾收集、JIT 编译等运行数据，**在没有 GUI 的服务器上，jstat 是运行期定位虚拟机性能问题的首选工具**。

Usage:
```
jstat -help|-options
jstat -<option> [-t] [-h<lines>] <vmid> [<interval> [<count>]]
```
其中：
- 如果是本地虚拟机进程，VMID 与 LVMID 是一致的，如果是远程虚拟机进程，那 VMID 的格式应当是 `[protocol:] [//]lvmid[@hostname[:port]/servername]`。
- 参数 interval 和 count 代表查询间隔和次数，**如果省略这两个参数，说明只查询一次**。
- options 代表希望查询的虚拟机信息，主要分为 3 类：类装载、垃圾收集和运行期编译状况：
  - `-class`: 监视类装载、卸载数量、总空间以及类装载所耗时间。
  - `-gc`: 监视 JAVA 堆状况，包括 Eden 区、两个 Survivor 区、老年代、永久代等的容量、已用空间，GC 已用时间合计等信息。
  - `-gccapacity`: 监视内容与 `-gc` 基本相同，但输出主要关注 java 堆各个区域使用到的最大、最小空间。
  - `-gcutil`: 监视内容与 `-gc` 基本相同，但**输出主要关注已使用空间占总空间的百分比，因此这也是最常使用的 option**。
  - `-gccause`: 与 `-gcutil` 功能一样，但是会额外输出导致 一次 GC 产生的原因。
  - `-gcnew`: 监视新生代 GC 状态。
  - `-gcnewcapacity`: 监视内容与 -gcnew 基本相同，输出最要关注使用到的最大、最小空间。
  - `-gcold`: 监视老年代 GC 状况。
  - `-gcoldcapacity`: 监视内容与 -gcold 基本相同，输出主要关注使用到的最大、最小空间。
  - `-gcpermcapacity`: 监视永久代使用到的最大、最小空间。
  - `-compiler`: 输入 JIT 编译器编译过的方法，耗时等信息。
  - `-printcompilation`: 输出已经被 JIT 编译的方法。

i.e.
- 每 250 秒查询一次进程 2764 垃圾收集情况，一共查询 20 次：`jstat -gc 2764 250 20`.
- 查询一次进程 7563 垃圾收集情况，要求包含各区域的空间大小百分比：
  ```
  $ jstat -gcutil 7563

    S0     S1     E      O      P     YGC     YGCT    FGC    FGCT     GCT
    0.00  44.10  84.66  60.00  99.53    147    0.570     7    0.721    1.291
  ```
  其中：
  - `S0`: Heap 上的 Survivor space 0 区已使用空间的百分比为空。
  - `S1`: Heap 上的 Survivor space 1 区已使用空间的百分比，44.10%。
  - `E`: Heap 上的 Eden space 区已使用空间的百分比，新生代 Eden 区使用了 84.66% 的空间。
  - `O`: Heap 上的 Old space 区已使用空间的百分比，老年代 Old 区使用了 60.00% 的空间。
  - `P`: Perm space 区已使用空间的百分比，永久代 Permanent 区使用了 99.53% 的空间。
  - `YGC`: 从应用程序启动到采样时发生 Young GC 的次数，147 次。
  - `YGCT`: 从应用程序启动到采样时 Young GC 所用的时间（秒），总耗时 0.570 秒。
  - `FGC`: 从应用程序启动到采样时发生 Full GC 的次数，7 次。
  - `FGCT`: 从应用程序启动到采样时 Full GC 所用的时间（秒） ，总耗时 0.721 秒。
  - `GCT`: 从应用程序启动到采样时用于垃圾回收的总时间（秒），所有 gc 总耗时 1.291 秒。
- 显示加载 class 的数量：
  ```
  $ jstat -class 7563
    Loaded  Bytes  Unloaded  Bytes     Time
    2277  4700.2        0     0.0       1.65
  ```
- 显示 JVM 实时编译的数量：
  ```
  $ jstat -gccapacity 7563
    NGCMN    NGCMX     NGC     S0C   S1C       EC      OGCMN      OGCMX       OGC         OC      PGCMN    PGCMX     PGC       PC     YGC    FGC
    10688.0 171328.0  38464.0 3840.0 3840.0  30784.0    21440.0   342720.0    76820.0    76820.0  21248.0  83968.0  50368.0  50368.0    154     7
  ```

### 1.3. jstatd

[jstatd (Java Statistics Monitoring Daemon)](https://docs.oracle.com/javase/10/tools/jstatd.htm) 是一个基于提供远程监控接口的 RMI（Remove Method Invocation，远程方法调用）的服务器程序，它用于监控基于 HotSpot 的 JVM 中资源的创建及销毁过程中资源占用情况，它是一个 Daemon 程序，要保证远程监控软件连接到本地的话需要 jstatd 始终保持运行，默认端口是 1099。

### 1.4. jinfo

[jinfo (Configuration Info for Java)](https://docs.oracle.com/javase/10/tools/jinfo.htm) Java 配置信息工具，用于实时查看和调整虚拟机的各项参数。

使用 jps 的命令的 -v 参数可以查看虚拟机启动时显示指定的参数列表，但如果想知道未被显示指定的参数的系统默认值，除了去找资料外，就只能使用 jinfo 的 `-flag` 选项进行查询了（如果只限于 JDK1.6 或以上版本的话，使用 `java -XX:+PrintFlagsFinal` 查看参数默认值也是一个很好的选择），jinfo 还可以使用 `-sysprops` 选项把虚拟机进程的 `System.getProperties()` 的内容打印出来。这个命令在 JDK1.5 时期已经随着 Linux 版的 JDK 发布，当时只提供了信息查询的功能，JDK1.6 之后，jinfo 在 Windows 和 Linux 平台都有提供，并且加入了运行期修改参数的能力，可以使用 `-flag[+|-]name` 或 `-flag name=valule` 修改一部分运行期可写的虚拟机参数值。**JDK1.6 中，jinfo 对于 Windows 平台的功能仍然有较大的限制，只提供了最基本的 `-flag` 选项**。

Usage:
```
jinfo [ option ] pid
```

Options:
- `-flag <name>` ：可查看虚拟机启动时显式指定的参数列表。
- `-flag [+|-]<name>`：设置或取消 VM 参数。
- `-flag <name>=<value>`：给 VM 参数设置新值。
- `-flags`：可查看所有 VM 参数。
- `-sysprops`：查看 java 系统参数。
- `<no option>`：表示在不给定任何选项时，打印出以上所有的 JVM 参数。

### 1.5. jmap

[jmap (Memory Map for Java)](https://docs.oracle.com/javase/10/tools/jmap.htm) Java 内存映像工具，用于生成虚拟机的堆内存转储快照（heapdump / dump 文件），同时还可以查询 finalize 执行队列，Java 堆和永久代的详细信息，如空间使用率、当前用的是那种收集器等。

P.S. 如果不使用 jmap 命令，要向获取 Java 堆转储快照还有一些比较”暴力“的手段：譬如 `-XX:+HeapDumpOnOutOfMemoryError` 参数，可以让虚拟机在 OOM 异常出现之后自动生生成 dump 文件，通过 `-XX:+HeapDumpOnCtrlBreak` 参数则可以使用 `[Ctrl]+[Break]` 键让虚拟机生成 dump 文件，又或者在 Linux 系统下通过 `Kill -3` 命令发送进程退出信号”恐吓“一下虚拟机，也能拿到 dump 文件。

**在创建 dump 文件的过程中，所有 Java 程序都将被暂停，因此不要在系统执行过程中使用该命令**。

和 jinfo 命令一样，jmap 命令的很多功能在 Windows 平台下都是受限的。除了生成 dump 文件的 `-dump` 选项和用于查看每个类的实例、空间占用统计的 `-histo` 选项，其余选项只能在 Linux/Solaris 平台下使用。

Usage:
```
jmap [ option ] vmid
```

Options:
- `-dump`: 生成 java 堆转储快照，格式为：`-dump[live, ] format=b, file=<filename>`, 其中 live 子参数说明是否只 dump 出存活对象。
- `-finalizerinfo`: 显示在 F-Queue 等待 Finalizer 线程执行 finalize 方法的对象。只在 Linux/Solaris 平台下有效。
- `-heap`: 显示 java 堆详细信息，如使用哪种回收器、参数配置、分代状况等。只在 Linux/Solaris 平台下有效。
- `-histo`: 显示堆中对象统计信息，包括类、实例数量、合计容量。
- `-permstat`: 以 ClassLoader 为统计口径显示永久代内存状态，只在 Linux/Solaris 平台有效。
- `-F`: 当虚拟机进程对 `-dump` 选项没有响应时，可使用这个选项强制生成 dump 快照，只在 Linux/Solaris 平台有效。

### 1.6. jhat

jhat (JVM Heap Analysis Tool) 虚拟机堆转储快照分析工具，一般与 jmap 搭配使用，用于分析 heapdump 文件。jhat 会建立一个 HTTP/HTML 服务器，让用户可以在浏览器上查看分析结果。

i.e. `jhat idea.bin`.

NOTE:
- jhat 是基于命令行的分析工具，分析功能比较简陋。
- 分析工作是一个耗时且消耗硬件资源的过程，一般都会在非生产机器上进行。因此，既然都要在其它机器上进行，就没有必要收到无 GUI 的命令行工具的限制了。

因此在实际工作中，**一般不会直接使用 jhat 命令来分析 dump 文件**而是直接使用更加专业更加强大的分析工具（如 Eclipse Memory Analyzer、IBM HeapAnalyzer 等）来分析。

### 1.7. jstack

[jstack (Stack Trace for Java)](https://docs.oracle.com/javase/10/tools/jstack.htm) Java 堆栈跟踪工具，用于显示虚拟机当前时刻的线程快照（一般称为 threaddump 或者 javacore 文件），即当前虚拟机内每一条线程正在执行的方法堆栈集合。

生成线程快照的主要目的是定位线程出现长时间停顿的原因，如线程间死锁、死循环、请求外部资源导致的长时间等待等都是导致线程长时间停顿的常见原因。线程出现停顿的时候通过 jstack 来查询各个线程的堆栈，就可以知道没有响应的线程到底在后台做些什么事情，或者等待着什么资源。

Usage:
```
jstack [ option ] vmid
```

Options:
- `-F`: 当正常输出的请求不被响应时，强制输出线程堆栈。
- `-I`: 除堆栈外，显示 关于锁的附加信息。
- `-m`: 如果调用到本地方法的话，可以显示 C/C++ 的堆栈。

P.S.

在 JDk1.5 中，`java.lang.Thread` 类新增了一个 `getAllStackTraces()` 方法用于获取虚拟机中所有线程的 StackTraceElement 对象。使用这个方法可以通过简单的几行代码就完成 jstack 的大部分功能：
```jsp
<@ page import="java.util.Map"%>
<html>
<head>
<title>服务器线程信息</title>
</head>
<body>
<pre>
<%
    for(Map.Entry<Thread,StackTraceElement[]> stackTrace :Thread.getAllStackTraces().entrySet()){
        Thread thread = (Thread) stackTrace.getKey();
        StackTraceElement[] stack = (StackTraceElement[]) stackTrace.getValue();
        if(thread.equals(Thread.currentThread())){
            continue;
        }
        out.print("\n 线程：" + thread.getName() + "\n");
        for(StackTraceElement element :stack) {
            out.print("\t”+ element + "\n");
        }
}
%>
</pre>
</body>
</html>
```

### 1.9. hprof

[HPROF](https://docs.oracle.com/javase/7/docs/technotes/samples/hprof.html) 实际上是 JVM 中的一个 native 的库，它会在 JVM 启动的时候通过命令行参数来动态加载，并成为 JVM 进程的一部分，用于对 java 程序的 CPU 和 Heap 进行 profiling。

要使用 hprof，可以通过在运行 java 程序时指定 `-agentlib` 或者 `-Xrunhprof` 参数来使用，它会将 cpu、heap 等相关信息保存到一个本地文件中（默认情况是当前目录的 `.hprof` 文件）。这些日志可以用来跟踪和分析 java 进程的性能问题和瓶颈，解决内存使用上不优的地方或者程序实现上的不优之处。HPROF 产生的 profiling 数据可以是二进制的，也可以是文本格式的，而二进制格式的日志还可以被 JVM 中的 HAT 工具来进行浏览和分析，用以观察 java 进程的 heap 中各种类型和数据的情况。

### 1.8. HSDIS

随着技术发展，高性能虚拟机真正的细节实现方式已经渐渐与虚拟机规范所描述的内容产生了巨大的差异，虚拟机规范在一定程度成为了虚拟机实现的“概念模型”。因此，分析程序的执行语义问题时，在字节码层面上分析完全可行，但分析程序的执行行为问题（虚拟机是怎样做的、性能如何）时，在字节码层面上分析就没有什么意义了，需要通过其它方式解决。

HSDIS 是 Sun 官方推荐的 HotSpot VM JIT 编译代码的反汇编插件，包含在了 HotSpot VM 的源代码之中，但没有提供编译后的程序。HSDIS 的作用是让 HotSpot 的 `-XX:+PrintAssembly` 指令调用它来把动态生成的本地代码还原为汇编代码输出，同时会生成大量非常有价值的注释，便于我们通过输出的代码来分析问题。

### 1.10. javap

[javap](https://docs.oracle.com/javase/10/tools/javap.htm) 用于对代码进行反编译，也可以查看 java 编译器生成的字节码。

大多数情况下，很少有人会直接使用 javap 对 `.class` 文件进行反编译，因为有很多更加专业的反编译工具可以使用（如 jad）。javap 更多的是被用于查看 java 编译器为我们生成的字节码，通过它来对照源代码和字节码，从而了解很多编译器内部的工作。

Options:
- `-help`: 帮助。
- `-l`: 输出行和变量的表。
- `-public`: 只输出 public 方法和域。
- `-protected`: 只输出 public 和 protected 类和成员。
- `-package`: 只输出包，public 和 protected 类和成员，这是默认的。
- `-p` / `-private`:  输出所有类和成员。
- `-s`: 输出内部类型签名。
- `-c`: 输出分解后的代码，例如，类中每一个方法内，包含 java 字节码的指令。
- `-verbose`: 输出栈大小，方法参数的个数。
- `-constants`: 输出静态 final 常量。

i.e. 对于 Java 类：
```java
public class DocFooter extends Applet {
    String date;
    String email;

    public void init() {
        resize(500,100);
        date = getParameter("LAST_UPDATED");
        email = getParameter("EMAIL");
    }

    public void paint(Graphics g) {
        g.drawString(date + " by ", 100, 15);
        g.drawString(email,290,15);
    }
}
```
使用 javap 进行反编译，查看编译后生成的字节码：
```
$ javap -c DocFooter

Compiled from "DocFooter.java"
public class DocFooter extends java.applet.Applet {
  java.lang.String date;

  java.lang.String email;

  public DocFooter();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/applet/Applet."<init>":()V
       4: return

  public void init();
    Code:
       0: aload_0
       1: sipush        500
       4: bipush        100
       6: invokevirtual #2                  // Method resize:(II)V
       9: aload_0
      10: aload_0
      11: ldc           #3                  // String LAST_UPDATED
      13: invokevirtual #4                  // Method getParameter:(Ljava/lang/String;)Ljava/lang/String;
      16: putfield      #5                  // Field date:Ljava/lang/String;
      19: aload_0
      20: aload_0
      21: ldc           #6                  // String EMAIL
      23: invokevirtual #4                  // Method getParameter:(Ljava/lang/String;)Ljava/lang/String;
      26: putfield      #7                  // Field email:Ljava/lang/String;
      29: return

  public void paint(java.awt.Graphics);
    Code:
       0: aload_1
       1: new           #8                  // class java/lang/StringBuilder
       4: dup
       5: invokespecial #9                  // Method java/lang/StringBuilder."<init>":()V
       8: aload_0
       9: getfield      #5                  // Field date:Ljava/lang/String;
      12: invokevirtual #10                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      15: ldc           #11                 // String  by
      17: invokevirtual #10                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      20: invokevirtual #12                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      23: bipush        100
      25: bipush        15
      27: invokevirtual #13                 // Method java/awt/Graphics.drawString:(Ljava/lang/String;II)V
      30: aload_1
      31: aload_0
      32: getfield      #7                  // Field email:Ljava/lang/String;
      35: sipush        290
      38: bipush        15
      40: invokevirtual #13                 // Method java/awt/Graphics.drawString:(Ljava/lang/String;II)V
      43: return
}
```

## 2. 可视化工具

JDK 除了提供了大量的命令行工具外，还提供了 2 个功能非常强大的可视化工具：JConsole 和 VisualVM，且这 2 个工具都是 JDK 的正式成员，并没有被贴上 "unsupported and experimental" 的标签。

### 2.1. JConsole

### 2.2. VisualVM

## 3. Refer Links

[jvm 系列（四):jvm 调优 - 命令篇](http://www.ityouknow.com/jvm/2017/09/03/jvm-command.html)

[JDK 下的 java 各个命令](https://blog.csdn.net/u010325193/article/details/79949852)

[hprof 教程](https://blog.csdn.net/jediael_lu/article/details/44016871)