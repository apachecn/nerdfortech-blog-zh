# Matt 的花絮# 93—React Native 入门

> 原文：<https://medium.com/nerd-for-tech/matts-tidbits-93-getting-started-with-react-native-acdec42c67f?source=collection_archive---------11----------------------->

![](img/38aac84142469fb3e19295382d341213.png)

[上次我分享了一些 IDE 相关的花絮。](/nerd-for-tech/matts-tidbits-92-a-few-neat-ide-tidbits-9222a69be53f)本周，我有一些快速入门 React Native 的花絮！

我的最新项目是使用 React Native(带 TypeScript)，所以我一直在做大量的学习和准备。

我浏览的一个教程让我使用 React Native 的 TypeScript 模板建立一个新项目。不幸的是，我发现它不适用于 iOS！稍微搜索了一下，发现有一个已知的问题:【https://github.com/facebook/react-native/issues/30836

在我的示例项目中，我采用了注释掉“Flipper”依赖项的方法，这使得应用程序能够成功构建。

我知道 React Native 受到了很多负面报道，但在我看来，React 中建模的基本原则(单向数据流等。)非常棒，原生 Android 开发可以从使用这样的模式中受益匪浅！如果你有兴趣了解更多，这篇来自 React 官方网站的“React 中的思考”文章(以及其余的“主要概念”文档)特别有帮助:[https://reactjs.org/docs/thinking-in-react.html](https://reactjs.org/docs/thinking-in-react.html)

作为一名 Android 开发人员，我以前没有太多使用 Xcode 的理由，但我对开始为 iOS 开发感到兴奋(在过去的 25 年多时间里，我一直是苹果的粉丝)。我确实遇到了一个问题(至少在我有些锁定的工作笔记本电脑上)——通过`gem`命令安装 Cocoa Pods 的标准指令对我不起作用。但是，我可以通过`brew`安装它们来让它工作。

确保在 Xcode->偏好设置->位置中设置 Xcode 命令行工具路径也很重要。如果不是，你将无法在 React Native 中构建 iOS 应用！

对于开始使用 React Native 的人来说，有没有其他有用的花絮？请在评论中告诉我！

有兴趣和我一起在埃森哲出色的数字产品团队工作吗？我们正在招聘！