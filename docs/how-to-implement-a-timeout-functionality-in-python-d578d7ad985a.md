# 如何在 Python 中实现超时功能

> 原文：<https://medium.com/nerd-for-tech/how-to-implement-a-timeout-functionality-in-python-d578d7ad985a?source=collection_archive---------0----------------------->

![](img/3fec6fe74eae76118de5c38abc2b6bd4.png)

卢卡斯·布拉塞克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Quora 和堆栈溢出中经常出现的一个问题是，如何在 Python 中为一些函数调用或线程设置超时。

我想在一篇具体的文章中总结实现这种特性的多种方法，其中所有建议的解决方案都是平台(等等..UNIX/LINUX，Windows)独立。

## I .使用多线程和…