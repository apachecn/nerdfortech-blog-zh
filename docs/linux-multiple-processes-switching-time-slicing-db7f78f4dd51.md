# Linux 多进程切换(时间分片)

> 原文：<https://medium.com/nerd-for-tech/linux-multiple-processes-switching-time-slicing-db7f78f4dd51?source=collection_archive---------14----------------------->

**简介:**

作为一名计算机科学毕业生和软件专业人员，我必须每天与计算机打交道，并且已经这样做了很长时间。从表面上看，我们与如此多的应用程序交互，这些应用程序不过是在 CPU 上执行的单个进程。但是 CPU 实际上是如何处理的，这是我想在这篇文章中展示的。

**时间切片**

Linux 是一个多进程操作系统。这意味着操作系统有能力执行多个进程。这是否意味着…