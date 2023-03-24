# Debian 系列:Crontabs

> 原文：<https://medium.com/nerd-for-tech/the-debian-series-crontabs-624d4e2be2cb?source=collection_archive---------7----------------------->

暂时离开硬盘驱动器和分区，我意识到 crontabs 和 bash 脚本是至关重要的日常活动。我不想强调 **Bash 脚本**，但是我不得不强调，因为这两个概念彼此紧密相连。

BASH 是所有基于 Linux 的操作系统的默认控制台(终端)。基本上是一个解释命令的程序。这是今天服务器管理员和程序员的日常环境。

为 **bash 终端**编写的程序有一些基于编程最佳实践、条件评估、循环、数据结构甚至编程范例的规则。

另一方面， **crontab** 是一个程序，它按照用户设置的时间间隔运行某些进程或服务。你可以根据假设设定你的逻辑时间，以便为任何特殊原因执行任务。

这可以通过分钟、小时、星期几、月来确定。这是由某个用户完成的，当然，需要一个程序来运行，通常是一个. sh(用于 bash)文件，包含 cronjob 必须处理的任何内容。

**Crontabs** 和任何事物一样，都有利弊。有利因素不言自明，但主要的不利因素是操作系统必须在执行中才能运行。在内部，crontab 得到了 **anacrontab 的协助，**实际上被称为 **Anacron** ，并检查 **cron** 是否工作。

这个操作系统服务是 **cron** 本身的备份，也清理未完成的进程。Anacron 在系统启动时被执行，并根据位于`/etc`目录的文件寻找特定的文件。

这些文件如下:

```
cat /etc/cron.hourly
cat /etc/cron.daily
cat /etc/cron.monthly
cat /etc/cron.weekly
...
```

## 文件位置

在内部，服务可以存在于两个位置，`/usr/bin/crontab`是一个文件，其中存放了不是根用户的用户的内部进程(基本上就是您所做的事情)。而`/usr/sbin/cron`是服务的定义，守护进程本身。

## 该结构

cronjob 需要填写 7 个字段才能有效。这是点的菜。

```
m - minutes : 0 - 59
h - hours : 0 - 23
d - day of the month : 1 - 31
m - month : 1 - 12
dow - days of the week: 0 - 6, 1 - 7
user - user that runs the process
command - path to the .sh file or direct bash instructions.
```

我一会儿会给你们看一个例子。在此之前，在 **cron** 定义中，`*`通配符指代所有动作。如果它位于`days`位置，cronjob 将每天运行。

**Pro-tip** :如果你愿意在 **crontab** 上运行一个`program.sh`，你必须需要确保对程序的执行权限，Google about`chmod`命令来实现这一点。

最后！

要将 cronjob 添加到 crontab 配置中，只需键入`crontab -e` (e 表示编辑)，如果想列出所有元素，可以运行`crontab -l`即可。

`30 22 2 3 * david /opt/workshop/server/program.sh`

这将由用户`david`在 2020 年 3 月 2 日 22:30 执行`program.sh`

仅此而已。

这个工具非常非常有用，非常适合异步工作，并且可以在服务器活动较少的时候或者任何方便的时候编程。

本系列的下一部分也是最后一部分是网络实用程序。

把那个装起来。

快乐编码:)