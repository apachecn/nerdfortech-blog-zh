# 使用 Android Studio 和 Unity

> 原文：<https://medium.com/nerd-for-tech/using-android-studio-with-unity-bda2dd5b2068?source=collection_archive---------7----------------------->

**目的:**从 Android 设备上观察一个游戏的调试日志。

我们已经安装了来自 https://developer.android.com/[的 Android Studio。现在我们将选择*配置文件或调试 APK* 。](https://developer.android.com/)

![](img/7088110255fc4954a5d2420094a3bb1a.png)

然后我们选择*。我们创建的 apk* 文件。

![](img/7d779f8145991fdc95f26e2bb5d8d738.png)

现在，我们将 Android 设备连接到 PC，在设备上启用开发人员模式，然后在设备上启用 USB 调试。每个设备的这个过程都不同——你可能需要谷歌一下如何为你的设备做到这一点。

连接后，我们将单击 *Logcat* 选项卡，并从下拉列表中选择我们的设备。从最右边的下拉列表中，我们将选择*编辑过滤器配置*。

![](img/2c68cf27dd0584caed5c035944a8fca1.png)

我们将创建名为“Unity”的新过滤器，并将*日志标记*指定为“Unity”*。*

![](img/6465cf831ebffc59c5c719db365e7507.png)

现在，当我们启动应用程序时，我们将看到该应用程序的日志信息。下图，我看过一个游戏内广告，赚了 100 钻石。我们的调试模式正在工作！

![](img/83b7ae45feb51cb3875165bcfaadeaa4.png)