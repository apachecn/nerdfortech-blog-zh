# 什么是负载平衡器？

> 原文：<https://medium.com/nerd-for-tech/what-is-a-load-balancer-9af0517551dc?source=collection_archive---------3----------------------->

![](img/ac51ffa715701a186240353125d3c95f.png)

[廷杰伤害律师事务所](https://unsplash.com/@tingeyinjurylawfirm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

T 这篇文章探讨了系统设计的一个重要主题，负载平衡器。让我们自己熟悉系统设计的基本概念和术语将极大地帮助我们有效地设计一个更加可伸缩和健壮的系统。

负载平衡是在多台服务器之间分配网络流量的过程。这确保了没有一台服务器承担过多的工作负载。通过平均分配工作，负载平衡提高了应用程序的响应能力。它还增加了应用程序和网站对用户的可用性。没有负载平衡器，现代应用程序就无法运行。

负载平衡器管理服务器和终端设备(PC、笔记本电脑、平板电脑或智能手机)之间的信息流。服务器可以是本地的，在数据中心或公共云中。服务器也可以是物理的或虚拟的。负载平衡器帮助服务器高效地移动数据，并防止服务器过载。负载平衡器对服务器进行连续的健康检查，以确保它们能够处理请求。如有必要，负载平衡器还可以从池中删除不健康的服务器，直到它们被恢复。一些负载平衡器甚至会触发新的虚拟化应用服务器的创建，以应对不断增长的需求。

## 硬件与软件负载平衡器

负载平衡器通常有两种类型:基于硬件的和基于软件的。基于硬件的解决方案的供应商将专有软件加载到他们提供的机器上，这些机器通常使用专门的处理器。为了应对网站日益增长的流量，你必须从供应商那里购买更多或更大的机器。软件解决方案通常运行在商用硬件上，使它们更便宜、更灵活。您可以将软件安装在自己选择的硬件上，或者安装在 AWS EC2 这样的云环境中。

## 软件负载平衡器

优势:

*   根据不断变化的需求进行调整的灵活性。
*   通过添加更多软件实例来扩展超出初始容量的能力。
*   比购买和维护物理机器的成本更低。软件可以在任何标准设备上运行，这往往更便宜。
*   允许云中的负载平衡，这提供了一个可管理的异地解决方案，可以从弹性的服务器网络中获取资源。云计算还允许混合托管和内部解决方案的灵活性。主负载平衡器可以是内部的，而备份是云负载平衡器。

缺点:

*   当扩展超过初始容量时，配置负载平衡器软件可能会有一些延迟。
*   升级的持续成本。

## 硬件负载平衡器

优势:

*   由于软件运行在专用处理器上，因此吞吐量很高。
*   提高安全性，因为只有组织可以物理访问服务器。
*   购买后的固定成本。

缺点:

*   需要更多的员工和专业知识来配置和编程物理机。
*   当达到设定的连接数限制时，无法扩展。在购买和安装额外的机器之前，连接被拒绝或服务被降级。
*   购买和维护物理网络负载平衡器的成本更高。拥有一个硬件负载平衡器可能还需要花钱请顾问来管理它。

## 动态负载平衡算法

*   *最少连接:*检查哪些服务器当时打开的连接最少，并向这些服务器发送流量。这假设所有连接需要大致相等的处理能力。
*   *加权最少连接:*让管理员能够为每台服务器分配不同的权重，假设一些服务器可以比其他服务器处理更多的连接。
*   *加权响应时间:*平均每个服务器的响应时间，并将其与每个服务器打开的连接数量相结合，以确定将流量发送到哪里。通过以最快的响应时间向服务器发送流量，该算法确保为用户提供更快的服务。
*   *基于资源:*根据每个服务器当时可用的资源来分配负载。运行在每台服务器上的专用软件(称为“代理”)测量该服务器的可用 CPU 和内存，负载平衡器在向该服务器分发流量之前查询代理。

## 静态负载平衡算法

*   *循环调度:*循环调度负载平衡使用域名系统(DNS)轮流将流量分配给一系列服务器。权威的域名服务器将拥有一个域的不同 A 记录的列表，并在响应每个 DNS 查询时提供不同的 A 记录。
*   *加权循环法:*允许管理员为每台服务器分配不同的权重。被认为能够处理更多流量的服务器将会收到稍微多一点的流量。可以在 DNS 记录中配置权重。
*   *IP 哈希:*结合传入流量的源和目标 IP 地址，并使用数学函数将其转换为哈希。根据哈希，连接被分配给特定的服务器。

理解负载平衡的概念在学习系统设计中至关重要。在以后的文章中，我们将讨论这些负载平衡算法是如何实现的。