# 反应钩子:你需要知道的 5 个钩子

> 原文：<https://medium.com/nerd-for-tech/reacts-hooks-5-hooks-you-need-to-know-e0bf61b50cfd?source=collection_archive---------17----------------------->

## 5 种常见挂钩的备忘单…

![](img/ad258fd3a65774dcf7d09a808fcf0c9e.png)

在之前的博客中，我介绍了 React Hooks 的主题，这是一个越来越受欢迎的新特性。在我的项目和志愿者工作过程中，我想分享你可能遇到的 6 个常见问题。

# 使用状态

`const [state, setState] = useState(initialState);`

这个 react 钩子返回一个状态值和函数，我们可以用它来改变这个值。结合起来，我们使用 setState 函数来更新状态。(该值可以是任何数据类型！).

# 使用效果

```
useEffect(didUpdate);
```

这个 react 钩子接受一个函数，这个函数将在应用程序每次渲染后运行。这允许我们在组件状态改变后改变某些值。这就是为什么我们可以认为这些行为是副作用。

# 使用上下文

```
const value = useContext(MyContext);
```

这个 react 钩子帮助我们将多个道具传递给子组件。有时候通过不需要的组件传递道具是多余的。

# useRef

```
const refContainer = useRef(initialValue);
```

这个 react 钩子允许我们返回一个 ref 对象。引用是可以在所有 react 组件上找到的属性。就像名称一样，当组件被安装时，它们引用给定的元素或组件。要使用这个钩子，你必须首先初始化它，这正是这个钩子的用途！

# 用户教育

```
const [state, dispatch] = useReducer(reducer, initialArg, init);
```

这个 react 钩子实际上非常类似于 useState 钩子。它像 Redux 一样工作，所以你可以把它想成状态管理，它依赖于一个 reducer。它接受一个缩减器，并返回当前状态和一个调度方法。