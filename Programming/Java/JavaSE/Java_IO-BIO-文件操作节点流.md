- [Java IO BIO 文件操作节点流](#java-io-bio-%E6%96%87%E4%BB%B6%E6%93%8D%E4%BD%9C%E8%8A%82%E7%82%B9%E6%B5%81)
	- [1. FileInputStream 类](#1-fileinputstream-%E7%B1%BB)
		- [1.1. 基本概念](#11-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [1.2. 类定义](#12-%E7%B1%BB%E5%AE%9A%E4%B9%89)
		- [1.3. 常用 API](#13-%E5%B8%B8%E7%94%A8-api)
	- [2. FileOutputStream 类](#2-fileoutputstream-%E7%B1%BB)
		- [2.1. 基本概念](#21-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [2.2. 类定义](#22-%E7%B1%BB%E5%AE%9A%E4%B9%89)
		- [2.3. 常用 API](#23-%E5%B8%B8%E7%94%A8-api)
	- [3. FileReader 类](#3-filereader-%E7%B1%BB)
		- [3.1. 基本概念](#31-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [3.2. 类定义](#32-%E7%B1%BB%E5%AE%9A%E4%B9%89)
		- [3.3. 常用 API](#33-%E5%B8%B8%E7%94%A8-api)
	- [4. FileWriter 类](#4-filewriter-%E7%B1%BB)
		- [4.1. 基本概念](#41-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [4.2. 类定义](#42-%E7%B1%BB%E5%AE%9A%E4%B9%89)
		- [4.3. 常用 API](#43-%E5%B8%B8%E7%94%A8-api)
	- [5. 应用](#5-%E5%BA%94%E7%94%A8)
		- [5.1. 用字节流进行文件拷贝](#51-%E7%94%A8%E5%AD%97%E8%8A%82%E6%B5%81%E8%BF%9B%E8%A1%8C%E6%96%87%E4%BB%B6%E6%8B%B7%E8%B4%9D)
		- [5.2. 用字符流进行文件拷贝](#52-%E7%94%A8%E5%AD%97%E7%AC%A6%E6%B5%81%E8%BF%9B%E8%A1%8C%E6%96%87%E4%BB%B6%E6%8B%B7%E8%B4%9D)
	- [6. Refer Links](#6-refer-links)

# Java IO BIO 文件操作节点流

<!-- TODO: 待补充完善 -->

## 1. FileInputStream 类

### 1.1. 基本概念

### 1.2. 类定义

### 1.3. 常用 API

例：用字节流进行文件读取
```java
public static void main(String[] args) {
    try (
        InputStream is = new FileInputStream("E:/study/java/HelloWorld.java");
		) {
        byte[] car = new byte[1024];
        int len = 0; 
        StringBuilder sb = new StringBuilder();
        while (-1 != (len = is.read(car))) {
            String info = new String(car, 0, len);
            sb.append(info);
        }
        System.out.println(sb.toString());
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

## 2. FileOutputStream 类

### 2.1. 基本概念

### 2.2. 类定义

### 2.3. 常用 API

例：用字节流进行文件写出
```java
public static void main(String[] args) {
    try (
        OutputStream os=new FileOutputStream("E:/study/java/test.txt");  
		) {  
        String str="hahahaha";  
        byte[] data = str.getBytes();  
        os.write(data, 0, data.length);  
    } catch (IOException e) {  
        e.printStackTrace();  
    }
}  
```
## 3. FileReader 类

### 3.1. 基本概念

### 3.2. 类定义

### 3.3. 常用 API

例：用字符流进行纯文本读取
```java
public static void main(String[] args) {  
		try (
			Reader = fr = new FileReader("F:/Picture/test/test.txt");
		) {  
				char[] cbuf = new char[1024];  
				int hasRead = 0;  // 保存实际读取的字符数
				while((hasRead = fr.read(cbuf) > 0)){  
						System.out.println(new String(cbuf, 0, hasRead));  
				}  
		} catch (IOException e) {  
				e.printStackTrace();  
		}
}
```

## 4. FileWriter 类

### 4.1. 基本概念

### 4.2. 类定义

### 4.3. 常用 API

例：用字符流进行纯文本写出
```java
public static void main(String[] args) {  
		try (
				// true 表示追加文件，false 表示覆盖，默认 false 覆盖
				Writer fw = new FileWriter("F:/Picture/test/test2.txt", true);  
		) {  
				fw.write("abc\r\n");  
				fw.write("def\r\n");  
				fw.write("ghi\r\n");  
		} catch (IOException e) {  
				e.printStackTrace();  
		}
}
```

## 5. 应用

### 5.1. 用字节流进行文件拷贝

```java
private static void testCopyByFileStream() {  
    try (
        InputStream in = new FileInputStream("G:/test/a.txt");
				// true 表示追加文件，false 表示覆盖，默认 false 覆盖
        OutputStream out = new FileOutputStream("G:/test/b.txt", true)
    ) {
        int len;
        byte[] b = new byte[1024];
        while ((len = in.read(b)) != -1) {
            out.write(b, 0, len);
        }
    } catch (IOException e) {
        log.error("文件读取写入失败！", e);
    }
}
```

### 5.2. 用字符流进行文件拷贝

```java
public static void main(String[] args) {  
		try (
				Reader re = new FileReader("F:/Picture/test/test.txt"); 
				Writer wr = new FileWriter("F:/Picture/test/test3.txt");  
		) {  
				char[] buffer = new char[1024];  
				int hasRead = 0;  
				while((hasRead = re.read(buffer)) > 0) {  
						wr.write(buffer);  
				}  
		} catch (IOException e) {  
				e.printStackTrace();  
		}
}
```

## 6. Refer Links

[JAVA IO 分析一：File 类、字节流、字符流、字节字符转换流](https://www.cnblogs.com/pony1223/p/8030200.html)