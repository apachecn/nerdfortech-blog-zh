# 反应钩子:我从中学到了什么？

> 原文：<https://medium.com/nerd-for-tech/react-hooks-what-i-learnt-from-abd070c810b6?source=collection_archive---------11----------------------->

[React Hooks](https://epicreact.dev/learn/) 是[肯特·c·多兹](https://twitter.com/kentcdodds)的课程，也是[史诗 React](https://epicreact.dev/learn) 的一部分

![](img/84e8eab4be4dbaf01a6c8b2bce04872a.png)

**免责声明:**本文的目的不是总结课程内容，也不是回顾它，而是记录它对我的影响。

在[上一篇博客](/nerd-for-tech/react-fundamentals-what-i-learnt-from-710134ade203)中，我忘了提到我对学习结构的印象——这让我感觉像是在大学里，我需要阅读指定的文章和做功课。

## 第一点让我印象深刻的不是主要内容

不出所料，主题是关于常见的钩子，即 useState、useEffect、自定义钩子、useRef。我没有预料到的是，我已经学会了这些挂钩的实践，其中一些可以应用于其他框架，例如受控输入、[提升状态、](https://reactjs.org/docs/lifting-state-up.html)[使用状态而不是加载布尔值](https://kentcdodds.com/blog/stop-using-isloading-booleans)，如下所述。

## 第二点，当我看第二遍的时候，我仍然在学习

当我第一次看的时候，我相信我可以捕捉到几乎所有的东西。第二次，我可以捕捉我忽略的细粒度信息，例如关于钩子流的细节。

## 第三点从简单的设计开始

正如在[之前的博客](/nerd-for-tech/react-fundamentals-what-i-learnt-from-710134ade203)中提到的，我喜欢这些课程是因为它们创建了我的心智模型——首先向我展示如何用更简单的代码实现事情，然后指出需要改进的项目。

**第一个例子** : Kent 并没有立即销毁 useState 的返回值，而是展示了如何在将每一项赋值给变量之前将该值作为数组获取——这对于每天都看到这种模式的中级 React 开发人员来说可能并不明显。

之前:

```
const array = React.useState(initialName);
const name = array[0];
const setName = array[1];
```

之后:

```
const [ name, setName ] = React.useState(initialName);
```

**第二个例子**:该课程从基本的钩子开始，即 useState 和 useEffect，然后演示创建自定义钩子有意义的更复杂的场景。

## 第四点，我以前在其他框架中看到过这些相同的模式

**第一个例子**:我对[提升状态](https://reactjs.org/docs/lifting-state-up.html)和[钻道具](https://kentcdodds.com/blog/prop-drilling)并不感到意外，因为这些都是我在 Angular 里做的。

**第二个例子**:我在使用 isLoading(包括 errors 对象和 response 对象)定义应用程序的状态时遇到过类似的问题。这种模式引入了复杂的 if 语句逻辑，导致了应用程序的技术缺陷和可维护性。本课程建议使用状态机[或 enum](https://kentcdodds.com/blog/stop-using-isloading-booleans) 作为更好的选择。

## 第 5 点:我看到了以前从未见过的模式

这是我第一次看到[错误边界](https://reactjs.org/docs/error-boundaries.html)概念，它的声明性给我留下了深刻的印象。

# 结果

如前所述，我们的目的不是总结课程内容。我只记下对我有用的东西。你可能会注意到，我并没有过多地提到 React 钩子，这是本课程的主题。这些都是我期望看到的，我对此很满意。另一方面，我提到了从 React hooks 引入的学习方法和概念。