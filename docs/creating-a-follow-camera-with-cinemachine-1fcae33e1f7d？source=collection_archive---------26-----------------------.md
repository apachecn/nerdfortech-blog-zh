# 使用 Cinemachine 创建跟踪摄像机

> 原文：<https://medium.com/nerd-for-tech/creating-a-follow-camera-with-cinemachine-1fcae33e1f7d?source=collection_archive---------26----------------------->

目标:使用 Cinemachine 创建一个虚拟摄像机，它会跟随玩家角色，让他们出现在屏幕上。

![](img/f8b5a8a29a51f876cd78caa27b6ff541.png)

我们首先进入*窗口*菜单，选择*软件包管理器*导入 Cinemachine。

![](img/bc53c511347d1446b2fd538577d27477.png)

点击菜单栏上的 *Cinemachine* ，选择*创建虚拟摄像机*。

![](img/e11b7af58edb3d3c9e0f655191a44ae1.png)

将玩家对象分配给虚拟摄像机的 *Follow* 属性。

![](img/5939df7690b897922ba3c0cdb89aae8b.png)

我们可以通过调整虚拟摄像机的*主体*组件上的*阻尼*属性来微调跟随效果的响应。

![](img/61081c739e567c3eca04611203b50eb5.png)