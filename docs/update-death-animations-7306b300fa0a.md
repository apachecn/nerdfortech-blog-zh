# 更新:死亡动画

> 原文：<https://medium.com/nerd-for-tech/update-death-animations-7306b300fa0a?source=collection_archive---------17----------------------->

![](img/370575144e6075c024f93d6f061f0d51.png)

我已经给敌人和玩家添加了死亡动画。

这是通过检查每个敌人和玩家并记录死亡动画来完成的。

![](img/8b8fd22cefcfdb91f8b13dd98135a526.png)![](img/edeaa02c9c95c4d5be742a4a98bb1790.png)![](img/173834dcd32e15ab8fbf8a8dd821a5fc.png)![](img/79c39b1662710b498eab264730b85b07.png)

使用触发参数从任何状态触发死亡动画。

![](img/15b6e0af2b2ec6554b24a6e293b2cf89.png)

对于敌人，在敌人类中创建一个协程，并在受保护的方法中调用它。

![](img/eddb204cd9e6f912ae25677e50dab414.png)

然后调用方法在每个敌人上启动协程。

![](img/315054fe532ebad9aeeaca1558f6f3c7.png)

对于玩家来说也是类似的。在播放器动画脚本中设置触发器。

![](img/29d3c6982126d493a7803feb0b7e3d86.png)

在播放器脚本中创建一个协程。

![](img/96803ceffae2b8ea31be0adde91a7574.png)

然后在玩家生命耗尽时启动协程。

![](img/59f799da79697c4c88716cb79348ca3c.png)

现在基本战斗系统完成了。

![](img/e4b7138796e2acea6c4a59d676623b50.png)