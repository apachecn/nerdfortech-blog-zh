# 事情的状况

> 原文：<https://medium.com/nerd-for-tech/the-state-of-things-575122378da4?source=collection_archive---------18----------------------->

![](img/e72c1e24697df25170303a5c762e988c.png)

图片来源于 [React.js](https://reactjs.org/)

本周，我收到了一些关于如何成长为一名开发人员的有价值的反馈——那就是回到基础。我一直用 React 作为前端库，React 和任何前端库最重要的特性之一是状态管理。有许多管理状态的方法。在这篇博客中，我将回顾三种不同的方法:useState、Redux 和 useContext。

**状态**是 React 中三个关键特性之一:状态、组件和道具。状态被定义为“一个内置的对象…它允许组件创建和管理它们自己的数据”( [Egyi](https://www.freecodecamp.org/news/react-js-for-beginners-props-state-explained/) )。我喜欢把状态看作是一个跟踪用户如何与组件交互的对象，并且基于这种行为，可以呈现或显示某些信息。

## 使用状态

状态管理的第一种方法是 useState，这是一个流行的 React 挂钩。这个钩子“允许你在功能组件中拥有状态变量”(LogRocket)。useState“返回一个有状态的值，以及一个更新它的函数”( [ReactJS](https://reactjs.org/docs/hooks-reference.html#usestate) )。这个钩子利用本地状态管理。为了维护状态并允许其他组件访问它，您需要将它作为道具传递下去。使用状态可以在重新呈现时更改。

```
import {useState} from 'react'export default function WelcomePage(){
  const [counter, setCounter] = useState(0)
  const increaseCounter = () => {
    setCounter(counter +1)
  } return (
    <div>
       <h3>I've clicked on this button {counter} times</h3>
       <button onClick={increaseCounter}>Click here</button>
    </div>
  )
}
```

## 使用上下文

我之前写过关于 [useContext](https://javascript.plainenglish.io/putting-context-into-usecontext-5dbe77c276d1) 的文章，但是 useContext 是另一个 React 钩子。与 useState 不同，useContext 用于管理和维护全局状态管理。一旦为 useContext 分配了内容，就可以在任何组件中导入 useContext。useContext 挂钩的好处是它不会在重新渲染时改变，因为它是全局设置的。

```
import {useContext} from 'react'export const UserContext = React.createContext()export default function WelcomePage(){ const [userInfo, setUserInfo] = useState([]) const fetchAPI = () => {
    fetch('https://localhost:3000/[your route here]')
      .then(response => response.json())
      .then(data => {
         setUserInfo(data)
           return data
       })
  } return (
    <UserContext.Provider
       value={{fetchAPI}}
    >    {/* other data here */ } </UserContext.Provider>
  )
}
```

## Redux

Redux 是为管理状态而创建的 React 库。它被定义为“一个可预测的状态容器，旨在帮助您编写跨客户端、服务器和本机环境表现一致且易于测试的 JavaScript 应用”( [Ighodaro](https://blog.logrocket.com/why-use-redux-reasons-with-clear-examples-d21bffd5835/) )。状态通过存储来维护，您可以指定应用程序中的哪些组件可以访问状态，以及哪些组件可以使用 reducers 和操作来更改状态。

使用 Redux 有很多好处:它易于调试，使状态更可预测，如果您需要您的应用程序扩展，它会很有帮助。对于所有的 React 应用程序来说，**不是，**却是必需的。事实上，Redux 的合著者丹·阿布拉莫夫(Dan Abramov)也说过同样的话:“[如果你发现确实需要它，或者如果你想尝试一些新的东西，请回到 Redux。但是要小心对待它，就像对待任何固执己见的工具一样。](/@dan_abramov/you-might-not-need-redux-be46360cf367)

请查看以下资源，了解更多关于状态管理的信息。

## 资源

[](https://reactjs.org/docs/hooks-reference.html#usestate) [## 钩子 API 参考-反应

### 钩子是 React 16.8 中的新增功能。它们允许您使用状态和其他 React 特性，而无需编写类。这个…

reactjs.org](https://reactjs.org/docs/hooks-reference.html#usestate) [](https://blog.logrocket.com/a-guide-to-usestate-in-react-ecb9952e406c/) [## React 中的使用状态:一个完整的指南

### useState 是一个钩子，它允许你在函数组件中拥有状态变量。你把初始状态传递给这个…

blog.logrocket.com](https://blog.logrocket.com/a-guide-to-usestate-in-react-ecb9952e406c/) [](https://blog.logrocket.com/why-use-redux-reasons-with-clear-examples-d21bffd5835/) [## 为什么要用 Redux？带有示例的教程- LogRocket 博客

### 编者按:这个 React Redux 教程最后一次更新是在 2021 年 1 月 28 日，包括了一个关于 Redux 中间件和…

blog.logrocket.com](https://blog.logrocket.com/why-use-redux-reasons-with-clear-examples-d21bffd5835/)  [## Redux Essentials，第 1 部分:Redux 概述和概念

### 欢迎来到 Redux Essentials 教程！本教程将向您介绍 Redux，并教您如何使用它…

redux.js.org](https://redux.js.org/tutorials/essentials/part-1-overview-concepts#what-is-redux) [](https://redux.js.org/tutorials/essentials/part-1-overview-concepts) [## Redux Essentials，第 1 部分:Redux 概述和概念

### 欢迎来到 Redux Essentials 教程！本教程将向您介绍 Redux，并教您如何使用它…

redux.js.org](https://redux.js.org/tutorials/essentials/part-1-overview-concepts)  [## 你可能不需要 Redux

### 人们往往在需要之前就选择 Redux。“如果没有它，我们的应用无法扩展怎么办？”后来，开发商皱眉…

medium.com](/@dan_abramov/you-might-not-need-redux-be46360cf367) [](https://www.freecodecamp.org/news/react-js-for-beginners-props-state-explained/) [## React.js 初学者-道具和状态解释

### React.js 是每个前端开发者都应该知道的使用最广泛的 JavaScript 库之一。理解…

www.freecodecamp.org](https://www.freecodecamp.org/news/react-js-for-beginners-props-state-explained/)