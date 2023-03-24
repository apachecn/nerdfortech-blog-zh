# 使用 React Native 的全栈 Youtube 克隆—第 0 部分

> 原文：<https://medium.com/nerd-for-tech/full-stack-youtube-clone-using-react-native-part-0-315d606ca107?source=collection_archive---------9----------------------->

![](img/c60e604dd4e654e2497eacf32ce1787b.png)

# 注意

[这个项目将分为 5 部分*展示，因为这是一个大型项目，很难在一篇文章中解释所有的事情，我也不想让这篇文章长达 1 小时🙂🙂。*](https://youtu.be/fSUao0YzhJI)*还有，这篇文章**不适合完全的初学者。你应该对 redux、react-navigation 等有所了解。虽然我会尽力解释所有的事情。另外，如果你是一个完全的初学者，我推荐你去 udemy 上看一些教程。***

*大家好！！！最近，我用 React native 和 Youtube API 创建了自己的 Youtube 克隆。在进一步讨论之前，让我们向您展示一下我们将要构建的内容和这个项目的一些特性。*

*所以在看完这个视频后，你可能已经对这个项目的所有特点有了一个概念，但这是这个项目的一些特点*

# *应用程序的功能*

1.  **首先，当应用程序启动时，我们会看到随机视频**
2.  **根据用户看到的视频类型，主屏幕上的数据会被相应替换(类似视频)。**
3.  **所有屏幕无限制滚动(用户到达末尾时可以看到更多视频)。**
4.  **搜索视频**
5.  **所有基本的 youtube 功能**

*这个项目将是一个伟大的项目，也是一次实践，因为我们将实现许多重要的概念*

# *我们将要做的一些重要的事情*

1.  ***各种 React 钩子(** useState，useCallback，useEffect，useRef)*
2.  ***React 导航(** createStackNavigator，createbottomtab navigator**)**
    (*你也可以使用各种其他导航，但我想创建一种真正的 Youtube 格式**
3.  ***平面列表***
4.  ***React Redux***
5.  ***模态***

*我们将使用各种常用的重要模型。此外，例如，当我们将创建我们的搜索屏幕，我们将使用一个模块*

```
*react-native-search-header*
```

*这是用来显示一个标题，显示类似的搜索结果，如果你不想使用任何额外的模块，*我已经创建了自己的组件(没有任何模块)，显示相同的功能。**

*顺便说一句，正如你在笔记中看到的，这是该项目的*第 0 部分(简介)*和**这将是我们将在不同部分做的事情。***

# *文章将如何进行*

## *第 0 部分(本文)*

*文章简介*

## *第一部分([链接](https://decodebuzzing.medium.com/full-stack-youtube-clone-using-react-native-part-1-12fd7ed771e2))*

*为项目进行导航工作，并创建标题和选项卡(底部)*

## *[第二部分](/codex/full-stack-youtube-clone-using-react-native-part-2-22ac1b0330d2)*

*处理搜索屏幕(标题和内容)。我们将在最后创建主屏幕，因为我们显示的主要数据将与我们播放的视频相关。*

## *[第三部](https://decodebuzzing.medium.com/full-stack-youtube-clone-using-react-native-part-3-c99f44b127f9)*

*在屏幕上工作，将播放视频，也显示评论。顺便说一下，像“无限滚动”这样的工作将只在我们工作的屏幕上进行。*

## *第四部分*

*创建主屏幕*

## *第五部分*

*添加 redux，做一些样式上的更改，检查错误，解释如何在没有 expo 的情况下构建应用程序，以及如何扩展这个项目。*

*不同模块的说明和安装将仅在特定部分显示。所以，我认为这是该项目的第 0 部分，第 1 部分将很快发布。*

*在那之前保持安全，保持健康*

*谢谢你*