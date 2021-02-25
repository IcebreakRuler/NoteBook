# TCP协议的特点
1. TCP是面向连接(虚连接)的传输层协议(打call)
2. 每一条TCP连接只能有两个端点，每一条TCP连接只能是点对点的

![](https://files.mdnice.com/user/8332/04498ed1-9db5-4a14-9910-3afde569d416.png)



![](https://files.mdnice.com/user/8332/ac12475f-d0aa-45a7-91b4-9e53b7d0afba.png)

## TCP报文段的首部格式

![](https://files.mdnice.com/user/8332/da491cd6-167d-4662-a39c-adbbabc0f98f.png)



![](https://files.mdnice.com/user/8332/584ec6c6-29f8-4871-bf14-1b9418844850.png)

![](https://files.mdnice.com/user/8332/4389940f-d8fd-4a5b-82b4-84bd61c7d2cd.png)



![](https://files.mdnice.com/user/8332/af34cfb6-b8de-4d31-8dff-4c8e5c1ff0b3.png)
填充：4字节 填充


## TCP连接管理
点对点，一对一通信，全双工。数据发送方也可数据接收方。

类似打call


### TCP连接建立

三个阶段：

连接建立  数据通信  连接释放

 
![](https://files.mdnice.com/user/8332/0e880f62-0bbc-4c82-b975-487f9729e251.png)


![](https://files.mdnice.com/user/8332/a8b18847-3646-416e-8c28-6100e0865276.png)

ack=x+1： 确认号。 期望收到下一个报文段第一个数据字节的序号
seq：序号 主机随机分配



### TCP连接释放

![](https://files.mdnice.com/user/8332/8a0f3ae1-1d41-4bc1-b205-be26abcc7e68.png)



![](https://files.mdnice.com/user/8332/4aca5ef0-011b-470e-b765-b985b3b1845d.png)


重传


## 攻击
![](https://files.mdnice.com/user/8332/88ecb7ec-edc5-4426-84ce-65d1a0bacc76.png)
