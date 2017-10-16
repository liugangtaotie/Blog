
## 概述

## 数据传输

### 双工通信

- 全双工：两根数据管道，一个用来传输，一个用来接收。通信允许数据在两个方向上同时传输，它在能力上相当于两个单工通信方式的结合。全双工指可以同时（瞬时）进行信号的双向传输（A→B且B→A）。指A→B的同时B→A，是瞬时同步的。（公路的双行道）
- 半双工：一根数据管道，在发送数据时不接收数据，或接收数据时不发送数据，同一时间只能用来传输或接收（公路的单边放行）
- 单工：只允许A→B传送信息，而不能B→A传送 。（公路的单行道）

### 三次握手（建立连接）

![三次握手](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/internet-tcp-01.jpeg)

- 第一次握手 B 知晓 A  可以接受 B 可以发送
- 第二次握手 A 知晓 AB 可以发送 可以接受
- 第三次握手 B 知晓 A  可以接收 B 可以发送
 
### TCP报文段（数据包）

### 四次挥手（关闭连接）

![四次挥手](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/internet-tcp-02.jpeg)

- 第一次挥手 A 告诉 B , A 端发送数据完毕，关闭发送数据通道，准备关闭接收数据通道
- 第二次挥手 B 告诉 A , B 端已知道
- 第三次挥手 B 告诉 A , B 端发送数据完毕，关闭发送数据通道，准备关闭接收数据通道
- 第四次挥手 A 告诉 B , A 端已知道，B 端关闭接收数据通道
- 等待2MSL  A 端关闭接收数据通道

**A端为什么要等待 2MSL 的时间才关闭？**

MSL: Maximum Segment Lifetime 报文最大生存时间

- 情况一：A 发送的最后一个 ACK 报文能够到达 B 。如果第四次挥手时，如果 B 没有收到 A 的确认报文，会超时重传第三次挥手的报文。
- 情况二：防止 A 中已经失效的报文出现在这个链接中。

**B端为什么要存在一个保活状态？**

如果第三次挥手完成后，A 突然故障死机了，那 B 的连接资源什么时候能释放呢？因此 B 存在一个保活状态，就是保活时间到了后，B 会发送探测信息，以决定是否释放连接。

### 数据传输

![数据传输](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/internet-base-03.png)

### 定时器

**建立连接定时器(connection-establishment timer)**

**重传定时器(retransmission timer)**

**延迟应答定时器(delayed ACK timer)**

**坚持定时器(persist timer)**

**保活定时器(keepalive timer)**

**FIN_WAIT_2定时器(FIN_WAIT_2 timer)**

**TIME_WAIT定时器 (TIME_WAIT timer, 也叫2MSL timer)**

## 参考

- http://matt33.com/2016/08/30/http-protocol/
- http://blog.csdn.net/goodboy1881/article/category/204448
- http://www.cnblogs.com/Jessy/p/3535612.html
- http://blog.csdn.net/whuslei/article/details/6667471
- http://blog.csdn.net/xulu_258/article/details/51146489
- http://www.debugrun.com/a/KqfjJer.html