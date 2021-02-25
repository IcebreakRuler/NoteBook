# ARP协议
由于在实际网络的链路上传送数据帧时，最终必须使用MAC地址。

ARP协议：完成主机或路由器IP地址到MAC地址的映射(解决吓一跳走哪的问题)


![](https://files.mdnice.com/user/8332/030451dc-1dd7-4641-89e4-9f01edad90ea.png)

ARP协议自动进行地址解析。


![](https://files.mdnice.com/user/8332/97b296f5-a1bf-4298-8cb1-8ba20bb78b0f.png)
为ip协议提供服务
## 发送数据的过程
有一个传输单元 MTU 来决定是否分片，

每个主机和路由器都会有一个ARP高速缓存。


IP地址与MAC地址的映射     

表项  3号主机的ip 以及所对应的mac地址


目的物理地址 全1， 广播ARP请求分组

从1号主机发出来，如果是广播地址，放到交换机，从所有端口转发出去，帧的概念

单播ARP相应分组


![](https://files.mdnice.com/user/8332/551788dd-2b25-468a-bb5b-327c8f5cbc8a.png)


## 不在同一局域网

![](https://files.mdnice.com/user/8332/e960c707-49ad-4c98-8bb1-bdab01a85c46.png)


![](https://files.mdnice.com/user/8332/51112678-e624-486a-b73b-a6db7b8d0ace.png)

对遥远的5号主机进行通信

路由器的端口有mac地址

传输层分成报文段，



