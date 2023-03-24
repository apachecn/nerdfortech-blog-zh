# 使用 Tshark 为流量监控设置代理服务器

> 原文：<https://medium.com/nerd-for-tech/setting-up-a-proxy-server-for-traffic-monitoring-with-tshark-97719307c41a?source=collection_archive---------1----------------------->

在一些国家，根据互联网的使用有很多限制，限制网站，禁止网络服务，或任何类型的内部审查。大多数时候，人们呼吁匿名服务绕过这个障碍，一个常见的因素是网络代理。

一个 **Web** 或 **HTTP 代理**是一个在两个通信(客户端-服务器)之间充当**中间人的服务器，以某种方式，这个**代理会通过给你一个**替代互联网身份**来屏蔽你的 IP 地址**。**

代理不仅仅用于隐私，这个工具有助于绕过过滤器、日志记录、窃听、缓存、过滤阻塞、操纵和修改网络流量等等。

我将在本文中强调网络流量。你有没有想过如果你使用免费的代理服务会发生什么？你的隐私能得到保障？。

有很多付费工具是可以信任的，但是很多人不为这项服务付费。代理的个人或群组控制者可以看到你的流量，尤其是如果是免费代理的话。

# 它是如何工作的？

外面有很多代理人。我将简要解释代理的类型及其工作原理。

## 转发代理

对于这种类型，客户端向代理发送请求，并代表代理获取响应。其中客户端可以知道也可以不知道代理，但是服务器根本不知道代理

顺序应该是这样的:

—客户端= >代理(互联网)<= Server

## Reverse Proxies

This is used to hide behind network architecture for distributing load traffic to the servers. The connections are made to the proxy instead of the servers, and then, the same proxy will handle the request.

It’s some like this:

​ — Client =>代理(互联网)= >服务器

## 透明代理

这种代理在没有任何客户端配置的情况下不会修改任何信息。这通常用于 ISP，以获得更快的响应。

—客户端= >代理(互联网)= >服务器

## 太好了，我们如何连接到代理服务器？

这更多的是一个浏览器设置，而不是做任何技术。让我们在这个场景中使用 **Firefox**

打开**火狐**浏览器- >设置- >进阶网络- >手动代理设置

您将在这里添加由**代理服务**提供的 IP 地址和端口

此时，我们已经准备好开始使用一个**代理**，但是这里还缺少一些东西，我们需要安装和配置一个代理服务器。我会很快解释清楚，因为稍后，我会详细解释。

