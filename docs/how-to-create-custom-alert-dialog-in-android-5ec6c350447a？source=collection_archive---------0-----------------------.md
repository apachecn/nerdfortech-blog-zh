# 如何在 Android 中创建自定义提醒对话框

> 原文：<https://medium.com/nerd-for-tech/how-to-create-custom-alert-dialog-in-android-5ec6c350447a?source=collection_archive---------0----------------------->

让我们看看如何在 Android 中创建一个自定义的提醒对话框。

![](img/e11c7bebec83315d275f0a4521bd1785.png)

Android 应用程序开发

首先，Android 中默认的提醒对话框是这样的。这是一个非常基本的按钮，只有一个正按钮和一个负按钮。

![](img/bf750afee5c33e7b861e787a0593df1e.png)

那么，如何才能让它变得有吸引力呢？让我们看看。

首先，像往常一样创建一个新项目。创建一个布局资源文件并将其命名为 **dialog.xml** 。

现在，让我们在 MainActivity.java 的**文件中创建一个方法。**

如您所见，我们已经将 dialog.xml 设置为该对话框的**内容视图**。

现在，我们可以在 **onCreate()** 方法中调用这个方法来显示*警告对话框*。

现在，您已经准备好运行您的应用程序。您应该能够看到如下所示的输出:

![](img/9861884bee56705817fbc6e131d19981.png)

自定义警报对话框

您可以自定义颜色，使其更具吸引力。所以，这是一个在 Android 中创建自定义提醒对话框的简单方法。

希望你喜欢阅读这篇文章！

如果您有任何疑问，请在下面的**评论**部分发帖。在 [LinkedIn](https://www.linkedin.com/in/vaidhyanathansm/) 上与我联系。此外，如果你想看看我开发的惊人的应用程序集，别忘了查看谷歌 Play 商店。

更多了解我[这里](https://vaidhyanathansm.tech/)。

话虽如此，感谢您阅读我的文章和*快乐编码！*