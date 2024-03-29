## 1、TCP 连接，一端断电和进程崩溃有什么区别？

如果「**客户端进程崩溃**」，客户端的进程在发生崩溃的时候，内核会发送 FIN 报文，与服务端进行四次挥手。

但是，「**客户端主机宕机**」，那么是不会发生四次挥手的

- 如果服务端会发送数据，由于客户端已经不存在，收不到数据报文的响应报文，服务端的数据报文会超时重传，当重传总间隔时长达到一定阈值（内核会根据 tcp_retries2 设置的值计算出一个阈值）后，会断开 TCP 连接；
- 如果服务端一直不会发送数据，再看服务端有没有开启 TCP keepalive 机制？
  - 如果有开启，服务端在一段时间没有进行数据交互时，会触发 TCP keepalive 机制，探测对方是否存在，如果探测到对方已经消亡，则会断开自身的 TCP 连接；
  - 如果没有开启，服务端的 TCP 连接会一直存在，并且一直保持在 ESTABLISHED 状态。



>  客户端主机宕机，又迅速重启

**只要有一方重启完成后，收到之前 TCP 连接的报文，都会回复 RST 报文，以断开连接**



> 超时重传次数

 tcp_retries2 设置了 15 次，并不代表 TCP 超时重传了 15 次才会通知应用程序终止该 TCP 连接，**内核会根据 tcp_retries2 设置的值，计算出一个 timeout**（*如果 tcp_retries2 =15，那么计算得到的 timeout = 924600 ms*），**如果重传间隔超过这个 timeout，则认为超过了阈值，就会停止重传，然后就会断开 TCP 连接**。



## 2、拔掉网线后， 原本的 TCP 连接还存在吗？

有数据传输的情况：

- 在客户端拔掉网线后，如果服务端发送了数据报文，那么在服务端重传次数没有达到最大值之前，客户端就插回了网线，那么双方原本的 TCP 连接还是能正常存在，就好像什么事情都没有发生。
- 在客户端拔掉网线后，如果服务端发送了数据报文，在客户端插回网线之前，服务端重传次数达到了最大值时，服务端就会断开 TCP 连接。等到客户端插回网线后，向服务端发送了数据，因为服务端已经断开了与客户端相同四元组的 TCP 连接，所以就会回 RST 报文，客户端收到后就会断开 TCP 连接。至此， 双方的 TCP 连接都断开了。

没有数据传输的情况：

- 如果双方都没有开启 TCP keepalive 机制，那么在客户端拔掉网线后，如果客户端一直不插回网线，那么客户端和服务端的 TCP 连接状态将会一直保持存在。
- 如果双方都开启了 TCP keepalive 机制，那么在客户端拔掉网线后，如果客户端一直不插回网线，TCP keepalive 机制会探测到对方的 TCP 连接没有存活，于是就会断开 TCP 连接。而如果在 TCP 探测期间，客户端插回了网线，那么双方原本的 TCP 连接还是能正常存在。



## 3、tcp_tw_reuse 为什么默认是关闭的？

tcp_tw_reuse 的作用是让客户端快速复用处于 TIME_WAIT 状态的端口，相当于跳过了 TIME_WAIT 状态，这可能会出现这样的两个问题：

- 历史 RST 报文可能会终止后面相同四元组的连接，因为 PAWS 检查到即使 RST 是过期的，也不会丢弃。

  开启 tcp_tw_reuse 的同时，也需要开启 tcp_timestamps，可以用时间戳的方式有效的判断回绕序列号的历史报文。

  RST 报文的时间戳即使过期了，只要 RST 报文的序列号在对方的接收窗口内，也是能被接受的。

  **当之前被网络延迟 RST 报文抵达了新连接客户端，而且 RST 报文的序列号在客户端的接收窗口内，由于防回绕序列号算法不会防止过期的 RST，所以 RST 报文会被客户端接受了，于是客户端的连接就断开了**。

- 如果第四次挥手的 ACK 报文丢失了，有可能被动关闭连接的一方不能被正常的关闭;



