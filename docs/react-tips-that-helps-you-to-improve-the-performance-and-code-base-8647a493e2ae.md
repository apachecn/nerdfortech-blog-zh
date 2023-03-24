# 帮助您提高性能和代码库的 React 技巧。

> 原文：<https://medium.com/nerd-for-tech/react-tips-that-helps-you-to-improve-the-performance-and-code-base-8647a493e2ae?source=collection_archive---------27----------------------->

![](img/b768552acbb021f6768e6e5e586244df.png)

F 在过去的八个月里，我们已经建立了一个指导平台，名为 [SEF](https://sefglobal.org/) 开发团队，名为 [ScholarX](https://sefglobal.org/scholarx/2021/) 。该项目的技术栈是 React，前端是 typescript，后端是 spring boot。我大部分时间都在前端工作。经过几个月的工作，我们已经完全准备好推出它。在上次的开发团队会议中，我们讨论了如何改进项目的性能和代码库。所以我被分配到这方面的研究，我想如果我写一篇文章，让其他人也能学到一些东西，那会很酷。

这对于我们的项目来说是特定的，但是对你也是有用的。让我们开始吧。

## 创建可重用组件

使用 react，可重用性很重要。通过在整个项目中重用组件，您可以实现一致性，并且将阻止您创建难以维护的庞大组件。为了提高组件的可重用性，您可以使用一个功能=一个组件的规则。

## 使用片段

```
return
 ( **<div>** 
      <Component1 />
       <Component2 />
**</div>** )return
 ( **<>** 
      <Component1 />
       <Component2 />
**</>** )
```

总是在 Div 上使用 Fragment。它保持了代码的整洁，并且对性能也有好处，因为在虚拟 DOM 中少创建了一个节点。

## 进口订单

在 JS 文件中对导入进行排序有很多好处。首先，可以更容易地看到你从特定的包中导入了什么，如果你将它们分组，你可以很容易地区分哪些是从第三方包导入的，哪些是本地导入的。

经验法则是这样保持导入顺序:
1。内置
2。外部
3。内部的

```
import React from 'react';

import { PropTypes } from 'prop-types';
import MentorCard from 'components/MentorCard';

import pladeholderImage from '../../assets/images/placholder.png';
import colors from '../../styles/colors';
```

## 不要在字符串中使用花括号

通常当我们传递字符串时，我们会这样使用它，

`name={“takumi”}`

但是我们可以像这样使用`name=“takumi”`来代替它。

## 破坏道具

使用重构使你的代码干净。

代替这个，

```
console.log(props.user.name)
```

我们可以这样使用它，

```
const { user } = props
console.log(user.name)
```

## 创建布局

当你创建不同的视图时，最好使用布局作为你的基础，否则你必须花时间基于每个视图编写 css 来保持布局的一致性。

## TypeScript 不使用任何类型

我知道这与反应无关，但大多数情况下这两者是相辅相成的。如果你正在使用`any`的话，不要使用`any`类型。使用类型脚本是没有意义的。

![](img/2c29988e05f0f190af6252b37f99045c.png)

## 类型脚本接口与类型

这个人也没有反应过来。说到接口和类型，你可能会认为是一样的。是的，这是相似的，但关键的区别是，类型不能被重新打开来添加新的属性，而接口总是可扩展的。

例如，这可以这样做，

```
interface Window {
  title: string
}

interface Window {
  ts: TypeScriptAPI
}
```

但是这个不行，

```
type Window = {
  title: string
}

type Window = {
  ts: TypeScriptAPI
}
```

现在，您可以根据自己的需求在它们之间进行选择。

这些是有助于改进我们代码库的一些东西。有许多最佳实践。在 SEF，我们正在跟踪他们中的大多数。如果你有兴趣加入 SEF，你可以给我们的任何一个[项目](https://github.com/sef-global)发一份 PR，你也可以加入我们的[话语](https://sef.discourse.group/)小组。

说完，我们来到了文章的结尾。别忘了看看我的其他文章。下次再见了。在那之前保持安全！✌️