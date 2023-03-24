# Debian 系列:LVM 与 lvm2

> 原文：<https://medium.com/nerd-for-tech/the-debian-series-lvm-with-lvm2-cc1d6f9a6474?source=collection_archive---------6----------------------->

磁盘管理工作对于充分利用存储资源极其重要。我已经在 Debian 系列的前几篇文章中讨论了如何执行磁盘分区系列、分区的配置文件系统、交换供应等等。

现在是时候谈谈我们如何优化磁盘空间，使其对操作系统来说是最佳和有用的了。我很确定创建逻辑卷可以通过某种方式使用实用程序来“硬”完成。在我的情况下，我将使用 LVM，以及个人指南，将遵循通过实现目标。

一个 **LVM** 或**逻辑卷管理器**是一个帮助你添加具有不同特性的卷或硬盘的工具。

你需要使用`aptitude`来安装它，就像这样

`aptitude install lvm2`

注意:`aptitude`在 Debian 9+实例中不存在。我一直在 VirtualBox 虚拟机上使用虚拟化的 Debian 7 实例。

## 我们到底要做什么？

在实际场景中，我们有**物理卷**(基本上是硬盘)，就像`/etc/sdb, /etc/sdc, /etc/sdd`一样，对吗？

我们还有**卷组**，它们代表一组**物理卷的占位符名称。**根据 [RedHat 定义](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/logical_volume_manager_administration/volume_group_overview)，VGs“创建一个磁盘空间池，从中可以分配逻辑卷。”

这表明我们可以将不同大小的**物理磁盘**分组，也可以作为卷组引用，因为**分组的磁盘**创建了一个**卷**

这对我来说很酷，这是我能做的解释 LVM 做什么的最简单的方法。

## 创建逻辑卷

在这种情况下，我们将使用 **LVM** 对分别具有 5 和 10 千兆字节的 2 个磁盘进行分组。目标:利用操作系统中的组空间。

优点:我们可以使用不同功能的磁盘，它们被组合成一个单元。

所有这些都可以通过 6 个步骤完成，我们将使用`lvm2`命令、实用程序，如`fdisk`、`mkfs`和著名的`/etc/fstab`(这一点上没有什么新的东西)

让我们从用`fdisk`创建 **LVM** 分区开始，所以:

`fdisk /dev/sdb`按照向导，我们将只创建一个占用整个扇区大小的主分区，棘手的部分是将磁盘类型设置为`LINUX LVM`

a 将暂停一会儿，因为如果您从头开始看这个系列，您会注意到我们有 3 个磁盘在运行。为了以最佳方式执行它，我们需要将这个完全相同的配置复制到安装的其他两个磁盘上。

好了，说了这么多，让我们使用`pvcreate`创建一个**物理卷**，如下所示:

`pvcreate /dev/sdb1`

如果你需要检查你的进度，你可以使用`pvdisplay`，这将列出安装的硬盘。

之后，我们现在用`vgcreate`命令创建**卷组**

`vgcreate volname /dev/sdb1 /dev/sdc1 /dev/sdd1`

由于显而易见的原因，`sda`是不可触及的。

现在，让我们创建**逻辑卷**

`lvcreate -L 5G volname`

这创建了具有 5gb 的逻辑卷**(大小可以改变)**

之后，让我们用`mkfs`设置文件名类型

`mkfs ext4 /dev/volgroup/volname`

最后，我们需要以持久的方式`mount`分区。

```
cd /
mkdir grouped
mount /dev/mapper/volgroup-lvol0 /grouped
nano /etc/fstab
...
```

从这一点上，你将需要添加结果 UUID，就像我们在前面的故事。

毕竟记得重启。

这是一个艰难的故事。我将在一段时间内重温这一点，并试图以最有效的方式扩展它。

请随意投稿。错了就修。并分享它。

带着 **Crontab** 和 **Anacrontab** 移动到 **cronjobs** 旁边。

敬请关注。

快乐编码:)