# 连接 Google Colab 和 Google Drive

> 原文：<https://medium.com/nerd-for-tech/connecting-your-google-colab-and-google-drive-b9bccb31e47c?source=collection_archive---------3----------------------->

![](img/6b50515ce7b9c20d6337901054231ed7.png)

由 [Toa Heftiba](https://unsplash.com/@heftiba?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/connection?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

连接 google drive 和 google colab 是简单却必要的知识。但是有时候我们想不通该怎么做。让我们从在 google colab 中打开新笔记本开始。

![](img/ad2f0e6f852e8bf9b1b969cee2db022f.png)

之后，你需要把这个功能从谷歌 colab，以连接到你的驱动器

![](img/829ba166fbd943f4c371d0a1e02d7463.png)

接下来，您需要在您的 colab 环境中挂载这个驱动器。它会要求你像这样打开新的链接

![](img/1e3ef6263fee325108f2cc1e6baa998e.png)

只要打开链接，复制验证码

![](img/9d28ebe308474e1706f8939ebec607cc.png)

将验证码粘贴到 google colab 内的框中，然后按 enter 键

![](img/785020e0473dbd6cc50f4f6a7f714858.png)

如果您验证成功，输出将是这样的

![](img/36c9500c0f2eeae69cf74e773cc91a39.png)

所以每当你在笔记本上输入`!ls`，你就会得到一个名为 drive 的新目录。

![](img/44f241ceeaf8f37e26f3ff7f3e854bf9.png)

此外，如果你想使用图形用户界面模式，只需按下左面板中的目录，你会得到你之前安装的驱动器

![](img/c6d4701fbdd119eb5a1333d535bd4522.png)

# 奖金

仅供参考，我认为谷歌 colab 是基于 Linux 的系统，所以你可以在那里输入 Linux 的任何命令。在我的例子中，我想进入我的驱动器中的数据集目录。要通过笔记本访问它，你应该使用`%`而不是`!`，因为功能不同。`!`是暂时的，但`%`将是持久的。这是一个例子

![](img/871147e52577fa5d97862efdde66e5a5.png)

如果你是那种不相信 CLI 命令的人，你可以检查一下 GUI 版本，看看这个目录'/content/drive/MyDrive/dataset '里面有什么

![](img/2e67c5ed156be4ee94945c83bb723aae.png)

现在，如果你想解压 zip 文件，你可以使用如下命令

```
!unzip '{ZIP Files}' -d '/new_directory/{ZIP Files}'
```

试试吧，看看你会得到什么！！！

感谢阅读

## 注意

*   挂载点绝对是你的，但是很多人使用像'/content/drive '这样的公共目录，让它与人相同是很好的，因为无论何时你作为团队工作，你的团队都不会混淆。
*   验证码实际上每次都会改变，所以你不需要担心代码被共享。我只是喜欢把它变成那样，变成一种习惯。
*   “%”命令对于在目录中移动非常有用，它实际上可以给你更干净的代码
*   unzip 中的命令'-d '用于提取新目录中的数据，而不创建新目录