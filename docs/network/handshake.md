![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210501161803.png)

## 三次握手

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210501162817.png)

### 为什么 TCP 需要三次握手

- 从通信理论来看，只有三次双方才能明确自己和对方的收、发能力是正常的
- 为了防止失效的连接请求报文段被服务端接收，从而产生错误。

## 四次挥手

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210501163521.png)

### 为什么需要四次挥手

关闭连接时，当收到对方的 FIN 报文时，仅仅表示对方不再发送数据了但是还能接收数据，己方是否现在关闭发送数据通道，需要上层应用来决定，因此，己方 ACK 和 FIN 一般都会分开发送。

### 为什么要等待 2MSL（Maximum Segment Lifetime）

为了确认对方已收到 ACK 报文/重发可能丢失的 ACK 报文

## 参考

[https://zhuanlan.zhihu.com/p/53374516](https://zhuanlan.zhihu.com/p/53374516)
[https://hit-alibaba.github.io/interview/basic/network/TCP.html](https://hit-alibaba.github.io/interview/basic/network/TCP.html)
