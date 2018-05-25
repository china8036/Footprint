- [Java 多线程 - 线程安全 - 阻塞同步方案 - API 层面：Lock 框架](#java-%E5%A4%9A%E7%BA%BF%E7%A8%8B---%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8---%E9%98%BB%E5%A1%9E%E5%90%8C%E6%AD%A5%E6%96%B9%E6%A1%88---api-%E5%B1%82%E9%9D%A2%EF%BC%9Alock-%E6%A1%86%E6%9E%B6)
	- [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [1.1. 功能特性](#11-%E5%8A%9F%E8%83%BD%E7%89%B9%E6%80%A7)
		- [1.2. 类谱图](#12-%E7%B1%BB%E8%B0%B1%E5%9B%BE)
	- [2. Lock 接口](#2-lock-%E6%8E%A5%E5%8F%A3)
	- [3. ReentrantLock 实现类](#3-reentrantlock-%E5%AE%9E%E7%8E%B0%E7%B1%BB)
		- [3.1. 常用 API](#31-%E5%B8%B8%E7%94%A8-api)
		- [3.2. 实现原理](#32-%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
			- [3.2.1. 内部类](#321-%E5%86%85%E9%83%A8%E7%B1%BB)
			- [3.2.2. 构造方法](#322-%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95)
	- [4. ReadWriteLock 接口](#4-readwritelock-%E6%8E%A5%E5%8F%A3)
	- [5. ReentrantReadWriteLock 实现类](#5-reentrantreadwritelock-%E5%AE%9E%E7%8E%B0%E7%B1%BB)
		- [5.1. 常用 API](#51-%E5%B8%B8%E7%94%A8-api)
		- [5.2. 实现原理](#52-%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
	- [6. StampedLock 类](#6-stampedlock-%E7%B1%BB)
	- [7. 死锁](#7-%E6%AD%BB%E9%94%81)
	- [8. Refer Links](#8-refer-links)

# Java 多线程 - 线程安全 - 阻塞同步方案 - API 层面：Lock 框架

从 JDK1.5 开始，Java 在 API 层面提供了一种功能更为强大的互斥同步锁，即 **java.util.Concurrent.locks 包中与锁相关的的一系列接口及其实现类，统称为 Lock 框架**。相比 synchronized 关键字，Lock 框架提供了更广泛、更灵活、粒度更细的同步锁定操作，能够以更优雅的方式处理线程同步问题。

## 1. 基本概念

### 1.1. 功能特性

synchronized 关键字是一种基于 JVM 层面实现的原生语法层面的互斥锁，Lock 框架是基于 JDK 层面实现的 Java 接口，**既然 synchronized 可以实现对临界资源的同步互斥访问，为什么还会出现 Lock 框架呢？**
- 等待可中断

	假如占有锁的线程由于要等待 IO 或者其他原因（比如调用 sleep 方法）被阻塞了，但是又没有释放锁，如果使用 synchronized 的同步机制，那么其他线程就只能一直等待，别无他法。

	若使用 Lock 框架，可通过设置超时等待 `tryLock(long time, TimeUnit unit)` 或响应中断 `lockInterruptibly()` 来解决这个问题，不让等待的线程一直无期限地等待下去。

- 读写分离

	当多个线程读写文件时，读操作和写操作会发生冲突现象，写操作和写操作也会发生冲突现象，但是读操作和读操作不会发生冲突现象。但是如果采用 synchronized 关键字实现同步的话，就会导致一个问题，即当多个线程都只是进行读操作时，也只有一个线程在可以进行读操作，其他线程只能等待锁的释放而无法进行读操作。

	若使用 Lock 框架中的 ReentrantReadWriteLock，可使得当多个线程都只是进行读操作时，线程之间不会发生冲突。

- 公平锁
 
	公平锁指的是多个线程在等待同一个锁时，必须按照申请锁的时间先后顺序来依次获得锁，而非公平锁不能保证这一点。synchronized 中的锁是非公平的，ReentrantLock 默认情况下也是非公平的，但可通过参数设置使用公平锁。

NOTE: 
- 采用 synchronized 方式不需要程序员去手动释放锁，当 synchronized 方法或 synchronized 代码块执行完之后，JVM 会自动让线程释放对锁的占用。而 Lock 则必须要程序员去手动释放锁 （即使发生异常也不会自动释放锁)，如果没有主动释放锁，就有可能导致死锁现象。

- 在 JDK1.5 中，也就是 Lock 框架刚刚发布时，多线程环境下的 ReentrantLock 可以表现出比 synchronized 关键字更加稳定和优秀的性能。但随着 JDK1.6 中针对 synchronized 关键字的重量级锁加入了一系列优化措施，使得 synchronized 与 ReentrantLock 的性能基本完全持平了。
	
	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/1/c2ac6ace6273d1ddfab32dbc31183a66.jpg)

	由于 JVM 在未来的性能改进中也更偏向于原生语法层面的 synchronized，因此**提倡在 synchronized 能满足并发需求的情况下，优先使用 synchronized 来实现并发同步；而当确实需要 Lock 对象所提供的功能方法时，才使用 API 层面的 Lock 锁。**

### 1.2. 类谱图

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/1/945544f8307ebb1fe25de6b777f9ec7a.jpg)

JDK1.5 在 [java.util.Concurrent.locks](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/locks/package-summary.html) 包中提供了两个根接口：Lock 和 ReadWriteLock，并为 Lock 接口提供了 ReentrantLock 实现类，为 ReadWriteLock 接口提供了 ReentrantReadWriteLock 实现类。

JDK1.8 新增了 StampedLock 类，在大多数场景中它可以替代传统的 ReentrantReadWriteLock。

## 2. Lock 接口

[Lock](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/locks/Lock.html) 接口是 Lock 框架的接口之一。

查看 Lock 接口源码（省略注释部分）：
```java
public interface Lock {
    void lock();
		// 相当于一个超时时间设置为无限的 tryLock 方法。可以响应中断，若线程在阻塞期间被中断，会抛出 InterruptException
    void lockInterruptibly() throws InterruptedException;  
		// 尝试获得锁。若成功获取锁，返回 true；否则，立即返回 false 而不会阻塞
    boolean tryLock();
		// 尝试获得锁，阻塞时间不会超过给定的值。可以响应中断，若线程在阻塞期间被中断，会抛出 InterruptException
    boolean tryLock(long time, TimeUnit unit) throws InterruptedException;  
    void unlock();
    Condition newCondition();
}
```
其中：
- lock()、tryLock()、tryLock(long time, TimeUnit unit) 和 lockInterruptibly() 都是用于获取锁。

	- lock()

		lock() 方法是平常使用得最多的一个方法，就是用来获取锁。如果锁已被其他线程获取，则进行等待。如果采用 Lock，必须主动去释放锁，并且在发生异常时，不会自动释放锁。因此，一般来说，使用 Lock 必须在 try…catch…块中进行，并且将释放锁的操作放在 finally 块中进行，以保证锁一定被被释放，防止死锁的发生。
		```java
		Lock lock = ...;
		lock.lock();
		try {
				// 处理任务
		} catch (Exception ex) {

		} finally {
				lock.unlock();   // 释放锁
		}
		```
		NOTE: 若将 Lock 声明为方法内的局部变量，则每个线程执行该方法时都会拥有一个新的 Lock 对象，导致每个线程执行到 lock.lock() 处获取的是不同的锁，不会对临界资源形成同步互斥访问。因此，这种情况下应将 Lock 对象声明为多个线程可共享的类成员变量。

	- tryLock() & tryLock(long time, TimeUnit unit)

		tryLock() 方法表示用来尝试获取锁，如果获取成功，则返回 true；如果获取失败（即锁已被其他线程获取），则立即返回 false 而不进行等待。

		tryLock(long time, TimeUnit unit) 方法在 tryLock() 方法的基础上，设置了允许等待的超时时间。如果一开始拿到锁或者在等待期间内拿到了锁，则立即返回 true。如果拿不到锁，会等待一定的时间，在时间期限之内如果还拿不到锁，就返回 false，同时可以响应中断。
		```java
		Lock lock = ...;
		if (lock.tryLock()) {
				try {
						// 处理任务
				} catch(Exception ex) {

				} finally {
						lock.unlock();   // 释放锁
				} 
		}else {
				// 如果不能获取锁，则直接做其他事情
		}
		```

	- lockInterruptibly()

		lockInterruptibly() 方法比较特殊，它相当于超时时间设置为无限的 tryLock() 方法，且当通过这个方法去获取锁时，该方法能够响应中断，中断线程的等待状态。例如，当两个线程同时通过 lock.lockInterruptibly() 想获取某个锁时，假若此时线程 A 获取到了锁，而线程 B 只有在等待，那么对线程 B 调用 threadB.interrupt() 方法能够中断线程 B 的等待过程。
		```java
		// 推荐在调用 lockInterruptibly() 的方法外声明抛出 InterruptedException
		public void method() throws InterruptedException {
				lock.lockInterruptibly();
				try {  
				//.....
				}
				finally {
						lock.unlock();
				}  
		}
		```

- unLock() 方法用于释放锁。

- newCondition() 返回绑定到此 Lock 的新的 Condition 实例，用于线程间的协作。TODO:

## 3. ReentrantLock 实现类

可重入锁 [ReentrantLock](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/locks/ReentrantLock.html) 是 Lock 接口唯一的实现类，可确保在任何时刻只有一个线程进入临界区，如果锁同时被另一个线程拥有则发生阻塞，从而保证同步的安全。

ReentrantLock 锁是可重入的。线程可以重复地获得已持有的锁，锁保持一个持有计数来跟踪对 lock 方法的嵌套调用。线程在每一次调用 lock 后都要调用 unlock 来释放锁。**基于这一特性，被一个锁保护的代码可以调用另一个使用相同的锁的方法**。

### 3.1. 常用 API

构造方法：
- `ReentrantLock​()`：构建一个可重入锁
- `ReentrantLock​(boolean fair)`：构建一个带有公平策略的可重入锁。公平锁偏向于调度等待时间最长的线程，但公平锁会大大降低性能。实际上，即便使用公平锁也无法保证线程调度器是公平的。

常用 API：
- `int getHoldCount()`: 查询当前线程保持此锁的次数。
- `protected  Thread   getOwner()`: 返回目前拥有此锁的线程，如果此锁不被任何线程拥有，则返回 null。      
- `protected  Collection<Thread>   getQueuedThreads()`: 返回一个 collection，它包含可能正等待获取此锁的线程，其内部维持一个队列，这点稍后会分析。 
- `int getQueueLength();`: 返回正等待获取此锁的线程估计数。 
- `protected  Collection<Thread>   getWaitingThreads(Condition condition); `: 返回一个 collection，它包含可能正在等待与此锁相关给定条件的那些线程。
- `int getWaitQueueLength(Condition condition);`: 返回等待与此锁相关的给定条件的线程估计数。  
- `boolean hasQueuedThread(Thread thread); `: 查询给定线程是否正在等待获取此锁。    
- `boolean hasQueuedThreads();`: 查询是否有些线程正在等待获取此锁。 
- `boolean hasWaiters(Condition condition); `: 查询是否有些线程正在等待与此锁有关的给定条件。    
- `boolean isFair() `: 如果此锁的公平设置为 true，则返回 true。 
- `boolean isHeldByCurrentThread() `: 查询当前线程是否保持此锁。
- `boolean isLocked()`: 查询此锁是否由任意线程保持。    

例：
```java
Lock mylock = new ReentrantLock();
// ...
mylock.lock();
try {
	// 临界区
} finally {
	mylock.unlock();
}
```

### 3.2. 实现原理

ReentrantLock 内部基于 AQS 实现：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/1/1febf446dbc79ceb7a0be69a9a3aacb4.jpg)

#### 3.2.1. 内部类

ReentrantLock 内部存在 3 个实现类，分别是 Sync、NonfairSync、FairSync，其中 Sync 继承自 AQS 实现了解锁 tryRelease() 方法，而 NonfairSync（非公平锁)、FairSync（公平锁) 则继承自 Sync，实现了获取锁的 tryAcquire() 方法，**ReentrantLock 的所有方法调用都通过间接调用 AQS 和 Sync 类及其子类来完成的**。

Sync：抽象内部类，继承自 AbstractQueuedSynchronizer，实现了释放锁的操作 (tryRelease() 方法)，并提供了 lock 抽象方法，由其子类实现。
```java
abstract static class Sync extends AbstractQueuedSynchronizer {
		private static final long serialVersionUID = -5179523762034025860L;

		/**
			* Performs {@link Lock#lock}. The main reason for subclassing
			* is to allow fast path for nonfair version.
			*/
		abstract void lock();

		// 实现 AQS 中没有实现的非公平获取锁方法
		final boolean nonfairTryAcquire(int acquires) {
				final Thread current = Thread.currentThread();
				int c = getState();
				// 判断同步状态是否为 0，并尝试再次获取同步状态
				if (c == 0) {
						// 执行 CAS 操作
						if (compareAndSetState(0, acquires)) {
								setExclusiveOwnerThread(current);
								return true;
						}
				}
				// 如果当前线程已获取锁，属于重入锁，再次获取锁后将 status 值加 1
				else if (current == getExclusiveOwnerThread()) {
						int nextc = c + acquires;
						if (nextc < 0) // overflow
								throw new Error("Maximum lock count exceeded");
						// 设置当前同步状态，当前只有一个线程持有锁，因为不会发生线程安全问题，可以直接执行 setState(nextc)
						setState(nextc);
						return true;
				}
				return false;
		}

		protected final boolean tryRelease(int releases) {
				int c = getState() - releases;
				if (Thread.currentThread() != getExclusiveOwnerThread())
						throw new IllegalMonitorStateException();
				boolean free = false;
				if (c == 0) {
						free = true;
						setExclusiveOwnerThread(null);
				}
				setState(c);
				return free;
		}

		protected final boolean isHeldExclusively() {
				// While we must in general read state before owner,
				// we don't need to do so to check if current thread is owner
				return getExclusiveOwnerThread() == Thread.currentThread();
		}

		final ConditionObject newCondition() {
				return new ConditionObject();
		}

		// Methods relayed from outer class

		final Thread getOwner() {
				return getState() == 0 ? null : getExclusiveOwnerThread();
		}

		final int getHoldCount() {
				return isHeldExclusively() ? getState() : 0;
		}

		final boolean isLocked() {
				return getState() != 0;
		}

		/**
			* Reconstitutes the instance from a stream (that is, deserializes it).
			*/
		private void readObject(java.io.ObjectInputStream s)
				throws java.io.IOException, ClassNotFoundException {
				s.defaultReadObject();
				setState(0); // reset to unlocked state
		}
}
```

NonfairSync：继承自 Sync，非公平锁的实现类。
```java
static final class NonfairSync extends Sync {
		private static final long serialVersionUID = 7316153563782823691L;

		/**
			* Performs lock.  Try immediate barge, backing up to normal
			* acquire on failure.
			*/
		final void lock() {
				// 执行封装在 AQS 中的 CAS 操作，获取同步状态
				// 可能同时存在多个线程设置 state 变量，因此使用 CAS 操作保证 state 变量操作的原子性
				if (compareAndSetState(0, 1))
						// 成功则将独占锁线程设置为当前线程  
						setExclusiveOwnerThread(Thread.currentThread());
				else
						// 失败则再次请求同步状态，即执行封装在 AQS 中的 acquire 方法
						acquire(1);
		}

		// 调用 Syn 中的 tryAcquire 实现
		protected final boolean tryAcquire(int acquires) {
				return nonfairTryAcquire(acquires);
		}
}
```

FairSync：继承自 Sync，公平锁的实现类。
```java
static final class FairSync extends Sync {
		private static final long serialVersionUID = -3000897897090466540L;

		final void lock() {
				acquire(1);
		}

		/**
			* Fair version of tryAcquire.  Don't grant access unless
			* recursive call or no waiters or is first.
			*/
		protected final boolean tryAcquire(int acquires) {
				final Thread current = Thread.currentThread();
				int c = getState();
				if (c == 0) {
						if (!hasQueuedPredecessors() &&
								compareAndSetState(0, acquires)) {
								setExclusiveOwnerThread(current);
								return true;
						}
				}
				else if (current == getExclusiveOwnerThread()) {
						int nextc = c + acquires;
						if (nextc < 0)
								throw new Error("Maximum lock count exceeded");
						setState(nextc);
						return true;
				}
				return false;
		}
}
```

#### 3.2.2. 构造方法

```java
// 默认构造，创建非公平锁 NonfairSync
public ReentrantLock() {
    sync = new NonfairSync();
}
// 根据传入参数创建锁类型
public ReentrantLock(boolean fair) {
    sync = fair ? new FairSync() : new NonfairSync();
}
```

## 4. ReadWriteLock 接口

[ReadWriteLock](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/locks/ReadWriteLock.html) 接口是 Lock 框架的接口之一。

ReadWriteLock 的源码定义非常简单，只包含了两个方法：一个用来获取读锁，一个用来获取写锁。

```java
public interface ReadWriteLock {
    /**
     * Returns the lock used for reading.
     *
     * @return the lock used for reading
     */
    Lock readLock();

    /**
     * Returns the lock used for writing.
     *
     * @return the lock used for writing
     */
    Lock writeLock();
}

```

## 5. ReentrantReadWriteLock 实现类

可重入读写锁 [ReentrantReadWriteLock](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/locks/ReentrantReadWriteLock.html) 是 ReadWriteLock 接口的实现类，可通过使用该类实现对读者、写者采用不同的访问控制策略（读者线程可共享访问，写者线程互斥访问），大大提升了读操作的效率。

### 5.1. 常用 API

构造方法：
- `ReentrantReadWriteLock​()`: Creates a new ReentrantReadWriteLock with default (nonfair) ordering properties.
- `ReentrantReadWriteLock​(boolean fair)`: Creates a new ReentrantReadWriteLock with the given fairness policy.

常用 API：
- `ReentrantReadWriteLock.ReadLock	readLock​()`：得到一个可以被多个读操作共用的读锁，但会排斥所有写操作。其中，ReadLock 是 ReentrantReadWriteLock 的静态内部类，实现了 Lock 接口。
- `ReentrantReadWriteLock.WriteLock	writeLock​()`：得到一个写锁，排斥所有其它读操作和写操作。其中，WriteLock 是 ReentrantReadWriteLock 的静态内部类，实现了 Lock 接口。

例：
```java
ReentrantReadWriteLock rwl = new ReentrantReadWriteLock();
rlock = rwl.readLock();
wlock = rwl.writeLock();

// 对所有读方法加读锁
public int getxxx() {
	rlock.lock();
	try {}
	finally {
		rlock.unlock();
	}
}

// 对所有写方法加写锁
public void setxxx() {
	wlock.lock();
	try {}
	finally {
		wlock.unlock();
	}
}
```

### 5.2. 实现原理

TODO:

## 6. StampedLock 类

[StampedLock](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/locks/StampedLock.html) 是 java8 在 java.util.concurrent.locks 新增的一个 API，在大多数场景中它可以替代传统的 ReentrantReadWriteLock，当线程数增多时，StampedLock 能比 ReentrantReadWriteLock 的吞吐量高出几倍。

ReentrantReadWriteLock 在沒有任何读写锁时，才可以取得写入锁。这等于实现了**悲观读取（Pessimistic Reading）策略，即悲观地认为会有写操作与读操作同时进行，进而导致线程不安全问题，因此在进行写操作时，不允许执行读操作**。

然而，如果读取执行情况很多，写入很少的情况下，使用 ReentrantReadWriteLock 的悲观读取策略可能会使写入线程遭遇饥饿（Starvation）问题，也就是写入线程迟迟无法竞争到锁定而一直处于等待状态。

StampedLock 控制锁有三种模式（写，读，乐观读），一个 StampedLock 状态是由版本和模式两个部分组成，锁获取方法返回一个数字作为票据 stamp，它用相应的锁状态表示并控制访问，数字 0 表示没有写锁被授权访问。在读锁上分为悲观锁和乐观锁。

所谓的乐观读取策略，即若读的操作很多，写的操作很少的情况下，**乐观地认为写操作与读操作同时发生几率极小，因此允许在进行写操作时，允许进行读操作**。程序可以查看读取资料之后，是否遭到写入执行的变更，再采取后续的措施（重新读取变更信息，或者抛出异常） ，这一个小小改进，可大幅度提高程序的吞吐量。

例：
```java
class Point {
		private double x, y;
		private final StampedLock sl = new StampedLock();
		void move(double deltaX, double deltaY) { // an exclusively locked method
				long stamp = sl.writeLock();
				try {
						x += deltaX;
						y += deltaY;
				} finally {
						sl.unlockWrite(stamp);
				}
		}

		// 下面看看乐观读锁案例
		double distanceFromOrigin() { // A read-only method
				long stamp = sl.tryOptimisticRead(); // 获得一个乐观读锁
				double currentX = x, currentY = y; // 将两个字段读入本地局部变量
				if (!sl.validate(stamp)) { // 检查发出乐观读锁后同时是否有其他写锁发生？
							stamp = sl.readLock(); // 如果没有，我们再次获得一个读悲观锁
							try {
									currentX = x; // 将两个字段读入本地局部变量
									currentY = y; // 将两个字段读入本地局部变量
							} finally {
									sl.unlockRead(stamp);
							}
				}
				return Math.sqrt(currentX * currentX + currentY * currentY);
		}

		// 下面是悲观读锁案例
		void moveIfAtOrigin(double newX, double newY) { // upgrade
				// Could instead start with optimistic, not read mode
				long stamp = sl.readLock();
				try {
						while (x == 0.0 && y == 0.0) { // 循环，检查当前状态是否符合
								long ws = sl.tryConvertToWriteLock(stamp); // 将读锁转为写锁
								if (ws != 0L) { // 这是确认转为写锁是否成功
										stamp = ws; // 如果成功 替换票据
										x = newX; // 进行状态改变
										y = newY; // 进行状态改变
										break;
								}
								else { // 如果不能成功转换为写锁
										sl.unlockRead(stamp); // 我们显式释放读锁
										stamp = sl.writeLock(); // 显式直接进行写锁 然后再通过循环再试
								}
						}
				} finally {
						sl.unlock(stamp); // 释放读锁或写锁
				}
		}
 }
```

## 7. 死锁

JVM 中没有任何可以检测死锁、避免死锁或打破死锁的机制，因此开发者必须仔细设计并发的应用程序，在并发编程中采取避免死锁的措施。一旦出现死锁，程序既不会发生任何异常，也不会给出任何提示，只是所有的线程处理阻塞状态，无法继续。

例：一个会导致死锁的 Java 程序
```java
public class DeadLockDemo {  
    /*  
     * This method request two locks, first String and then Integer  
     **/  
    public void method1() {  
        synchronized (String.class) {  
            System.out.println("Aquired lock on String.class object");  
            synchronized (Integer.class) {  
                System.out.println("Aquired lock on Integer.class object");  
            }  
        }  
    }  
  
    /* 
     * * This method also requests same two lock but in exactly * Opposite order 
     * i.e. first Integer and then String. * This creates potential deadlock, if 
     * one thread holds String lock * and other holds Integer lock and they wait 
     * for each other, forever. 
     */  
    public void method2() {  
        synchronized (Integer.class) {  
            System.out.println("Aquired lock on Integer.class object");  
            synchronized (String.class) {  
                System.out.println("Aquired lock on String.class object");  
            }  
        }  
    }  
}  
```
如果 method1() 和 method2() 都被两个或多个线程调用，死锁就很有可能发生。因为如果线程 1 在执行 method1() 时获取对 Sting 对象的锁，并且线程 2 在执行 method2() 时获取对 Integer 对象的锁，那么这两个线程都将在等待对方释放 Integer 和 String 锁，这将永远不会有结果。

那么应该如何修改代码以避免死锁？

以上代码导致死锁的真正原因并不是多线程，而是它们请求锁定的方式，如果你提供了一个有序的访问，那么问题就会解决，可以通过在没有抢占的情况下避免循环等待从而解决死锁问题。
```java
public class DeadLockFixed {  
    /** 
     * * Both method are now requesting lock in same order, first Integer and 
     * then String. * You could have also done reverse e.g. first String and 
     * then Integer, * both will solve the problem, as long as both method are 
     * requesting lock * in consistent order. 
     */  
    public void method1() {  
        synchronized (Integer.class) {  
            System.out.println("Aquired lock on Integer.class object");  
            synchronized (String.class) {  
                System.out.println("Aquired lock on String.class object");  
            }  
        }  
    }  
  
    public void method2() {  
        synchronized (Integer.class) {  
            System.out.println("Aquired lock on Integer.class object");  
            synchronized (String.class) {  
                System.out.println("Aquired lock on String.class object");  
            }  
        }  
    }  
}  
```
现在不会有任何死锁，因为两个方法都以相同的顺序访问 Integer 和 String 类的锁。因此，如果线程 A 获得了 Integer 对象上的锁，则线程 B 将不会继续，直到线程 A 释放 Integer 锁定为止，即使线程 B 持有 String 锁，线程 A 也不会被阻塞，因为现在线程 B 不会期望线程 A 释放 Integer 锁继续前进。

## 8. Refer Links

[Java 并发：Lock 框架详解](https://blog.csdn.net/justloveyou_/article/details/54972105)

[深入剖析基于并发 AQS 的（独占锁) 重入锁 (ReetrantLock) 及其 Condition 实现原理](https://blog.csdn.net/javazejian/article/details/75043422)

[Java 线程面试题 (02) Java 线程中如何避免死锁](https://blog.csdn.net/jinguangliu/article/details/78591477)

[Java 8 新特性探究（十）StampedLock 将是解决同步问题的新宠](https://my.oschina.net/benhaile/blog/264383)