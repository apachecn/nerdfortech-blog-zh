# 轻松使用 Makefile

> 原文：<https://medium.com/nerd-for-tech/take-it-easy-with-makefile-c1e31fb38928?source=collection_archive---------3----------------------->

![](img/ab7a5a3597ed6d963beadc1075bfdf30.png)

[韦斯利·廷吉](https://unsplash.com/@wesleyphotography?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

M 虽然 Makefile 乍听起来很简单，但它让使用一个命令执行一长串命令和函数变得非常容易。

例如:创建虚拟环境、设置环境变量和安装依赖项可以使用一个`make install`命令来完成。酷吧？

# 先决条件

要验证 make 是否安装在您的系统中，请在命令行中运行`make`。如果没有安装，您可以使用

```
sudo apt-get install build-essential
```

# **入门**

Makefile 可以被认为是以预定义结构编写的规则集合。规则通常具有以下格式。

一个 ***目标*** 可以是可执行文件或一个目标文件的名称。它也可以是规则或操作的名称。
一个 ***先决条件*** 是一个用作目标输入的动作或文件。在运行目标的命令之前，这些文件/操作需要存在。这些也叫 ***依赖*** 。我们可以为一个目标声明多个依赖项，用空格分隔它们。有些规则可能不需要任何依赖关系。
一个 ***命令*** 是一个需要完成的单一工作单元，以便“制定”目标。这些需要以一个*制表符开始。*

> 如果您喜欢使用 tab 以外的字符，您可以将`.RECIPEPREFIX`变量设置为替代字符。

## 例子

现在让我们编写第一个`make`命令，在被调用时打印“Hello World”。

要运行这个命令，请转到您的终端，进入与 Makefile 相同的目录，并使用:

```
make run
```

您将看到类似于以下内容的输出:

```
echo "Hello World"
Hello World
```

正如您所看到的，实际的命令在命令输出之前打印出来。要停止这种行为，您可以在命令的开头添加一个“`@`”符号，以防止它被打印出来。

可以在 Makefile 中使用 v *变量*来保存文件信息、一些参数，或者甚至是命令的一部分。变量被写入**大写**，后跟一个`=`符号。变量通常在文件的顶部声明。我们可以使用`${VARIABLE_NAME}`来访问它们的值，其中 **VARIABLE_NAME** 是您想要访问其值的变量。

有时，我们也用符号`?=`来设定以前没有的变量。如果变量已经设置，赋值操作将被忽略。

现在让我们试着为一个简单的 Django 项目编写一个稍微酷一点的 Makefile，它能够设置一个虚拟环境，安装依赖项，并运行基本但常用的 Django 命令。

我们可以使用像`make install`、`make format`这样的命令来快速设置工作空间并开始工作。相当整洁！

默认情况下，如果我们只在命令行中键入`make`，那么将执行第一个定义的规则。要执行其他一些规则，我们需要在 Makefile 中设置`.DEFAULT_GOAL`变量。比如我们想默认执行`install`，定义`.DEFAULT_GOAL=install`。现在，当我们在命令行中运行`make`时，我们应该会看到以下输出:

```
-> Making Virtual Environment
-> Installing Dependencies
.
.
.
(installation proceeds)
```

感谢阅读！如果你有任何疑问或建议，请在评论中告诉我。

# 参考

[](https://opensource.com/article/18/8/what-how-makefile) [## 什么是 Makefile，它是如何工作的？

### 如果您想在某些文件更新时运行或更新任务，make 实用程序会派上用场。品牌……

opensource.com](https://opensource.com/article/18/8/what-how-makefile) [](https://makefiletutorial.com/) [## Makefile 示例教程

### 我创建这个指南是因为我从来没有完全理解过 Makefiles。他们似乎充斥着潜规则和…

makefiletutorial.com](https://makefiletutorial.com/)