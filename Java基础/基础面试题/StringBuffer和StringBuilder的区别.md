## StringBuffer
线程安全

加了同步锁

## StringBuilder
线程不安全


都继承AbstractStringBuilder类

使用字符数组来保存字符串，但是没有用final关键字修饰，所以这两种对象可变



## 性能
String：每次进行改变的时候，都会重新生成一个新的String对象

操作少量数据，适用于string
单线程数据多：StringBuilder
多线程数据多：StringBuffer


