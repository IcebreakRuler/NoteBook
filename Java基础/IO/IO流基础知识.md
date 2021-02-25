# IO

按处理数据单元分类：
  - 字节流：以字节为单位，顶级父类 InputStream,OutputStream
  - 字符流：以字符为单位，顶级父类 Reader,Writer
  
按处理对象不同分类：
- 节点流：操作必须通过它们进行
- 处理流：提高性能，灵活性，对节点流进行包装，



![](https://files.mdnice.com/user/8332/b5441f1c-51c7-4530-bfce-4cf2199aaa66.png)


![](https://files.mdnice.com/user/8332/aabdf866-4dbf-4795-977c-c67590d9635a.png)



![](https://files.mdnice.com/user/8332/bce0f98a-b91a-4a63-9338-263d617c1f3e.png)

![](https://files.mdnice.com/user/8332/bbb4a0e1-6c8d-45af-bf9d-2e8c45a84a9c.png)
## File类
file类不能对文件的内容进行操作，只能使用IO流来进行操作
### API
1. getName()： 文件名称
2. getAbsolutePath()：绝对路径
3. length()： 长度
4. exists()： 是否存在
5. canRead()： 可读
6. canWrite()： 可写
7. canExecute()： 可执行
8. isFile()： 是否是文件
9. isDirectory()： 是否是文件夹
10. listFiles()： 返回一个file数组
11. list()：返回文件名，返回一个string类型的数组
12. lastModified()：最后修改的时间
13. delete()：删除文件
14. createNewFile()：创建新文件
15. getParentFile()：获取上级文件夹
16. mkdir()：新建文件夹 一级文件夹
17. mkdirs()：新建多级文件夹 
## FilterInputStream 
Read()
write()

## Buffered 处理流
简化操作，
提高性能

## 对象流
- ObjectInputStream
- ObjectOutputStream
只有处理流，不是节点流
只有字节流，没有字符流
数据流只能操作基本数据类型和字符串，对象流可以操作对象，写入的是二进制数据，无法直接通过记事本进行查看，写入数据需要使用对应的输入流来读取


之前使用文件流，缓冲流读取文件只能按照字节、数组方式读取，最方便的也是按行读取，能否很方便的实现对各种基本类型和引用类型数据的读写，并保留其本身的类型。

数据流DataInputStream和DataOutputStream和对象流ObjectInputStream和ObjectOutputStream可以解决这个问题，最大的优势就是提供了方便操作各种数据类型的方法，直接调用，简单方便。


## 序列化与反序列化
序列化：将对象的状态信息转换为可以存储或传输的形式的过程
对象(内存)-》》字节数组--》字节序列(外存，网络)
反序列化 反之
## 装饰者模式

## 适配器模式
InputStreamReader
OutPUtStreamWriter



![](https://files.mdnice.com/user/8332/65f54e6e-c362-4c85-bc00-b84490995a62.png)

![](https://files.mdnice.com/user/8332/70a94471-b649-43ee-8e61-d078c0a92fa6.png)
