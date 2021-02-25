# NIO

Non Blocking IO 非阻塞IO

## 概念
New IO是从java1.4版本开始引入的一个新的IO API，可以替代标准的JAVA IO API。NIO与原来的IO有同样的作用和目的，但是使用的方式i完全不同，NIO支持**面向缓冲区、基于通道的IO操作**。NIO将以更加高效的方式进行文件的读写操作。



## NIO与IO的区别
IO:面向流，阻塞IO，

NIO:面向缓冲区，非阻塞IO，选择器

![](https://files.mdnice.com/user/8332/862b37a5-e22e-443e-adb7-8cf96040d6f0.png)
通道只作为连接。传输需要缓冲区

缓冲区作为火车  通道可以当作铁路

缓冲区是双向的，IO流为单向的

## 通道和缓冲区
Java NIO核心在于：通道(channel)和缓冲区(buffer)。通道表示打开到IO设备(例如：文件,套接字)的连接。若需要用到NIO系统，需要获取用于连接IO设备的通道以及用于容纳数据的缓冲区。然后操作缓冲区，对数据进行处理。

简而言之：Channel负责传输，Buffer负责存储
## 缓冲区类型
- Bytebuffer
- Charbuffer
- Shortbuffer
- IntBuffer
- LongBuffer
- FloatBuffer
- DoubleBuffer

## 缓冲区属性
1. put()：存入数据到缓冲区
2. get()：获取缓冲区中的数据
3. capacity：容量，表示缓冲区最大的存储数据的容量，一旦声明，不可更改
4. limit：界限，表示缓冲区中可以操作数据的大小。(limit 后数据不能进行读写)
5. position：位置，表示缓冲区正在操作数据的位置
6. mark()；记录当前position的位置，可以通过reset()恢复到mark的位置
7. reset():恢复到之前mark标记的位置
8. 
0<=mark<=position<=limit<=capacity


![](https://files.mdnice.com/user/8332/8ab38ab0-5371-4fb4-9100-9f5205fe5d85.png)


## 直接缓冲区与非直接缓冲区
### 非直接缓冲区
- 非直接缓冲区：通过allcoate()方法分配缓冲区，将缓冲区建立在JVM的内存中
![](https://files.mdnice.com/user/8332/3fc7c3bb-c9b4-426f-ac3e-76b984b5e452.png)

![](https://files.mdnice.com/user/8332/cb9b4137-0b28-48aa-bd38-6cfe66c976e4.png)

### 直接缓冲区
- 直接缓冲区：通过allocateDirect()方法分配直接缓冲区，并将缓冲区建立在物理内存中，可以提高效率
![](https://files.mdnice.com/user/8332/f3e212fa-3991-41d0-b536-53f9503cc3f1.png)




## 通道(channel)


![](https://files.mdnice.com/user/8332/1856c7f9-bdeb-4130-a08f-e58843354461.png)

### DMA
控制IO接口，与内存交互
![](https://files.mdnice.com/user/8332/cee343f9-5866-481b-961d-53d33941a0c8.png)


### channel
通道：一个完全的独立的处理器，专门用于io操作，拥有自己的命令，不用去找cpu申请权限。

用于源节点与目标节点的连接，负责数据的传输

channel本身不存储数据，配合缓冲区进行传输。

1. NIO的通道类似于流，而流只能读或者只能写
2. 通道可以实现异步读写数据
3. 通道可以从缓冲读数据，也可以写数据到缓冲 
### 实现类
**java.nio.channels.Channel接口：**
- FileChannel：用于读取，写入，映射和操作文件的通道
- SocketChannel：通过TCP读写网络中的数据
- ServerSocketChannel：可以监听新进来的TCP连接，对每一个新进来的连接都会创建一个SocketChannel(ServerSocketChannel类似ServerSocket，SocketChannel类似Socket)
- DatagramChannel：通过UDP读写网络中的数据通道

### 获取通道
1. java针对支持通道的类提供了getChannel()方法
**本地IO:**
- fileInputStream/FileOutputStream
- RandomAccessFile

**网络IO:**
- Socket
- ServerSocket
- DatagramSocket

2, jdk1.7中的NIO.2，AIO，针对各个通道了提供了一个静态方法open()，可以直接获取通道

Files的工具类的newByteChannel()；也可以直接获取通道

3. 通道之间的数据传输
- transferFrom()
- transferTo()
![](https://files.mdnice.com/user/8332/2fdbb0b5-54a8-4dc0-8471-11419381902a.png)

## Selector选择器
一个I/O线程可以并发处理N个客户端连接和读写操作，这从根本上解决了传统同步阻塞I/O一连接一模型
- 读：selectionKey.OP_READ
- 写：SelectionKey.OP_WRITE
- 连接：SelectionKey.OP_CONNECT
- 接受：SelectionKey.OP_ACCEPT

若连接时不止监听一个事件，则可以使用位或操作符连接




## AIO
![](https://files.mdnice.com/user/8332/abd27865-f4fb-4b5c-88f4-82f571bfac79.png)

## 总结

![](https://files.mdnice.com/user/8332/2244ec52-1850-4c0c-8587-40f225738261.png)
