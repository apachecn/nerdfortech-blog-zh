# 谈谈 Go:网络编程— TCP 通信

> 原文：<https://medium.com/nerd-for-tech/talk-about-go-network-programming-tcp-communication-e579fe789626?source=collection_archive---------1----------------------->

在网络分层的七层协议中，我们知道 TCP 在 HTTP 层的下面。本质上，HTTP 数据包主体解析是基于底层 TCP 连接建立的。

![](img/62192102b2ee7f2a3e813a77e33e20a5.png)

照片由[约恩·博耶](https://unsplash.com/@yoannboyer?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# TCP 协议摘要

**TCP 连接 Distinct** :计算机之间网络连接的建立，也称为握手，本质上是两个文件句柄的关联，即 fd，每个网络连接由四个属性唯一标识:`<source IP, source port, destination IP, destination port >`，所以一台机器的连接数受文件句柄`ulimit`的限制。
**OS 套接字**:操作系统套接字作为端点，用于在服务器端和客户端程序之间建立双向网络通信链接。

# 不同情况下保持活动的说明

*   **HTTP keepalive:**众所周知，HTTP 连接是无状态的。通常情况下，连接用完就被破坏了。开启 keepalive 可以告诉它保持一段时间的连接，避免频繁的连接重建。
*   **TCP keepalive:**

> 许多现有的 TCP 协议通过定义某种心跳机制来支持这种错误处理方式，该机制要求每个端点定期发送 PING/PONG 探测，以便检测网络问题以及服务健康状况。

# Linux 网络参数

在 Linux 机器上，可以通过以下网络参数设置 TCP keepalive 机制:

以上是默认设置，这意味着**初始连接创建将在两小时后每 75 秒重新发送一次(7200 秒)。如果连续 9 次没有收到 ACK 响应，则连接被标记为断开。**

# 代码演示

在 Go native net 包中，可以使用 TCP 连接的保活机制设置以下功能:

*   `func (c *TCPConn) SetKeepAlive(keepalive bool) error`
    是否启用连接检测
*   `func (c *TCPConn) SetKeepAlivePeriod(d time.Duration) error`
    连接检测间隔，默认情况下会使用操作系统参数设置

接下来我们先用一个 TCP 连接演示来交互，然后下次用连接池来集中管理 TCP 连接。

**传输结构**
我们为客户端和服务器端的交互定义了两种结构，传输协议由 **json** 演示

**服务器端**

**客户端**

**定义连接的选项**

```
type Option struct {
	addr        string
	size        int
	readTimeout time.Duration
	dialTimeout time.Duration
	keepAlive   time.Duration
}
```

然后创建如下连接代码:

**异步接收结果**
`receiveResp()`函数主要执行异步轮询，有几个函数:

*   意识到上下文关闭，通常会执行连接的 cancel()
*   从服务器接收数据并写入结果通道`retChan`，其类型是并发安全的`sync.Map`
*   监听服务器错误并关闭异常连接

**发送请求**

```
func (c *Conn) Send(ctx context.Context, msg *body.Message) (ch chan string, err error) {
	ch = make(chan string)
	c.retChan.Store(msg.Uid, ch)
	js, _ := json.Marshal(msg) _, err = c.writer.Write(js)
	if err != nil {
		return
	} err = c.writer.Flush()
	// the connection is not closed, could be put into the connection pool later
	//c.tcp.CloseWrite()
	return
}
```

# 跑步步骤

1.  开始服务器端监听:

```
=== RUN   TestListenAndServer
2021/05/10 16:58:20 Start server...
```

2.提出请求:

```
var OPT = &Option{
	addr:        "0.0.0.0:3000",
	size:        3,
	readTimeout: 3 * time.Second,
	dialTimeout: 3 * time.Second,
	keepAlive:   1 * time.Second,
}func createConn(opt *Option) *Conn {
	c, err := NewConn(opt)
	if err != nil {
		panic(err)
	}
	return c
}func TestSendMsg(t *testing.T) {
	c := createConn(OPT)
	msg := &body.Message{Uid: "pixel-1", Val: "pixelpig!"}
	rec, err := c.Send(context.Background(), msg)
	if err != nil {
		t.Error(err)
	} else {
		t.Logf("rec1: %+v", <-rec)
	} msg.Val = "another pig!"
	rec2, err := c.Send(context.Background(), msg)
	if err != nil {
		t.Error(err)
	} else {
		t.Logf("rec2: %+v", <-rec2)
	}
	t.Log("finished")
}
```

3.客户端输出:

```
=== RUN   TestSendMsg
    TestSendMsg: conn_test.go:56: rec1: : pixelpig!
    TestSendMsg: conn_test.go:64: rec2: : another pig!
    TestSendMsg: conn_test.go:66: finished
--- PASS: TestSendMsg (9.94s)
PASS
```

# 超时和池化

以上是比较简单的点对点交互。实际上，连接交互超时也可以在后面考虑:

1.  虽然连接结果是异步响应，但我们有必要让响应超时，以防止单个连接继续阻塞。
2.  我们需要考虑重用，即将健康的连接放入连接池中进行管理。

**超时判断**
判断超时的方法有很多种，比较常见的是用一个`select{}`挡和`time.After()`。让我们来看看常见的实现:

```
rec3, err := c.Send(context.Background(), msg)
if err == nil {
	select {
	case resp := <-rec3:
		t.Logf("rec3: %+v", resp)
		return
	case <-time.After(time.Second * 1):
		t.Error("Wait for resp timeout!")
		return
	}
} else {
	t.Error(err)
}
```

**例如:**

```
=== RUN   TestSendMsg
    TestSendMsg: conn_test.go:56: rec1: : pixelpig!
    TestSendMsg: conn_test.go:76: Wait for resp timeout!
--- FAIL: TestSendMsg (17.99s)
FAIL
```

**池管理**
这里要考虑的情况稍微复杂一点。你可以先列出困难，然后一个一个地打破它们:

1.  池中的最大连接数
2.  更新空闲连接数
3.  连接获取和返回
4.  连接关闭

连接池操作的长度可能比较长，详细解释在本系列的下一部分有所描述: [**说说 Go: TCP 连接池管理**](/@pixelpig/talk-about-go-network-programming-tcp-connection-management-f7630a526e17)

# 参考链接

**关于 TCP keepalive 在 Go 中的注意事项**
[https://thenotexpert.com/golang-tcp-keepalive/](https://thenotexpert.com/golang-tcp-keepalive/)
**在 Linux 下使用 TCP Keepalive**
[https://tldp . org/how to/TCP-Keepalive-how to/Using Keepalive . html](https://tldp.org/HOWTO/TCP-Keepalive-HOWTO/usingkeepalive.html)