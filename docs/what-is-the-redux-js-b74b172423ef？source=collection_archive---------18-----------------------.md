# Redux.js 是什么？

> 原文：<https://medium.com/nerd-for-tech/what-is-the-redux-js-b74b172423ef?source=collection_archive---------18----------------------->

Redux 是一个用于处理 web 应用程序状态的开源 js 库。这个库可以在 React 和 Angular 等不同的 web app 框架中使用。下面概述了如何在 react web 应用程序中使用 redux。

![](img/540183472b385b2a232ed6101bfeca78.png)

# 如何安装

如果您想要初始化一个新的 react 应用程序，或者将 redux 添加到您当前的 react 应用程序，有两种方法可以将 redux 添加到您的项目中。

## 用 redux 创建一个 react 应用

如果您想使用 redux 创建一个 react 应用程序，可以简单地编写以下命令:

```
npx create-react-app my-app --template **redux**
```

否则，如果您喜欢通过 TypeScript 进行编码，您可以运行以下命令:

```
npx create-react-app my-app --template **redux-typescript**
```

通过运行上面的代码，它创建了一个集成了 Redux 的 react app。

## Redux 包的手动安装

通过运行以下 npm 命令，您可以在当前项目中安装 redux 包:

```
npm install **redux**
```

或者

```
yarn add **redux**
```

此外，您还需要安装 react-redux 和 redux-devtools-extension 包:

```
npm install **react-redux** npm install **redux-devtools-extension**
```

或者

```
yarn add **react-redux** yarn add **redux-devtools-extension**
```

# 如何实施

## 商店

首先，您需要为应用程序创建一个全局商店(store.js):

```
import { applyMiddleware, createStore } from 'redux';
import { composeWithDevTools } from 'redux-devtools-extension'
import reducers from './reducers';

const middlewares = [];
const ***store*** = createStore(reducers, composeWithDevTools(applyMiddleware(...middlewares)));
export default ***store***;
```

上面的代码通过添加 reducers 和中间件创建了一个全局存储。在这种情况下，为了简单起见，中间件是空的，但您可以添加路由器和 saga 中间件作为示例。

然后，需要将商店添加到 app (index.js)中:

```
import Reactfrom 'react';
import ReactDOM from 'react-dom';
import App from './App';
import { Provider } from 'react-redux';
import ***store*** from './store';

ReactDOM.render(
    <React.StrictMode>
        <Provider store={***store***}>
            <App/>
        </Provider>
    </React.StrictMode>,
    document.getElementById('root')
);
```

## 还原剂

Reducers 是纯函数，它将当前状态和一个动作作为参数，并返回更新后的状态。reducer 被组合起来添加到存储(reducer.js)中。

```
import { combineReducers } from 'redux'
import exampleReducers from "./exampleReducers";
const ***reducers*** = combineReducers({
    example: exampleReducers,
});
export default ***reducers***;
```

这里，作为一个例子，只有一个 reducer 被添加到存储中(exampleReducers.js)。

```
import exampleTypes from './exampleTypes';

const initialState = {
    name: '',
    age: 0,
}

const ***exampleReducers*** = (state = initialState, action) => {
    switch (action.type) {
        case exampleTypes.CHANGE_NAME:
            return {
                ...state,
                name: action.payload
            }
        case exampleTypes.CHANGE_AGE:
            return {
                ...state,
                age: action.payload
            }
        default:
            return state;
    }
}

export default ***exampleReducers***;
```

如您所见，reducer 函数是一个返回更新状态的纯函数。在上面的例子中，有两种不同的类型可以改变状态，CHANGE_NAME 用于改变名字，CHANGE_AGE 用于改变年龄。

## 类型

它显示了可用的操作类型。例如，有两种不同的类型 CHANGE_NAME 和 CHANGE_AGE:

```
const ***exampleTypes*** = {
    CHANGE_NAME: `CHANGE_NAME`,
    CHANGE_AGE: `CHANGE_AGE`,
}
export default ***exampleTypes***;
```

## 行动

动作是基于输入参数(例如 Actions.js)返回动作对象(类型和有效负载)的纯函数

```
import exampleTypes from "./exampleTypes";
const ***exampleActions*** = {
    changeName: (name) => ({
        type: exampleTypes.CHANGE_NAME,
        payload: name
    }),
    changeAge: (age) => ({
        type: exampleTypes.CHANGE_AGE,
        payload: age
    }),
}

export default ***exampleActions***;
```

## 选择器

选择器是采用 redux 状态并从该状态返回派生数据的纯函数(例如 Selectors.js)。

```
const ***exampleSelectors*** = {
    getName: (state) => state.example.name,
    getAge: (state) => state.example.age,
}

export default ***exampleSelectors***
```

# 如何使用反应功能元件

为了分派动作，您需要编写:

```
const dispatch = useDispatch();
dispatch(exampleActions.changeName('Test'))
```

为了获得状态变量的值，我们使用 useSelector:

```
const name = useSelector(exampleSelectors.getName);
```

最后，这是如何调度操作并在 react 组件(App.js)上显示状态值:

```
import React, { useEffect } from 'react';
import { useDispatch, useSelector } from 'react-redux';
import exampleSelectors from './exampleSelectors';
import exampleActions from './exampleActions';

const ***App*** = () => {
    const dispatch = useDispatch();
    const name = useSelector(exampleSelectors.getName);
    const age = useSelector(exampleSelectors.getAge);

    useEffect(()=>{
        dispatch(exampleActions.changeName('Test'))
        dispatch(exampleActions.changeAge(20))
    }, []);

    return (
        <>
            <div>Name: ${name}</div>
            <div>Age: ${age}</div>
        </>
    );
}

export default ***App***;
```

下一步是添加 Redux Saga:点击[这里](https://zaraco.medium.com/redux-saga-54d7a4489ce0)