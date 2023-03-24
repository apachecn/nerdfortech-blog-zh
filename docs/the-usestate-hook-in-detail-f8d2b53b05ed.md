# 详细介绍了 useState 挂钩

> 原文：<https://medium.com/nerd-for-tech/the-usestate-hook-in-detail-f8d2b53b05ed?source=collection_archive---------28----------------------->

你们中的许多人已经熟悉 React 中的钩子，它允许我们在功能组件中使用状态和生命周期方法。在这篇博文中，我想更详细地看看 useState 钩子。(顺便说一下，如果你对为什么要在 React 中切换到功能组件感兴趣，可以查看我的博文 [*这里*](https://javascript.plainenglish.io/why-you-should-stop-using-class-components-in-react-and-switch-to-functional-components-with-hooks-e2c33ae098ce?source=your_stories_page-------------------------------------&gi=d644e7d1a01a) *)。*

![](img/bacebdb76e4091262c6c22868b5cf4b5.png)

照片由[蒂莫西·戴克斯](https://unsplash.com/@timothycdykes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

为什么我们需要**使用状态**钩子？在 2018 年引入钩子之前，我们只能在类组件中使用状态，在 setState 方法中更新它。在很长一段时间里，功能组件不能独立管理状态，只能用来渲染 UI 元素。在 React 16.8 中，这个功能是固定的，现在我们也可以处理功能组件中的状态。

在我们继续讨论使用状态钩子之前，我想提醒你一些使用钩子的重要规则。

*   首先，不要在循环、条件或者嵌套函数内部调用钩子。在任何早期返回之前，总是在函数的顶层使用它们。
*   React 依赖于钩子的调用顺序，这允许您在同一个组件中使用多个状态或效果钩子。
*   不要从常规的 JavaScript 函数中调用钩子；从 React 函数组件或自定义挂钩中调用它们。

# **从使用状态钩子开始**

**useState** 挂钩使您能够向功能组件添加状态。这对于本地组件状态尤其有用。要开始使用状态挂钩，我们首先需要从 React 导入它。

```
Import React, { useState } from ‘react’;
```

现在让我们将**使用状态**添加到组件中。

**useState** 钩子将状态变量的初始值作为参数，初始状态可以是你想要的任何类型(字符串、数字、数组、对象)或函数。初始值将仅在初始渲染时分配。对 **useState** 的每次调用都返回一个包含两个元素的数组。 ***数组的第一个元素是状态变量，第二个是更新变量的值的函数。***

有几种方法可以从返回的数组中检索元素。

首先，使用一个索引:

或者您可以使用数组析构:

第二种方法更可取，因为它允许您像使用任何其他变量一样使用状态变量。

让我们仔细看看 Count 函数中发生了什么。我们调用了 **useState** 钩子，它接受初始值 1 作为参数。之后，我们添加了 incrementNumber 函数，该函数在按钮被单击时改变初始值，并将当前状态加 1。因此，每单击一次按钮，新值就会增加，数字也会增加。

现在让我们看看下一个例子，我们必须使用几个状态挂钩。

当我们单击按钮时，初始值只会增加 1，尽管我们期望它会增加 2。为了让这段代码工作，您应该在第一个参数中调用函数，该函数将返回数字的前一个状态。每次调用 setNumber 时，数值都会改变。

现在，每次我们点击按钮，我们的结果将增加 2。

在一个组件中可以使用多少个 **useState** 钩子没有限制。你可以多次调用 **useState** 或者将所有内容都放入一个对象中。

```
const [dog, setDog] = useState({
  name: '',
  breed: '',
  age: 0
});
```

现在，我们可以使用 dog.name、dog.breed 等检索 dog 值。

一旦应用程序增长，状态也会成比例增长，因此在 **useState** 中存储多个值似乎不再是一个好主意。在这种情况下，您可以尝试使用更适合管理多值状态的 **useReducer** 钩子。

今天我研究了如何使用 **useState** 钩子，并且花了一些时间来回忆使用钩子的一些基本规则。感谢阅读。希望你学到了新东西。