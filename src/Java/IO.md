## 1、什么是字节输入流

`InputStream`用于从源头（通常是文件）读取数据（字节信息）到内存中



`InputStream` 常用方法 ：

- `read()` ：返回输入流中下一个字节的数据。返回的值介于 0 到 255 之间。如果未读取任何字节，则代码返回 `-1` ，表示文件结束。
- `read(byte b[ ])` : 从输入流中读取一些字节存储到数组 `b` 中。如果数组 `b` 的长度为零，则不读取。如果没有可用字节读取，返回 `-1`。如果有可用字节读取，则最多读取的字节数最多等于 `b.length` ， 返回读取的字节数。这个方法等价于 `read(b, 0, b.length)`。
- `read(byte b[], int off, int len)` ：在`read(byte b[ ])` 方法的基础上增加了 `off` 参数（偏移量）和 `len` 参数（要读取的最大字节数）。
- `skip(long n)` ：忽略输入流中的 n 个字节 ,返回实际忽略的字节数。
- `available()` ：返回输入流中可以读取的字节数。
- `close()` ：关闭输入流释放相关的系统资源。
- `readAllBytes()` ：读取输入流中的所有字节，返回字节数组。
- `readNBytes(byte[] b, int off, int len)` ：阻塞直到读取 `len` 个字节。
- `transferTo(OutputStream out)` ： 将所有字节从一个输入流传递到一个输出流。



> FileInputStream

字节输入流对象，可直接指定文件路径，可以直接读取单字节数据，也可以读取至字节数组中



> DataInputStream

读取指定类型数据，不能单独使用，必须结合 `FileInputStream`



> ObjectInputStream

从输入流中读取 Java 对象（反序列化）



> 



## 2、什么是字节输出流

OutputStream，将数据（字节信息）写入到目的地（通常是文件）

`OutputStream` 常用方法 ：

- `write(int b)` ：将特定字节写入输出流。
- `write(byte b[ ])` : 将数组`b` 写入到输出流，等价于 `write(b, 0, b.length)` 。
- `write(byte[] b, int off, int len)` : 在`write(byte b[ ])` 方法的基础上增加了 `off` 参数（偏移量）和 `len` 参数（要读取的最大字节数）。
- `flush()` ：刷新此输出流并强制写出所有缓冲的输出字节。
- `close()` ：关闭输出流释放相关的系统资源



> FileOutputStream

字节输出流对象，可直接指定文件路径，可以直接输出单字节数据，也可以输出指定的字节数组



> DataOutputStream

写入指定类型数据，不能单独使用，必须结合 `FileOutputStream`



> ObjectOutputStream 

将对象写入到输出流



## 3、为什么 I/O 流操作要分为字节流操作和字符流操作？

如果以字节方式传输，会出现以下问题：

- 如果将字符流从字节流转换回字符，是由 Java 虚拟机将字节转换得到的，这个过程还算是比较耗时。
- 如果我们不知道字符的具体编码类型，将字节流转化为字符易出现乱码问题。

所以干脆使用字符流传输，避免中间状态的转换

如果音频文件、图片等媒体文件用字节流比较好，如果涉及到字符的话使用字符流比较好。



## 4、什么是字符输入流，有哪些常用操作

`Reader`用于从源头（通常是文件）读取数据（字符信息）到内存中

`Reader` 常用方法 ：

- `read()` : 从输入流读取一个字符。
- `read(char[] cbuf)` : 从输入流中读取一些字符，并将它们存储到字符数组 `cbuf`中，等价于 `read(cbuf, 0, cbuf.length)` 。
- `read(char[] cbuf, int off, int len)` ：在`read(char[] cbuf)` 方法的基础上增加了 `off` 参数（偏移量）和 `len` 参数（要读取的最大字节数）。
- `skip(long n)` ：忽略输入流中的 n 个字符 ,返回实际忽略的字符数。
- `close()` : 关闭输入流并释放相关的系统资源。



> InputStreamReader

字节流转换为字符流的桥梁，其子类 `FileReader` 是基于该基础上的封装，可以直接操作字符文件



## 5、什么是字符输出流，有哪些常用操作

`Writer`用于将数据（字符信息）写入到目的地（通常是文件）

`Writer` 常用方法 ：

