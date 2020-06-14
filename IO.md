# ![image-20200523134324871](C:\Users\Wyatt\AppData\Roaming\Typora\typora-user-images\image-20200523134324871.png)第一章 IO概述

## 1.1 什么是IO

按照数据传输（流动）的方向，以内存为基准，分为输入input和output，即流向内存的是输入流，流出内存的是输出流。

Java中I/O操作主要是指使用java.io包下的内容，进行输入、输出操作。输入也叫做读取数据，输出也叫做作写出数据。

## 1.2 IO分类

根据数据的流向分为：输入流、输出流

- **输入流**：把数据从其他设备上读取到内存的流
- **输出流**：把数据从内存写出到其他设备的流

根据数据类型分为：字节流、字符流

- **字节流**：以字节为单位 读写数据的流
- **字符流**：以字符为单位 读写数据的流

## 1.3 顶级父类们

|            |   输入流    |    输出流    |
| :--------: | :---------: | :----------: |
| **字节流** | InputStream | OutputStream |
| **字符流** |   Reader    |    Writer    |



# 第二章 字节流

一切文件数据（文本、图片、视频等）在存储时，都是以二进制数字的形式保存，都是一个一个的字节，那么传输时一样如此。所以，字节流可以传输任意文件数据。在操作流的时候，我们要时刻明确，无论使用什么样的流对象，底层传输的始终为二进制数据。

## 2.1 字节输出流

**Class OutputStream**

- java.lang.Object
- - java.io.OutputStream

All Implemented Interfaces:

​		Closeable ， Flushable ， AutoCloseable

已知直接子类：

​		ByteArrayOutputStream ， FileOutputStream ， FilterOutputStream ， 		ObjectOutputStream ， OutputStream ， PipedOutputStream

`java.io.OutputStream`抽象类是表示字节输出流的所有类的超类。 输出流接收输出字节并将其发送到某个接收器。定义了字节输出流的基本共性方法。

```Java
public abstract class OutputStream extends Object
implements Closeable, Flushable
```

| Modifier and Type | Method and Description                                       |
| ----------------- | ------------------------------------------------------------ |
| `void`            | **`close()`** 关闭此输出流并释放与此流相关联的任何系统资源。 |
| `void`            | **`flush()`** 刷新此输出流并强制任何缓冲的输出字节被写出。   |
| `void`            | **`write(byte[] b)`** 将 `b.length`字节从指定的字节数组写入此输出流。 |
| `void`            | **`write(byte[] b, int off, int len)`** 从指定的字节数组写入 `len`个字节，从偏移 `off`开始输出到此输出流。 |
| `abstract void`   | **`write(int b)`** 将指定的字节写入此输出流。                |

### FileOutputStream

java.io.FileOutputStream extends OutputStream

文件字节输出流：把内存中的数据写入到硬盘的文件中 

FileOutputStream的使用步骤：
   	1.创建~对象，构造方法中传入写入数据的目的地
   	2.调用~的Write方法，把数据写入到文件中
   	3.释放资源

#### **构造方法**

| Constructor and Description                                  |
| ------------------------------------------------------------ |
| `FileOutputStream(File file)` 创建文件输出流以写入由指定的 `File`对象表示的文件。 |
| `FileOutputStream(File file, boolean append)` 创建文件输出流以写入由指定的 `File`对象表示的文件。 |
| `FileOutputStream(FileDescriptor fdObj)` 创建文件输出流以写入指定的文件描述符，表示与文件系统中实际文件的现有连接。 |
| `FileOutputStream(String name)` 创建文件输出流以指定的名称写入文件。 |
| `FileOutputStream(String name, boolean append)` 创建文件输出流以指定的名称写入文件。 |

​		`Fileoutputstream(String name)`创建一个向具有指定名称的文件中写入数据的输出文件流。
​		`Fileoutputstream(File file)`创建一个向指定File对象表示的文件中写入数据的文件输出流。
​		参数：写入数据的目的
​				String name：目的地是一个文件的路径
​				File file：目的地是一个文件
​		构造方法的作用：
​				1.创建一个FileoutputStream对象
​				2.会根据构造方法中传递的文件/文件路径，创建一个空的文件
​				3.会把FileoutputStream对象指向创建好的文件

#### 写出字节数据

**写入数据的原理（内存-->硬盘）:**
		java程序-->JVM（java虚拟机）-->OS（操作系统）-->OS调用写数据的方法-->把数据写入到文件中

**步骤：**

1. 创建FileOutputStream对象，传入文件名
2. 使用write方法写硬盘文件
3. 释放资源

```java
import java.io.FileOutputStream;
import java.io.IOException;

public class Demo {
    public static void main(String[] args) throws IOException {
        FileOutputStream fos=new FileOutputStream("D:\\papi.txt");
        fos.write(97);
        fos.close();
        }
}/* 写数据时会把10进制数转为二进制数写入
fos.write(1100001); 97-->1100001 
任意的文本编辑器在打开文件时，会查询编码表，将字节转化为字符表示
0~127：查ASCII表
其他：查询系统默认码表（如中文的GBK）
所以papi.txt显示 a */
```