## 4、HTTPS 中 TLS 和 TCP 能同时握手吗？

正常情况下，服务端只有在收到客户端的 **TCP 的第三次握手后**，才能和客户端进行后续 **TLSv1.3 握手**；

即便TCP第三次握手可以携带数据，也不能进行TLC1.3的一次握手。



客户端和服务端同时支持 **TCP Fast Open** ，在**第二次连接**的时候，可以绕过三次握手，直接发送SYN，cookie，数据；

此时若是**TLS1.3**，可以携带TLS握手，即**TLS和TCP同时握手**。

若是TLSv1.3 0-RTT 会话恢复过程，HTTP 请求也可以在这期间内一同完成，否则在TLS握手后进行。



## 5、TCP Keepalive 和 HTTP Keep-Alive 是一个东西吗？

为了实现同一个TCP连接发送接收多个请求，HTTP 的提出了Keep-Alive 机制，也就是HTTP长连接，由应用程序进行实现，只要任意一端没有明确提出断开连接，则保持 TCP 连接状态，减少了 HTTP 短连接带来的多次 TCP 连接建立和释放的开销。



TCP 的 Keepalive 也叫 TCP 保活机制，该功能是由「内核」实现的，当客户端和服务端长达一定时间没有进行数据交互时，达到了触发 TCP 保活机制的条件，内核为了确保该连接是否还有效，就会发送探测报文，来检测对方是否还在线，然后来决定是否要关闭该连接。



## 6、TCP 协议有什么缺陷？

> 升级 TCP 的工作很困难

TCP 协议是在内核中实现的，应用程序只能使用不能修改，如果要想升级 TCP 协议，那么只能升级内核。

内核升级涉及到底层软件和运行库的更新，需要测试是否兼容新的内核版本，所以服务器的内核升级也比较保守和缓慢。

> TCP 建立连接的延迟以及安全性

在 TCP 三次握手之后，还需要经过 TLS 四次握手后，才能进行 HTTP 数据的传输，这在一定程序上增加了数据传输的延迟

TLS 是在应用层实现的握手，而 TCP 是在内核实现的握手，这两个握手过程是无法结合在一起的，总是得先完成 TCP 握手，才能进行 TLS 握手。

哪怕是基于TLS1.3和TCPFastOpen，在第二次连接时会出现同时握手的情况，它在握手信息处理顺序上也是先进行的TCP握手。

也正是 TCP 是在内核实现的，所以 TLS 是无法对 TCP 头部加密的，这意味着 TCP 的序列号都是明文传输，所以就存安全的问题。为了避免RST攻击，TCP每次连接都使用两个随机的序列号，但是TLS是无法对序列号进行加密的，因此存在序列号别截的风险，然后伪造RST进行攻击。

> TCP 存在队头阻塞问题

TCP 是字节流协议，TCP 层必须保证收到的字节数据是完整且有序的，如果序列号较低的 TCP 段在网络传输中丢失了，即使序列号较高的 TCP 段已经被接收了，应用层也无法从内核中读取到这部分数据。

只有等到丢失的低序列号重传后，接收方的应用层才可以从内核中读取到数据。

> 网络迁移需要重新建立 TCP 连接

基于 TCP 传输协议的 HTTP 协议，由于是通过四元组（源 IP、源端口、目的 IP、目的端口）确定一条 TCP 连接。

当移动设备的网络从 4G 切换到 WIFI 时，意味着 IP 地址变化了，那么就必须要断开连接，然后重新建立 TCP 连接。

而建立连接的过程包含 TCP 三次握手和 TLS 四次握手的时延，以及 TCP 慢启动的减速过程，给用户的感觉就是网络突然卡顿了一下，因此连接的迁移成本是很高的



## 7、QUIC 是如何实现可靠传输的？

HTTP/3在应用层加了三个头部信息

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220915191654370.png" alt="image-20220915191654370" style="zoom:50%;" />

在首次连接和输出连接的Package头信息是不一样的；

