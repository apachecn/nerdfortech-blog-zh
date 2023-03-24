# Unity 中的例外

> 原文：<https://medium.com/nerd-for-tech/exceptions-in-unity-cc52df30e8a7?source=collection_archive---------9----------------------->

![](img/c44a6a8f4fbf93f0d290f4447da899fe.png)

异常是在程序执行过程中发生的问题，它会导致程序异常结束。在创建程序时，如果希望代码正常工作，就需要处理这些异常。有两种不同类型的异常。

首先是检查异常。这些异常是编译代码时出现的语法错误。这种类型的异常将导致代码下面出现红色曲线。

![](img/ca5a9e773e88ef9dbf9165a88bc4df8c.png)

第二种类型是未检查的异常。这些异常是代码执行过程中出现的逻辑错误。这些错误不容易被发现，因为它们不会在您的代码下产生红色的曲线。

![](img/38da8dc2060ab4fb0bcb63c49a071dbc.png)![](img/c18b75e4f0870668f6331b8b8fbf06a6.png)

处理这些异常的一种方法是使用 try-catch 块。try-catch 块通过尝试执行代码块来工作。如果发生异常，它将被捕获并执行不同的代码块。这将捕捉任何异常，并允许程序继续执行。

![](img/68b63a6e34a721d24099552242738fb3.png)![](img/a2f543b781fbc3c68d915b6c4b123b7b.png)![](img/b2c9a95afe0c7b006d2fe32aa4d5be4c.png)![](img/91b94528627b0760af3b0f68f5d56b9f.png)![](img/127ad841b1dc0333c5f508c729bc8d6d.png)