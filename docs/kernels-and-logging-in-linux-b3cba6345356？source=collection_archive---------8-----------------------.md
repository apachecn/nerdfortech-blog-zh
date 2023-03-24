# Linux 中的内核和日志

> 原文：<https://medium.com/nerd-for-tech/kernels-and-logging-in-linux-b3cba6345356?source=collection_archive---------8----------------------->

Linux 系统中的引导过程是一个相当完整的旅程，它有一系列完成特定任务的阶段，只是为了以一种有效的方式调优您的工作站或服务器。

整个想法是写一个非常简洁的概述，介绍在 Linux 系统中，特别是在 CentOS 版本中，内核是如何被触发的，日志是如何执行的。我将非常直接地使用一些术语，因为这方面有很多可用的信息，但是，我将尝试以简洁明了的方式涵盖尽可能多的内容。

# 引导系统

从固件代码执行开始，到引导加载程序、内核、内存磁盘初始化，直到任何流程管理系统，您的服务器或工作站都需要执行所有周期/阶段，以便为系统工作做好准备。

先预热一下，我们可以设置固件以两种方式执行代码。在旧系统中，计算机在 BIOS 中执行代码，实际上，这种方法在遗留系统中很常见。另一方面，较新的计算机在`UEFI`固件中执行代码。

这里常见的方法是两者都在被称为`POST`过程的`power-on-self-test`中执行

