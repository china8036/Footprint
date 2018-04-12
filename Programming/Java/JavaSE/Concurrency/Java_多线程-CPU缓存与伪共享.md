- [Java 多线程 - CPU 缓存与伪共享](#java-%E5%A4%9A%E7%BA%BF%E7%A8%8B---cpu-%E7%BC%93%E5%AD%98%E4%B8%8E%E4%BC%AA%E5%85%B1%E4%BA%AB)
	- [1. CPU 缓存](#1-cpu-%E7%BC%93%E5%AD%98)
		- [1.1. 基本概念](#11-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [1.2. 多级缓存](#12-%E5%A4%9A%E7%BA%A7%E7%BC%93%E5%AD%98)
	- [2. MESI 协议及 RFO 请求](#2-mesi-%E5%8D%8F%E8%AE%AE%E5%8F%8A-rfo-%E8%AF%B7%E6%B1%82)
		- [2.1. 跨核访问](#21-%E8%B7%A8%E6%A0%B8%E8%AE%BF%E9%97%AE)
		- [2.2. MESI 协议](#22-mesi-%E5%8D%8F%E8%AE%AE)
	- [3. 缓存行](#3-%E7%BC%93%E5%AD%98%E8%A1%8C)
	- [4. 伪共享](#4-%E4%BC%AA%E5%85%B1%E4%BA%AB)
		- [4.1. 基本概念](#41-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [4.2. 实例分析](#42-%E5%AE%9E%E4%BE%8B%E5%88%86%E6%9E%90)
		- [4.3. 避免方案](#43-%E9%81%BF%E5%85%8D%E6%96%B9%E6%A1%88)
		- [4.4. 实际开发](#44-%E5%AE%9E%E9%99%85%E5%BC%80%E5%8F%91)
	- [5. Refer Links](#5-refer-links)

# Java 多线程 - CPU 缓存与伪共享

## 1. CPU 缓存

### 1.1. 基本概念

CPU 缓存（Cache Memory）是位于 CPU 与内存之间的临时存储器，它的容量比内存小的多但是交换速度却比内存要快得多。

高速缓存的出现主要是为了解决 CPU 运算速度与内存读写速度不匹配的矛盾，因为 CPU 运算速度要比内存读写速度快很多，这样会使 CPU 花费很长时间等待数据到来或把数据写入内存。

在缓存中的数据是内存中的一小部分，但这一小部分是短时间内 CPU 即将访问的，当 CPU 调用大量数据时，就可避开内存直接从缓存中调用，从而加快读取速度。

### 1.2. 多级缓存

CPU 和主内存之间有好几层缓存，因为即使直接访问主内存也是非常慢的。如果你正在多次对一块数据做相同的运算，那么在执行运算的时候把它加载到离 CPU 很近的地方就有意义了。

按照数据读取顺序和与 CPU 结合的紧密程度，CPU 缓存可以分为一级缓存，二级缓存，部分高端 CPU 还具有三级缓存。每一级缓存中所储存的全部数据都是下一级缓存的一部分，越靠近 CPU 的缓存越快也越小。所以 L1 缓存很小但很快（译注：L1 表示一级缓存)，并且紧靠着在使用它的 CPU 内核。L2 大一些，也慢一些，并且仍然只能被一个单独的 CPU 核使用。L3 在现代多核机器中更普遍，仍然更大，更慢，并且被单个插槽上的所有 CPU 核共享。最后，你拥有一块主存，由全部插槽上的所有 CPU 核共享。拥有三级缓存的的 CPU，到三级缓存时能够达到 95% 的命中率，只有不到 5% 的数据需要从内存中查询。

多核机器的存储结构如下图所示：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/3/e7b9151c4bd4984600a9fd0530da61e2.jpg)

当 CPU 执行运算的时候，它先去 L1 查找所需的数据，再去 L2，然后是 L3，最后如果这些缓存中都没有，所需的数据就要去主内存拿。走得越远，运算耗费的时间就越长。所以如果你在做一些很频繁的事，你要确保数据在 L1 缓存中。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/3/d636a8686baf26e153e7fcbd1010d37c.jpg)

## 2. MESI 协议及 RFO 请求

### 2.1. 跨核访问

每个核都有自己私有的 L1,、L2 缓存。那么多线程编程时，另外一个核的线程想要访问当前核内 L1、L2 缓存行的数据，该怎么办呢？

跨核访问需要通过 Memory Controller（内存控制器，是计算机系统内部控制内存并且通过内存控制器使内存与 CPU 之间交换数据的重要组成部分），典型的情况是第 2 个核经常访问第 1 个核的这条数据，那么每次都有跨核的消耗。更糟的情况是，有可能第 2 个核与第 1 个核不在一个插槽内，况且 Memory Controller 的总线带宽是有限的，扛不住这么多数据传输。所以，CPU 设计者们更偏向于另一种办法： 如果第 2 个核需要这份数据，由第 1 个核直接把数据内容发过去，数据只需要传一次。

### 2.2. MESI 协议

现在主流的处理器都是通过 MESI 协议来保证缓存的相干性和内存的相干性。

M、E、S 和 I 代表使用 MESI 协议时缓存行所处的四个状态：
> M（修改，Modified）：本地处理器已经修改缓存行，即是脏行，它的内容与内存中的内容不一样，并且此 cache 只有本地一个拷贝（专有)；
> E（专有，Exclusive）：缓存行内容和内存中的一样，而且其它处理器都没有这行数据；
> S（共享，Shared）：缓存行内容和内存中的一样，有可能其它处理器也存在此缓存行的拷贝；
> I（无效，Invalid）：缓存行失效，不能使用。

- 初始：一开始时，缓存行没有加载任何数据，所以它处于 I 状态。

- 本地写（Local Write）：如果本地处理器写数据至处于 I 状态的缓存行，则缓存行的状态变成 M。

- 本地读（Local Read）：如果本地处理器读取处于 I 状态的缓存行，很明显此缓存没有数据给它。此时分两种情况：(1) 其它处理器的缓存里也没有此行数据，则从内存加载数据到此缓存行后，再将它设成 E 状态，表示只有我一家有这条数据，其它处理器都没有；(2) 其它处理器的缓存有此行数据，则将此缓存行的状态设为 S 状态。（备注：如果处于 M 状态的缓存行，再由本地处理器写入 / 读出，状态是不会改变的）

- 远程读（Remote Read）：假设我们有两个处理器 c1 和 c2，如果 c2 需要读另外一个处理器 c1 的缓存行内容，c1 需要把它缓存行的内容通过内存控制器 (Memory Controller) 发送给 c2，c2 接到后将相应的缓存行状态设为 S。在设置之前，内存也得从总线上得到这份数据并保存。

- 远程写（Remote Write）：其实确切地说不是远程写，而是 c2 得到 c1 的数据后，不是为了读，而是为了写。也算是本地写，只是 c1 也拥有这份数据的拷贝，这该怎么办呢？c2 将发出一个 RFO (Request For Owner) 请求，它需要拥有这行数据的权限，其它处理器的相应缓存行设为 I，除了它自已，谁不能动这行数据。这保证了数据的安全，同时处理 RFO 请求以及设置 I 的过程将给写操作带来很大的性能消耗。

状态转换如下图：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/3/3c5bdc78444d28c7428cac6c3a6b2460.jpg)

## 3. 缓存行

缓存系统中是以缓存行（cache line）为单位存储的。缓存行通常是 64 字节（译注：本文基于 64 字节，其他长度的如 32 字节等不适本文讨论的重点），并且它有效地引用主内存中的一块地址。

一个 Java 的 long 类型是 8 字节，因此在一个缓存行中可以存 8 个 long 类型的变量。所以，如果你访问一个 long 数组，当数组中的一个值被加载到缓存中，它会额外加载另外 7 个，以致你能非常快地遍历这个数组。事实上，你可以非常快速的遍历在连续的内存块中分配的任意数据结构。而如果你在数据结构中的项在内存中不是彼此相邻的（如链表），你将得不到免费缓存加载所带来的优势，并且在这些数据结构中的每一个项都可能会出现缓存未命中。

## 4. 伪共享

### 4.1. 基本概念

当多个线程同时操作不同的成员变量，但是这些成员变量都处于相同的缓存行上时，就会出现伪共享问题。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/3/0a929b8ff506170ce759dcd0fb906b62.jpg)

上图中，一个运行在处理器 core1 上的线程想要更新变量 X 的值，同时另外一个运行在处理器 core2 上的线程想要更新变量 Y 的值。但是，这两个频繁改动的变量都处于同一条缓存行。两个线程就会轮番发送 RFO 消息，占得此缓存行的拥有权。

当 core1 取得了拥有权开始更新 X，则 core2 对应的缓存行需要设为 I 状态。当 core2 取得了拥有权开始更新 Y，则 core1 对应的缓存行需要设为 I 状态（失效态)。轮番夺取拥有权不但带来大量的 RFO 消息，而且如果某个线程需要读此行数据时，L1 和 L2 缓存上都是失效数据，只有 L3 缓存上是同步好的数据。读 L3 的数据非常影响性能。更坏的情况是跨槽读取，L3 都要 miss，只能从内存上加载。

表面上 X 和 Y 都是被独立线程操作的，而且两操作之间也没有任何关系。只不过它们共享了一个缓存行，但所有竞争冲突都是来源于共享，效率退化为串行操作。

例如：在 JDK 的 LinkedBlockingQueue 中，存在指向队列头的引用 head 和指向队列尾的引用 tail。而这种队列经常在异步编程中使用，这两个引用的值经常的被不同的线程修改，但它们却很可能在同一个缓存行，于是就产生了伪共享。线程越多，核越多，对性能产生的负面效果就越大。

### 4.2. 实例分析

以下代码构造一个伪共享问题：
```java
public class FalseShareTest implements Runnable {
    public static int NUM_THREADS = 4;
    public final static long ITERATIONS = 500L * 1000L * 1000L;
    private final int arrayIndex;
    private static VolatileLong[] longs;
    public static long SUM_TIME = 0l;
    public FalseShareTest(final int arrayIndex) {
        this.arrayIndex = arrayIndex;
    }
    public static void main(final String[] args) throws Exception {
        Thread.sleep(10000);
        for(int j=0; j<10; j++){
            System.out.println(j);
            if (args.length == 1) {
                NUM_THREADS = Integer.parseInt(args[0]);
            }
            longs = new VolatileLong[NUM_THREADS];
            for (int i = 0; i < longs.length; i++) {
                longs[i] = new VolatileLong();
            }
            final long start = System.nanoTime();
            runTest();
            final long end = System.nanoTime();
            SUM_TIME += end - start;
        }
        System.out.println("平均耗时："+SUM_TIME/10);
    }
    private static void runTest() throws InterruptedException {
        Thread[] threads = new Thread[NUM_THREADS];
        for (int i = 0; i < threads.length; i++) {
            threads[i] = new Thread(new FalseShareTest(i));
        }
        for (Thread t : threads) {
            t.start();
        }
        for (Thread t : threads) {
            t.join();
        }
    }
    public void run() {
        long i = ITERATIONS + 1;
        while (0 != --i) {
            longs[arrayIndex].value = i;
        }
    }
    public final static class VolatileLong {
        public volatile long value = 0L;
        public long p1, p2, p3, p4, p5, p6;     // 屏蔽此行
    }
}
```
四个线程修改一数组不同元素的内容。元素的类型是 VolatileLong，只有一个长整型成员 value 和 6 个没用到的长整型成员。value 设为 volatile 是为了让 value 的修改对所有线程都可见。程序分两种情况执行，第一种情况为不屏蔽倒数第三行（见"屏蔽此行"字样），第二种情况为屏蔽倒数第三行。前者 longs 数组的 4 个元素，由于 VolatileLong 只有 1 个长整型成员，所以整个数组都将被加载至同一缓存行，但有 4 个线程同时操作这条缓存行，于是伪共享就悄悄地发生了。

为了"保证"数据的相对可靠性，程序取 10 次执行的平均时间。两个逻辑一模一样的程序，第一种情况的耗时大概是第二种情况的 2.5 倍。

### 4.3. 避免方案

- 使用缓存行填充（Padding）：让不同线程操作的对象处于不同的缓存行

  分析上面的例子，我们知道一条缓存行有 64 字节，而 Java 程序的对象头固定占 8 字节 (32 位系统) 或 12 字节 ( 64 位系统默认开启压缩，不开压缩为 16 字节)，所以我们只需要填 6 个无用的长整型补上 6*8=48 字节，让不同的 VolatileLong 对象处于不同的缓存行，就避免了伪共享 ( 64 位系统超过缓存行的 64 字节也无所谓，只要保证不同线程不操作同一缓存行就可以)。

- 使用编译指示：强制使每一个变量对齐

- 使用`@sun.misc.Contended`注解

  在 Java 8 中，提供了 @sun.misc.Contended 注解来避免伪共享，原理是在使用此注解的对象或字段的前后各增加 128 字节大小的 padding，使用 2 倍于大多数硬件缓存行的大小来避免相邻扇区预取导致的伪共享冲突。参考[这里](https://link.jianshu.com/?t=http://mail.openjdk.java.net/pipermail/hotspot-dev/2012-November/007309.html)。

	e.g.

	```java
	public class FalseSharing implements Runnable {

			public final static int NUM_THREADS = 4; // change
			public final static long ITERATIONS = 500L * 1000L * 1000L;
			private final int arrayIndex;

			private static VolatileLong[] longs = new VolatileLong[NUM_THREADS];
			// private static VolatileLong2[] longs = new VolatileLong2[NUM_THREADS];
			// private static VolatileLong3[] longs = new VolatileLong3[NUM_THREADS];

			static {
					for (int i = 0; i < longs.length; i++) {
							longs[i] = new VolatileLong();
					}
			}

			public FalseSharing(final int arrayIndex) {
					this.arrayIndex = arrayIndex;
			}

			public static void main(final String[] args) throws Exception {
					long start = System.nanoTime();
					runTest();
					System.out.println("duration = " + (System.nanoTime() - start));
			}

			private static void runTest() throws InterruptedException {
					Thread[] threads = new Thread[NUM_THREADS];

					for (int i = 0; i < threads.length; i++) {
							threads[i] = new Thread(new FalseSharing(i));
					}

					for (Thread t : threads) {
							t.start();
					}

					for (Thread t : threads) {
							t.join();
					}
			}

			public void run() {
					long i = ITERATIONS + 1;
					while (0 != --i) {
							longs[arrayIndex].value = i;
					}
			}

			public final static class VolatileLong {
					public volatile long value = 0L;
			}

			// long padding 避免 false sharing
			// 按理说 jdk7 以后 long padding 应该被优化掉了，但是从测试结果看 padding 仍然起作用
			public final static class VolatileLong2 {
					volatile long p0, p1, p2, p3, p4, p5, p6;
					public volatile long value = 0L;
					volatile long q0, q1, q2, q3, q4, q5, q6;
			}

			/**
			* jdk8 新特性，@sun.misc.Contended 注解避免 false sharing
			* Restricted on user classpath
			* Unlock: -XX:-RestrictContended
			*/
			@sun.misc.Contended
			public final static class VolatileLong3 {
					public volatile long value = 0L;
			}
	}
	```
	`@sun.misc.Contended`注解还可以指定某个字段，并且可以为字段进行分组。

	```java
	/**
	* VM Options: 
	* -javaagent:/Users/sangjian/dev/source-files/classmexer-0_03/classmexer.jar
	* -XX:-RestrictContended
	*/
	public class ContendedTest {

			byte a;
			@sun.misc.Contended("a")
			long b;
			@sun.misc.Contended("a")
			long c;
			int d;

			private static Unsafe UNSAFE;

			static {
					try {
							Field f = Unsafe.class.getDeclaredField("theUnsafe");
							f.setAccessible(true);
							UNSAFE = (Unsafe) f.get(null);
					} catch (NoSuchFieldException e) {
							e.printStackTrace();
					} catch (IllegalAccessException e) {
							e.printStackTrace();
					}
			}

			public static void main(String[] args) throws NoSuchFieldException {
					System.out.println("offset-a: " + UNSAFE.objectFieldOffset(ContendedTest.class.getDeclaredField("a")));
					System.out.println("offset-b: " + UNSAFE.objectFieldOffset(ContendedTest.class.getDeclaredField("b")));
					System.out.println("offset-c: " + UNSAFE.objectFieldOffset(ContendedTest.class.getDeclaredField("c")));
					System.out.println("offset-d: " + UNSAFE.objectFieldOffset(ContendedTest.class.getDeclaredField("d")));

					ContendedTest contendedTest = new ContendedTest();

					// 打印对象的 shallow size
					System.out.println("Shallow Size: " + MemoryUtil.memoryUsageOf(contendedTest) + " bytes");
					// 打印对象的 retained size
					System.out.println("Retained Size: " + MemoryUtil.deepMemoryUsageOf(contendedTest) + " bytes");
			}

	}
	```

### 4.4. 实际开发

伪共享是很隐蔽的，我们暂时无法从系统层面上通过工具来探测伪共享事件。其次，不同类型的计算机具有不同的微架构（如 32 位系统和 64 位系统的 java 对象所占自己数就不一样），如果设计到跨平台的设计，那就更难以把握了，一个确切的填充方案只适用于一个特定的操作系统。还有，缓存的资源是有限的，如果填充会浪费珍贵的 cache 资源，并不适合大范围应用。

因此，在实际的生产开发过程中，并不是每个系统都适合花大量精力去解决潜在的伪共享问题。

## 5. Refer Links

[伪共享（false sharing），并发编程无声的性能杀手](https://www.cnblogs.com/cyfonly/p/5800758.html)

[CPU 缓存与伪共享](http://geek.csdn.net/news/detail/114619)

[Java8 使用 @sun.misc.Contended 避免伪共享](https://www.jianshu.com/p/c3c108c3dcfd)