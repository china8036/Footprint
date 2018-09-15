- [Java 多线程 并发集合框架：阻塞集合](#java-%E5%A4%9A%E7%BA%BF%E7%A8%8B-%E5%B9%B6%E5%8F%91%E9%9B%86%E5%90%88%E6%A1%86%E6%9E%B6%EF%BC%9A%E9%98%BB%E5%A1%9E%E9%9B%86%E5%90%88)
	- [1. BlockingQueue 接口](#1-blockingqueue-%E6%8E%A5%E5%8F%A3)
		- [1.1. 常用 API](#11-%E5%B8%B8%E7%94%A8-api)
		- [1.2. 实现原理](#12-%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
		- [1.3. 存在问题](#13-%E5%AD%98%E5%9C%A8%E9%97%AE%E9%A2%98)
	- [2. ArrayBlockingQueue 实现类](#2-arrayblockingqueue-%E5%AE%9E%E7%8E%B0%E7%B1%BB)
		- [2.1. 常用 API](#21-%E5%B8%B8%E7%94%A8-api)
		- [2.2. 实现原理](#22-%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
	- [3. LinkedBlockingQueue 实现类](#3-linkedblockingqueue-%E5%AE%9E%E7%8E%B0%E7%B1%BB)
		- [3.1. 常用 API](#31-%E5%B8%B8%E7%94%A8-api)
		- [3.2. 实现原理](#32-%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
	- [4. PriorityBlockingQueue 实现类](#4-priorityblockingqueue-%E5%AE%9E%E7%8E%B0%E7%B1%BB)
	- [5. DelayQueue 实现类](#5-delayqueue-%E5%AE%9E%E7%8E%B0%E7%B1%BB)
	- [6. LinkedTransferQueue 实现类](#6-linkedtransferqueue-%E5%AE%9E%E7%8E%B0%E7%B1%BB)
	- [7. Refer Links](#7-refer-links)

# Java 多线程 并发集合框架：阻塞集合

阻塞集合 (Blocking Collection) 包括添加和移除数据的方法。当集合已满或者为空时，被调用的添加或移除方法就不能立即执行，那么调用这个方法的线程将被阻塞，一直到该方法可以被成功执行为止。

并发集合框架中的阻塞集合主要是 BlockingQueue 接口的一系列实现类，也就是阻塞队列：
- `LinkedBlockingQueue`：无限阻塞队列
- `LinkedBlockingDeque`：无限双端阻塞队列
- `ArrayBlockingQueue`: 有限阻塞队列
- `PriorityBlockingQueue`: 无限优先级阻塞队列
- `DelayQueue`：延迟队列
- `LinkedTransferQueue`：允许生产者线程等待，直到消费者线程准备就绪可以接收一个元素

阻塞队列与普通队列 (LinkedList 或 ArrayList 等) 的最大不同点，在于阻塞队列支持阻塞添加和阻塞删除方法：
- 阻塞添加：当阻塞队列元素已满时，队列会阻塞加入元素的线程，直队列元素不满时才重新唤醒线程执行元素加入操作。
- 阻塞删除：当阻塞队列元素为空时，删除队列元素的线程将被阻塞，直到队列不为空再执行删除操作（一般都会返回被删除的元素）。

通过阻塞队列实现生产者消费者模式，生产者和消费者彼此之间不直接通信，而通过阻塞队列来进行通信，所以生产者生产完数据之后不用等待消费者处理，直接扔给阻塞队列，消费者不找生产者要数据，而是直接从阻塞队列中获取，阻塞队列就相当于一个缓冲区，平衡了生产者和消费者的处理能力。

通过不加锁的方式实现的队列都是无界的（无法保证队列的长度在确定的范围内），而加锁的方式，可以实现有界队列。在稳定性要求特别高的系统中，为了防止生产者速度过快而导致内存溢出，只能选择有界队列。同时，为了减少 Java 的垃圾回收对系统性能的影响，会尽量选择 array/heap 格式的数据结构而不是 linkedlist。这样筛选下来，符合条件的队列就只有 ArrayBlockingQueue。

## 1. BlockingQueue 接口

### 1.1. 常用 API

- 插入方法
  - `add(E e)`: 添加成功返回 true，失败抛 IllegalStateException 异常。
  - `offer(E e)`: 成功返回 true，如果此队列已满，则返回 false。
  - `put(E e)`: 将元素插入此队列的尾部，如果该队列已满，则一直阻塞。
- 删除方法
  - `remove(Object o)`: 移除指定元素，成功返回 true，失败返回 false。
  - `poll()`: 获取并移除此队列的头元素，若队列为空，则返回 null。
  - `take()`：获取并移除此队列头元素，若没有元素则一直阻塞。
- 检查方法
  - `element()`：获取但不移除此队列的头元素，没有元素则抛异常。
  - `peek()`: 获取但不移除此队列的头；若队列为空，则返回 null。

### 1.2. 源码分析

```java
public interface BlockingQueue<E> extends Queue<E> {

    // 将指定的元素插入到此队列的尾部（如果立即可行且不会超过该队列的容量）
    // 在成功时返回 true，如果此队列已满，则抛 IllegalStateException。 
    boolean add(E e); 

    // 将指定的元素插入到此队列的尾部（如果立即可行且不会超过该队列的容量） 
    // 将指定的元素插入此队列的尾部，如果该队列已满， 
    // 则在到达指定的等待时间之前等待可用的空间，该方法可中断 
    boolean offer(E e, long timeout, TimeUnit unit) throws InterruptedException; 

    // 将指定的元素插入此队列的尾部，如果该队列已满，则一直等到（阻塞）。 
    void put(E e) throws InterruptedException; 

    // 获取并移除此队列的头部，如果没有元素则等待（阻塞）， 
    // 直到有元素将唤醒等待线程执行该操作 
    E take() throws InterruptedException; 

    // 获取并移除此队列的头部，在指定的等待时间前一直等到获取元素，超过时间方法将结束
    E poll(long timeout, TimeUnit unit) throws InterruptedException; 

    // 从此队列中移除指定元素的单个实例（如果存在）。 
    boolean remove(Object o); 

    // 除了上述方法还有继承自 Queue 接口的方法 
    // 获取但不移除此队列的头元素，没有则跑异常 NoSuchElementException 
    E element(); 

    // 获取但不移除此队列的头；如果此队列为空，则返回 null。 
    E peek(); 

    // 获取并移除此队列的头，如果此队列为空，则返回 null。 
    E poll();
}
```

### 1.3. 存在问题

在 BlockingQueue 实现类（ArrayBlockingQueue/LinkedBlockingQueue）实际使用过程中，会因为加锁和伪共享等出现严重的性能问题：

- 加锁

  现实编程过程中，加锁通常会严重地影响性能。线程会因为竞争不到锁而被挂起，等锁被释放的时候，线程又会被恢复，这个过程中存在着很大的开销，并且通常会有较长时间的中断，因为当一个线程正在等待锁时，它不能做任何其他事情。如果一个线程在持有锁的情况下被延迟执行，例如发生了缺页错误、调度延迟或者其它类似情况，那么所有需要这个锁的线程都无法执行下去。如果被阻塞线程的优先级较高，而持有锁的线程优先级较低，就会发生优先级反转。

- 伪共享

	ArrayBlockingQueue 中包含了三个成员变量：
	- takeIndex：需要被取走的元素下标
	- putIndex：可被元素插入的位置的下标
	- count：队列中元素的数量
	这三个变量很容易放到一个缓存行中，但是之间修改没有太多的关联。所以每次修改，都会使之前缓存的数据失效，从而不能完全达到共享的效果。

若在使用 BlockingQueue 时这些性能问题非常突出，可使用并发框架 [Disraptor](https://tech.meituan.com/disruptor.html)，其通过精巧的无锁设计实现了在高并发情形下的高性能。

## 2. ArrayBlockingQueue 实现类

[ArrayBlockingQueue](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/ArrayBlockingQueue.html) 是 BlockingQueue 接口的实现类，是一个用数组实现的有界阻塞队列，其内部按先进先出的原则对元素进行排序，队列的头部是在队列中存在时间最长的元素，尾部是在队列中存在时间最短的元素。其中 put 方法和 take 方法为添加和删除的阻塞方法。

ArrayBlockingQueue 是一个典型的“有界缓冲区”，固定大小的数组在其中保持生产者插入的元素和使用者提取的元素。一旦创建了这样的缓存区，就不能再增加其容量。试图向已满队列中放入元素会导致操作受阻塞；试图从空队列中提取元素将导致类似阻塞。

### 2.1. 常用 API

- `void put(E e)`: 阻塞添加元素。

- `E take()`: 阻塞删除元素。

- `void clear() `: 自动移除此队列中的所有元素。
  
- `boolean contains(Object o) `: 如果此队列包含指定的元素，则返回 true。   

- `int drainTo(Collection<? super E> c) `: 移除此队列中所有可用的元素，并将它们添加到给定 collection 中。    
  
- `int drainTo(Collection<? super E> c, int maxElements) `: 最多从此队列中移除给定数量的可用元素，并将这些元素添加到给定 collection 中。   

- `Iterator<E> iterator() `: 返回在此队列中的元素上按适当顺序进行迭代的迭代器。         

- `int remainingCapacity() `: 返回队列还能添加元素的数量。

- `int size() `: 返回此队列中元素的数量。  

- `Object[] toArray() `: 返回一个按适当顺序包含此队列中所有元素的数组。
  
- `<T> T[] toArray(T[] a)`: 返回一个按适当顺序包含此队列中所有元素的数组；返回数组的运行时类型是指定数组的运行时类型。 

例：通过 ArrayBlockingQueue 队列实现一个生产者消费者模型。
```java
public class ArrayBlockingQueueDemo {
    private final static ArrayBlockingQueue<Apple> queue= new ArrayBlockingQueue<>(1);
    public static void main(String[] args){
        new Thread(new Producer(queue)).start();
        new Thread(new Consumer(queue)).start();
    }
}

 class Apple {
    public Apple(){
    }
 }

/**
 * 生产者线程
 */
class Producer implements Runnable{
    private final ArrayBlockingQueue<Apple> mAbq;
    Producer(ArrayBlockingQueue<Apple> arrayBlockingQueue){
        this.mAbq = arrayBlockingQueue;
    }

    @Override
    public void run() {
        while (true) {
            Produce();
        }
    }

    private void Produce(){
        try {
            Apple apple = new Apple();
            mAbq.put(apple);
            System.out.println("生产："+apple);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

/**
 * 消费者线程
 */
class Consumer implements Runnable{

    private ArrayBlockingQueue<Apple> mAbq;
    Consumer(ArrayBlockingQueue<Apple> arrayBlockingQueue){
        this.mAbq = arrayBlockingQueue;
    }

    @Override
    public void run() {
        while (true){
            try {
                TimeUnit.MILLISECONDS.sleep(1000);
                comsume();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    private void comsume() throws InterruptedException {
        Apple apple = mAbq.take();
        System.out.println("消费 Apple="+apple);
    }
}
```

Consumer 消费者和 Producer 生产者，通过 ArrayBlockingQueue 队列获取和添加元素，其中消费者调用了 take() 方法获取元素当队列没有元素就阻塞，生产者调用 put() 方法添加元素，当队列满时就阻塞，通过这种方式便实现了生产者消费者模式。

**使用 ArrayBlockingQueue 实现生产者消费者比直接使用等待唤醒机制或者 Condition 条件变量来得更加简单**。

**ArrayBlockingQueue 内部的阻塞队列是通过重入锁 ReenterLock 和 Condition 条件队列实现的**，所以支持对等待的生产者线程和使用者线程进行排序的可选公平策略。默认情况下，不保证是这种排序。然而，通过将公平性 (fairness) 设置为 true 而构造的队列允许按照 FIFO 顺序访问线程。公平性通常会降低吞吐量，但也减少了可变性和避免了“不平衡性”。

### 2.2. 实现原理

**ArrayBlockingQueue 的内部通过一个可重入锁 ReentrantLock 和两个 Condition 条件对象来实现阻塞**。
[TODO:](https://blog.csdn.net/javazejian/article/details/77410889#arrayblockingqueue%E7%9A%84%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86%E5%89%96%E6%9E%90)

## 3. LinkedBlockingQueue 实现类

[LinkedBlockingQueue](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/LinkedBlockingQueue.html) 一个由链表实现的有界队列阻塞队列。与 ArrayBlockingQueue 一样，队列按 FIFO（先进先出）排序元素。队列的头部是在队列中时间最长的元素，队列的尾部是在队列中时间最短的元素，新元素插入到队列的尾部，并且队列获取操作会获得位于队列头部的元素。

由于 LinkedBlockingQueue 内部实现添加和删除操作使用的两个 ReenterLock 来控制并发执行，而 ArrayBlockingQueue 内部只是使用一个 ReenterLock 控制并发，因此在正常情况下，链接队列的吞吐量要高于基于数组的队列（ArrayBlockingQueue）。但是在大多数并发应用程序中，其可预知的性能要低。

如果构造一个 LinkedBlockingQueue 对象，而没有指定其容量大小，LinkedBlockingQueue 会默认创建一个类似无限大小的容量（Integer.MAX_VALUE），这样的话，如果生产者的速度一旦大于消费者的速度，也许还没有等到队列满阻塞产生，系统内存就有可能已被消耗殆尽了。因此建议使用 LinkedBlockingQueue 时手动传值，为其提供我们所需的大小，避免队列过大造成机器负载或者内存爆满等情况。

### 3.1. 常用 API

LinkedBlockingQueue 和 ArrayBlockingQueue 的 API 几乎是一样的，但它们的内部实现原理不太相同。

### 3.2. 实现原理

LinkedBlockingQueue 是一个基于链表的阻塞队列，其内部维持一个基于链表的数据队列，实际上我们对 LinkedBlockingQueue 的 API 操作都是间接操作该数据队列。

**与 ArrayBlockingQueue 不同的是，LinkedBlockingQueue 内部分别使用了 takeLock 和 putLock 对并发进行控制，也就是说，添加和删除操作并不是互斥操作，可以同时进行，这样也就可以大大提高吞吐量**。

**除了对添加和移除方法使用单独的锁控制外，LinkedBlockingQueue 和 ArrayBlockingQueue 一样，都使用了不同的 Condition 条件对象作为等待队列，用于挂起 take 线程和 put 线程**。

[TODO:](https://blog.csdn.net/javazejian/article/details/77410889#linkedblockingqueue%E7%9A%84%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86%E5%89%96%E6%9E%90)

## 4. PriorityBlockingQueue 实现类

## 5. DelayQueue 实现类

## 6. LinkedTransferQueue 实现类

## 7. Refer Links

[深入剖析 java 并发之阻塞队列 LinkedBlockingQueue 与 ArrayBlockingQueue](https://blog.csdn.net/javazejian/article/details/77410889)