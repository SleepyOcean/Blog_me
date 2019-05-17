[TOC]

# Java IO小结

> java IO流结构图：

![Alt java_IO_struct](http://images.cnitblog.com/blog2015/431968/201504/171417339952173.jpg)

![Alt java_IO_Struct](http://www.myexception.cn/img/2015/04/18/123910561.jpg)

## RandomAccessFile

`java.io.File`类用于表示文件（目录）
`File`类只用于表示文件（目录）的信息（名称、大小等），不能用于文件内容的访问

`RandomAccessFile` java提供的对文件内容的访问，既可以读文件，也可以写文件。
`RandomAccessFile`支持随机访问文件，可以访问文件的任意位置

1. java文件模型

    在硬盘上的文件是byte byte byte...存储的,是数据的集合


2. 打开文件

    有两种模式"rw"(读写)  "r"（只读)

    `RandomAccessFile raf = new RandomeAccessFile(file,"rw")`

    文件指针，打开文件时指针在开头 pointer = 0;


3.  写方法

   `raf.write(int)`-->只写一个字节（后8位),同时指针指向下一个位置，准备再次写入


4. 读方法

   `int b = raf.read()`--->读一个字节

5. 文件读写完成以后一定要关闭（Oracle官方说明）



## 序列化与基本类型序列化

1. 将类型int 转换成4 byte或将其他数据类型转换成byte的过程叫序列化

    数据---->n byte

2. 反序列化

   将n个byte 转换成一个数据的过程
   n byte ---> 数据

3. `RandomAccessFile`提供基本类型的读写方法，可以将基本类型数据序列化到文件或者将文件内容反序列化为数据



## IO流(输入流、输出流)

 __字节流、字符流__

1. __字节流__

   * InputStream、OutputStream

     InputStream抽象了应用程序读取数据的方式
     OutputStream抽象了应用程序写出数据的方式 

   * EOF = End   读到-1就读到结尾

   ```java
   public static void copyFile(File srcFile,File destFile)throws IOException{
           FileInputStream in = new FileInputStream(srcFile);
           FileOutputStream out = new FileOutputStream(destFile);
           byte[] buf = new byte[8*1024];
           // in.read(buf);
           // out.write(buf);
           int b;
           /**
            * b的值为一次读取的byte数，以下while循环的意思为：
            *   b的最大值即为buf.length，当一次读不完整个文件时，指针会继续后移，当b读不到字节时
            * b的值即为-1，这时退出循环。倒数第二次可能出现b<buf.length的情况。
            */
           while((b=in.read(buf, 0, buf.length))!=-1){
               out.write(buf, 0, b);
               out.flush();
           }
           in.close();
           out.close();
       }
   ```

   * 输入流基本方法

        `int  b = in.read();`读取一个字节无符号填充到int低八位.-1是 EOF
        `in.read(byte[] buf)` 
        `in.read(byte[] buf,int start,int size)`

   * 输出流基本方法

       `out.write(int b)`  写出一个byte到流，b的低8位
       `out.write(byte[] buf)`将buf字节数组都写入到流
       `out.write(byte[] buf,int start,int size)`

   * FileInputStream--->具体实现了在文件上读取数据

   * FileOutputStream 实现了向文件中写出byte数据的方法

   * DataOutputStream/DataInputStream

     对"流"功能的扩展，可以更加方面的读取int,long，字符等类型数据

     * DataOutputStream

        writeInt()/writeDouble()/writeUTF()

   * BufferedInputStream&BufferedOutputStream

     这两个流类位IO提供了带缓冲区的操作，一般打开文件进行写入或读取操作时，都会加上缓冲，这种流模式提高了IO的性能从应用程序中把输入放入文件，相当于将一缸水倒入到另一个缸中:
      FileOutputStream--->write()方法相当于一滴一滴地把水“转移”过去
      DataOutputStream-->writeXxx()方法会方便一些，相当于一瓢一瓢把水“转移”过去
      BufferedOutputStream--->write方法更方便，相当于一飘一瓢先放入桶中，再从桶中倒入到另一个缸中，性能提高了

2. __字符流__

   * 编码问题

   * 认识文本和文本文件

      java的文本(char)是16位无符号整数，是字符的unicode编码（双字节编码)
      文件是byte byte byte ...的数据序列
     文本文件是文本(char)序列按照某种编码方案(utf-8,utf-16be,gbk)序列化为byte的存储结

   * 字符流(Reader Writer)---->操作的是文本文本文件

     字符的处理，一次处理一个字符
     字符的底层任然是基本的字节序列
     字符流的基本实现
      InputStreamReader   完成byte流解析为char流,按照编码解析
      OutputStreamWriter  提供char流到byte流，按照编码处理  

      FileReader/FileWriter
     字符流的过滤器
      BufferedReader   ---->readLine 一次读一行
      BufferedWriter/PrintWriter   ---->写一行    

3. 对象的序列化，反序列化

   * 对象序列化，就是将Object转换成byte序列，反之叫对象的反序列化 


   * 序列化流(ObjectOutputStream),是过滤流----writeObject

      反序列化流(ObjectInputStream)---readObject

   * 序列化接口(Serializable)

      对象必须实现序列化接口 ，才能进行序列化，否则将出现异常
      这个接口，没有任何方法，只是一个标准

   * transient关键字
   * 序列化中 子类和父类构造函数的调用问题

```java
private void writeObject(java.io.ObjectOutputStream s)
	        throws java.io.IOException
private void readObject(java.io.ObjectInputStream s)
	        throws java.io.IOException, ClassNotFoundException
```

   分析ArrayList源码中序列化和反序列化的问题

​	

 

 

 

   

   

   

   