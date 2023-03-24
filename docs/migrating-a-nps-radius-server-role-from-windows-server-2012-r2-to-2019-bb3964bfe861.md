# 将 NPS (Radius)服务器角色从 Windows Server 2012 R2 迁移到 2019

> 原文：<https://medium.com/nerd-for-tech/migrating-a-nps-radius-server-role-from-windows-server-2012-r2-to-2019-bb3964bfe861?source=collection_archive---------4----------------------->

这似乎是一个简单的任务…

在旧服务器上，在 NPS MMC 管理单元中，在 NPS 根目录上，右键单击并选择“导出配置”。

在新服务器上，安装 NPS 角色，然后导入配置。

所有的配置都被复制过来。防火墙端口规则已添加到 Windows 防火墙。任务完成。？

没那么快。当试图加入一台机器时，服务器上显示以下 ID 为 14 的错误，客户端无法…