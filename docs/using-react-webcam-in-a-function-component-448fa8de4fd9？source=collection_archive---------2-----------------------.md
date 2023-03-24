# 在功能组件中使用 react-webcam

> 原文：<https://medium.com/nerd-for-tech/using-react-webcam-in-a-function-component-448fa8de4fd9?source=collection_archive---------2----------------------->

在我们的一个项目中，我一直在寻找在 React **功能组件**中使用 [react-webcam](https://www.npmjs.com/package/react-webcam) 的方法。虽然我找到了一些关于使用这个包的有用的帖子，但是我找不到一个可以回答我关于在 React 函数组件中使用它的问题的帖子。这让我写了这个小帖子，希望有人能从中受益！

我们开始吧！

## 安装软件包

您可以根据您的软件包管理器使用以下命令之一安装 react-webcam。

```
//With Yarn
Yarn add react-webcam//With npm
npm install react-webcam
```

## 引用包

您可以按如下方式引用该包:

```
import Webcam from 'react-webcam';
```

## 编写函数组件

我不会深入讨论关于编写一个 React 函数组件的每个细节，因为我相信你已经很熟悉了。此外，我不会涵盖 react-webcam 的一般用法，因为它的作者[在这里](https://www.npmjs.com/package/react-webcam)对它进行了很好的描述。不过，我会将我的函数组件的源代码粘贴在这篇帖子的底部，以便您可以看到实现。

这里需要强调几件事:

## 开始和停止网络摄像头馈送

如果我们需要通过点击按钮或类似事件来切换网络摄像头，那么我们必须有条件地渲染网络摄像头元素。我们可以借助布尔状态变量来实现这一点。请参见下面的代码片段:

```
<div className="camView">
{
  **isShowVideo** &&
  <Webcam audio={false} ref={videoElement} videoConstraints=        {videoConstraints} />
}
</div>
```

例如，我们可以实现一个绑定到按钮的 click 事件的事件处理程序，然后更新状态变量来切换网络摄像头元素的条件呈现。然而，在我的例子中，我使用了两个按钮分别启动和停止凸轮。

对于这个实现，我们必须使用 **useState** 和 **useRef** 钩子。我们需要布尔状态变量的 useState 钩子，我们用它来有条件地渲染网络摄像头。useRef 挂钩用于获取事件处理程序中的网络摄像头元素，以停止摄像头馈送。

## 最后，让我们看一下代码

编码快乐！！