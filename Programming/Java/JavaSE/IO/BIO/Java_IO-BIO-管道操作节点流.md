- [Java IO BIO 管道操作节点流](#java-io-bio-%E7%AE%A1%E9%81%93%E6%93%8D%E4%BD%9C%E8%8A%82%E7%82%B9%E6%B5%81)
  - [1. PipedInputStream](#1-pipedinputstream)
    - [1.1. 基本概念](#11-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
    - [1.2. 类定义](#12-%E7%B1%BB%E5%AE%9A%E4%B9%89)
    - [1.3. 常用 API](#13-%E5%B8%B8%E7%94%A8-api)
  - [2. PipedOutStream](#2-pipedoutstream)
    - [2.1. 基本概念](#21-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
    - [2.2. 类定义](#22-%E7%B1%BB%E5%AE%9A%E4%B9%89)
    - [2.3. 常用 API](#23-%E5%B8%B8%E7%94%A8-api)
    - [2.4. 应用](#24-%E5%BA%94%E7%94%A8)
  - [3. PipedReader](#3-pipedreader)
    - [3.1. 基本概念](#31-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
    - [3.2. 类定义](#32-%E7%B1%BB%E5%AE%9A%E4%B9%89)
    - [3.3. 常用 API](#33-%E5%B8%B8%E7%94%A8-api)
  - [4. PipedWriter](#4-pipedwriter)
    - [4.1. 基本概念](#41-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
    - [4.2. 类定义](#42-%E7%B1%BB%E5%AE%9A%E4%B9%89)
    - [4.3. 常用 API](#43-%E5%B8%B8%E7%94%A8-api)
  - [5. Refer Links](#5-refer-links)

# Java IO BIO 管道操作节点流

<!-- TODO: 待补充完善 -->

## 1. PipedInputStream

### 1.1. 基本概念

### 1.2. 类定义

### 1.3. 常用 API

## 2. PipedOutStream

### 2.1. 基本概念

### 2.2. 类定义

### 2.3. 常用 API

### 2.4. 应用

例：使用管道流在多个线程间通信
```java
/**
 * 发送消息的线程。
 */
public class Sender extends Thread {

    private static final Logger log = LoggerFactory.getLogger(Sender.class);

    /** 管道输出流对象，它和管道输入流 (PipedInputStream) 对象绑定。从而可以将数据发送给“管道输入流”. */
    private PipedOutputStream pipedOut;

    public Sender(PipedOutputStream pipedOut) {
        this.pipedOut = pipedOut;
    }

    public PipedOutputStream getPipedOut() {
        return pipedOut;
    }

    @Override
    public void run() {
        String strInfo = "Hello World!" ;
        try {
            pipedOut.write(strInfo.getBytes());
            pipedOut.close();
        } catch (IOException e) {
            log.error("向管道中写入数据出错！", e);
        }
    }

}
/**
 * 接收消息的线程。
 */
public class Receiver extends Thread {

    private static final Logger log = LoggerFactory.getLogger(Receiver.class);

    /** 管道输入流对象，它和管道输出流 (PipedOutputStream) 对象绑定。从而可以接收“管道输出流”的数据。*/
    private PipedInputStream pipedIn;

    public Receiver(PipedInputStream pipedIn) {
        this.pipedIn = pipedIn;
    }

    public PipedInputStream getPipedIn() {
        return pipedIn;
    }

    @Override
    public void run() {
        byte[] buf = new byte[2048];
        try {
            int len = pipedIn.read(buf);
            log.info("{}", new String(buf, 0, len));
            pipedIn.close();
        } catch (IOException e) {
            log.error("从管道中读取数据出错！", e);
        }
    }

}
/**
 * PipedInputStream 和 PipedOutputStream 的测试类。
 */
public class PipedStreamTest {

    private static final Logger log = LoggerFactory.getLogger(PipedStreamTest.class);

    public static void main(String[] args) {
        Sender sender = new Sender(new PipedOutputStream());
        Receiver receiver = new Receiver(new PipedInputStream());

        try {
            // 将管道输入流和管道的输出流进行连接。
            receiver.getPipedIn().connect(sender.getPipedOut());

            // 启动线程
            sender.start();
            receiver.start();
        } catch (IOException e) {
            log.info("发送接收消息出错！", e);
        }
    }

}
```

## 3. PipedReader

### 3.1. 基本概念

### 3.2. 类定义

### 3.3. 常用 API

## 4. PipedWriter

### 4.1. 基本概念

### 4.2. 类定义

### 4.3. 常用 API

## 5. Refer Links