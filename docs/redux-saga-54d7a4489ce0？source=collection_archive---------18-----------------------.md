# 还原传奇

> 原文：<https://medium.com/nerd-for-tech/redux-saga-54d7a4489ce0?source=collection_archive---------18----------------------->

Redux-Saga 是一个中间件库，处理带有副作用的异步任务，比如 API。此外，我们能够读取状态并在 Saga 上调用操作。它使用生成器函数来完成异步任务。

![](img/4e66691aa668401a60ef7d108953cb0c.png)

以下概述了我们为什么使用 Saga 以及如何安装它。然后，讨论了处理 API 的有用的 Saga 方法。

# 为什么我们要使用 Redux-Saga？

Saga 的主要职责是处理 API 调用，并根据端点的响应更新状态！redux-saga 的主要优势之一是测试友好。

> 在使用 Redux-Saga 之前，你应该熟悉 Redux。

阅读 Redux，点击[这里](https://zaraco.medium.com/what-is-the-redux-js-b74b172423ef)。

# 如何安装 Redux-saga？

基于您的软件包管理器，有两种安装 redux-saga 的方法:

```
npm install redux-saga
```

或者

```
yarn add redux-saga
```

然后，您需要创建一个示例 redux 中间件。例如:

```
import { call, put, takeEvery } from 'redux-saga/effects'
import ***Api*** from '...'

function* **fetchUser**(action) {
    try {
        const user = yield call(***Api***.fetchUser, action.payload.userId);
        yield put({type: "**USER_FETCH_SUCCEEDED**", user: user});
    } catch (e) {
        yield put({type: "**USER_FETCH_FAILED**", message: e.message});
    }
}

function* **mySaga**() {
    yield takeEvery("**USER_FETCH_REQUESTED**", fetchUser);
}

export default **mySaga**;
```

在这个例子中，当 *USER_FETCH_REQUESTED* 动作被分派时，它向 API 发送一个调用请求。如果成功，它将使用用户负载调度一个类型为 *USER_FETCH_SUCCEEDED* 的操作。否则，在出现错误状态的情况下，它会调度一个带有类型 *USER_FETCH_FAILED* 和错误消息有效负载的操作。

API 可能是这样的:

```
import axios from 'axios';

const ***Api*** = {
    fetchUser: (userId) => {
        return axios.get(`...?userId=${userId}`)
    }
};

export default ***Api***;
```

接下来，您需要将您的传奇故事添加到 redux 商店:

```
import { createStore, applyMiddleware } from 'redux'
import createSagaMiddleware from 'redux-saga'

import reducer from './reducers'
import mySaga from './sagas'

**const sagaMiddleware = createSagaMiddleware();**

const store = createStore(
    reducer,
    **applyMiddleware(sagaMiddleware)**
);

**sagaMiddleware.run(mySaga);**
```

在上面的代码中，saga 的中间件被创建并应用于商店，最后，它运行。

# 佐贺方法

下面将解释最常用的方法:

## 挑选

它从选择器(state)中读取数据，下面是 select 调用的一个示例。

```
const userId = yield select(userSelectors.getUserId);
```

## 放

它调度操作，例如:

```
yield put({type: "USER_FETCH_SUCCEEDED", user: user});
```

## 呼叫

它调用一个异步函数，比如 API，例如:

```
const user = yield call(***Api***.fetchUser, action.payload.userId);
```

## 拿走每一个

它监视每个动作调度以调用一个生成器函数。

```
yield takeEvery("**USER_FETCH_REQUESTED**", fetchUser);
```

## 获取最新信息

它与 takeEvery 相同，唯一的区别是，当前一个呼叫未决时，不会再次呼叫。

```
yield takeLatest("**USER_FETCH_REQUESTED**", fetchUser);
```

# 结论

Redux-saga 是一个在 Redux 中处理异步任务的库。在这篇文章中，讨论了为什么和如何使用 Saga 和方法。