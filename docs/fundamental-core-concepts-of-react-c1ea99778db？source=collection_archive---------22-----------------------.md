# React 的基本核心概念

> 原文：<https://medium.com/nerd-for-tech/fundamental-core-concepts-of-react-c1ea99778db?source=collection_archive---------22----------------------->

![](img/39bad6d34703f15ef363dc23c47dc14e.png)

React 是一个开源的 javascript 库。它于 2013 年 5 月 29 日首次发布。关于 React 有一个大问题，它是库还是框架。为了得到这个答案，你需要知道什么是框架。框架是预先构建的结构。从一个框架开始，你不需要包含任何东西。对于一个创业团队来说，这将是一个很好的解决方案，因为一切都在那里得到优化。此外，您需要以特定的方式编码。

但是在 React 中，您需要为特定的任务使用不同的库。对于使用 React 构建灵活的应用程序来说，这是一个很好的解决方案。React 允许以自己的方式构建任何类型的应用程序。这里我将讨论 React 的一些重要主题。让我们开始吧。

## 树协调算法

使 React 更有效的最重要的想法是虚拟 dom。react 使用 web 浏览器的方式略有不同。当 react 第一次呈现数据时，它创建一棵树的虚拟表示，并将数据存储在内存中。当我们更新数据时，react 创建了树的另一个虚拟表示。然后将它与先前存储的树进行比较。然后找到需要更新的代码，替换需要更新的代码。这就是原因，它的工作速度更快。它在 react 树协调算法中被称为。

## React DOM 如何呈现数据

在 web DOM API 中，如果我们想要呈现数据，我们基本上使用 getElementbyId 或 querySelector 选择节点，然后我们设置 innerHTML 或 innerText。但是在 React API 中就不同了，我们需要在 ReactDOM.render 里面渲染代码，在这个里面我们用 React.createElement 和 createElement 需要两点。一个是什么需要渲染，另一个是哪里需要渲染。下面是一个代码示例。

## 差分算法

React 在实际 DOM 中的工作方式非常有趣。如果我们想尝试用浏览器 web API 更新 DOM 中的一些东西。所有的 DOM 树将被一次性更新。但是使用 React API，如果我们想要更新特定的东西，所有的 DOM 树都不会被更新。只是只有特定的代码会被更新。这就是所谓的 React 差分算法，它使 React 更加强大。

## JSX

JSX 的意思是 Javascript XML。JSX 允许在 JavaScript 中修改 HTML 代码，也允许在 HTML 代码中修改 JavaScript。这个很厉害。但是在浏览器中 JSX 不起作用。React API React.CreateElement 渲染的 JSX 代码，浏览器可以理解这些代码。

## React 中编写组件的规则

当我们在 react 环境中编写组件时，我们不是在编写 HTML，而是在编写 JavaScript 对象。当我们正确地编写一个组件时，它会被 Babel(编译器)绑定，并在浏览器中可视化。写组件名有一个规则，组件名要大写。因为，当 Babel 绑定 JavaScript 代码时，如果它获得小写的组件名，它会将其作为 HTML 标记名。它将在浏览器中抛出一个错误。因此，组件名应该用大写字母书写。

## React 中的道具

在 HTML 元素中，我们传递一些交互的属性。类似地，在 React 中，我们传递道具。Props 允许进行更多的交互，并允许将数据从一个组件传递到另一个组件。析构是在组件中使用道具的好方法。

## JSX 表情

在 JSX，JavaScript 表达式可以在成对的花括号内使用。它允许在 HTML 代码中编写 JavaScript，但是它有使用限制。不能在花括号内声明 if-else 语句，甚至不能声明 for 循环。但是您可以在括号内使用三元运算符或映射。

## 类别与功能组件

在 React 中有两种使用组件的方法。一个是基于类的组件，另一个是功能性的。在 16.8(React 版本)之前，React 开发人员大多使用基于类的组件来维护复杂的代码。因为维护类内组件的状态很容易。2019 年发布 16.8 后，React 自带“React hooks”。它使得维护功能组件中的状态变得更加容易和高效。如今功能部件越来越受欢迎。因为它易于维护、功能强大且可伸缩。

## 组件的优势

当我们开始在应用程序中编写复杂的代码时。它失去了可读性，但使用组件我们可以增加更多的可读性。不仅仅是可读性，绕过道具我们可以让它重用。

## 钩住

React 钩子非常强大。它使功能组件使任何复杂的系统变得更容易，并用 useState 维护状态。React 钩子有好几个，最常用的有 useState、useEffect、useRef、useReducer、useContext。这些钩子有不同的用途。