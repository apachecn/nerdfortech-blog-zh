# 使用 Wait()系统调用的进程同步

> 原文：<https://medium.com/nerd-for-tech/process-synchronization-using-wait-system-call-c1c8ce9b734e?source=collection_archive---------16----------------------->

**简介:**

在我的[上一篇文章](https://kartiksiddhabathula.medium.com/linux-multiple-processes-switching-time-slicing-db7f78f4dd51)中，我写了 CPU 如何使用时间片来调度多个进程。我们看到了打印语句是如何交织在一起的。现在问题来了，如果我们想让一个进程先完成，然后再执行第二个进程呢？在本文中，我将展示如何实现这一点，即使用 wait()系统调用实现流程同步。

**进程同步**

> 确保操作系统完成一个进程的执行，然后…