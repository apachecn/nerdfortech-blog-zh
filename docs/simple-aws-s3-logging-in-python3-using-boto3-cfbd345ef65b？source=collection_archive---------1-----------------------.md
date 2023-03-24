# 使用 Boto3 在 Python3 中进行简单的 AWS S3 日志记录

> 原文：<https://medium.com/nerd-for-tech/simple-aws-s3-logging-in-python3-using-boto3-cfbd345ef65b?source=collection_archive---------1----------------------->

![](img/71d4c5548b0d8b2be740175cd698df9a.png)

斯文·舍尔梅尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 用 Python3 实现 S3 日志的简单方法

软件框架的一个关键方面是记录必要的信息并保存它们。使用 boto3 和本地 python logger，很容易将日志保存到 python 程序内的 AWS S3 中。

日志将有助于调试、监控、审计和理解框架在任何给定时间点的行为。当谈到云中的应用程序时，我们不会将重要信息存储在本地机器上，而是存储在像 AWS S3 这样的云存储服务中。

最近，我一直在研究一个使用 python 来自动化大数据 ETL 部署的框架。要求是将日志保存到 S3 以备将来参考，而不需要使用复杂的代码或做太多的代码更改。要求是不能在 S3 实时记录日志。

我的意图是利用我一直用于所有 python 代码的现有 Python 日志记录器和 Boto3 SDK 与 AWS S3 进行交互。这里有一个将 Python logger 生成的日志存储到 S3 的简单方法。

# 方法

**创建字符串输入/输出记录器:**

Python logger 允许我们添加不同种类的处理程序。我们可以利用它来添加一个字符串 I/O 流处理程序，它基本上是一个字符串缓冲区，可以存储 logger.info 或 logger.error 函数生成的所有日志。

**把内容放到 S3:**

Boto3 是一个 python SDK，用于与 AWS 服务进行交互。在这里，我创建了一个函数 put_content_to_s3()，把给定的内容放在指定的 s3 路径中。此函数还将备份密钥作为参数，如果文件已经存在于指定的 S3 位置，它将根据指定的备份策略备份备份文件夹或备份文件中的旧内容。这种备份策略可以根据需要进行定制。

运行代码的实例应该对指定的 S3 路径具有读写权限。

**如何使用 S3 测井:**

该代码的主要功能是创建一个字符串 I/O 缓冲区和字符串 I/O 记录器。代码将有一个带有 logger.info 和 logger.error 的正常流程。

在 finally 块中，调用 put_content_to_s3()函数将日志内容从字符串 I/O 缓冲区放入 s3。

这是使用现有的 python logger 和 Boto3 在 Python3 中实现 S3 日志记录的简单易行的方法之一。在开放源码中，很少有用于此目的的库，但这里的目的是不使用任何不可信的 python 库，也不使事情过于复杂。

编码快乐！