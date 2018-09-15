- [Java 多线程 - 线程池 - Fork-Join 框架](#java-%E5%A4%9A%E7%BA%BF%E7%A8%8B---%E7%BA%BF%E7%A8%8B%E6%B1%A0---fork-join-%E6%A1%86%E6%9E%B6)
	- [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
	- [2. 工作窃取模式 (work-stealing)](#2-%E5%B7%A5%E4%BD%9C%E7%AA%83%E5%8F%96%E6%A8%A1%E5%BC%8F-work-stealing)
	- [3. 使用示例](#3-%E4%BD%BF%E7%94%A8%E7%A4%BA%E4%BE%8B)
		- [3.1. 拆分任务有返回值](#31-%E6%8B%86%E5%88%86%E4%BB%BB%E5%8A%A1%E6%9C%89%E8%BF%94%E5%9B%9E%E5%80%BC)
		- [3.2. 拆分任务无返回值](#32-%E6%8B%86%E5%88%86%E4%BB%BB%E5%8A%A1%E6%97%A0%E8%BF%94%E5%9B%9E%E5%80%BC)
		- [3.3. 使用 Fork/Join 实现归并排序](#33-%E4%BD%BF%E7%94%A8-forkjoin-%E5%AE%9E%E7%8E%B0%E5%BD%92%E5%B9%B6%E6%8E%92%E5%BA%8F)
	- [4. ForkJoinTask](#4-forkjointask)
		- [4.1. 基本 API](#41-%E5%9F%BA%E6%9C%AC-api)
		- [4.2. 源码分析](#42-%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90)
			- [4.2.1. 类定义](#421-%E7%B1%BB%E5%AE%9A%E4%B9%89)
			- [4.2.2. 关键方法](#422-%E5%85%B3%E9%94%AE%E6%96%B9%E6%B3%95)
	- [5. RecursiveTask](#5-recursivetask)
	- [6. RecursiveAction](#6-recursiveaction)
	- [7. ForkJoinWorkerThread](#7-forkjoinworkerthread)
	- [8. ForkJoinPool](#8-forkjoinpool)
		- [8.1. 基本概念](#81-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [8.2. 基本 API](#82-%E5%9F%BA%E6%9C%AC-api)
			- [8.2.1. 构造方法](#821-%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95)
			- [8.2.2. 关键方法](#822-%E5%85%B3%E9%94%AE%E6%96%B9%E6%B3%95)
		- [8.3. 源码分析](#83-%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90)
			- [8.3.1. 类定义](#831-%E7%B1%BB%E5%AE%9A%E4%B9%89)
			- [8.3.2. 内部属性](#832-%E5%86%85%E9%83%A8%E5%B1%9E%E6%80%A7)
			- [8.3.3. 构造方法](#833-%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95)
			- [8.3.4. 任务提交](#834-%E4%BB%BB%E5%8A%A1%E6%8F%90%E4%BA%A4)
				- [8.3.4.1. invoke()](#8341-invoke)
				- [8.3.4.2. execute()](#8342-execute)
				- [8.3.4.3. submit()](#8343-submit)
			- [8.3.5. 创建通用池](#835-%E5%88%9B%E5%BB%BA%E9%80%9A%E7%94%A8%E6%B1%A0)
	- [9. Refer Links](#9-refer-links)

# Java 多线程 - 线程池 - Fork-Join 框架

## 1. 基本概念

虽然硬件上的多核 CPU 已经十分成熟，但很多应用程序并不能很好地利用这种多核 CPU 的性能优势。**为充分利用多 CPU、多核 CPU 的优势，不让某个 CPU 处于空闲状态，可以考虑一种分治的策略：将一个任务拆分成多个小任务，把这多个小任务放在多个 CPU 上并行执行，待多个小任务都执行完毕后，再将子任务结果合并成最后的结果**。

JDK1.7 在 J.U.C 包中提供了 Fork-Join 框架，且在 JDK1.8 中得到进一步改进。Fork-Join 框架可以递归地将一个大的任务拆分成多个子任务进行并行处理，最后将子任务结果合并成最后的计算结果，并进行输出。Fork-Join 框架的总体设计参考了为英特尔 Cilk 语言设计的 work-stealing 框架。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/12/1435b58cb0a13b100b382554b9d374ae.jpg)

Fork-Join 框架主要包括：
- ForkJoinTask 抽象类：表示一个可以拆分、合并、并行的任务，是一种能在 Fork/Join 框架中运行的特定任务。提供了在任务中执行 fork() 和 join() 操作的机制，并且这两个方法控制任务的状态。
- ForkJoinTask 扩展抽象类 RecursiveTask: 表示一个有返回值的任务。 
- ForkJoinTask 扩展抽象类 RecursiveAction: 表示一个没有返回值的任务。
- ForkJoinWorkerThread 实现类：是一种在 Fork/Join 框架中运行的特性线程，它除了具有普通线程的特性外，最主要的特点是每一个 ForkJoinWorkerThread 线程都具有一个独立的任务等待队列（work queue），这个任务队列用于存储在本线程中被拆分的若干子任务。
- ForkJoinPool 核心实现类：执行 ForkJoin 任务的线程池，实现了 ExecutorService 接口和 work-stealing 算法，用于管理工作线程、提供关于任务的状态和它们执行的信息。

## 2. 工作窃取模式 (work-stealing)

> A ForkJoinPool differs from other kinds of ExecutorService mainly by virtue of employing work-stealing: all threads in the pool attempt to find and execute tasks submitted to the pool and/or created by other active tasks (eventually blocking waiting for work if none exist). This enables efficient processing when most tasks spawn other subtasks (as do most ForkJoinTasks), as well as when many small tasks are submitted to the pool from external clients. Especially when setting asyncMode to true in constructors, ForkJoinPools may also be appropriate for use with event-style tasks that are never joined. All worker threads are initialized with Thread.isDaemon() set true.

**Fork-Join 框架的实现基于工作窃取模式，这也是它与 Executor 框架的主要区别**。

Fork/Join 在实现上，大任务拆分出来的小任务会被分发到不同的队列里面，每一个队列都会用一个线程来消费，这是为了获取任务时的多线程竞争。

但是某些线程会提前消费完自己的队列，而有些线程没有及时消费完队列。这个时候，完成了任务的线程就会去窃取那些没有消费完成的线程的任务队列。**为了减少线程竞争，Fork/Join 使用双端队列来存取小任务，分配给这个队列的线程会一直从头取得一个任务然后执行，而窃取线程总是从队列的尾端拉取 task**。

通过这种方式，线程充分利用它们的运行时间，从而提高了应用程序的性能。

为实现这个目标，Fork/Join 框架执行的任务有以下局限性：
- 任务只能使用 fork() 和 join() 作为同步机制。如果使用其他同步机制，当它们在同步操作时工作线程不能执行其他任务。比如，在 Fork/Join 框架中，若使任务进入睡眠，那么正在执行这个任务的工作线程在这睡眠期间内将不会执行其他任务。
- 任务不应该执行 I/O 操作，如读或写数据文件。
- 任务不能抛出检查异常，必须在程序中使用必要的代码来处理它们。

## 3. 使用示例

为了使用 Fork/Join 框架，我们只需要继承类 RecursiveTask 或者 RecursiveAction。前者适用于有返回值的场景，而后者适合于没有返回值的场景。

典型用法如下：
```java
Result solve(Problem problem) {
    if (problem is small) {
        directly solve problem
    } else {
        split problem into independent parts
        fork new subtasks to solve each part
        join all subtasks
        compose result from subresults
    }
}
```

### 3.1. 拆分任务有返回值

e.g. 通过 Fork/Join 求解数组的最大值
```java
public class ForkJoinDemo {
		private static class MaxNumber extends RecursiveTask<Integer> {
				// 通过设置不同的阈值来拆分成小任务，阈值越小代表拆出来的小任务越多
				private int threshold = 2;
				private int[] array; // the data array
				private int index0 = 0;
				private int index1 = 0;

				public MaxNumber(int[] array, int index0, int index1) {
						this.array = array;
						this.index0 = index0;
						this.index1 = index1;
				}

				@Override
				protected Integer compute() {
						int max = Integer.MIN_VALUE;
						if ((index1 - index0) <= threshold) {
								for (int i = index0;i <= index1; i ++) {
										max = Math.max(max, array[i]);
								}
						} else {
								//fork/join
								int mid = index0 + (index1 - index0) / 2;
								MaxNumber lMax = new MaxNumber(array, index0, mid);
								MaxNumber rMax = new MaxNumber(array, mid + 1, index1);
								// lMax.fork();
								// rMax.fork();
								// invokeAll 的 N 个任务中，其中 N-1 个任务会使用 fork() 交给其它线程执行，剩下一个任务留给自己执行，这样，就充分利用了线程池，保证没有空闲的不干活的线程
								invokeAll(lMax, rMax);
								int lm = lMax.join();
								int rm = rMax.join();
								max = Math.max(lm, rm);
						}
						return max;
				}
		}

		public static void main(String [] args) throws ExecutionException, InterruptedException, TimeoutException {
				// 创建一个通用池
				ForkJoinPool pool = new ForkJoinPool();
				int[] array = {100,400,200,90,80,300,600,10,20,-10,30,2000,1000};
				MaxNumber task = new MaxNumber(array, 0, array.length - 1);
				// 提交可分解的任务
				Future<Integer> future = pool.submit(task);
				System.out.println("Result:" + future.get(1, TimeUnit.SECONDS));
				// 关闭线程池
				pool.shutdown();
		}
}
```

### 3.2. 拆分任务无返回值

### 3.3. 使用 Fork/Join 实现归并排序

归并排序算法是目前所有排序算法中，平均时间复杂度较好（O(nlgn)），算法稳定性较好的一种排序算法。它的核心算法思路将大的问题分解成多个小问题，并将结果进行合并。

但是随着待排序集合中数据规模继续增大，普通的归并算法实现就有一些力不从心了，对上亿条随机数集合进行排序时，耗时已到达分钟级别。因此，**我们可以充分利用多核 CPU，使用 Fork/Join 框架来优化归并算法的执行性能，将拆分后的子任务实例化成多个 ForkJoinTask 任务放入待执行队列，并由 Fork/Join 框架在多个 ForkJoinWorkerThread 线程间调度这些任务**。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/12/cf7640df405402c95bdc2ab7ff009741.jpg)

```java
public class Merge2 {

    private static int MAX = 100000000;

    private static int inits[] = new int[MAX];

    // 同样进行随机队列初始化，这里就不再赘述了
    static {
        ......
    }

    public static void main(String[] args) throws Exception {   
        // 正式开始
        long beginTime = System.currentTimeMillis();
        ForkJoinPool pool = new ForkJoinPool();
        MyTask task = new MyTask(inits);
        ForkJoinTask<int[]> taskResult = pool.submit(task);
        try {
            taskResult.get();
        } catch (InterruptedException | ExecutionException e) {
            e.printStackTrace(System.out);
        }
        long endTime = System.currentTimeMillis();
        System.out.println("耗时 =" + (endTime - beginTime));      
    }

    /**
     * 单个排序的子任务
     * @author yinwenjie
     */
    static class MyTask extends RecursiveTask<int[]> {

        private int source[];

        public MyTask(int source[]) {
            this.source = source;
        }

        /* (non-Javadoc)
         * @see java.util.concurrent.RecursiveTask#compute()
         */
        @Override
        protected int[] compute() {
            int sourceLen = source.length;
            // 如果条件成立，说明任务中要进行排序的集合还不够小
            if(sourceLen > 2) {
                int midIndex = sourceLen / 2;
                // 拆分成两个子任务
                MyTask task1 = new MyTask(Arrays.copyOf(source, midIndex));
                task1.fork();
                MyTask task2 = new MyTask(Arrays.copyOfRange(source, midIndex , sourceLen));
                task2.fork();
                // 将两个有序的数组，合并成一个有序的数组
                int result1[] = task1.join();
                int result2[] = task2.join();
                int mer[] = joinInts(result1 , result2);
                return mer;
            } 
            // 否则说明集合中只有一个或者两个元素，可以进行这两个元素的比较排序了
            else {
                // 如果条件成立，说明数组中只有一个元素，或者是数组中的元素都已经排列好位置了
                if(sourceLen == 1
                    || source[0] <= source[1]) {
                    return source;
                } else {
                    int targetp[] = new int[sourceLen];
                    targetp[0] = source[1];
                    targetp[1] = source[0];
                    return targetp;
                }
            }
        }

        private int[] joinInts(int array1[] , int array2[]) {
            // 和上文中出现的代码一致
        }
    }
}
```
总体上这样的方式比不使用 Fork/Join 框架的归并排序算法在性能上有 30% 左右的性能提升。除了归并算法代码实现内部可优化的细节处，使用 Fork/Join 框架后，我们基本上在保证操作系统线程规模的情况下，将每一个 CPU 内核的运算资源同时发挥了出来。

## 4. ForkJoinTask

https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/ForkJoinTask.html

### 4.1. 基本 API

Fork/Join 框架中提供的 fork 方法和 join 方法，可以说是该框架中提供的最重要的两个方法。

- `final ForkJoinTask<V> fork()`: Arranges to asynchronously execute this task in the pool the current task is running in, if applicable, or using the ForkJoinPool.commonPool() if not inForkJoinPool().

  fork() 方法会启动一个新的并行 Fork/Join 子任务，并将新创建的子任务放入当前线程的 work queue 队列中。
  
  Fork/Join 框架将根据当前正在并发执行 ForkJoinTask 任务的 ForkJoinWorkerThread 线程状态，决定是让这个任务在队列中等待，还是创建一个新的 ForkJoinWorkerThread 线程运行它，又或者是唤起其它正在等待任务的 ForkJoinWorkerThread 线程运行它。

- `final V join()`: Returns the result of the computation when it is done.

  join() 方法用于让当前线程阻塞，直到对应任务的所有子任务完成运行并返回执行结果。
  
  如果这个子任务存在于当前线程的任务等待队列（work queue）中，则取出这个子任务进行“递归”执行。其目的是尽快得到当前子任务的运行结果，然后继续执行。

	 `static void	invokeAll​(ForkJoinTask<?>... tasks)`: Forks the given tasks, returning when isDone holds for each task or an (unchecked) exception is encountered, in which case the exception is rethrown.

	invokeAll() 的 N 个任务中，其中 N-1 个任务会使用 fork() 交给其它线程执行，但是，它还会留一个任务自己执行，这样，就充分利用了线程池，保证没有空闲的不干活的线程。

### 4.2. 源码分析

#### 4.2.1. 类定义

```java
public abstract class ForkJoinTask<V> implements Future<V>, Serializable {}
```

#### 4.2.2. 关键方法

- fork()
	```java
	public final ForkJoinTask<V> fork() {
			Thread t;
			// 首先取到了当前线程，然后判断是否是我们的 ForkJoinPool 专用线程
			if ((t = Thread.currentThread()) instanceof ForkJoinWorkerThread)
					// 如果是，则强制类型转换（向下转换）成 ForkJoinWorkerThread，然后将任务 push 到这个线程负责的队列里面去
					((ForkJoinWorkerThread)t).workQueue.push(this);
			else
					// 如果当前线程不是 ForkJoinWorkerThread 类型的线程
					// 首先尝试将任务提交给当前线程，如果不成功，则使用例外的处理方法
					ForkJoinPool.common.externalPush(this);
			return this;
	}
	```

- join()
	```java
	public final V join() {
			int s;
			if ((s = doJoin() & DONE_MASK) != NORMAL)
					reportException(s);
			return getRawResult();
	}

	private int doJoin() {
			int s; Thread t; ForkJoinWorkerThread wt; ForkJoinPool.WorkQueue w;
			return (s = status) < 0 ? s :
					((t = Thread.currentThread()) instanceof ForkJoinWorkerThread) ?
					(w = (wt = (ForkJoinWorkerThread)t).workQueue).
					tryUnpush(this) && (s = doExec()) < 0 ? s :
					wt.pool.awaitJoin(w, this, 0L) :
					externalAwaitDone();
	}

	final int doExec() {
			int s; boolean completed;
			if ((s = status) >= 0) {
					try {
							completed = exec();
					} catch (Throwable rex) {
							return setExceptionalCompletion(rex);
					}
					if (completed)
							s = setCompletion(NORMAL);
			}
			return s;
	}

	/**
	* Implements execution conventions for RecursiveTask.
	*/
	protected final boolean exec() {
			result = compute();
			return true;
	}
	```

- invokeAll()
	```java
	public static void invokeAll(ForkJoinTask<?> t1, ForkJoinTask<?> t2) {
			int s1, s2;
			t2.fork();
			if ((s1 = t1.doInvoke() & DONE_MASK) != NORMAL)
					t1.reportException(s1);
			if ((s2 = t2.doJoin() & DONE_MASK) != NORMAL)
					t2.reportException(s2);
	}

	public static void invokeAll(ForkJoinTask<?>... tasks) {
			Throwable ex = null;
			int last = tasks.length - 1;
			for (int i = last; i >= 0; --i) {
					ForkJoinTask<?> t = tasks[i];
					if (t == null) {
							if (ex == null)
									ex = new NullPointerException();
					}
					else if (i != 0)
							t.fork();
					else if (t.doInvoke() < NORMAL && ex == null)
							ex = t.getException();
			}
			for (int i = 1; i <= last; ++i) {
					ForkJoinTask<?> t = tasks[i];
					if (t != null) {
							if (ex != null)
									t.cancel(false);
							else if (t.doJoin() < NORMAL)
									ex = t.getException();
					}
			}
			if (ex != null)
					rethrow(ex);
	}

	public static <T extends ForkJoinTask<?>> Collection<T> invokeAll(Collection<T> tasks) {
			if (!(tasks instanceof RandomAccess) || !(tasks instanceof List<?>)) {
					invokeAll(tasks.toArray(new ForkJoinTask<?>[tasks.size()]));
					return tasks;
			}
			@SuppressWarnings("unchecked")
			List<? extends ForkJoinTask<?>> ts =
					(List<? extends ForkJoinTask<?>>) tasks;
			Throwable ex = null;
			int last = ts.size() - 1;
			for (int i = last; i >= 0; --i) {
					ForkJoinTask<?> t = ts.get(i);
					if (t == null) {
							if (ex == null)
									ex = new NullPointerException();
					}
					else if (i != 0)
							t.fork();
					else if (t.doInvoke() < NORMAL && ex == null)
							ex = t.getException();
			}
			for (int i = 1; i <= last; ++i) {
					ForkJoinTask<?> t = ts.get(i);
					if (t != null) {
							if (ex != null)
									t.cancel(false);
							else if (t.doJoin() < NORMAL)
									ex = t.getException();
					}
			}
			if (ex != null)
					rethrow(ex);
			return tasks;
	}
	```

## 5. RecursiveTask

https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/RecursiveTask.html

```java
public abstract class RecursiveTask<V> extends ForkJoinTask<V> {
    private static final long serialVersionUID = 5232453952276485270L;

    /**
     * The result of the computation.
     */
    V result;

    /**
     * The main computation performed by this task.
     * @return the result of the computation
     */
    protected abstract V compute();

    public final V getRawResult() {
        return result;
    }

    protected final void setRawResult(V value) {
        result = value;
    }

    /**
     * Implements execution conventions for RecursiveTask.
     */
    protected final boolean exec() {
        result = compute();
        return true;
    }

}
```

## 6. RecursiveAction

https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/RecursiveAction.html

```java
public abstract class RecursiveAction extends ForkJoinTask<Void> {
    private static final long serialVersionUID = 5232453952276485070L;

    /**
     * The main computation performed by this task.
     */
    protected abstract void compute();

    /**
     * Always returns {@code null}.
     *
     * @return {@code null} always
     */
    public final Void getRawResult() { return null; }

    /**
     * Requires null completion value.
     */
    protected final void setRawResult(Void mustBeNull) { }

    /**
     * Implements execution conventions for RecursiveActions.
     */
    protected final boolean exec() {
        compute();
        return true;
    }

}
```

## 7. ForkJoinWorkerThread

[ForkJoinWorkerThread](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/ForkJoinWorkerThread.html) 线程是一种在 Fork/Join 框架中运行的特性线程，它除了具有普通线程的特性外，最主要的特点是每一个 ForkJoinWorkerThread 线程都具有一个独立的任务等待队列（work queue），这个任务队列用于存储在本线程中被拆分的若干子任务。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/12/340f100f12e90caf2a8ea72e48077f6d.jpg)

## 8. ForkJoinPool

### 8.1. 基本概念

https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/ForkJoinPool.html

### 8.2. 基本 API

#### 8.2.1. 构造方法

- `ForkJoinPool(int parallelism, ForkJoinWorkerThreadFactory factory, UncaughtExceptionHandler handler, boolean asyncMode)`
  
  参数说明：
  - parallelism: 
    
    可并行级别，Fork/Join 框架将依据这个并行级别的设定，决定框架内并行执行的线程数量。

    推荐基于当前操作系统可以使用的 CPU 内核数作为 Fork/Join 框架内最大并行任务数量，这样可以保证 CPU 在处理并行任务时，尽量少发生任务线程间的运行状态切换。
  
  - factory: 
    
    当 Fork/Join 框架创建一个新的线程时，同样会用到线程创建工厂。只不过这个线程工厂不再需要实现 ThreadFactory 接口，而是需要实现 ForkJoinWorkerThreadFactory 接口。
    
    ForkJoinWorkerThreadFactory 接口是一个函数式接口，只需要实现一个名叫 newThread 的方法。在 Fork/Join 框架中有一个默认的 ForkJoinWorkerThreadFactory 接口实现：DefaultForkJoinWorkerThreadFactory。
  
  - handler: 

    异常捕获处理器。当执行的任务中出现异常，并从任务中被抛出时，就会被 handler 捕获。

  - asyncMode: 

    Fork/Join 框架中为每一个独立工作的线程准备了对应的待执行任务队列，这个任务队列是使用数组进行组合的双向队列。当 asyncMode 设置为 ture 的时候，队列采用先进先出方式工作；反之则是采用后进先出的方式工作，该值默认为 false。

- `ForkJoinPool(int parallelism)`

- `ForkJoinPool()`

#### 8.2.2. 关键方法

- `static ForkJoinPool commonPool()`: 

  JDK1.8 新增的方法，返回一个通用池实例。通用池的运行状态不会受 shutdown() 或 shutdownNow() 方法的影响。当然，若程序直接通过 `System.exit(0)` 来退出 JVM，通用池以及通用池中正在执行的任务都会被终止。

  在 ForkJoinPool 主类的注释说明中，有这样一句话：
  > A static commonPool() is available and appropriate for most applications. The common pool is used by any ForkJoinTask that is not explicitly submitted to a specified pool. 
  > Using the common pool normally reduces resource usage (its threads are slowly reclaimed during periods of non-use, and reinstated upon subsequent use).

  即静态方法 commonPool() 所获得的 ForkJoinPools 实例是由整个应用进程共享的，并且它适合绝大多数的应用系统场景。使用 commonPool 通常可以帮助应用程序中多种需要进行归并计算的任务共享计算资源，从而使后者发挥最大作用。因此，**这种获取 ForkJoinPool 实例的方式，才是 Doug Lea 推荐的使用方式**。
  ```java
  ForkJoinPool commonPool =  ForkJoinPool.commonPool();
  ```


- `boolean awaitTermination​(long timeout, TimeUnit unit)`: Blocks until all tasks have completed execution after a shutdown request, or the timeout occurs, or the current thread is interrupted, whichever happens first.

- `execute​()`
  - `void	execute​(Runnable task)`: Executes the given command at some time in the future.
  	 `void	execute​(ForkJoinTask<?> task)`: Arranges for (asynchronous) execution of the given task.

  	`<T> T	invoke​(ForkJoinTask<T> task)`: Performs the given task, returning its result upon completion.

- `submit​()`
  - `ForkJoinTask<?>	submit​(Runnable task)`: Submits a Runnable task for execution and returns a Future representing that task.
  	 `<T> ForkJoinTask<T>	submit​(Runnable task, T result)`: Submits a Runnable task for execution and returns a Future representing that task.
  	 `<T> ForkJoinTask<T>	submit​(Callable<T> task)`: Submits a value-returning task for execution and returns a Future representing the pending results of the task.
  	 `<T> ForkJoinTask<T>	submit​(ForkJoinTask<T> task)`: Submits a ForkJoinTask for execution.

  	`void	shutdown​()`: Possibly initiates an orderly shutdown in which previously submitted tasks are executed, but no new tasks will be accepted.

  	`List<Runnable>	shutdownNow​()`: Possibly attempts to cancel and/or stop all tasks, and reject all subsequently submitted tasks.

### 8.3. 源码分析

#### 8.3.1. 类定义

```java
@sun.misc.Contended
public class ForkJoinPool extends AbstractExecutorService {
}
```

#### 8.3.2. 内部属性

workQueues 用于保存向 ForkJoinPool 提交的任务，而具体的执行有 ForkJoinWorkerThread 执行，而 ForkJoinWorkerThreadFactory 可以用于生产出 ForkJoinWorkerThread。
```java
volatile WorkQueue[] workQueues;     // main registry
final ForkJoinWorkerThreadFactory factory;
```

#### 8.3.3. 构造方法

```java
public ForkJoinPool() {
    this(Math.min(MAX_CAP, Runtime.getRuntime().availableProcessors()),
          defaultForkJoinWorkerThreadFactory, null, false);
}
public ForkJoinPool(int parallelism) {
    this(parallelism, defaultForkJoinWorkerThreadFactory, null, false);
}
public ForkJoinPool(int parallelism,
                    ForkJoinWorkerThreadFactory factory,
                    UncaughtExceptionHandler handler,
                    boolean asyncMode) {
    this(checkParallelism(parallelism),
          checkFactory(factory),
          handler,
          asyncMode ? FIFO_QUEUE : LIFO_QUEUE,
          "ForkJoinPool-" + nextPoolId() + "-worker-");
    checkPermission();
}

private ForkJoinPool(int parallelism,
                      ForkJoinWorkerThreadFactory factory,
                      UncaughtExceptionHandler handler,
                      int mode,
                      String workerNamePrefix) {
    this.workerNamePrefix = workerNamePrefix;
    this.factory = factory;
    this.ueh = handler;
    this.config = (parallelism & SMASK) | mode;
    long np = (long)(-parallelism); // offset ctl counts
    this.ctl = ((np << AC_SHIFT) & AC_MASK) | ((np << TC_SHIFT) & TC_MASK);
}
private static void checkPermission() {
    SecurityManager security = System.getSecurityManager();
    if (security != null)
        security.checkPermission(modifyThreadPermission);
}
private static ForkJoinWorkerThreadFactory checkFactory
    (ForkJoinWorkerThreadFactory factory) {
    if (factory == null)
        throw new NullPointerException();
    return factory;
}
```

#### 8.3.4. 任务提交

在 ForkJoinPool 中包含了 3 种任务提交的方法：invoke()、execute() 和 submit()。

若要提交的任务是 ForkJoinTask 且属于当前 ForkJoinPool：
- execute() 和 submit() 方法直接调用 externalPush() 将 task 加入到工作线程的任务队列后返回。
- invoke() 会调用 externalPush() 方法后还调用 ForkJoinTask 的 join() 方法，因此不会立即返回，直到传递的参数任务完成它的执行。

若要提交的任务不是 ForkJoinTask 或不属于当前 ForkJoinPool：
- execute() 和 submit() 方法都是将非 ForkJoinTask 类型的任务封装为 ForkJoinTask，然后再调用 externalPush() 将 task 加入到工作线程的任务队列。
- invoke() 方法不接受非 ForkJoinTask 类型的参数。

除此之外，submit()，invoke() 有返回值（返回指派的任务的计算结果），而 execute() 没有返回值。

##### 8.3.4.1. invoke()

```java
public <T> T invoke(ForkJoinTask<T> task) {
		if (task == null)
				throw new NullPointerException();
		externalPush(task);
		return task.join();
}
```

##### 8.3.4.2. execute()

```java
public void execute(ForkJoinTask<?> task) {
		if (task == null)
				throw new NullPointerException();
		externalPush(task);
}

// AbstractExecutorService methods

public void execute(Runnable task) {
		if (task == null)
				throw new NullPointerException();
		ForkJoinTask<?> job;
		if (task instanceof ForkJoinTask<?>) // avoid re-wrap
				job = (ForkJoinTask<?>) task;
		else
				job = new ForkJoinTask.RunnableExecuteAction(task);
		externalPush(job);
}

final void externalPush(ForkJoinTask<?> task) {
		WorkQueue[] ws; WorkQueue q; int m;
		int r = ThreadLocalRandom.getProbe();
		int rs = runState;
		if ((ws = workQueues) != null && (m = (ws.length - 1)) >= 0 &&
				(q = ws[m & r & SQMASK]) != null && r != 0 && rs > 0 &&
				U.compareAndSwapInt(q, QLOCK, 0, 1)) {
				ForkJoinTask<?>[] a; int am, n, s;
				if ((a = q.array) != null &&
						(am = a.length - 1) > (n = (s = q.top) - q.base)) {
						int j = ((am & s) << ASHIFT) + ABASE;
						U.putOrderedObject(a, j, task);
						U.putOrderedInt(q, QTOP, s + 1);
						U.putIntVolatile(q, QLOCK, 0);
						if (n <= 1)
								signalWork(ws, q);
						return;
				}
				U.compareAndSwapInt(q, QLOCK, 1, 0);
		}
		externalSubmit(task);
}

private void externalSubmit(ForkJoinTask<?> task) {
		int r;                                    // initialize caller's probe
		if ((r = ThreadLocalRandom.getProbe()) == 0) {
				ThreadLocalRandom.localInit();
				r = ThreadLocalRandom.getProbe();
		}
		for (;;) {
				WorkQueue[] ws; WorkQueue q; int rs, m, k;
				boolean move = false;
				if ((rs = runState) < 0) {
						tryTerminate(false, false);     // help terminate
						throw new RejectedExecutionException();
				}
				else if ((rs & STARTED) == 0 ||     // initialize
									((ws = workQueues) == null || (m = ws.length - 1) < 0)) {
						int ns = 0;
						rs = lockRunState();
						try {
								if ((rs & STARTED) == 0) {
										U.compareAndSwapObject(this, STEALCOUNTER, null,
																						new AtomicLong());
										// create workQueues array with size a power of two
										int p = config & SMASK; // ensure at least 2 slots
										int n = (p > 1) ? p - 1 : 1;
										n |= n >>> 1; n |= n >>> 2;  n |= n >>> 4;
										n |= n >>> 8; n |= n >>> 16; n = (n + 1) << 1;
										workQueues = new WorkQueue[n];
										ns = STARTED;
								}
						} finally {
								unlockRunState(rs, (rs & ~RSLOCK) | ns);
						}
				}
				else if ((q = ws[k = r & m & SQMASK]) != null) {
						if (q.qlock == 0 && U.compareAndSwapInt(q, QLOCK, 0, 1)) {
								ForkJoinTask<?>[] a = q.array;
								int s = q.top;
								boolean submitted = false; // initial submission or resizing
								try {                      // locked version of push
										if ((a != null && a.length > s + 1 - q.base) ||
												(a = q.growArray()) != null) {
												int j = (((a.length - 1) & s) << ASHIFT) + ABASE;
												U.putOrderedObject(a, j, task);
												U.putOrderedInt(q, QTOP, s + 1);
												submitted = true;
										}
								} finally {
										U.compareAndSwapInt(q, QLOCK, 1, 0);
								}
								if (submitted) {
										signalWork(ws, q);
										return;
								}
						}
						move = true;                   // move on failure
				}
				else if (((rs = runState) & RSLOCK) == 0) { // create new queue
						q = new WorkQueue(this, null);
						q.hint = r;
						q.config = k | SHARED_QUEUE;
						q.scanState = INACTIVE;
						rs = lockRunState();           // publish index
						if (rs > 0 &&  (ws = workQueues) != null &&
								k < ws.length && ws[k] == null)
								ws[k] = q;                 // else terminated
						unlockRunState(rs, rs & ~RSLOCK);
				}
				else
						move = true;                   // move if busy
				if (move)
						r = ThreadLocalRandom.advanceProbe(r);
		}
}
```

##### 8.3.4.3. submit()

```java
public <T> ForkJoinTask<T> submit(ForkJoinTask<T> task) {
		if (task == null)
				throw new NullPointerException();
		externalPush(task);
		return task;
}

public <T> ForkJoinTask<T> submit(Callable<T> task) {
		ForkJoinTask<T> job = new ForkJoinTask.AdaptedCallable<T>(task);
		externalPush(job);
		return job;
}

public <T> ForkJoinTask<T> submit(Runnable task, T result) {
		ForkJoinTask<T> job = new ForkJoinTask.AdaptedRunnable<T>(task, result);
		externalPush(job);
		return job;
}

public ForkJoinTask<?> submit(Runnable task) {
		if (task == null)
				throw new NullPointerException();
		ForkJoinTask<?> job;
		if (task instanceof ForkJoinTask<?>) // avoid re-wrap
				job = (ForkJoinTask<?>) task;
		else
				job = new ForkJoinTask.AdaptedRunnableAction(task);
		externalPush(job);
		return job;
}
```

#### 8.3.5. 创建通用池

```java
static {
  // ......
	// 排除 Java security 安全策略框架的权限检查
  common = java.security.AccessController.doPrivileged
            (new java.security.PrivilegedAction<ForkJoinPool>() {
                public ForkJoinPool run() { return makeCommonPool(); }});
  // report 1 even if threads disabled
  int par = common.config & SMASK; 
  commonParallelism = par > 0 ? par : 1;
  // ......
}

// ......

// 这是主要的创建过程
private static ForkJoinPool makeCommonPool() {
   int parallelism = -1;
   ForkJoinWorkerThreadFactory factory = null;
   UncaughtExceptionHandler handler = null;
   // 可以在程序启动时，指定这些参数的方式来完成并行等级、线程工厂、异常处理类的指定工作
   try {
       // 首先确认在启动应用程序时是否指定了这些参数，来控制 CommonPool 的创建过程
       // ignore exceptions in accessing/parsing properties
       String pp =
       System.getProperty("java.util.concurrent.ForkJoinPool.common.parallelism");
       String fp = 
       System.getProperty("java.util.concurrent.ForkJoinPool.common.threadFactory");
       String hp =
       System.getProperty("java.util.concurrent.ForkJoinPool.common.exceptionHandler");
       if (pp != null)
           parallelism = Integer.parseInt(pp);
       if (fp != null)
           factory = ((ForkJoinWorkerThreadFactory)ClassLoader.getSystemClassLoader().loadClass(fp).newInstance());
       if (hp != null)
           handler = ((UncaughtExceptionHandler)ClassLoader.getSystemClassLoader().loadClass(hp).newInstance());
   } catch (Exception ignore) {
   }
   // 没有在启动时指定以上参数，启动默认参数
   if (factory == null) {
       // 如果当前没有启动 SecurityManager，安全策略管理器
       // 这时使用 defaultForkJoinWorkerThreadFactory 这个工厂对象
       // 它是 java.util.concurrent.ForkJoinPool.DefaultForkJoinWorkerThreadFactory 这个类的实例
       if (System.getSecurityManager() == null)
           factory = defaultForkJoinWorkerThreadFactory;
       else 
           // use security-managed default
           factory = new InnocuousForkJoinWorkerThreadFactory();
   }

   // 如果并行等级小于 0，并且当前应用程序可用 CPU 内核数为 1，那么设定 parallelism 并行等级为 1
   if (parallelism < 0 && // default 1 less than #cores
       (parallelism = Runtime.getRuntime().availableProcessors() - 1) <= 0)
       parallelism = 1;
   if (parallelism > MAX_CAP)
       parallelism = MAX_CAP;

   // 最后使用构造函数初始化 commonPool
   return new ForkJoinPool(parallelism, factory, handler, LIFO_QUEUE, "ForkJoinPool.commonPool-worker-");
}
```

## 9. Refer Links

[线程基础：多任务处理（12）——Fork/Join 框架（基本使用）](https://blog.csdn.net/yinwenjie/article/details/71524140)

[线程基础：多任务处理（13）——Fork/Join 框架（解决排序问题）](https://blog.csdn.net/yinwenjie/article/details/71915811)

[线程基础：多任务处理（14）——Fork/Join 框架（要点 1）](https://blog.csdn.net/yinwenjie/article/details/72520759)

[线程基础：多任务处理（15）——Fork/Join 框架（要点 2）](https://blog.csdn.net/yinwenjie/article/details/72639297)

[线程基础：多任务处理（16）——Fork/Join 框架（排序算法性能补充）](https://blog.csdn.net/yinwenjie/article/details/72828691)

[Java 的 Fork/Join 任务，你写对了吗？](https://www.liaoxuefeng.com/article/001493522711597674607c7f4f346628a76145477e2ff82000)

[Doug Lea: A Java Fork/Join Framework 译文](http://ifeve.com/java-fork-join-framework/#more-35602)