QUIC 也是需要三次握手来建立连接的，主要目的是为了协商连接 ID。协商出连接 ID 后，后续传输时，双方只需要固定住连接 ID，从而实现连接迁移功能。

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220915191642824.png" alt="image-20220915191642824" style="zoom: 67%;" />

Packet Number是为了解决丢包问题，QUIC发送和接收包需要确认号，但是重传时不会重传原来的编号，因此包编号是单调递增的。



Packet Number 单调递增的两个好处：

- 可以更加精确计算 RTT，没有 TCP 重传的歧义性问题；

  > 在TCP中客户端就无法判断出是「原始报文的响应」还是「重传报文的响应」，这样在计算 RTT（往返时间） 时应该选择从发送原始报文开始计算，还是重传原始报文开始计算呢？

- 可以支持乱序确认，因为丢包重传将当前窗口阻塞在原地，而 TCP 必须是顺序确认的，丢包时会导致窗口不滑动；

  > 待发送端获知数据包Packet N 丢失后，会将需要重传的数据包放到待发送队列，重新编号比如数据包Packet N+M 后重新发送给接收端，对重传数据包的处理跟发送新的数据包类似，这样就不会因为丢包重传将当前窗口阻塞在原地，从而解决了队头阻塞问题。



在每一个Package头会包含很多QUIC信息帧

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220915192139230.png" alt="image-20220915192139230" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220915192158567.png" alt="image-20220915192158567" style="zoom:50%;" />



- Stream ID 作用：多个并发传输的 HTTP 消息，通过不同的 Stream ID 加以区别，类似于 HTTP2 的 Stream ID；
- Offset 作用：类似于 TCP 协议中的 Seq 序号，**保证数据的顺序性和可靠性**，因为Package编号是单调的，无法确认哪个是重传的信息，因此，在QUIC帧中使用offset确认包的顺序。



**QUIC 通过单向递增的 Packet Number，配合 Stream ID 与 Offset 字段信息，可以支持乱序确认而不影响数据包的正确组装**，摆脱了TCP 必须按顺序确认应答 ACK 的限制，解决了 TCP 因某个数据包重传而阻塞后续所有待发送数据包的问题





## 8、QUIC 是如何解决 TCP 队头阻塞问题的？

TCP 队头阻塞的问题要从两个角度看，一个是**发送窗口的队头阻塞**，另外一个是**接收窗口的队头阻塞**。

发送窗口：

**如果某个数据报文丢失或者其对应的 ACK 报文在网络中丢失，会导致发送方无法移动发送窗口，这时就无法再发送新的数据**，只能超时重传这个数据报文，直到收到这个重传报文的 ACK，发送窗口才会移动，继续后面的发送行为。

接收窗口：

**当接收窗口收到的数据不是有序的，比如收到第 33～40 字节的数据，由于第 32 字节数据没有收到， 接收窗口无法向前滑动，那么即使先收到第 33～40 字节的数据，这些数据也无法被应用层读取的**。



QUIC 也借鉴 HTTP/2 里的 Stream 的概念，在一条 QUIC 连接上可以并发发送多个 HTTP 请求 (Stream)，

但是 **QUIC 给每一个 Stream 都分配了一个独立的滑动窗口，这样使得一个连接上的多个 Stream 之间没有依赖关系，都是相互独立的，各自控制的滑动窗口**。





## 9、QUIC 是如何做流量控制的？

TCP 流量控制是通过让「接收方」告诉「发送方」，它（接收方）的接收窗口有多大，从而让「发送方」根据「接收方」的实际接收能力控制发送的数据量。

QUIC 实现流量控制的方式：

- 通过 window_update 帧告诉对端自己可以接收的字节数，这样发送方就不会发送超过这个数量的数据。
- 通过 BlockFrame 告诉对端由于流量控制被阻塞了，无法发送数据。



QUIC 实现了两种级别的流量控制，分别为 Stream 和 Connection 两种级别：

