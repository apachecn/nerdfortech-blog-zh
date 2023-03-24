# Debian 系列:网络工具

> 原文：<https://medium.com/nerd-for-tech/the-debian-series-network-tools-ebcf21f47b0c?source=collection_archive---------3----------------------->

对于这最后一个故事，我将简单谈谈基本的网络工具和网络适配器。我想从这个特殊的故事中重温网络命令，以及简短的描述性解释和孤立的例子。

使用的所有工具命令都是由 Debian OS 提供的，不需要额外安装，当然也不需要配置，我们几乎不需要一个已经建立的互联网连接。

第一个命令是`ping`，这个工具帮助你检测到一个特定服务器的连接，就是这样。在幕后，`ping`命令向远程机器的目标发送网络数据包，执行的输出是一个格式化的消息，其中包含有关已建立连接的某些细节。

这里有一个对本地网络进行`ping`以检查 TCP/IP 协议健康状况的快速示例，但也可以用来查看从网络角度来看计算机是否可达

`ping 192.168.50.201`

`ping -c 3 192.168.1.1` (ping 网关)，`-c`标志表示要发送的数据包数量

让我们转到`arp`命令，这个命令获取包含 IP 地址的网络适配器的 MAC 地址。

这可以通过`arp -a`完成

`traceroute`命令在两台机器(本地—服务器)之间生成网络路由路径，并跟踪每一步。换句话说，`traceroute`命令是本地机器在节点、路由器、机器等之间“旅行”到指定的 IP 地址(目标)。

它决定了机器获取目标信息所需的路线和流量

`traceroute 192.168.1.1`

下一个是老掉牙的，`telnet`命令通过端口`21`建立到服务器的远程连接，几乎不用于测试服务器端口状态。通过 telnet 的通信是加密的，但在很多年后，“加密强度”被 infosec 的专业人员和黑客打破了。

您可以使用它，但不再推荐使用。

这里有一个例子:

`telnet ftp.microsoft.com 21`

`netstat`显示我的本地机器的开放和活动端口

`netstat -a`或`netstat -ntl`(检查该男子，以便更好地了解)。

当然还有显示网络适配器信息的`ifconfig`命令。`ifconfig`或`ifconfig eth0`(或任何可用的接口)

我们还可以(也将会这样做)，改变`eth0`接口的默认值，为了澄清事情，`eth0`是一个物理网络适配器，它让你连接到互联网，周围还有很多适配器，另一个值得一提的是`lo`适配器，称为**环回适配器**，用于本地测试。

让我们更改某个网络适配器的 IP。我们只是。

`ifconfig eth0 192.168.50.205/24 up`

我们可以看到这样的详细信息:

`ifconfig eth0`

让我们使用另一个名为`route`的命令来添加网关

`route add default gw 192.168.1.1`

route 命令修改 IP 路由表，根据 Debian 的[手册页](http://Its primary use is to set up static routes to specific hosts or networks via an interface after it has been configured with the ifconfig(8) program.)该命令“它的主要用途是在用`[ifconfig](https://manpages.debian.org/stretch/net-tools/ifconfig.8.en.html)`程序配置后，通过接口建立到特定主机或网络的静态路由。”

对于最近的实例(特别是 Debian 9 ),事情以不同的方式进行。幸运的是，我们可以安装一个包来使用这个`route`和`ifconfig`，就像旧版本一样

如果不重新启动并保持，将不会有有效的更改。为此。我们需要关闭接口，然后生成持久配置。我们可以再次设置接口并进行测试。

很好，让我们做最后一个例子。

```
ifconfig eth0 down
```

现在，让我们在`/etc/networks/interfaces`文件上保存配置

```
auto eth0
iface eth0 inet static 
address 192.168.50.210
netmask 255.255.255.0
gateway 192.168.1.1
```

让我们来消化一下每一行，`auto [interface]`在引导阶段启动界面。`iface`代表**接口族**,`inet`参数定义了一个 **IP 协议版本**，这里定义的 IP 是`static`，但是也可以通过`scripts`或者`dhcp`生成。

`address`、`netmask`和`gateway`是接口值。

一旦输入正确，我们需要像这样重新启动服务:

```
service networking restart
```

确保接口是打开的，应该就是这样。您可以再次使用`ifconfig`来验证生成的设置

当然，还有更复杂的事情要做，这个故事(以及整个 Debian 系列)是关于解释服务器管理或普通 Debian 或 UNIX 用户的每一个关键部分的小步骤。

就是这样。

更多即将推出。

快乐编码:)