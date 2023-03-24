# 反应基础

> 原文：<https://medium.com/nerd-for-tech/react-basics-7530fd72b456?source=collection_archive---------18----------------------->

![](img/dab4ec4560127dae2ff89894119d9891.png)

**什么是反应:**

R eact 是脸书创建的 JavaScript 库。它是一个构建 UI 组件的工具。React 不直接操作浏览器的 DOM，而是创建 DOM 的副本并保存在内存中。这种复制的 DOM 通常被称为“虚拟 DOM”。React 然后找出已经做了哪些更改，并且只更改 DOM 中的那一部分。

**技能学习 React:**
1。HTML & CSS
2。JSX
3。JavaScript 和 ES6 的基础
4。包管理器( **Npm** )
5。Git 和 CLI

**渲染函数:** React 使用 React DOM.render()函数将 HTML 渲染到网页。这个函数有两个参数，HTML 代码和 HTML 元素。这个函数的目的是在指定的元素中显示指定的 HTML 代码。

**在“根”元素内显示一个跨度**

结果显示在`<div id='root'>`元素中:

这里的 HTML 代码使用了 JSX，它允许您在 JavaScript 代码中编写 HTML 标记。

**JSX:** JSX 代表 JavaScript XML。它允许我们在 React 中编写 HTML。JSX 将 HTML 转换成 react 元素。

1.  **与 JSX:**

如果有人有兴趣在没有 JSX 的情况下工作，那么代码是

**没有 JSX**

从上面的例子中可以清楚地看到，编写最终在运行时将 HTML 移植到 JavaScript 的 JSX 要容易得多。

表达式可以用花括号{}写成 JSX 语。要写多行 HTML 代码，你必须在 HTML 上加圆括号，并把所有内容都放在一个顶级元素中。举个例子，

**功能组件:**组件是一个独立的、可重用的代码块，它将用户界面分成更小的部分。功能组件基本上是一个返回反应元素(JSX)的 JavaScript/ES6 函数。它需要被输出到其他地方使用。

为了使用它，我们需要导入它。

Props: Props 是 properties ant 的缩写，用于在 React 组件之间传递数据。组件之间的反应数据流是单向的(仅从父级到子级)。例如，如果你想把一些东西从应用程序传递到组件，你必须像一个有合适名字的属性一样传递它。这里，我将“name”从 App 组件传递到 Welcome 组件。如果你需要动态传递数据，就用花括号。

因此，在欢迎组件中，我们将获得“道具”中的数据。我们可以这样使用它。

**State:** React 还有另一个特殊的内置对象，叫做 State，它允许组件创建和管理自己的数据。因此，与 props 不同，组件不能用状态传递数据，但它们可以在内部创建和管理数据。
React 组件根据状态中的数据进行渲染(带状态)。状态保存初始信息。因此，当状态改变时，React 会得到通知，并立即重新呈现 DOM 中实际需要改变的部分。有一个名为“setState”的方法，它触发更新部分的重新呈现过程。React 得到通知，知道要修改哪些部分，并快速完成，而无需重新呈现整个 DOM。
在功能组件中，借助 React 钩子我们可以使用这个‘状态’。
我们将使用 React 的使用状态钩子实现一个简单的计数器

而使用这个组件的是 App.js 像这样:

**App.js**

**use effect:**functional React 组件使用 props 和/或 state 来计算输出。如果功能组件进行不以输出值为目标的计算，则这些计算被称为副作用。

useEffect()挂钩接受两个参数:

`useEffect(callback[, dependencies])`；

回调是包含副作用逻辑的回调函数。`useEffect()`在 React 将更改提交到屏幕后执行回调函数。dependencies 是可选的依赖项数组。`useEffect()`仅当渲染之间的依赖关系发生变化时执行回调。
将副作用逻辑放入回调函数中，然后使用 dependencies 参数来控制您希望副作用何时运行。这就是`useEffect()`的唯一目的。

**React 事件:**和 HTML 一样，React 可以基于用户事件执行动作。Reach 和 HTML 有相同的事件:点击、改变、鼠标漫游等。
React 事件被写入 camelCase sytax: `onClick`而不是`onclick`。

如果您想在事件处理程序中传递一个参数，那么您必须将该处理程序封装到一个匿名箭头函数中。

**React CSS:** 要用内联样式属性样式化一个元素，这个值必须是一个 JavaScript 对象。具有两个名称的属性，如`background-color`，必须用 camel case 语法编写。

您还可以创建带有样式信息的对象，并在样式属性中引用它: