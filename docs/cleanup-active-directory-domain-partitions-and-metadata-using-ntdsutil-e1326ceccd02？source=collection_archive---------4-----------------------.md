# 使用 NTDSUtil 清理 Active Directory 域分区和元数据

> 原文：<https://medium.com/nerd-for-tech/cleanup-active-directory-domain-partitions-and-metadata-using-ntdsutil-e1326ceccd02?source=collection_archive---------4----------------------->

# 问题的原因

如果域控制器(DC)在没有适当降级的情况下被删除，它将留下不需要的域分区和元数据，这可能会导致复制问题，并阻止新的域控制器升级到全局编录(GC)服务器。

在服务器事件日志中，当尝试添加新的 DC 时，您可能会看到如下内容:

```
event id 1800 with this error:
 A partial replica of partition…
```