- **Stream 级别的流量控制**：Stream 可以认为就是一条 HTTP 请求，每个 Stream 都有独立的滑动窗口，所以每个 Stream 都可以做流量控制，防止单个 Stream 消耗连接（Connection）的全部接收缓冲。
- **Connection 流量控制**：限制连接中所有 Stream 相加起来的总字节数，防止发送方超过连接的缓冲容量。



**Stream 级别的流量控制**

- TCP 的接收窗口只有在前面所有的 Segment 都接收的情况下才会移动左边界，当在前面还有字节未接收但收到后面字节的情况下，窗口也不会移动。
- QUIC 的接收窗口的左边界滑动条件取决于接收到的最大偏移字节数，接收窗口 = 最大窗口数 - 接收到的最大偏移数

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220915193136356.png" alt="image-20220915193136356" style="zoom:67%;" />

+ 当已收到并被读取的数据超过最大接收窗口的一半后，最大接收窗口向右移动，接收窗口的右边界也向右扩展，

  同时给对端发送「窗口更新帧」，当发送方收到接收方的窗口更新帧后，发送窗口的右边界也会往右扩展，以此达到窗口滑动的效果。

+ **如果中途丢失了数据包，导致已收到并被读取的数据没有超过最大接收窗口的一半，那接收窗口就无法滑动了**，这个只影响同一个 Stream，其他 Stream 是不会影响的，因为每个 Stream 都有各自的滑动窗口。



 **Connection 流量控制**

对于 Connection 级别的流量窗口，其接收窗口大小就是各个 Stream 接收窗口大小之和



## 10、QUIC 对拥塞控制的改进有哪些

QUIC 协议当前默认使用了 TCP 的 Cubic 拥塞控制算法（我们熟知的慢开始、拥塞避免、快重传、快恢复策略），同时也支持 CubicBytes、Reno、RenoBytes、BBR、PCC 等拥塞控制算法；



QUIC 是处于应用层的，应用程序层面就能实现不同的拥塞控制算法，不需要操作系统，不需要内核支持。这是一个飞跃，因为传统的 TCP 拥塞控制，必须要端到端的网络协议栈支持，才能实现控制效果。而内核和操作系统的部署成本非常高，升级周期很长，所以 TCP 拥塞控制算法迭代速度是很慢的。而 **QUIC 可以随浏览器更新，QUIC 的拥塞控制算法就可以有较快的迭代速度**



TCP 更改拥塞控制算法是对系统中所有应用都生效，无法根据不同应用设定不同的拥塞控制策略。但是因为 QUIC 处于应用层，所以就**可以针对不同的应用设置不同的拥塞控制算法**，这样灵活性就很高了



## 11、QUIC 如何建立更快的连接

TCP 和 TLS 是分层的，分别属于内核实现的传输层、openssl 库实现的表示层，因此它们难以合并在一起，需要分批次来握手，先 TCP 握手（1RTT），再 TLS 握手（2RTT）



**QUIC 内部包含了 TLS，它在自己的帧会携带 TLS 里的“记录”，再加上 QUIC 使用的是 TLS1.3，因此仅需 1 个 RTT 就可以「同时」完成建立连接与密钥协商，甚至在第二次连接的时候，应用数据包可以和 QUIC 握手信息（连接信息 + TLS 信息）一起发送，达到 0-RTT 的效果**



## 12、QUIC 是如何迁移连接的？

基于 TCP 传输协议的 HTTP 协议，由于是通过四元组（源 IP、源端口、目的 IP、目的端口）确定一条 TCP 连接。

**当移动设备的网络从 4G 切换到 WIFI 时，意味着 IP 地址变化了，那么就必须要断开连接，然后重新建立 TCP 连接**

而建立连接的过程包含 TCP 三次握手和 TLS 四次握手的时延，以及 TCP 慢启动的减速过程，给用户的感觉就是网络突然卡顿了一下，因此连接的迁移成本是很高的。



