# UNIX 中有趣的终止信号

> 原文：<https://medium.com/nerd-for-tech/interesting-kill-signals-in-unix-e61eee100bc0?source=collection_archive---------7----------------------->

# SIGTERM、SIGKILL、SIGSTOP 和 SIGCONT

# 什么是 SIGTERM？

SIGTERM 的意思是信号和终止，在类 UNIX 系统中，SIGTERM 信号用于终止程序。

SIGTERM 也可以被称为软删除，因为接收 SIGTERM 信号的进程可以选择忽略它。

换句话说，这是杀死一个进程的礼貌和安全的方式。

默认情况下，kill command 发送 SIGTERM 信号。你可以用-15 明确地提到它。

如何使用 SIGTERM 命令

```
kill <process_id>
```

或者

```
kill -15 <process_id>
```

或者

```
kill -SIGTERM <process_id>
```

# 什么是 SIGKILL？

SIGKILL 用于立即终止进程。该信号不能被忽略/阻止。该进程将与其线程一起终止。

这是扼杀一个进程的残酷方式，只应作为最后手段使用。假设您想要关闭一个没有响应的进程。在这种情况下可以使用 SIGKILL。

您可以使用选项-9 发送带有 KILL 命令的 SIGKILL 信号来立即终止进程

# SIGTERM vs SIGKILL:

*   SIGTERM 优雅地终止进程，而 SIGKILL 立即终止进程。
*   SIGTERM 信号可以被处理、忽略和阻塞，但 SIGKILL 不能被处理或阻塞。
*   SIGTERM 不杀死子进程。SIGKILL 也会杀死子进程。

有了 SIGTERM，进程就有时间将信息发送给它的父进程和子进程。它的子进程由 init 处理。

使用 SIGKILL 可能会导致创建一个僵尸进程，因为被杀死的进程没有机会告诉它的父进程它已经收到了一个杀死信号。

![](img/76eab33ee1570d23a8ad2eca646f6758.png)

信用: [*关闭.美国*](https://turnoff.us/geek/dont-sigkill/)

# 什么是 SIGSTOP？

维基百科对[信号停止](http://en.wikipedia.org/wiki/SIGSTOP)有很好的定义:

> 当 SIGSTOP 被发送到一个进程时，通常的行为是在当前状态下暂停该进程。只有向该进程发送 SIGCONT 信号，该进程才会恢复执行。SIGSTOP 和 SIGCONT 用于 Unix shell 中的作业控制，以及其他用途。SIGSTOP 不能被捕获或忽略。

# **和信号控制:**

![](img/bc4cb1fe3a7728eefbf4299c692156e2.png)

贷方: [*关闭. us*](https://turnoff.us/geek/dont-sigkill/)

> *当 SIGSTOP 或 SIGTSTP 被发送到一个进程时，通常的行为是将该进程暂停在其当前状态。*
> 
> *只有向进程发送 SIGCONT 信号，进程才会恢复执行。*
> 
> *SIGSTOP 和 SIGCONT 用于 Unix shell 中的作业控制，以及其他用途。*

简而言之，SIGSTOP 告诉一个进程“坚持住”，SIGCONT 告诉一个进程“从你停止的地方继续”。这对于 rsync 作业非常有用，因为您可以暂停作业，在目标设备上清理一些空间，然后恢复作业。源 rsync 进程认为目标 rsync 进程花费了很长时间

回复。

**如何使用它们**

```
kill -SIGSTOP <process_id>
kill -SIGCONT <process_id>
```

# T **帽子所有乡亲…**

如果你已经读到这里，那么…

![](img/dcf21bddde5fc157c9aac93e9c15ed20.png)