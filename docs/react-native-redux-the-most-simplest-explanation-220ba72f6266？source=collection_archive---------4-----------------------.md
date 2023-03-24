# React Native — Redux，最简单的解释

> 原文：<https://medium.com/nerd-for-tech/react-native-redux-the-most-simplest-explanation-220ba72f6266?source=collection_archive---------4----------------------->

![](img/49a9bd58e8628b10031c2c4d3418af0f.png)

如果你正在阅读这篇文章，那么你想了解 React Native 中的 Redux，并且已经阅读了一些博客和观看了一些 youtube 视频，但事情对你来说仍然不清楚！！

别担心，几个月前当我第一次尝试理解 Redux 时，我也在同样的地方，肯定这不是一个复杂的话题，但很难一下子掌握。对于初学者来说，网上的例子很难理解。所以我会创建一个最简单的例子，这样你会非常清楚地理解它。

首先是术语

1.  **动作** —您想要执行的动作，例如登录用户、从 API 获取帖子等。基本上它只是一个通常用 const 关键字创建的变量。例如:**const INCREMENT _ COUNTER = " INCREMENT _ COUNTER "。**
2.  **动作创建器—** 创建动作的箭头功能。这是必需的，因为有时动作不仅仅是可变的，一些额外的细节可能与它相关联。
3.  **Reducer —** 是一个纯函数，将当前状态和动作作为输入，返回更新后的状态。大多数状态管理逻辑都发生在这里。
4.  **Store —** 简单来说就是包含应用程序整个状态的容器。
5.  **use selector(Hook)——**该钩子用于订阅状态变化，这些变化稍后可用于渲染或其他一些过程。
6.  **useDispatch(Hook)——**用于调度动作。您将与它一起传递操作创建者，以便可以执行所需的操作。例如:如果您调度**INCREMENT _ COUNTER**action**，**那么具有计数值的状态变量将被更新。

现在让我们开始，我上面解释的所有术语都将在下面的例子中使用。