QUIC 协议没有用四元组的方式来“绑定”连接，而是通过**连接 ID**来标记通信的两个端点，客户端和服务器可以各自选择一组 ID 来标记自己，因此即使移动设备的网络变化后，导致 IP 地址变化了，只要仍保有上下文信息（比如连接 ID、TLS 密钥等），就可以“无缝”地复用原连接，消除重连的成本，没有丝毫卡顿感，达到了**连接迁移**的功能。



## 13、TCP 和 UDP 可以同时绑定相同的端口吗？

在数据链路层中，通过 MAC 地址来寻找局域网中的主机。

在网际层中，通过 IP 地址来寻找网络中互连的主机或路由器。

在传输层中，需要通过端口进行寻址，来识别同一计算机中同时通信的不同应用程序。



TCP可以绑定到某个端口，UDP也可以绑定到同一个端口，它们可以共享相同的端口号但使用不同的传输协议来处理传输数据。这允许应用程序根据自己的需求同时支持TCP和UDP连接。



## 14、多个 TCP 服务进程可以绑定同一个端口吗？

如果两个 TCP 服务进程同时绑定的 IP 地址和端口都相同，那么执行 bind() 时候就会出错，错误是“Address already in use”。



如果 TCP 服务进程 A 绑定的地址是 0.0.0.0 和端口 8888，而如果 TCP 服务进程 B 绑定的地址是 192.168.1.100 地址（或者其他地址）和端口 8888，那么执行 bind() 时候也会出错。

这是因为 0.0.0.0 地址比较特殊，代表任意地址，意味着绑定了 0.0.0.0 地址，相当于把主机上的所有 IP 地址都绑定了。



## 15、重启 TCP 服务进程时，为什么会有“Address in use”的报错信息？

当 TCP 服务进程重启时，服务端会出现 TIME_WAIT 状态的连接，

TIME_WAIT 状态的连接使用的 IP+PORT 仍然被认为是一个有效的 IP+PORT 组合，

相同机器上不能够在该 IP+PORT 组合上进行绑定，那么执行 bind() 函数的时候，就会返回了 Address already in use 的错误



## 16、重启 TCP 服务进程时，如何避免“Address in use”的报错信息？

可以在调用 bind 前，对 socket 设置 SO_REUSEADDR 属性，可以解决这个问题。

```c
int on = 1;
setsockopt(listenfd, SOL_SOCKET, SO_REUSEADDR, &on, sizeof(on));
```

因为 SO_REUSEADDR 作用是：**如果当前启动进程绑定的 IP+PORT 与处于TIME_WAIT 状态的连接占用的 IP+PORT 存在冲突，但是新启动的进程使用了 SO_REUSEADDR 选项，那么该进程就可以绑定成功**。



如果 TCP 服务进程 A 绑定的地址是 0.0.0.0 和端口 8888，而如果 TCP 服务进程 B 绑定的地址是 192.168.1.100 地址（或者其他地址）和端口 8888，那么执行 bind() 时候也会出错。

这个问题也可以由 SO_REUSEADDR 解决，因为它的**另外一个作用**：绑定的 IP地址 + 端口时，只要 IP 地址不是正好(exactly)相同，那么允许绑定。



## 17、客户端的端口可以重复使用吗？

TCP 连接是由四元组（源IP地址，源端口，目的IP地址，目的端口）唯一确认的，那么只要四元组中其中一个元素发生了变化，那么就表示不同的 TCP 连接的。





## 18、客户端如何执行端口

客户端是在调用 connect 函数的时候，由内核随机选取一个端口作为连接的端口。

而如果我们想自己指定连接的端口，就可以用 bind 函数来实现：客户端先通过 bind 函数绑定一个端口，然后调用 connect 函数就会跳过端口选择的过程了，转而使用 bind 时确定的端口。



## 18、客户端 bind 的端口可以复用吗？

如果同时bind的 IP 地址和端口都是相同的，那么执行 bind() 时候就会出错，错误是“Address already in use”，不能复用



## 19、客户端 TCP 连接 TIME_WAIT 状态过多，会导致端口资源耗尽而无法建立新的连接吗？

