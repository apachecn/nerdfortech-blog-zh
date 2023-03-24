# 完成处理器——简单解释

> 原文：<https://medium.com/nerd-for-tech/completion-handlers-simply-explained-b9d70c72462c?source=collection_archive---------2----------------------->

## iOS 编程指南

![](img/b7d2200ae49c48ce1704859b90adc670.png)

由宏向量创建—[www.freepik.com](http://www.freepik.com)

完成处理程序是 iOS-Swift/Obj-C 中广泛使用的概念之一，有些人甚至每天都在使用它，或者至少每天都会看到它。我喜欢它的工作方式。它封装了复杂/繁琐的实现方式，只给你好的一面。

但在我早期 iOS 开发的日子里却不是这样。学习它们就像训练一条龙。你先是讨厌他们，然后你会一直爱着他们。相信对于新手来说还是一样的情况。

![](img/32fe689e2baa353d2fb1bcad294314e3.png)

来吧伙计们，让我们驯服他们…

在使用完成处理程序之前，您需要知道两件事。

1.  关闭
2.  阻碍

# 什么是闭包？

> *"闭包*是自包含的功能块，可以在代码中传递和使用。Swift 中的闭包类似于 C 和 Objective-C 中的块，以及其他编程语言中的 lambdas。”— [*Swift docs*](https://docs.swift.org/swift-book/LanguageGuide/Closures.html)

在这里了解更多关于闭包的信息。

# 什么是积木？

> “您可以使用块来组成函数表达式，这些表达式可以传递给 API，可以选择存储，并由多个线程使用。块作为回调特别有用，因为块既包含回调时要执行的代码，也包含执行过程中需要的数据。— [*苹果开发文档*](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Blocks/Articles/00_Introduction.html)

[点击此处了解更多街区信息。](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Blocks/Articles/bxGettingStarted.html)

# 什么是完成处理程序？

> “TL；DR:完成处理程序是一个闭包([“一个自包含的功能块，可以在你的代码中传递和使用”](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html))。它作为参数传递给一个函数，然后在该函数完成时被调用。- [Grok Swift](https://grokswift.com/completion-handler-faqs/)

要点是在所有的工作完成后，如果需要的话，它会给我们状态，有时有数据或错误。

我强烈建议在深入学习之前学习一些关于闭包和块的基础知识。

记住这一点。

*完成处理程序*只不过是一种使用块/闭包实现回调功能的方法或技术。

**是时候看一些代码示例了。**

![](img/bf448abbf300bd04611628d11196c9c9.png)

创建一个带有两个参数的函数:一个用于传递一个布尔值，它将帮助我们完成主要操作，另一个用于完成处理程序。

```
func yourFunction(withArg arg: Bool, completion: (Bool) -> ())print(“Gets executed first”)// Perform the operations and get the boolean resultvar localValue:Boolif arg {                // |localValue = true       // | Our major operation 😂} else{                 // |localValue = false      // |}                       // |// Send the result to the other endcompletion(localValue)}
```

在需要的地方调用函数。如果你看下面，你将看不到我们主要操作的任何部分。您只是获得了布尔成功结果。现在就看你如何明智地使用它了。

```
yourFunction(withArg: true, completion: { (success) -> Void in print("Gets executed second")    if success { // Use the value send from the other end  

        label.text = "True"              } else {     

        label.text = "False"    

    }       

})
```

**让我们看看一些 Objective-C 代码。**

创建一个有三个参数的函数，两个用于传递输入值，这将有助于我们的数学运算，另一个用于完成处理程序。

```
-(**void**)addValues:(**int**)inputA withNumber:(**int**)inputB andCompletionHandler:(**void** (^)(**int** result))completionHandler{**int** result = inputA + inputB;     // |- Our math operation😁completionHandler(result);}
```

在需要的地方调用函数。

```
[**self** addValues:1 withNumber:2 andCompletionHandler:^(**int** result) {*// Log the result***NSLog**(@"The result is %d", result);}];}
```

如果你看上面，你不会看到我们数学运算的任何部分。你只是得到了答案作为结果。你想怎么用就怎么用吧。

就这样，伙计们。我该离开了。

![](img/7777dfed840e4d55f1f87c0897b524e8.png)

多读书。多练习。否则你很快就会被淘汰。

Swift 示例练习场地:

[https://github . com/Rajaikumar-IOs dev/Simple-Completion-Handler-Example](https://github.com/Rajaikumar-iOSDev/Simple-Completion-Handler-Example)

[](https://github.com/Rajaikumar-iOSDev/Simple-Completion-Handler-Example) [## rajaikumar-IOs dev/简单完成处理程序-示例

### 🏺这是一个在 iOS 中使用完成处理程序(Closures)的例子——Swift Permalink 未能加载最新提交…

github.com](https://github.com/Rajaikumar-iOSDev/Simple-Completion-Handler-Example) 

如果您需要 Objective-C 的示例项目，请告诉我。