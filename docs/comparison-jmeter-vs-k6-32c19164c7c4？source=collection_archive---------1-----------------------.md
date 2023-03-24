# 对比:Jmeter vs K6

> 原文：<https://medium.com/nerd-for-tech/comparison-jmeter-vs-k6-32c19164c7c4?source=collection_archive---------1----------------------->

只是考虑检查你的其他选择，看看什么是最适合你的项目。Jmeter 是一个神奇而强大的工具，然而，它可能是一个过于复杂、缓慢、难以维护的工具，这取决于您的需求(更轻的东西)。

# 写于

1.  Jmeter: Java
2.  去吧

# 脚本语言

1.  Jmeter:有限:一些 Java (Groovy、Beanshell 等)
2.  K6: Javascript

# **内置协议支持**

1.  Jmeter:通过插件支持大多数协议(通过 JMS、SMTP、POP3、IMAP、shell 脚本、TCP、Java 对象对 HTTP/1.1、FTP、JDBC、LDAP、MOM 的本地支持)
2.  K6:本机支持更少的现代协议(HTTP/1.1、HTTP/2、WebSockets、gRPC)

# 外部依赖性

1.  Jmeter: Java。许多功能都需要插件，但是有很多可用的插件。
2.  K6:不需要。大多数特性都是本机支持的，但插件支持是新的，可用性很低

# 资源利用

1.  Jmeter:差；一个负载生成器可以模拟几千个虚拟用户
2.  K6:很好；一个负载生成器可以模拟成千上万的虚拟用户

# 资源利用

1.  Jmeter:差；一个负载生成器可以模拟几千个虚拟用户。在一台机器上运行多个用户的测试非常繁重，会消耗更多的内存。[https://azevodorafaela . files . WordPress . com/2020/04/jmeterlocust 2 . png](https://azevedorafaela.files.wordpress.com/2020/04/jmeterlocust2.png)
2.  K6:很好；一个负载生成器可以模拟数万个虚拟用户。重量轻，不会占用你的机器太多的内存。[https://azevodorafaela . files . WordPress . com/2020/07/截图-2020-07-06-at-23.34.47.png](https://azevedorafaela.files.wordpress.com/2020/07/screenshot-2020-07-06-at-23.34.47.png)

# **速度写测试**

1.  Jmeter:慢
2.  K6:快

# **支持“按代码测试”**

1.  Jmeter:面向 GUI，可以创建脚本，但是，它们太复杂并且缺乏文档，维护非常困难，很弱(Java)。[GUI 驱动，带代码块]
2.  K6:面向脚本，JavaScript，更容易维护。【代码驱动；VSCode 插件]

# 脚本格式

1.  Jmeter: XML
2.  K6: javascript

# **上升灵活性**

1.  Jmeter:能够配置灵活负载的插件
2.  K6:支持上升阶段和灵活负载

# **测试结果分析**

1.  Jmeter:支持。默认和自定义 HTML 报告；通过监听器记录日志
2.  K6:支架。没有内置的预生成报告；与第三方仪表板的分析工具集成

# 线程模型

1.  Jmeter: 1 个线程:1 个虚拟用户；性能降低，资源成本增加
2.  K6: 1 Goroutine: 1 虚拟用户；更快的性能，更低的资源成本

# **易于与版本控制系统一起使用**

1.  杰姆特:没有
2.  K6:是的

# **并发用户数量**

1.  Jmeter:成千上万，有限制
2.  K6:成千上万

# 测试水平阈值

1.  Jmeter:不，仅个人请求
2.  K6:是的

# **录制功能**

1.  杰姆特:是的
2.  k6:不，但是它允许通过 HAR 文件自动生成 K6 脚本。

# **分布式执行**

1.  杰姆特:是的
2.  K6:是的

# **负载测试监控**

1.  Jmeter:添加监听器，但是消耗更多内存
2.  K6:不，通过控制台和付费版本登录以获得 K6 云报告，但您可以将指标发送到其他平台

# 合作

1.  Jmeter:难以同时工作；测试人员友好；需要 GUI 应用程序来编辑
2.  K6:开发者友好，易于版本化；Javascript 格式促进协作

# 维护

1.  Jmeter:冗长的脚本；XML 格式很难阅读
2.  K6:更简洁的脚本；JavaScript 很容易阅读

# 本地分布式负载生成

1.  杰姆特:是的
2.  K6:否(仅限高级版)

# 社区

1.  Jmeter:自 1998 年以来；很多第三方教程；大量文件；没有中心社区
2.  2017 年至今；大量文件；第三方教程较少；官方社区

# Jmeter 用于以下情况:

1.  你必须执行一个包含几种方法的复杂负载
2.  可以输入场景
3.  强大的生态系统支持和教育
4.  要求每个测试都有完整的场景
5.  如果需要使用特定的斜升设计来模拟特定的负载
6.  如果你更喜欢 UI 桌面应用或者对 Javascript/YAML/JSON 不够了解。

# K6 解决了一些特定的问题:

*   开发友好的 APIs CLI 工具。
*   您可以使用 HAR 文件生成记录会话。
*   目标测试、自动负载测试检查和阈值
*   开源、优秀的文档和支持
*   轻量级 Javascript 的使用
*   不要在 NodeJS 中运行或在浏览器中运行

参考—[https://k6.io/blog/k6-vs-jmeter/](https://k6.io/blog/k6-vs-jmeter/)