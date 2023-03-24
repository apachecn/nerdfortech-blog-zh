# 停止直接访问 React 上下文

> 原文：<https://medium.com/nerd-for-tech/stop-accessing-react-context-directly-ec37ae108b7c?source=collection_archive---------2----------------------->

![](img/d83064935042661cfdb0f3e0fb3eb5fc.png)

我正在为我的一个同事做代码审查，偶然发现了一些代码，这些代码立即在我的脑海中发出了一个危险信号。

```
<Checkbox label={translationContext.getTranslation(checkboxLabel)}/>
```

这类似于我写了很多次的代码，但是在这次特别的回顾中，这一行代码引起了我的注意。我开始思考，*我们要在这个组件中公开整个翻译上下文吗？如果我们需要改变它呢？一定有更好的方法！所以我开始查看我们剩下的代码，发现这不是这个开发者的错。这是一种到处都是的模式(或者我应该说是反模式)!在其他部分，我找到了一些这样的例子:*

```
... inside functional component
const getNextSection = () => {
  valid = context.actions.validateCurrentSection()

  if(!valid){
    return context.actions.showErrors()
  } else {
    return context.actions.getNextSection()
  }
}
```

我们已经创建了这个完整的模式，其中我们的上下文对象拥有这个`actions` 成员，它存储了处理上下文中状态变化的所有函数。幸运的是，我们使用了 typescript，所以所有存在于`actions`中的函数都被记录下来，并且可以自动完成。但是这意味着每个组件都需要访问整个上下文，不管它实际需要的信息有多少。更糟糕的是，这意味着每次上下文改变时，整个组件都要重新渲染！即使发生变化的数据实际上并未被相关组件使用。

这也引入了一些其他问题。如果我们将来需要将这些功能转移到不同的环境中，会怎么样呢？我们需要跟踪每次通过`context.actions.functionIWantToMove`访问该函数，并改变调用该函数的上下文，这意味着我必须给该组件完全访问第二个上下文的权限。所以它可以访问一个数据或者调用一个函数。

我做了一个快速的责备，看看谁可能实现了这样一个不负责任的模式，当然，是我。显然这是一种可怕的做事方式。我们需要一个更好的解决方案。

# 反应钩子救援！

![](img/092eb7b1ab2dd775c48eab00dc5a4c7d.png)

React 真正擅长的事情之一是分离关注点。没有什么比钩子更能体现这一点了。我回到了最初导致红旗的那一行，我扩展了代码以提供更多的上下文:

```
import React, {useContext} from "react"
import {TranslationContext} from "../contexts/translation-context.tsx"const CheckboxGroup = ({options}) => (
  translationContext = useContext(TranslationContext)
  <div>
    {options.map(option => (
      <Checkbox label={translationContext.getTranslation(option.text) />
      ))
    }
  </div>
)
```

我们很快就能制造出一个钩子，以一种更干净的方式完成这个任务。我们**的确**最终不得不将`Checkbox`组件包装在一个包装器组件中，但我认为这是值得的。

```
import React from "react"
import {useTranslate} from "../translate"const TranslatedCheckbox = ({label}) => {
  const translatedValue = useTranslate(label)

  return <Checkbox label={translatedValue} />
}
```

然后我的外部`CheckboxGroup`组件看起来像这样:

```
const CheckboxGroup = ({options}) => (
  <div>
    {options.map(
      option => <TranslatedCheckbox label={option.label}/>
     )
    }
  </div>
)
```

这最终会好得多。它不仅更干净，而且通过隐藏翻译的实现细节，修复了我们的抽象漏洞。现在，真正进行翻译的组件需要了解`TranslationContext`。

我只想指出，这里的代码片段是为了说明一个观点而设计的例子，经过编辑后只显示了相关的部分。我知道他们不会照原样运行。希望这一点仍能被理解。