为此，我推荐使用基于 Linux 的操作系统(最好是 Debian)。我们将安装`proxychains`，这个工具强制任何应用程序建立的任何 TCP 连接都通过一个代理。你可以在这里[查看](https://github.com/haad/proxychains)

## 安装

就`apt-get install proxychains`

## 检查配置

检查一下`/etc/proxychains.conf`

我们必须**注释**选项`strict_chain`，基本上这意味着如果这个列表的一个代理关闭，整个链将无法使用。

在此之后，您可以**取消**对`dynamic_chain`选项的注释，不管所有代理是否可用，链都将工作，死代理将被跳过。

## 在默认情况下，**代理列表**会使用 **Tor 网络**，以防你为了匿名而不在**代理列表**上放东西。

## 怎么用？

作为非超级用户，请执行以下操作:

`proxychains4 firefox`

让这个终端会话保持打开，以便保持连接，并且正确放置 **Firefox** 设置，检查你的公共 IP，网络速度可能会慢一些，但是你这样做会绕过一些隐私控制。

# 但是，如何建立自己的代理服务器呢？

我们将生成一个基于云的 Debian 机器，将安装一个名为 **Squid3** 的代理服务器，用于处理个人流量，演示如何使用 **Wireshark** 和 **Tshark 网络分析器**监控和轻松了解您的目的地和数据包详情。

我们开始吧。

## 带有 DigitalOcean 的云实例服务器(可以是另一个云提供商)

登录您的提供商并创建一个 Droplet(数字海洋)机器实例，其中包含 Debian 9 操作系统。

1.  选择一个 5 美元的计划
2.  选择数据中心所在的国家

## 生成 SSH 密钥并阻止 root 访问(最低安全控制)

1.  生成用于访问服务器的 SSH 密钥，执行下面的`ssh-keygen -t rsa`，一旦完成，您将在`.ssh`目录中为登录的用户名创建一个名为`id_rsa.pub`的文件。
2.  通过执行`cat id_rsa.pub`将生成的密钥复制到**数字海洋**实例中
3.  像这样通过 SSH 访问:`ssh root@IP`其中“IP”将是由 **DigitalOcean** 提供的 IP 地址实例。最后一个用户将发送一封带有 root 密码的电子邮件
4.  添加其他用户**(出于安全目的)**通过`adduser david`使用`passwd`命令设置密码
5.  更改`sshd`配置文件`/etc/ssh/sshd_config`上的认证方法，将`PermitRootLogin`设置为`no`，这将只允许用户访问 SSH。
6.  将 root authorized_keys 所有权复制并生成给同一个用户`david`这将禁止 root 访问服务器。

## 安装和配置代理服务器

1.  正在安装 squid3(代理服务器):`apt-get install squid3`
2.  备份配置文件`cp /etc/squid3/squid.conf /etc/squid3/squid.conf.original`
3.  更改备份权限`chmod -w /etc/squid3/squid.conf.original`(每个人都可以读取文件，但不能编辑)
4.  在 conf 文件中:`/etc/squid3/squid.conf`你可以改变 TCP 端口，默认是 3128
5.  在启动服务器之前，我们需要一个防火墙漏洞，你可以用`nmap`工具检查端口，方法是:`nmap [IP address]`
6.  通过执行`systemctl status squid3.service`检查 squid3 服务，如有必要，启动。
7.  像`http [IP address] 3128`一样在`/etc/proxychains.conf`上设置 **Squid3** 代理服务器，其中 **3128** 是**代理服务**提供的默认端口

如果使用 Nmap 扫描，squid3 服务的 3128 端口将不在服务器上

## 在 squid3 配置文件中

1.  取消注释:`http_access deny !Safe_ports`这是为了禁止端口，如果它不在端口 80 上，连接将会丢失。
2.  `http_access allow all`(默认为拒绝全部)，通过将此设置为允许，我们必须对代理中所做的事情负责。这是针对蜜罐代理服务器的
3.  更改后重新启动 squid 3 . service:`systemctl restart squid3.service`
4.  确保所有浏览器实例都已关闭
5.  使用`proxychains4 firefox`触发代理链 firefox

## 嗅探您的代理服务器

在同一个服务器实例上，我们通过安装 **Tshark** 开始嗅探

使用`apt-get install tshark`进行安装

通过 **TCP** 在 **3128 端口** : `tshark -i eth0 -f "tcp port 3128"`嗅探 **eth0** 接口

为了验证，您可以测试 **DNSleak 测试**和**公共 IP 地址**

## 设置从服务器到本地机器的管道

我们将通过 SSH 将流量传输到机器，并通过监控将其推送到 Wireshark

1.  在**数字海洋**实例上，只需安装`apt-get install tshark wireshark`(如有必要)
2.  设置 Wireshark 组中的用户(不能以 root 用户身份运行 Wireshark):`usermod -a -G wireshark david`
3.  `newgrp`命令配置用户登录的组成员。`newgrp wireshark`。这将让普通用户运行`tshark`命令
4.  在主机上。我们将通过管道输出嵌入的**t shark**文件。这是研讨会的棘手之处。

`wireshark -k -i <(ssh david@[IP DO instance] "tshark -F pcap -w - -f 'not tcp port 22'")`

其中:

1.  -i(接口)不需要网络接口，在这种情况下，是实时流信息->输出重定向
2.  该命令指定了 a -F `pcap`(默认格式为`tshark`)
3.  -w —在标准输出上写入

这将提示 Wireshark(以前安装的)的 GUI，并将嗅探传入的流量

您还可以像这样在 WireShark GUI 上过滤传入的流量

`tcp.port == 3128 && ip.src ==[IP DO machine] || tcp.port == 80 || tcp.port == 443`

该 Wireshark 过滤器应该显示来自 HTTP 端口和 TCP 代理端口的流量

## 我们能从中学到什么？

1.  控制代理服务器的人可以看到你访问的所有网站，你要去的所有地方，记录你的交通信息，等等。
2.  在 **3128 端口**上的大多数源 IP 地址将是您的家庭路由器，有人可以查看您的路由器并访问它，他们可以做很多事情，如更改 DNS 服务器，监控所有 URL 请求，您的 pc 可以放在路由器的 DMZ 上，等等
3.  你不容易看到加密的数据。
4.  匿名是相对的