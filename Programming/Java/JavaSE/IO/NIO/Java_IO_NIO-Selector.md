- [Java IO NIO: Selector](#java-io-nio-selector)
  - [1. SelectableChannel](#1-selectablechannel)
    - [1.1. 通道注册](#11- 通道注册)
    - [1.2. 其它方法](#12- 其它方法)
  - [2. SelectionKey](#2-selectionkey)
    - [2.1. IO 事件](#21-io- 事件)
    - [2.2. 事件集合](#22- 事件集合)
    - [2.3. 暂存对象](#23- 暂存对象)
  - [3. Selector](#3-selector)
    - [3.1. 初始化](#31- 初始化)
      - [3.1.1. poll 实现](#311-poll- 实现)
      - [3.1.2. epoll 实现](#312-epoll- 实现)
      - [3.1.3. devpoll 实现](#313-devpoll- 实现)
    - [3.2. SelectionKey 集合](#32-selectionkey- 集合)
    - [3.3. select 方法](#33-select- 方法)
      - [3.3.1. poll 实现](#331-poll- 实现)
      - [3.3.2. epoll 实现](#332-epoll- 实现)
      - [3.3.3. devpoll 实现](#333-devpoll- 实现)
  - [4. 实例](#4- 实例)
  - [5. Refer Links](#5-refer-links)

# Java IO NIO: Selector

为实现 IO 多路复用模型，Java NIO 为非阻塞式 IO 提供了一系列特殊功能类：Selector、SelectionKey、SelectableChannel 等。其中，Selector 是 Java NIO 中 IO 多路复用器的实现，可以同时监控多个非阻塞套接字通道。

## 1. SelectableChannel

> A channel that can be multiplexed via a Selector.

[SelectableChannel](https://docs.oracle.com/javase/9/docs/api/java/nio/channels/SelectableChannel.html) 代表一个可注册到 IO 多路复用选择器上的 Channel 对象。
```java
public abstract class SelectableChannel
    extends AbstractInterruptibleChannel
    implements Channel
```
SelectableChannel 是一个抽象类，所有需要复用的 Channel 都必须扩展此类。

### 1.1. 通道注册

> In order to be used with a selector, an instance of this class must first be registered via the register method. This method returns a new SelectionKey object that represents the channel's registration with the selector.

通道注册即将 Channel 感兴趣的 IO 事件告知 Selector，待事件发生时，Selector 即可返回就绪事件，我们就可以去做后续的事情了。如 ServerSocketChannel 通道通常对 OP_ACCEPT 事件感兴趣，那么我们就可以把这个事件注册给 Selector，待事件发生，即服务端接受客户端连接后，我们即可获取这个就绪的事件并做相应的操作。

SelectableChannel 为通道注册提供了以下 API：
- `SelectionKey	register​(Selector sel, int ops)`: Registers this channel with the given selector, returning a selection key.
- `abstract SelectionKey	register​(Selector sel, int ops, Object att)`: Registers this channel with the given selector, returning a selection key.
- `boolean	isRegistered​()`: Tells whether or not this channel is currently registered with any selectors.

```java
public abstract class SelectableChannel extends AbstractInterruptibleChannel implements Channel {
    public final SelectionKey register(Selector sel, int ops) throws ClosedChannelException {
        return register(sel, ops, null);
    }
    
    public abstract SelectionKey register(Selector sel, int ops, Object att) throws ClosedChannelException;
}
```
```java
public abstract class AbstractSelectableChannel extends SelectableChannel {
    private SelectionKey[] keys = null;
    
    public final SelectionKey register(Selector sel, int ops, Object att) throws ClosedChannelException {
        synchronized (regLock) {
            // ...
            // 从 keys 数组中查找，查找条件为 k.selector() == sel
            SelectionKey k = findKey(sel);
            if (k != null) { // 如果 k 不为空，则修改 k 所感兴趣的事件
                k.interestOps(ops);
                k.attach(att);
            }
            if (k == null) { // k 为空，则创建一个 SelectionKey，并存储到 keys 数组中
                // New registration
                synchronized (keyLock) {
                    if (!isOpen())
                        throw new ClosedChannelException();
                    k = ((AbstractSelector)sel).register(this, ops, att);
                    addKey(k);
                }
            }
            return k;
        }
    }
}
```
```java
public abstract class AbstractSelector extends Selector {
    //...
    protected abstract SelectionKey register(AbstractSelectableChannel ch,
                                         int ops, Object att);
}
```
```java
public abstract class SelectorImpl extends AbstractSelector {
    //...
    protected final SelectionKey register(AbstractSelectableChannel ch, int ops, Object attachment) {
        if (!(ch instanceof SelChImpl))
            throw new IllegalSelectorException();
        // 创建 SelectionKeyImpl 实例
        SelectionKeyImpl k = new SelectionKeyImpl((SelChImpl)ch, this);
        k.attach(attachment);
        synchronized (publicKeys) {
            implRegister(k);
        }
        k.interestOps(ops);
        return k;
    }
    protected abstract void implRegister(SelectionKeyImpl var1);
}
```
implRegister() 方法的实现根据系统不同而不同
- 在 Windows 平台下 implRegister() 方法实现在 WindowsSelectorImpl 中：
  ```java
  final class WindowsSelectorImpl extends SelectorImpl {
      // ...
      protected void implRegister(SelectionKeyImpl var1) {
          Object var2 = this.closeLock;
          synchronized(this.closeLock) {
              if (this.pollWrapper == null) {
                  throw new ClosedSelectorException();
              } else {
                  this.growIfNeeded();
                  this.channelArray[this.totalChannels] = var1;
                  var1.setIndex(this.totalChannels);
                  this.fdMap.put(var1);
                  this.keys.add(var1);
                  this.pollWrapper.addEntry(this.totalChannels, var1);
                  ++this.totalChannels;
              }
          }
      }
  }
  ```
- 在 Linux 平台下 implRegister() 方法实现在 EPollSelectorImpl 中：
  ```java
  final class EPollSelectorImpl extends SelectorImpl {
      // ...
      protected void implRegister(SelectionKeyImpl ski) {
          if (closed)
              throw new ClosedSelectorException();
          SelChImpl ch = ski.channel;
          int fd = Integer.valueOf(ch.getFDVal());
          // 存储 fd 和 SelectionKeyImpl 的映射关系
          fdToKey.put(fd, ski);
          
          pollWrapper.add(fd);
          // 将 SelectionKeyImpl 实例存储到 keys 中（这里的 keys 声明在 SelectorImpl 类中），keys 集合可由 selector.keys() 方法获取
          keys.add(ski);
      }
  }
  ```
  通过跟踪 EPollSelectorImpl 的调用栈可知，在通道注册的过程中并未在调用栈中调用 epoll_ctl 函数，而是先将事件集合 events 存放在 eventsLow 中。

### 1.2. 其它方法

- `SelectableChannel	configureBlocking​(boolean block)`: Adjusts this channel's blocking mode.
- `boolean	isBlocking​()`: Tells whether or not every I/O operation on this channel will block until it completes.
- `SelectionKey	keyFor​(Selector sel)`: Retrieves the key representing the channel's registration with the given selector.
- `int	validOps​()`: Returns an operation set identifying this channel's supported operations.

## 2. SelectionKey

> A token representing the registration of a SelectableChannel with a Selector.
> 
> A selection key is created each time a channel is registered with a selector. A key remains valid until it is cancelled by invoking its cancel method, by closing its channel, or by closing its selector. Cancelling a key does not immediately remove it from its selector; it is instead added to the selector's cancelled-key set for removal during the next selection operation. The validity of a key may be tested by invoking its isValid method. 

[SelectionKey](https://docs.oracle.com/javase/9/docs/api/java/nio/channels/SelectionKey.html) 代表了每一个 SelectableChannel 对象在 Selector 中的注册关系。

### 2.1. IO 事件

SelectionKey 中用静态变量定义了 4 种 IO 事件
```java
// Operation-set bit for read operations.
public static final int OP_READ = 1 << 0;
// Operation-set bit for write operations.
public static final int OP_WRITE = 1 << 2;
// Operation-set bit for socket-connect operations.
public static final int OP_CONNECT = 1 << 3;
// Operation-set bit for socket-accept operations.
public static final int OP_ACCEPT = 1 << 4;
```
可见，这 4 种操作以 int 整数存在，值分别为 1、4、8、16，且存在以下规律：
- 任意 2 个、3 个、4 个进行按位或的结果和相加的结果相等。
- 任意 2 个、3 个、4 个进行相加的结果互不相等。
因此，系统可以根据 validOps() 方法返回的相加值来确定该 SelectableChannel 所支持的操作类型，如返回 5 就代表了支持读（1）和写（4）操作。

### 2.2. 事件集合

- interest 事件集合
  
  感兴趣的事件集合，Channel 调用 register 方法注册到 Selector 中时会为绑定的 SelectionKey 设置此值，interestOps 可通过 interestOps(int ops) 方法设置，通过 interestOps​() 方法获取：
  - `int	interestOps​()`: Retrieves this key's interest set.
  - `SelectionKey	interestOps​(int ops)`: Sets this key's interest set to the given value.

- ready 事件集合

  就绪事件集合，可通过 readyOps() 获取：
  - `int	readyOps​()`: Retrieves this key's ready-operation set.

- 相关方法
  - `boolean	isAcceptable​()`: Tests whether this key's channel is ready to accept a new socket connection.
  - `boolean	isConnectable​()`: Tests whether this key's channel has either finished, or failed to finish, its socket-connection operation.
  - `boolean	isReadable​()`: Tests whether this key's channel is ready for reading.
  - `boolean	isWritable​()`: Tests whether this key's channel is ready for writing.

### 2.3. 暂存对象

SelectionKey 中提供了暂存对象的支持，通过这个方法，可以将对象暂存在 SelectionKey 中，待需要的时候直接取出来即可：
- `Object	attach​(Object ob)`: Attaches the given object to this key.
- `Object	attachment​()`: Retrieves the current attachment.

例如，在简单的 HTTP 服务器中，读取用户请求数据后（即 selectionKey.isReadable() 为 true），会去解析请求头，然后可以将请求头信息通过 attach() 方法放入 selectionKey 中。待通道可写后，再从 selectionKey 中取出请求头，并根据请求头回复客户端不同的消息。

## 3. Selector

[Selector](https://docs.oracle.com/javase/9/docs/api/java/nio/channels/Selector.html) 是 SelectableChannel 对象的多路复用器，所有非阻塞通信的 Channel 都应注册到 Selector 对象中，从而通过 Selector 同时监控多个 Channel 的 IO 状况。

```java
public abstract class Selector implements Closeable {}
```

Java NIO 中引入 Channel，表面上看它只是对传统 IO 资源（如 socket）的一层封装，看不出来什么意义。而实际上，Channel 是与 Selector 适配用的，Selector 直接管理传统 IO 资源很难实现，因此通过 Channel 做一次封装与之适配。可以说 Selector 是 Java NIO 的基石，是根本之所在。

### 3.1. 初始化

Selector 是一个抽象类，在不同的机器 / 系统平台上有不同的实现，所以不能直接创建。Selector 提供了一个 open() 方法，通过 open() 方法即可以创建选择器实例：
```java
public abstract class Selector implements Closeable {
    // ...
    public static Selector open() throws IOException {
        return SelectorProvider.provider().openSelector();
    }
    // ... 
}
```
```java
public abstract class SelectorProvider {
    // ...
    public static SelectorProvider provider() {
        synchronized (lock) {
            if (provider != null)
                return provider;
            return AccessController.doPrivileged(
                new PrivilegedAction<SelectorProvider>() {
                    public SelectorProvider run() {
                            if (loadProviderFromProperty())
                                return provider;
                            if (loadProviderAsService())
                                return provider;
                            provider = sun.nio.ch.DefaultSelectorProvider.create();
                            return provider;
                        }
                    });
        }
    }
    // ...
}
```
sun/nio/ch/DefaultSelectorProvider.java
```java
public class DefaultSelectorProvider {
    // ...    
    public static SelectorProvider create() {
        // 根据系统名称创建相应的 SelectorProvider
        String osname = AccessController
            .doPrivileged(new GetPropertyAction("os.name"));
        // 如果是 SunOS 内核（Solaris 系统），就会使用 DevPollSelectorProvider
        if (osname.equals("SunOS"))
            return createProvider("sun.nio.ch.DevPollSelectorProvider");
        // 如果是高版本的 Linux 内核，就会使用 EPollSelectorProvider
        if (osname.equals("Linux"))
            return createProvider("sun.nio.ch.EPollSelectorProvider");
        // 默认使用 PollSelectorProvider
        return new sun.nio.ch.PollSelectorProvider();
    }
    // ...    
}
```
从 DefaultSelectorProvider 的 create() 方法中可以看到，Java Selector 并非凭空创造，而是在底层操作系统提供的系统调用 devpoll/poll/epoll 上封装而来。Java Selector 会根据系统平台的不同而自动选择不同的底层 IO 多路复用的系统调用实现，包括了 devpoll、epoll 和 poll 共 3 种类型。

#### 3.1.1. poll 实现

```java
public class PollSelectorProvider extends SelectorProviderImpl {
    public AbstractSelector openSelector() throws IOException {
        return new PollSelectorImpl(this);
    }
}
```
sun/nio/ch/PollSelectorImpl.java
```java
class PollSelectorImpl extends SelectorImpl {
    PollSelectorImpl(SelectorProvider sp) throws IOException {
        // 调用父类构造方法
        super(sp);
        // ...
        // 其他代码略，这里最重要的是初始化 PollArrayWrapper 
        pollWrapper = new PollArrayWrapper();
        // ...
    }
}
```
其中
- 父类 SelectorImpl 构造方法中主要包含了对 publicKeys 和 publicSelectedKeys 的初始化，publicKeys 是 selector.keys() 方法所返回的集合，publicSelectedKeys 则是 selector.selectedKeys() 方法返回的集合。
- PollArrayWrapper 是一个重要的封装实现，属于 Java 层的最底层封装，起着承上启下的作用（上层是 Java 代码，下层是 C 代码）。

#### 3.1.2. epoll 实现

```java
public class EPollSelectorProvider extends SelectorProviderImpl {
    public AbstractSelector openSelector() throws IOException {
        return new EPollSelectorImpl(this);
    }
}
```
sun/nio/ch/EPollSelectorImpl.java
```java
class EPollSelectorImpl extends SelectorImpl {
    EPollSelectorImpl(SelectorProvider sp) throws IOException {
        // 调用父类构造方法
        super(sp);
        // 其他代码略，这里最重要的是初始化 EPollArrayWrapper 
        pollWrapper = new EPollArrayWrapper();
        // ...
    }
}
```
```java
class EPollArrayWrapper {
    EPollArrayWrapper() throws IOException {
        // 调用 epollCreate 方法创建 epoll 文件描述符
        epfd = epollCreate();
    
        // the epoll_event array passed to epoll_wait
        // 初始化 pollArray，该对象用于存储就绪文件描述符和事件
        int allocationSize = NUM_EPOLLEVENTS * SIZE_EPOLLEVENT;
        pollArray = new AllocatedNativeObject(allocationSize, true);
        pollArrayAddress = pollArray.address();
    
        // eventHigh needed when using file descriptors > 64k
        if (OPEN_MAX > MAX_UPDATE_ARRAY_SIZE)
            eventsHigh = new HashMap<>();
    }
    
    // epollCreate 方法是 native 类型的
    private native int epollCreate();
}
```
JNI 中的 epollCreate() 方法实现：
```c
JNIEXPORT jint JNICALL
Java_sun_nio_ch_EPollArrayWrapper_epollCreate(JNIEnv *env, jobject this)
{
    // 调用 epoll_create 函数创建 epoll 实例，并返回文件描述符 epfd
    int epfd = epoll_create(256);
    if (epfd < 0) {
       JNU_ThrowIOExceptionWithLastError(env, "epoll_create failed");
    }
    return epfd;
}
```

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/8/3d9cc34e2ef924ec4ba7a0928d142520.jpg)

#### 3.1.3. devpoll 实现

todo:

### 3.2. SelectionKey 集合

一个 Selector 实例中包含了 3 种 SelectionKey 集合：
- 所有 SelectionKey 对象的集合：代表了注册在该 Selector 上的 Channel，这个集合可通过 keys() 方法获取：
  - `Set<SelectionKey>	keys​()`: Returns this selector's key set.

- 被选择的 SelectionKey 对象的集合：代表了所有可通过 select() 方法获取的、需要进行 IO 处理的 Channel，这个集合可通过 selectedKeys() 方法获取：
  - `Set<SelectionKey>	selectedKeys​()`: Returns this selector's selected-key set.

- 被注销的 SelectionKey 对象的集合：代表了所被取消注册关系的 Channel，在下一次执行 select() 方法时，这些 Channel 对应的 SelectionKey 会被彻底删除，程序通常无须直接访问该集合。

### 3.3. select 方法

select() 方法用于监控所有注册的 Channel，当它们中间有需要处理的 IO 事件时将该 Channel 对应的 SelectionKey 加入 selectedKeys​集合中，并返回需要处理 IO 事件的 Channel 数量（即 selectedKeys​集合的大小）。

Selector 包含 3 种不同功能的 select() 方法：
- `abstract int	select​()`: Selects a set of keys whose corresponding channels are ready for I/O operations. 该方法会一直阻塞直到至少一个通道处于就绪状态时才返回。
- `abstract int	select​(long timeout)`: Selects a set of keys whose corresponding channels are ready for I/O operations. 该方法会一直阻塞直到至少一个通道处于就绪状态或阻塞超时后才返回，如果 timeout = 0，会一直阻塞线程。
- `abstract int	selectNow​()`: Selects a set of keys whose corresponding channels are ready for I/O operations. 该方法不会阻塞线程，调用后立即返回。

以上 3 个方法均返回 int 类型值，表示每次调用 select 或 selectNow 方法后，新就绪通道的数量。
- NOTE: 如果某个通道在上一次调用 select 方法时就已经处于就绪状态，但并未将该通道对应的 SelectionKey 对象从 selectedKeys 集合中移除。假设另一个的通道在本次调用 select 期间处于就绪状态，此时，select 返回 1，而不是 2。

Selector 还提供了一个 wakeup​() 方法，可使一个还未返回的 select() 方法立即返回：
- `Selector	wakeup​()`: Causes the first selection operation that has not yet returned to return immediately.

select() 方法的实现过程，实际上也就是针对系统平台进行 poll/epoll/devpoll 系统调用的封装调用过程，其基本逻辑如下：
1. 检查已取消键集合 cancelledKeys 是否为空，不为空则将 cancelledKeys 的键从 keys 和 selectedKeys 中移除，并将键和通道注销。
1. 调用操作系统的 IO 多路复用函数。
1. 再次执行步骤 1。
1. 更新 selectedKeys 集合，并返回就绪通道数量。

#### 3.3.1. poll 实现

sun/nio/ch/PollSelectorImpl.java
```java
protected int doSelect(long timeout)
    throws IOException
{   
    if (channelArray == null)
        throw new ClosedSelectorException();
    processDeregisterQueue();
    try {
        begin();
        pollWrapper.poll(totalChannels, 0, timeout);
    } finally {
        end();
    }   
    processDeregisterQueue();
    int numKeysUpdated = updateSelectedKeys();
    if (pollWrapper.getReventOps(0) != 0) {
        // Clear the wakeup pipe
        pollWrapper.putReventOps(0, 0); 
        synchronized (interruptLock) {
            IOUtil.drain(fd0);
            interruptTriggered = false;
        }   
    }   
    return numKeysUpdated;
}
```
sun/nio/ch/pollWrapper.java
```java
int poll(int numfds, int offset, long timeout) {
    return poll0(pollArrayAddress + (offset * SIZE_POLLFD),
                  numfds, timeout);
}   

private native int poll0(long pollAddress, int numfds, long timeout);
```
JNI 中的 poll0 方法实现：
```c
JNIEXPORT jint JNICALL
Java_sun_nio_ch_PollArrayWrapper_poll0(JNIEnv *env, jobject this,
                                       jlong address, jint numfds,
                                       jlong timeout)
{
    struct pollfd *a;
    int err = 0;

    a = (struct pollfd *) jlong_to_ptr(address);

    if (timeout <= 0) {           /* Indefinite or no wait */
        RESTARTABLE (poll(a, numfds, timeout), err);
    } else {                     /* Bounded wait; bounded restarts */
        err = ipoll(a, numfds, timeout);
    }

    if (err < 0) {
        JNU_ThrowIOExceptionWithLastError(env, "Poll failed");
    }
    return (jint)err;
}
```
可见，在默认实现中，select 调用栈的底层是调用了系统调用 poll 来实现的。

#### 3.3.2. epoll 实现

sun/nio/ch/EPollSelectorImpl.java
```java
protected int doSelect(long timeout)
    throws IOException
{   
    if (closed)
        throw new ClosedSelectorException();
    // 处理已取消键集合，对应步骤 1
    processDeregisterQueue();
    try {
        begin();
        // select 方法的核心，对应步骤 2
        pollWrapper.poll(timeout);
    } finally {
        end();
    }
    // 处理已取消键集合，对应步骤 3
    processDeregisterQueue();
    
    // 更新 selectedKeys 集合，并返回就绪通道数量，对应步骤 4
    int numKeysUpdated = updateSelectedKeys();
    if (pollWrapper.interrupted()) {
        // Clear the wakeup pipe
        pollWrapper.putEventOps(pollWrapper.interruptedIndex(), 0);
        synchronized (interruptLock) {
            pollWrapper.clearInterrupted();
            IOUtil.drain(fd0);
            interruptTriggered = false;
        }
    }
    return numKeysUpdated;
}
// 获取 cancelledKeys 集合，然后遍历集合，并对每个选择键及其对应的通道执行注销操作
void processDeregisterQueue() throws IOException {
    // Precondition: Synchronized on this, keys, and selectedKeys
    Set<SelectionKey> cks = cancelledKeys();
    synchronized (cks) {
        if (!cks.isEmpty()) {
            Iterator<SelectionKey> i = cks.iterator();
            // 遍历 cancelledKeys，执行注销操作
            while (i.hasNext()) {
                SelectionKeyImpl ski = (SelectionKeyImpl)i.next();
                try {
                    // 执行注销逻辑
                    implDereg(ski);
                } catch (SocketException se) {
                    throw new IOException("Error deregistering key", se);
                } finally {
                    i.remove();
                }
            }
        }
    }
}
protected void implDereg(SelectionKeyImpl ski) throws IOException {
    assert (ski.getIndex() >= 0);
    SelChImpl ch = ski.channel;
    int fd = ch.getFDVal();
    // 移除 fd 和选择键键的映射关系
    fdToKey.remove(Integer.valueOf(fd));
    // 从 epoll 实例中删除事件
    pollWrapper.remove(fd);
    ski.setIndex(-1);
    
    // 从 keys 和 selectedKeys 中移除选择键
    keys.remove(ski);
    selectedKeys.remove(ski);
    
    // 注销选择键
    deregister((AbstractSelectionKey)ski);
    
    // 注销通道
    SelectableChannel selch = ski.channel();
    if (!selch.isOpen() && !selch.isRegistered())
        ((SelChImpl)selch).kill();
}
```
sun/nio/ch/EPollArrayWrapper.java
```java
int poll(long timeout) throws IOException {
    // 调用 epoll_ctl 函数注册事件
    updateRegistrations();
    // 调用 epoll_wait 函数等待事件发生
    updated = epollWait(pollArrayAddress, NUM_EPOLLEVENTS, timeout, epfd);
    for (int i=0; i<updated; i++) {
        if (getDescriptor(i) == incomingInterruptFD) {
            interruptedIndex = i;
            interrupted = true;
            break;
        }
    }
    return updated;
}
// 执行事件注册的实现逻辑
private void updateRegistrations() {
    synchronized (updateLock) {
        int j = 0;
        while (j < updateCount) {
            // 获取 fd 和 events，这两个值在调用 register 方法时被存储到数组中
            int fd = updateDescriptors[j];
            short events = getUpdateEvents(fd);
            boolean isRegistered = registered.get(fd);
            int opcode = 0;

            if (events != KILLED) {
                // 确定 opcode 的值
                if (isRegistered) {
                    opcode = (events != 0) ? EPOLL_CTL_MOD : EPOLL_CTL_DEL;
                } else {
                    opcode = (events != 0) ? EPOLL_CTL_ADD : 0;
                }
                if (opcode != 0) {
                    // 调用 native 方法注册事件
                    epollCtl(epfd, opcode, fd, events);
                    // 设置 fd 的注册状态
                    if (opcode == EPOLL_CTL_ADD) {
                        registered.set(fd);
                    } else if (opcode == EPOLL_CTL_DEL) {
                        registered.clear(fd);
                    }
                }
            }
            j++;
        }
        updateCount = 0;
    }
}
private native void epollCtl(int epfd, int opcode, int fd, int events);
private native int epollWait(long pollAddress, int numfds, long timeout, int epfd) throws IOException;
```
由此可见，注册通道时实际上只是先将事件收集起来，并没有在操作系统的层面注册 IO 事件。而等到调用 select 方法时，才会一起通过 epoll_ctl 函数将事件注册到 epoll 实例中。

JNI 中的 epollCtl 和 epollWait 方法实现：
```c
// jdk/src/solaris/native/sun/nio/ch/EPollArrayWrapper.c
JNIEXPORT void JNICALL Java_sun_nio_ch_EPollArrayWrapper_epollCtl(JNIEnv *env, jobject this, jint epfd, jint opcode, jint fd, jint events) {
    struct epoll_event event;
    int res;

    event.events = events;
    event.data.fd = fd;

    // 调用 epoll_ctl 注册事件
    RESTARTABLE(epoll_ctl(epfd, (int)opcode, (int)fd, &event), res);

    if (res < 0 && errno != EBADF && errno != ENOENT && errno != EPERM) {
        JNU_ThrowIOExceptionWithLastError(env, "epoll_ctl failed");
    }
}

JNIEXPORT jint JNICALL
Java_sun_nio_ch_EPollArrayWrapper_epollWait(JNIEnv *env, jobject this,
                                            jlong address, jint numfds,
                                            jlong timeout, jint epfd)
{
    struct epoll_event *events = jlong_to_ptr(address);
    int res;

    if (timeout <= 0) {           /* Indefinite or no wait */
        // 调用 epoll_wait 等待事件
        RESTARTABLE(epoll_wait(epfd, events, numfds, timeout), res);
    } else {                      /* Bounded wait; bounded restarts */
        res = iepoll(epfd, events, numfds, timeout);
    }

    if (res < 0) {
        JNU_ThrowIOExceptionWithLastError(env, "epoll_wait failed");
    }
    return res;
}
```

#### 3.3.3. devpoll 实现

todo:

## 4. 实例

使用 NIO 选择器编程时，主干代码的结构一般比较固定，基本流程如下：
1. 创建一个 ServerSocketChannel 监听客户端连接并绑定监听端口，设置为非阻塞模式。
1. 创建一个 Selector，并且把这个 server channel 注册到 selector 上，注册时指定这个 channel 所感觉兴趣的事件是 SelectionKey.OP_ACCEPT，这个事件代表的是有客户端发起 TCP 连接请求。
1. 使用 select 方法阻塞住线程，当 select 返回的时候，线程被唤醒。再通过 selectedKeys 方法得到所有可用 channel 的集合。
1. 遍历这个集合，如果其中 channel 上有连接到达，就接受新的连接，然后把这个新的连接也注册到 selector 中去。
1. 如果有 channel 是读，那就把数据读出来，并且把它感兴趣的事件改成写。如果是写，就把数据写出去，并且把感兴趣的事件改成读。

eg:
```java
ServerSocketChannel ssc = ServerSocketChannel.open();
ssc.bind(new InetSocketAddress("localhost", 8080));
ssc.configureBlocking(false);

Selector selector = Selector.open();
ssc.register(selector, SelectionKey.OP_ACCEPT);

while(true) {
    int readyNum = selector.select();
    Set<SelectionKey> selectedKeys = selector.selectedKeys();
    Iterator<SelectionKey> it = selectedKeys.iterator();
    
    while(it.hasNext()) {
        SelectionKey key = it.next();
        it.remove();
        
        if(key.isAcceptable()) {
            // 接受连接并将连接 socket 注册到 selector 中
            SocketChannel socketChannel = ssc.accept();
            socketChannel.configureBlocking(false);
            socketChannel.register(selector, SelectionKey.OP_READ);
            // ...

        } else if (key.isReadable()) {
            // 若通道可读，获取该通道并进行相应操作
            SocketChannel socketChannel = (SocketChannel) key.channel();
            readBuff.clear();
            socketChannel.read(readBuff);

            readBuff.flip();
            System.out.println("received : " + new String(readBuff.array()));
            key.interestOps(SelectionKey.OP_WRITE);
            // ...

        } else if (key.isWritable()) {
            // 若通道可写，获取该通道并进行相应操作
            writeBuff.rewind();
            SocketChannel socketChannel = (SocketChannel) key.channel();
            socketChannel.write(writeBuff);
            key.interestOps(SelectionKey.OP_READ);
            // ...
        }
    }
}
```

## 5. Refer Links

[Java NIO 之选择器](http://www.coolblog.xyz/2018/04/03/Java-NIO%E4%B9%8B%E9%80%89%E6%8B%A9%E5%99%A8/)

[基于 Java NIO 实现简单的 HTTP 服务器](http://www.coolblog.xyz/2018/04/04/%E5%9F%BA%E4%BA%8E-Java-NIO-%E5%AE%9E%E7%8E%B0%E7%AE%80%E5%8D%95%E7%9A%84-HTTP-%E6%9C%8D%E5%8A%A1%E5%99%A8/)

[进击的 Java 新人：Java NIO(5): IO 多路复用](https://zhuanlan.zhihu.com/p/27419141)

[进击的 Java 新人：Java NIO(6): Selector](https://zhuanlan.zhihu.com/p/27434028)

[进击的 Java 新人：Java NIO(7): Epoll 版的 Selector](https://zhuanlan.zhihu.com/p/27441342)
