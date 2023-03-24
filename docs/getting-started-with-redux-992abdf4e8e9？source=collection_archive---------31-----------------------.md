# Redux 入门

> 原文：<https://medium.com/nerd-for-tech/getting-started-with-redux-992abdf4e8e9?source=collection_archive---------31----------------------->

状态管理是任何应用程序的一个关键方面。应用程序的状态是应用程序中任何时间点的值。应用程序中状态的一个常见示例是，当有一个简单的表单时，每个表单输入框中的初始值将是一个空字符串，一旦用户在框中键入输入，状态就会发生变化，状态中的值也会发生变化。

每当用户与应用程序进行交互时，许多值都有可能改变，并且会对状态产生影响。在一个小的应用程序中，比如一个简单的表单，跟踪这些状态变化是相当容易的。随着应用程序的增长，跟踪在应用程序的各个部分和应用程序的不同功能中使用和更改的状态会变得更加困难。

为了解决这个问题，状态管理变得很重要。Redux 库就是这样一个流行的状态管理工具。

Redux 库可以用于各种前端框架。在 Redux 库中，编排状态管理的三个主要部分是 ***、存储、缩减器和动作*** 。

# 商店

编排状态管理的第一部分是存储。存储将保存状态和负责更改、监听和检索状态的代码。通过从 Redux 库中调用 create store 函数并传入一个 reducer 函数来创建一个存储。

```
import { createStore } from "redux";
import mainReducer from "../reducers/index";const store = createStore(mainReducer);export default store;
```

# 该减速器

传递到存储中的 reducer 负责创建状态。这个缩减器是 redux 库中的一个函数，它有两个参数。当前状态和动作(改变状态的指令)。

redux reducer 的关键特性是它不会就地改变状态，它是一个纯函数。纯函数是指对于给定的输入会产生相同输出的函数。redux reducer 将返回一个新的状态，而不是原地改变状态。例如，React 框架不使用纯函数来改变状态。为了改变状态，您调用 setState()方法。这种方法就地改变了原始状态。

要查看减速器的运行情况，我们可以查看减速器的基本功能。

```
const state = {
 reviews: []
};function mainReducer(state , action){
 return state;
}
```

这个示例缩减器目前没有做太多工作。它只是返回状态。接下来，我们将深入研究这个动作做了什么，以及它在与 reducer 一起产生状态时的作用。

# 行动

动作是为了通知状态需要改变而分派的对象。这个动作有两个属性。类型和有效负载。type 属性是更改状态时将采取的操作类型。有效负载是将被合并到状态中的变更。type 属性是必需的，payload 属性不是必需的。这里有一个简单的例子。

```
payload: { title: 'New Restaurant Review', id: 1 }export function addReview(payload) {
  return { type: "ADD_REVIEW", payload }
};
```

在这里，您可以看到 action 对象被包装在一个函数中。这是 Redux 中的最佳实践。这使得对象的创建被抽象掉了。addReview 函数产生一个 action，其 action 类型为添加新的评论和一个包含要添加的新评论的有效负载。

# 把所有的放在一起

现在，为了展示 reducer 和 action 一起工作来产生一个状态，我们有下面的代码

```
const state = {
 reviews: []
}function mainReducer(state, action){
 if(action.type = "ADD_REVIEW"){
  return Object.assign({}, state, {
   reviews: state.reviews.concat(action.payload)
  });
 }
 return state
}export default mainReducer;
```

在这里，您可以看到这个 reducer 接收了一个操作，并且在 if 条件中检查了操作类型。如果类型是“ADD_REVIEW”类型，那么我们返回一个新的状态，小心地保持 reducer 是一个纯函数。为了保持它是一个纯粹的函数，它使用 Object.assign 将一个 reviews 属性分配给一个新的对象，并进行 concat，以便在不改变原始 reviews 数组的情况下向 reviews 数组中的当前值添加一个新的 review。

现在你可能在想——一个动作是如何发送的？，如何在状态改变时执行代码？还有怎么随时找回状态？为此，redux 库有三个可以在存储上调用的方法。这些方法是 getState、dispatch 和 subscribe。

*   getState 方法(store.getState())检索应用程序的状态。
*   分派方法(store.dispatch(action))发出一个动作来改变状态。
*   Subscribe 方法(store . Subscribe(()= > console . log(' run some code '))用于监听状态的变化，当状态发生变化时，可能会运行一些代码作为响应。