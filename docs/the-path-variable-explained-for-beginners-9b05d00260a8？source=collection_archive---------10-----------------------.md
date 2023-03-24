# 为初学者解释的路径变量

> 原文：<https://medium.com/nerd-for-tech/the-path-variable-explained-for-beginners-9b05d00260a8?source=collection_archive---------10----------------------->

什么是路径变量，为什么我需要它？我将为初学者解释一下，这样你会更好地理解这个概念，并且确切地知道什么时候以及为什么需要设置 PATH 变量。

![](img/9047f0d402c71ab0bbe138b5cf860fe4.png)

来自[佩克斯](https://www.pexels.com/photo/brown-wooden-arrow-signed-66100/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的[延斯·约翰森](https://www.pexels.com/@jens-johnsson-14223?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄的照片

# 到处都是小路(双关语)

通常，当你想学习一门(新的)编程语言时，入门和安装教程会要求你在环境变量中添加一个路径变量(例如，C:/Programs/Java/JDK-12.0.1)。你做到了，但是你知道为什么要设置 PATH 变量吗？网络上的解释有时非常专业，它们不会帮助你理解背后的概念。

# 解释的路径变量

电脑很蠢。真的！计算机会犯非常快、非常准确的错误。他们不知道你安装新编程语言的意图——你必须告诉他们。有了路径变量，你就可以做到这一点。

在 Windows、Linux 和 Mac OS X 上，使用 PATH 变量。通过在环境变量中设置 PATH 变量，您可以指定可执行程序的目录——在我们的例子中，就是我们的编程语言的目录。**用 PATH 变量，你告诉操作系统为了启动一个程序去哪里找，环境变量就是操作系统期待这种信息的地方。**

# 为什么我需要 PATH 变量？

你不知道——但这让生活变得更容易。在设置 PATH 变量之前，您必须在终端中编写您的可执行编程语言的完整路径。假设我们要编译一个 Java 程序。为此，我们必须键入以下内容

```
C:/Programs/Java/JDK-12.0.1/bin/javac HelloWorld.java
```

为了用 Java 编译一个写好的程序。那样又烦又费时。有了路径变量，无论何时你想执行一个程序，操作系统都知道在哪里寻找。现在你只需要写

```
javac HelloWorld.java
```

操作系统知道在你的机器上哪里可以找到可执行程序(在我们的例子中是“javac”)。很棒，不是吗？在高水平的基础上，这就是我们需要知道的一切。

看看你机器上的环境变量。您可能会看到一些安装的程序已经自己设置了其中的一些变量，还有一些已经由操作系统设置好了。

# 试试吧！

更好地理解某事的最好方法是用一个简单的例子来尝试。打开你选择的终端，试着打开一个程序——比方说 Chrome。它不会起作用，但如果你进入整个路径，它会起作用。现在，将 Chrome 安装路径添加到环境变量中，在没有完整路径的情况下再次尝试。现在应该可以了。

现在，您应该对 PATH 变量有了很好的理解，甚至能够向其他编程初学者解释它。

感谢阅读。

*原载于 2021 年 4 月 24 日*[*【https://codingflashlight.com】*](https://codingflashlight.com/the-path-variable-explained/)*。*