- `write(int c)` : 写入单个字符。
- `write(char[] cbuf)` ：写入字符数组 `cbuf`，等价于`write(cbuf, 0, cbuf.length)`。
- `write(char[] cbuf, int off, int len)` ：在`write(char[] cbuf)` 方法的基础上增加了 `off` 参数（偏移量）和 `len` 参数（要读取的最大字节数）。
- `write(String str)` ：写入字符串，等价于 `write(str, 0, str.length())` 。
- `write(String str, int off, int len)` ：在`write(String str)` 方法的基础上增加了 `off` 参数（偏移量）和 `len` 参数（要读取的最大字节数）。
- `append(CharSequence csq)` ：将指定的字符序列附加到指定的 `Writer` 对象并返回该 `Writer` 对象。
- `append(char c)` ：将指定的字符附加到指定的 `Writer` 对象并返回该 `Writer` 对象。
- `flush()` ：刷新此输出流并强制写出所有缓冲的输出字符。
- `close()`:关闭输出流释放相关的系统资源。



> OutputStreamWriter

`OutputStreamWriter` 是字符流转换为字节流的桥梁，其子类 `FileWriter` 是基于该基础上的封装，可以直接将字符写入到文件



## 6、什么是字节缓冲流，有什么用

`BufferedInputStream`缓冲流将数据加载至缓冲区，一次性读取/写入多个字节，从而避免频繁的 IO 操作，提高流的传输效率。

`BufferedOutputStream`将数据（字节信息）写入到目的地（通常是文件）的过程中不会一个字节一个字节的写入，而是会先将要写入的字节存放在缓存区，并从内部缓冲区中单独写入字节。这样大幅减少了 IO 次数，提高了读取效率



字节流和字节缓冲流的性能差别主要体现在我们使用两者的时候都是调用 `write(int b)` 和 `read()` 这两个一次只读取一个字节的方法的时候。

由于字节缓冲流内部有缓冲区（字节数组），因此，字节缓冲流会先将读取到的字节存放在缓存区，大幅减少 IO 次数，提高读取效率。



## 7、什么是字符缓冲流，有什么用

`BufferedReader` （字符缓冲输入流）和 `BufferedWriter`（字符缓冲输出流）类似于 `BufferedInputStream`（字节缓冲输入流）和`BufferedOutputStream`（字节缓冲输入流），内部都维护了一个字节数组作为缓冲区。



## 8、什么是打印流

`System.out` 实际是用于获取一个 `PrintStream` 对象，`print`方法实际调用的是 `PrintStream` 对象的 `write` 方法。



`PrintStream` 属于字节打印流，与之对应的是 `PrintWriter` （字符打印流）。

`PrintStream` 是 `OutputStream` 的子类，`PrintWriter` 是 `Writer` 的子类。



## 9、随机访问流的读写模式有哪些

读写模式主要有下面四种：

- `r` : 只读模式。
- `rw`: 读写模式
- `rws`: 相对于 `rw`，`rws` 同步更新对“文件的内容”或“元数据”的修改到外部存储设备。
- `rwd` : 相对于 `rw`，`rwd` 同步更新对“文件的内容”的修改到外部存储设备。

文件内容指的是文件中实际保存的数据，元数据则是用来描述文件属性比如文件的大小信息、创建和修改时间。



## 10、什么是随机访问流

`RandomAccessFile` 中有一个文件指针用来表示下一个将要被写入或者读取的字节所处的位置。

可以通过 `RandomAccessFile` 的 `seek(long pos)` 方法来设置文件指针的偏移量（距文件开头 `pos` 个字节处）。

如果想要获取文件指针当前的位置的话，可以使用 `getFilePointer()` 方法。



## 11、随机访问流有什么用

实现大文件的 **断点续传**

上传文件中途暂停或失败（比如遇到网络问题）之后，不需要重新上传，只需要上传那些未成功上传的文件分片即可。

分片（先将文件切分成多个文件分片）上传是断点续传的基础。

`RandomAccessFile` 可以合并文件分片



## 12、IO流是如何使用装饰器模式的

装饰器模式通过组合替代继承来扩展原始类的功能



`BufferedInputStream`、`DataInputStream` 可以增强`InputStream`子类，

`BufferedOutputStream`、`DataOutputStream`可以增强`OutputStream`子类，

`BufferedReader` 可以用来增加 `Reader` 子类的功能，

`BufferedWriter` 可以用来增加 `Writer` 子类的功能。



 `BufferedInputStream`增强 `FileInputStream` 的功能：