为了更好的理解这一点，可以参考[这篇](https://www.geeksforgeeks.org/what-is-postpower-on-self-test/)文章定义文章来自 [Geeksforgeeks](https://www.geeksforgeeks.org/) 。

一旦`POST`流程成功执行，`Bootloader`就开始了。此时，固件在驱动器上执行著名的引导加载程序`grub2`代码，`Bootloader`读取其配置并引导 Linux 内核。

在有`BIOS`的情况下，`grub`的配置文件将位于`/boot/grub2/grub.cfg`，在其他基于`UEFI`的系统情况下，您应该查看一下`/boot/efi/EFI/red/grub.efi`

在此之后，下一件大事是`Kernel`初始化，在这一步中`ramdisk`被加载到 RAM 内存中，`ramdisk`作为一个临时文件系统，包含重要的内核模块、驱动程序和一些 Kickstarter 文件，之后，内核自己卸载`ramdisk`并挂载实际的根文件系统。

你可以在这里找到更多关于 ramdisk [的信息](https://www.novell.com/documentation/suse91/suselinux-adminguide/html/ch12s03.html)。

一旦文件系统被挂载，初始化阶段就开始了。在这里，你会被某种服务管理系统所统治，无论是使用`SysV init`、`launchd`、`upStart`还是`Systemd`

# 简单的 grub 概述

你可能知道，CentOS 7 使用`grub2`作为他们的引导加载程序。

您可以在`/boot/grub2/grub.cfg`文件中查看 grub 加载程序的完整配置。这里有几行代码值得一提

1.  `## BEGIN /etc/grub.d/00_header`内部创建 GRUB 菜单标题并为其设置屏幕
2.  包含我们在启动时看到的实际内核选择。所有条目都以菜单项开始，并包括引导加载程序和内核的选项
3.  `linux16 /vmlinuz-3-10.0 …`是实际的 Linux 默认系统，这包括到根文件系统的路径。
4.  `initrd16 /initramsfs-3.10.0 …`指定与内核匹配的`ramdisk`。这是挂载根文件系统之前的前一步
5.  `### BEGIN /etc/grub.d/30_os-prober`如果您在驱动器上有不同的分区，这个选项会很有用，`os-probe`会找到它并为它创建一个启动条目。
6.  `### BEGIN /etc/grub.d/40_custom`这个用于自定义菜单项

为了修改执行的排序顺序，可以更改所有的名称。如果您执行任何更改，您将需要通过运行`grub2-mkconfig`命令来保存`grub`配置。

此外，我们有位于`/etc/default/grub`的默认 grub 设置文件，它包含用于正确设置`grub`的完整转储内容。一个很酷的例子是`GRUB_TIMEOUT`属性，它在自动引导默认内核条目之前为显示的菜单设置超时

默认的内核配置位于`/boot/grub2/grubenv`，为此，值得一提的是`saved_entry`列表，它只不过是`grub.cfg`文件的菜单项。

为了改变默认的`grub2`加载程序，您可以通过添加内核列表号来使用`grub2-set-default`命令。

类似于:`grub2-set-default 1`

之后，您需要重新配置 grub 配置文件，使用:

1.  `BIOS`方法:`grub2-mkconfig -o /boot/grub2/grub.cfg`
2.  `UEFI`方法:`grub2-mkconfig -o /boot/efi/EFI/redhat/grub.cfg`

# 内核概述

理解语义版本将有助于了解什么类型的内核更新可以适用于特定的系统。在 CentOS 系统中，您可以通过运行:`sudo yum -q install list kernel-*`来检查已安装内核的列表

下面是一个内核示例版本输出:`kernel.x86_64 3.10.0–514.el7`

其中:

1.  3:是主要版本
2.  10:是主要修订版
3.  0:是内核补丁
4.  514:指红帽版
5.  el7:指企业版 Linux 7

内核文件驻留在`/boot`目录中，因此，当您列出它时，您会发现代表 Linux 内核本身的`vmlinuz`文件，`initramfs`是 ramdisk 目录，此外，还有一些与 grub 相关的目录。

另一个重要的目录是`/proc`,它是在系统启动时在内存中创建的，当机器关闭时被删除。`/proc`目录作为内核本身的直接接口。在内部，您将看到一堆数字文件夹，它们代表每个运行的 PID 和执行细节，非数字文件夹包含关于内存系统的信息，这些信息以文本文件的形式表示。

## 附加模块

除了内核之外，Linux 还有必须与内核版本相匹配的模块，这些模块为内核增加了功能，包括文件系统和驱动程序。您可以在`/lib`和`/lib64`目录中找到它

下面是你如何过滤它:`ls /lib/modules/$(uname -r)/kernel`内部有用于架构、加密、驱动、文件系统、内核、网络等等的库，然而，你可以用这些 util 命令检查其中的一些。

1.  `lsmod`用于显示加载到内存中的模块
2.  `modinfo [module]`向特定模块显示信息
3.  `modprobe -v [module]`输出信息及其依赖关系

诸如此类。

Linux 自动处理硬件驱动程序，但有时需要强制解决问题，这是因为通过网络寻址的设备无法知道那里有什么，这是存储区域、网络和打印机的常见情况，但如果硬件是本地的，还有另一个名为`depmod`的实用工具，因此，为了在引导时自动加载，您必须:

1.  `mkdir /etc/modules-load.d`(应包括模块名称)
2.  然后:`sudo vi /etc/modules-load.d/dm_mirror.conf`(每个文件对应一个模块)，在里面输入模块的名字:`dm_mirror`就这样

对于反向列表(相反):

1.  `sudo vi /etc/modprobe.d/ctxfi.conf`在里面你应该输入模块的名字:`blacklist [module]`，就这样
2.  引导时，不会加载该模块。

# 服务

简而言之，服务是由操作系统启动的进程，由不太新但很有名的`systemctl`管理。`systemctl`命令让你管理(启动、停止、重启、启用、禁用等)服务。

在 CentOS 系统中,`SystemD`管理服务和其他对象，如设备、定时器和目标，其中每个对象都被称为`Units`,每个单元都有一个配置文件。

以下是一些例子:

1.  `systemctl list-unit-files -at service`这基本上显示所有启用的单元文件，服务配置为自动启动
2.  `systemctl list-units -at service`显示已启用的运行服务

公共输出信息由服务名、单元文件状态、生成状态、当前状态和描述组成。可以过滤为:`systemctl list-units -t service --state running`

然而，您可以输出如下的详细信息:`systemctl cat [service]`这将显示关于服务的详细信息，包括依赖性、命令执行、失败等等。

出于管理目的，我们举了以下例子:

1.  `sudo systemctl stop atd`其中`atd`是流程的名称
2.  `sudo systemctl status [process]`用于了解状态
3.  `sudo systemctl start [process]`开始一个流程

同样的模式适用于任何其他东西，你也可以运行:

1.  `sudo systemctl is-active [service]`了解服务是否启动
2.  `sudo systemctl disable [service]`去禁用
3.  要启用一项服务，基本上，这使它成为永久性的

# 记录

日志基本上是一个包含关于系统的消息的文件，不同的任务有不同的日志，您可以找到关于严重错误、失败错误、cronjobs 规约等等的日志。

CentOS 系统有两个记录系统`rsyslog`和`journald`

1.  `rsyslog`它与`sysklogd`兼容，用于持久日志。这些类型的日志是长文本文件，可以通过`TCP`或`UDP`远程捕获
2.  默认情况下,`journald`是非持久的，它们不会存活到重启。它们是二进制格式的，存储在 RAM 中，也有固定的大小。但是可以通过一些策略来坚持

您可以在`systemctl`命令的帮助下与`rsyslog`服务进行交互。所有的配置设置将在`/etc/rsyslog.conf`

`rsyslog`有一些规则，您可以设置哪个子系统可以生成系统消息及其标准(优先级)。此外，所有信息都将被写入`/var/log/messages`

另一方面，`journalctl`日志服务的处理方式不同。这里有一堆命令执行的例子

1.  `sudo journalctl`(所有条目)
2.  `sudo journalctl -k`
3.  `sudo journalctl /sbin/crond`
4.  `sudo journalctl -u crond`

为了使它永久保存，您需要为它创建一个特定的目录

```
[directory]
sudo mkdir -p /var/log/journal
[service]
sudo systemctl restart systemd-journald
[listing logs]
ls -l /var/log/journal
```

暂时就这些了。我敢肯定，更多的概念在这个时候失踪，所以请保持关注，这可能会定期更新。

所有这些概念对于 Linux 管理员来说都是必须知道的，所以您可以把它看作是一个复习或学习材料。

快乐编码:)