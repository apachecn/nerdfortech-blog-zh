# React/Redux 中的 Thunks 和动作创建器

> 原文：<https://medium.com/nerd-for-tech/thunks-and-action-creators-in-react-redux-3e34835e593b?source=collection_archive---------15----------------------->

![](img/d1ef3c66d47172952e0b75638577c3dc.png)

嘿，怎么了！希望你反应良好。所以，首先我要为在讨论 redux 基础知识之前直接深入到高级 redux 中而道歉，但我向你保证，在不久的将来，你将能够从我的博客中学习 redux 基础知识。但是如果你知道 redux 基础知识，并且知道使用 redux 工具包，那么这可能是一篇很好的文章，可以让你的应用程序状态管理技能更上一层楼。

## 当行动创造者开始行动时。

当使用 redux 时，不能使用 useEffect 或 react 生命周期方法在片内调用副作用。那我们怎么做呢？你可以用两种方法来做这件事。一种方法是调用组件内部的副作用，另一种方法是我们今天要讨论的动作创建器。基本上，你可以使用 action creators 将商品存储在后端的购物车中，这不会在服务器刷新时删除购物车状态中的商品。

## 什么是 thunk？

Thunk 是一个函数(async 是可能的),它由另一个函数返回，允许您在执行主代码之前执行一些代码。

所以首先你必须在你的终端上运行下面的命令来安装一些软件包

```
npm install redux(Not necessary if you directly run the third command)npm install react-reduxnpm install @reduxjs/toolkit
```

然后，让我们在我们的应用程序中创建一个 store a store，并对其进行如下配置。

步骤 1:在一个单独的文档中创建一个切片(不管你是否使用同一个文件)

步骤 2:从切片文件导入切片缩减器，并使用从 react-redux 库中导入的 ConfigureStore 方法配置存储

步骤 3:使用从 react-redux 库导入的 Provider 组件将 App 组件包装在 index.js 文件中，index . js 文件是组件树中最高的组件。(参考以下代码片段)

然后创建另一个文件名作为购物车动作创建者。并在文件中创建一个函数，该函数返回另一个函数，一旦购物车状态发生变化，该函数就将购物车数据发送到后端服务器。

然后，我们可以将这个动作创建器导入到组件中，并产生如下副作用。

正如你在上面看到的，我们可以随着购物车的变化向后端服务器发送数据。您也可以使用相同的方法从服务器获取数据，但是您必须调用 CartSlice 操作，用获取的数据替换最后的购物车状态。

原来如此。希望你能理解。非常感谢