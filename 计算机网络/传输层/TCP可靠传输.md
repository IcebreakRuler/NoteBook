可靠：保证接收方进程从缓存区读出来的字节流与发送方发出的字节流是是完全一样的。

传输层：使用TCP实现可靠传输


网络层：提供尽最大努力交付，不可靠传输



TCP实现可靠传输的机制：校验，序号，确认，重传


校验：与udp校验一样，增加首伪部


序号
![](https://files.mdnice.com/user/8332/35eb1f11-1e2b-491b-b442-b68287af8779.png)



确认机制：返回一个确认报文段，告诉发送方我已经收到了

稍待确认   累计确认


![](https://files.mdnice.com/user/8332/823bc72b-4530-4e29-a1df-7ee4f77d0eba.png)





![](https://files.mdnice.com/user/8332/4afa3aba-101f-41bc-ad48-4202168a6b74.png)




![](https://files.mdnice.com/user/8332/60a5a947-ee1f-45b7-893f-e7d7e0fe1bdf.png)