因为`BufferedInputStream`和`FileInputStream`实现了相同的接口`InputStream`，

`BufferedInputStream`聚合了`InputStream`对象，是一个标准的装饰器模式，在内部可以调用被增强对象的操作，也可以使用自己定义的一些增强操作



## 13、IO流是如何使用适配器模式的

IO 流中的字符流和字节流的接口不同，它们之间可以协调工作就是基于适配器模式来做的，更准确点来说是对象适配器。

通过适配器，我们可以将字节流对象适配成一个字符流对象，这样我们可以直接通过字节流对象来读取或者写入字符数据。

`InputStreamReader` 和 `OutputStreamWriter` 就是两个适配器(Adapter)，是字节流和字符流之间的桥梁。



`InputStreamReader` 继承了`Reader`，使用聚合了`InputStream`的 `StreamDecoder` 对字节进行解码，**实现字节流到字符流的转换，*；

 `OutputStreamWriter` 继承了`Writer`，使用聚合了`OutputStream`的`StreamEncoder`对字符进行编码，**实现字符流到字节流的转换。**



`InputStream` 和 `OutputStream` 的子类是被适配者

 `InputStreamReader` 和 `OutputStreamWriter`是适配器。



## 14、NIO是如何使用工厂模式的

 `Files` 类的 `newInputStream` 方法用于创建 `InputStream` 对象（静态工厂）

 `Paths` 类的 `get` 方法创建 `Path` 对象（静态工厂）

`ZipFileSystem` 类的 `getPath` 的方法创建 `Path` 对象（简单工厂）



## 15、NIO是如何使用 观察者模式的

NIO 中的文件目录监听服务使用到了观察者模式。

NIO 中的文件目录监听服务基于 `WatchService` 接口和 `Watchable` 接口。

`WatchService` 属于观察者，`Watchable` 属于被观察者。



1、`Watchable` 接口定义了一个用于将对象注册到 `WatchService`（监控服务） 并绑定监听事件的方法 `register` 。

2、`WatchService` 用于监听文件目录的变化，同一个 `WatchService` 对象能够监听多个文件目录，可以同时监听多种事件。

3、`WatchService` 内部是通过一个 daemon thread（守护线程）采用定期轮询的方式来检测文件的变化



> 常用的监听事件有 3 种：
>
> - `StandardWatchEventKinds.ENTRY_CREATE` ：文件创建。
> - `StandardWatchEventKinds.ENTRY_DELETE` : 文件删除。
> - `StandardWatchEventKinds.ENTRY_MODIFY` : 文件修改。





## 16、什么是IO

从计算机结构的视角来看的话， I/O 描述了计算机系统与外部设备之间通信的过程，输入设备向计算机输入数据，输出设备接收计算机输出的数据

从应用程序的视角来看的话，我们的应用程序想要获取系统资源，就要进行IO操作，而用户程序是运行在用户态的，操作系统资源必须进入内核态，因此必须对操作系统的内核发起 IO 调用（系统调用），操作系统负责的内核执行具体的 IO 操作。



## 17、 有哪些常见的 IO 模型?

**同步阻塞 I/O**

> 应用程序发起 read 调用后，会一直阻塞，直到内核把数据拷贝到用户空间

**同步非阻塞 I/O**

> 应用程序会一遍干其它事情一边轮询，发起 read 调用，查看数据是否准备好，如果准备好了，就阻塞
>
> 等待数据从内核空间拷贝到用户空间的这段时间里，线程依然是阻塞的，直到在内核把数据拷贝到用户空间。

**I/O 多路复用**

> 发起select或epoll让系统准备数据，然后就离开，相比阻塞非同步少了轮询，而是数据准备好后由系统通知应用程序，再发起read去阻塞拷贝数据

> **选择器 ( Selector )** 的概念，也可以被称为 **多路复用器**。通过它，只需要一个线程便可以管理多个客户端连接。当客户端数据到了之后，才会为其服务。

**信号驱动 I/O**

**异步 I/O**

> 应用操作之后会直接返回，不会堵塞在那里，当后台处理完成，操作系统会通知相应的线程进行后续的操作。
>
> 后台处理包括数据的准备和拷贝到用户空间



## 18、BIO、NIO、AIO有什么区别

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220913205849782.png" alt="image-20220913205849782" style="zoom:50%;" />