```java
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.File;

public class Demo {
    public static void main(String[] args) throws IOException {
        FileOutputStream fos=new FileOutputStream(new File("D:\\papi.txt"));
        //在文件中显示100，要写三个字节
        fos.write(49);
        fos.write(48);
        fos.write(48);
        byte[] b1={65,66,67,68,69};//ABCDE
        byte[] b2={-65,-66,-67,68,69};//烤紻E
        /*
        public void write（byte[]b）：
        	将b.Length字节从指定的字节数组写入此输出流。
       	一次写多个字节：
            如果写的第一个字节是正数（0~127），那么显示的时候会查询ASCII表
            如果写的第一个字节是负数，那第一个字节会和第二个字节，两个字节组成一个中文显示，查		询系统默认码表（GBK）
        */	
        fos.write(b1);
        fos.write(b2);//文件目前会显示：100ABCDE烤紻E
        
        /*
        public void write(byte[] b,int off,int len):把字节数组的一部分写入文件中
        	int off:数组开始的索引
        	int len:写入几个字节
        */
        fos.write(b1,1,2);//BC  文件目前会显示：100ABCDE烤紻EBC
        
        /*写入字符的方法：
        	利用String的getBytes()方法，将字符转化为字节数组，再写入
        */
        byte[] b3="秘制晓汉堡".getBytes();
        System.out.println(Arrays.toString(b3));//UTF-8三个字节为一个中文，GBK两个字节
        fos.write(b3);//文件目前会显示：100ABCDE烤紻EBC秘制晓汉堡
        
        fos.close();
        }
}

```

#### 数据追加续写

使用FileOutputStream的两个参数的构造方法

​		`	FileOutputStream(File file, boolean append)` 

​		`FileOutputStream(String name, boolean append)`

参数：

​		File file、String name：写入数据的目的地

​		boolean append：追加写的开关，true就追加不覆盖文件，false就覆盖

写换行：换行符号

​		windows：`\r\n`		linux：`\n`		mac：`\r`



## 2.2字节输入流

**Class InputStream**

- java.lang.Object
- - java.io.InputStream

All Implemented Interfaces:

​	Closeable ， AutoCloseable

已知直接子类：

​	AudioInputStream ， ByteArrayInputStream ， FileInputStream ， FilterInputStream ， InputStream ， ObjectInputStream ， PipedInputStream ， SequenceInputStream ， StringBufferInputStream

`java.io.IutputStream`抽象类是表示字节输出流的所有类的超类。定义了字节输出流的基本共性方法。

```Java
public abstract class InputStream extends Object
implements Closeable
```

| Modifier and Type | Method and Description                                       |
| ----------------- | ------------------------------------------------------------ |
| `int`             | `available()` 返回从该输入流中可以读取（或跳过）的字节数的估计值，而不会被下一次调用此输入流的方法阻塞。 |
| `void`            | `close()` 关闭此输入流并释放与流相关联的任何系统资源。       |
| `void`            | `mark(int readlimit)` 标记此输入流中的当前位置。             |
| `boolean`         | `markSupported()` 测试这个输入流是否支持 `mark`和 `reset`方法。 |
| `abstract int`    | `read()` 从输入流读取数据的下一个字节。                      |
| `int`             | `read(byte[] b)` 从输入流读取一些字节数，并将它们存储到缓冲区 `b` 。 |
| `int`             | `read(byte[] b, int off, int len)` 从输入流读取最多 `len`字节的数据到一个字节数组。 |
| `void`            | `reset()` 将此流重新定位到上次在此输入流上调用 `mark`方法时的位置。 |
| `long`            | `skip(long n)` 跳过并丢弃来自此输入流的 `n`字节数据。        |

### FileInputStream

java.io.FileInputStream extends InputStream

- 将硬盘文件中的数据读入内存中使用；
- 用于读取诸如图像数据的原始字节流。 
- 要阅读字符串，请考虑使用`FileReader` 。 



#### 构造方法

| Constructor and Description                                  |
| ------------------------------------------------------------ |
| `FileInputStream(File file)` 通过打开与实际文件的连接创建一个 `FileInputStream` ，该文件由文件系统中的 `File`对象 `file`命名。 |
| `FileInputStream(FileDescriptor fdObj)` 创建 `FileInputStream`通过使用文件描述符 `fdObj` ，其表示在文件系统中的现有连接到一个实际的文件。 |
| `FileInputStream(String name)` 通过打开与实际文件的连接来创建一个 `FileInputStream` ，该文件由文件系统中的路径名 `name`命名。 |

参数：要读取文件的数据源

​		String name：目标文件的路径			File file：目标文件

构造方法的作用：
				1.创建一个FileInputStream对象
			    2.把FileInputStream对象指向目标文件

​		

#### 读取字节数据

硬盘-->内存的原理：

​	Java程序-->JVM-->OS-->OS读取数据的方法-->读取文件

**步骤：**

1. 创建FileInputStream对象，传入文件名
2. 使用read方法读硬盘文件
3. 释放资源

```Java
public class Demo {
    public static void main(String[] args) throws IOException {
        FileInputStream fis=new FileInputStream("step1\\papi.txt");//
        //read()读取文件中的一个字节并返回它，读取到文件末尾返回-1，每次read后都会移动指针
        int len=0;
        while((len =fis.read())!=-1){//所以循环时一定要依靠一个临时变量接收read值
            System.out.print((char)len);
        }
        fis.close();
        
        
    }
    
```