1.  **创建项目(我假设您使用 react-native CLI，如果您使用 expo，只需参考 expo 文档** [**此处**](https://docs.expo.io/workflow/expo-cli/) **)，了解命令。)**

```
**react-native init Redux_pracice** (execute this command on terminal at your convenient directory)
```

2.将目录更改为新创建的项目

```
cd Redux_pracice
```

2a。安装 redux 和 react-redux，在 react 内部使用 redux 需要这 2 个包。

```
npm install redux react-redux
```

3.从 App.js 中删除所有默认内容，并保留如下所示的内容。

```
import React from 'react';import { createStore, combineReducers } from "redux";const store = createStore(rootReducer);export default App = () => {return ();};
```

现在让我们从实际的 redux 实现开始。

4.在根目录下创建一个名为**的文件夹存储**。

5.创建一个名为 **actions，reducers** 的文件夹

6.在 actions 文件夹中创建一个名为 **modifyCounter.js、**的文件，下面是 action creator 的内容。

```
export const INCREASE_COUNTER = "INCREASE_COUNTER"; **//Action**export const DECREASE_COUNTER = "DECREASE_COUNTER"; **//Action**export const incrementCounter = () => { **//Action Creator**return {type: INCREASE_COUNTER}}export const decrementCounter = () => { **//Action Creator**return {type: DECREASE_COUNTER}}
```

如上所述，这里有两个操作，一个是 incrementCounter，另一个是 DECREASE_COUNTER，还有两个名为 incrementCounter、decrementCounter 的操作创建者。在这种情况下，动作创建者只有类型属性，但是在您的实时项目中，您可能需要将许多属性与动作相关联。根据添加的属性。

**注意:动作创建者不是必须的！！仅仅动作就足以执行状态管理，但是建议使用动作创建者，因为将来如果任何新的属性伴随着动作出现，变化将是最小的。**

7.在 **reducer** 文件夹中创建一个名为 **modifyCounter.js** 的文件，并添加以下代码。

```
import { DECREASE_COUNTER, INCREASE_COUNTER } from "../actions/UpdateCounter";
// **Previously created actions are imported here**const initialState = {count: 0}**//reducer named modifyCounterReducer is created below.**const modifyCounterReducer = (state = initialState, action) => { switch (action.type) { case DECREASE_COUNTER: let previousCount = state.count; previousCount--; return {...state, count: previousCount} case INCREASE_COUNTER: let nextCount = state.count; nextCount++; return {...state, count: nextCount} default: return state }}export default modifyCounterReducer
```

在上面的 modifyCounterReducer(一个 Reducer 函数)中，我们将当前状态和动作作为输入，并将新状态作为输出返回。这里我们切换 action.type 并根据它增加或减少计数。

仔细观察状态变化这里，我们从状态变量中读取当前状态，并增加或减少它，然后返回更新后的值。这里要注意的重要一点是，状态本身没有被修改，变量 **initialState** 是应用程序的实际状态。我们把它作为一个论点来发挥作用，并修改论点。因为这个**状态在 redux 中是不可变的。我们也可以使用**[**redux-dev tools**](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=en)**chrome 扩展来调试 redux 的状态变化，只是因为我们没有修改实际的状态。**

现在，我们已经完成了最重要的步骤，剩下的就是创建存储和修改应用程序的状态。

8.在根层级创建一个名为 **screens/home.js** 的文件夹。下面应该是它的内容。

```
import React from 'react'import { View } from 'react-native'function Home() {return (<View></View>)}export default Home
```

9.现在，在 App.js 中导入主页并创建商店和提供者，新的 App.js 文件如下所示。

```
import React from 'react';import { createStore, combineReducers } from "redux";
import Home from '../screens/home.js'const rootReducer = combineReducers({ modifyCount: modifyCounterReducer});**// CreateStore takes reducer as argument, combineReducers is not //required in our case, but I'm keeping it, so that if you have //more than one reducer you would be using above syntax.** const store = createStore(rootReducer); export default App = () => {return (**//Provider makes the store available to all the components across //which it is nested. In our case we had just home, so provider is // around it. If you have complex navigation then keep it at the top //of it.**<Provider store={store}> <Home /></Provider>);};
```

10.现在让我们访问 Home 组件中的商店。以下是 Home.js 的更新内容

```
import { Text, View, TouchableOpacity, Alert } from 'react-native'import { useSelector, useDispatch } from "react-redux";import { decrementCounter, incrementCounter } from '../../Store/actions/UpdateCounter';export default function Home() {**// as said before useSelector hook is used to subscribe to state //changes, on RHS we have state.modifyCount here modifyCount is the //alias name of reducer that we have in store creation at app.js**  
// **If you have not used combineReducer then just use state.count** const currentCount = useSelector(state => state.modifyCount.count)**// This gives access to all the dispatch events available in the //store**
const dispatch = useDispatch();return (<View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>**// By default it will be zero. Because initialState is zero.**
<Text>{currentCount}</Text>**// Here your dispatching the event by passing the action creator
// Do not pass action itself !! as your switching the action.type
// in reducer**<TouchableOpacityonPress={() => {dispatch(incrementCounter())}}><Text>Increment</Text></TouchableOpacity><TouchableOpacityonPress={() => {dispatch(decrementCounter())}}><Text>Decrement</Text></TouchableOpacity></View>)}
```

遵循上面解释的简单步骤，您的 react 本机应用程序将获得 Redux 的功能。一旦你理解了这个例子，然后一步一步地将不同组件的状态转移到 Redux store，在需要的地方创建多个动作创建者和减少者。

同一作者的其他文章

1.  [JavaScript 中的一切如何都是对象？](https://mevasanth.medium.com/how-everything-is-object-in-javascript-a4164d7e6a2d)
2.  [如何在 React Native 中动态使用本地图像](https://mevasanth.medium.com/how-to-use-local-images-dynamically-in-react-native-71b9f3b0db20)
3.  [JavaScript 中的事件冒泡和涓滴/捕捉——常见面试问题](https://mevasanth.medium.com/event-bubbling-and-trickling-capturing-in-javascript-common-interview-question-8817238df70a)

在 mevasanth.medium.com[阅读更多信息](https://mevasanth.medium.com)