如果客户端都是与同一个服务器（目标地址和目标端口一样）建立连接，那么如果客户端 TIME_WAIT 状态的连接过多，当端口资源被耗尽，就无法与这个服务器再建立连接了。



但是，**因为只要客户端连接的服务器不同，端口资源可以重复使用的**。

所以，如果客户端都是与不同的服务器建立连接，即使客户端端口资源只有几万个， 客户端发起百万级连接也是没问题的



## 20、如何解决客户端 TCP 连接 TIME_WAIT 过多，导致无法与同一个服务器建立连接的问题？

打开 `net.ipv4.tcp_tw_reuse` 这个内核参数。

**因为开启了这个内核参数后，客户端调用 connect 函数时，如果选择到的端口，已经被相同四元组的连接占用的时候，就会判断该连接是否处于 TIME_WAIT 状态，如果该连接处于 TIME_WAIT 状态并且 TIME_WAIT 状态持续的时间超过了 1 秒，那么就会重用这个连接，然后就可以正常使用该端口了。**



## 21、没有 listen可以连接TCP吗？

**服务端如果只 bind 了 IP 地址和端口，而没有调用 listen 的话，当收到TCP连接请求时，服务端会回 RST 报文**



过程：

+ Linux 内核处理收到 TCP 报文后，会调用 __inet_lookup_skb 函数找到 TCP 报文所属 socket 。

+ __inet_lookup_skb 函数首先查找连接建立状态的socket，在没有命中的情况下，才会查找监听套接口，找不到对应的 socket，跳转到no_tcp_socket。

+ 检车收到的报文（skb）的「校验和」，没有问题的话内核发送 RST 中止这个连接



如果socket已经bind了IP和端口，并**调用connect方法**，内核会连接信息放入一个 hash 表中，当收到TCP连接信息，会从这个内核找到socket，正常进行TCP三次握手建立连接。这也是客户端不需要调用listen也能和服务端建立TCP连接的原因。



所以，没有listen的情况下，如果双方都调用connect方法的情况下，是可以建立TCP连接的；

当然，如果是TCP自连接，也可以建立TCP连接，原理同双方调用connext



## 22、没有 accept，能建立 TCP 连接吗？

对socket执行bind方法可以绑定监听端口，然后执行`listen方法`后，就会进入监听（`LISTEN`）状态。内核会为每一个处于`LISTEN`状态的`socket` 分配两个队列，分别叫**半连接队列和全连接队列**。



服务端收到**第一次握手**后，会将`sock`加入到这个队列中，队列内的`sock`都处于`SYN_RECV` 状态。

服务端收到**第三次握手**后，会将半连接队列的`sock`取出，放到全连接队列中。队列里的`sock`都处于 `ESTABLISHED`状态。这里面的连接，就**等着服务端执行accept()后被取出了。**



建立连接的过程中根本不需要`accept()`参与， **执行accept()只是为了从全连接队列里取出一条连接。**



## 23、为什么半连接队列要设计成哈希表

第三次握手来了，需要从半连接队列里把相应IP端口的连接取出，

**如果半连接队列还是个链表，那我们就需要依次遍历，才能拿到我们想要的那个连接**

如果将半连接队列设计成哈希表，那么查找半连接的算法复杂度就是`O(1)`



## 24、TCP 四次挥手，可以变成三次吗？

当被动关闭方在 TCP 挥手过程中，如果「没有数据要发送」，同时「没有开启 TCP_QUICKACK（默认情况就是没有开启，没有开启 TCP_QUICKACK，等于就是在使用 TCP 延迟确认机制）」，那么第二和第三次挥手就会合并传输，这样就出现了三次挥手。

**所以，出现三次挥手现象，是因为 TCP 延迟确认机制导致的。**



## 25、什么是TCP延迟确认

当发送没有携带数据的 ACK，它的网络效率也是很低的，因为它也有 40 个字节的 IP 头 和 TCP 头，但却没有携带数据报文。 为了解决 ACK 传输效率低问题，所以就衍生出了 **TCP 延迟确认**。 

TCP 延迟确认的策略：

- 当有响应数据要发送时，ACK 会随着响应数据一起立刻发送给对方
- 当没有响应数据要发送时，ACK 将会延迟一段时间，以等待是否有响应数据可以一起发送
- 如果在延迟等待发送 ACK 期间，对方的第二个数据报文又到达了，这时就会立刻发送 ACK



## 26、常见数据丢包

> 建立连接时丢包

如果全连接队列溢出，或者半连接队列溢出且没有启 syncookies，连接数据会被丢弃；



> 流量控制丢包

数据按一定的规则排个队依次处理，也就是所谓的**qdisc**(**Q**ueueing **Disc**iplines，排队规则)，这也是我们常说的**流量控制**机制

当发送数据过快，流控队列长度`txqueuelen`又不够大时，就容易出现**丢包**现象。

> 网卡丢包

**RingBuffer过小导致丢包**

在接收数据时，会将数据暂存到RingBuffer接收缓冲区中，然后等着内核触发软中断慢慢收走。如果这个缓冲区过小，而这时候发送的数据又过快，就有可能发生溢出，此时也会产生丢包

**网卡性能不足**

网卡作为硬件，传输速度是有上限的。当网络传输速度过大，达到网卡上限时，就会发生丢包。这种情况一般常见于压测场景。

> 接收缓冲区丢包

使用`TCP socket`进行网络编程的时候，内核都会分配一个**发送缓冲区**和一个**接收缓冲区**。

当我们想要发一个数据包，会在代码里执行`send(msg)`，这时候数据包并不是一把梭直接就走网卡飞出去的。而是将数据拷贝到内核**发送缓冲区**就完事**返回**了，至于**什么时候发数据，发多少数据**，这个后续由内核自己做决定。

**缓冲区的大小会在设定的会在min和max之间动态调整**



如果缓冲区设置过小，执行send的时候，如果是**阻塞**调用，那就会等，等到缓冲区有空位可以发数据。如果是**非阻塞**调用，就会**立刻返回**一个 `EAGAIN` 错误信息，意思是 `Try again`。让应用程序下次再重试。这种情况下一般不会发生丢包。

当接受缓冲区满了，它的TCP接收窗口会变为0，也就是所谓的**零窗口**，并且会通过数据包里的`win=0`，告诉发送端。

一般这种情况下，发送端就该停止发消息了，但如果这时候确实还有数据发来，就会发生**丢包**。



> 两端之间的网络丢包

两端之间的链路都属于外部网络，这中间有各种路由器和交换机还有光缆啥的，丢包也是很经常发生的。



## 27、如何查看两端之间的丢包

**ping命令查看丢包**

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220916164918634.png" alt="image-20220916164918634" style="zoom:67%;" />

倒数第二行里有个100% packet loss，意思是丢包率100%。

只能知道发送机器和目的机器之间有没有丢包。



**mtr命令**

![image-20220916165005633](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220916165005633.png)

 -r 是指report，以报告的形式打印结果

`Host`那一列，出现的都是链路中间每一跳的机器，`Loss`的那一列就是指这一跳对应的丢包率。

有一些是host是`???`，那个是因为**mtr默认用的是ICMP包**，有些节点限制了**ICMP包**，导致不能正常展示。

在mtr命令里加个`-u`，也就是使用**udp包**，就能看到部分???对应的IP

`Loss`列，在icmp的场景下，关注**最后一行**，如果是0%，那不管前面loss是100%还是80%都无所谓，都是**节点限制**导致的**虚报**

如果**最后一行是20%，再往前几行都是20%左右**，那说明丢包就是从最接近的那一行开始产生的，长时间是这样，那很可能这一跳出了点问题。



## 28、用了TCP协议就一定不会丢包吗

TCP保证的可靠性，是**传输层的可靠性**。

也就是说，**TCP只保证数据从A机器的传输层可靠地发到B机器的传输层。**

在应用层仍然可能导致数据丢包