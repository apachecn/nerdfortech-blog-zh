# React18:新功能和更新

> 原文：<https://medium.com/nerd-for-tech/react18-new-features-and-updates-939624e07301?source=collection_archive---------0----------------------->

![](img/53e3ba5848ddf5dfbc57955d8849d3b9.png)

React 18 现已在 npm 上可用，并为 React.js 开发社区提供了一些令人兴奋的功能和更新。所有更新的主要目的是通过引入开箱即用的功能和改进来维护第三方库。

React 18 新功能和改进是可能的，这要归功于 React 18 中新的选择加入“并发渲染”机制，该机制使 React 能够同时创建多个版本的 UI。虽然这种变化主要是在幕后，但它将开启新的可能性，以提高应用程序的性能。

# 安装

要安装 React 的最新版本:

```
npm install react react-dom
```

或者如果你用的是纱线:

```
yarn add react react-dom
```

# 介绍新的根 API

React 中的一个**根**指向呈现树的顶层数据结构。在 React 18 中，我们将有两个根 API:遗留的根 API 和新的根 API。

# 传统根 API

遗留根 API 是用 **ReactDOM.render** 方法调用的现有 API。它将创建一个在**遗留**模式下运行的根，这类似于 React 版本 17 中的用法。它将逐步强制使用新的根 API。在即将到来的版本中，旧的根 API 将被弃用。

请参考下面的代码示例。

```
import * as ReactDOM from 'react-dom';
import App from 'App';const container = document.getElementById(‘root’);// Initial render.
ReactDOM.render(<App tab="home" />, container);// During an update, React will access the container element again. 
ReactDOM.render(<App tab="profile" />, container);
```

# 新建根 API

新的根 API 将通过 **ReactDOM.createRoot** 方法调用。要使用它，首先，我们必须用 root 元素作为参数通过 **createRoot** 方法创建根。然后，我们调用 ***root.render*** 方法，将 app 组件作为参数传递。通过使用新的根 API，我们可以使用 React 18 中所有可用的增强和并发特性。

参考下面的代码。

```
import * as ReactDOM from 'react-dom';
import App from 'App';// Create a root.
const root = ReactDOM.createRoot(document.getElementById('root')); // Initial render: Render an element to the root.
root.render(<App tab="home" />);// During an update, there's no need to access the container again since we have already defined the root instance. 
root.render(<App tab="profile" />);
```

# 水合物法的变化

**水合物**方法类似于渲染方法。但是它有助于将事件监听器附加到由服务器端的 **ReactDOMServer** 方法呈现的容器中的 HTML 元素。

React 18 将这种**水合物**方法替换为**水合物**方法。

请参考下面的代码示例。

```
import * as ReactDOM from 'react-dom';import App from 'App';const container = document.getElementById('app');// Create and render a root with hydration.
const root = ReactDOM.hydrateRoot(container, <App tab="home" />);
// Unlike the createRoot method, you don't need a separate root.render() call here.
```

# 渲染回调中的更改

渲染回调从新的根 API 中移除。但是我们可以将它作为属性传递给根组件。

请参考下面的代码示例。

```
import * as ReactDOM from 'react-dom';function App({ callback }) {
 // Callback will be called when the div element is first created.
 return (
 <div ref={callback}>
   <h1>Hello </h1>
 </div>
 );
}
const rootElement = document.getElementById("root");const root = ReactDOM.createRoot(rootElement);
root.render(<App callback={() => console.log("renderered")} />);
```

# 自动配料

批处理是一次重新渲染中的多次反应状态更新。

```
function Counter() {
  const [count, setCount] = useState(0);
  const [disabled, setDisabled] = useState(false); function handleClick() {
    setCount(c => c + 1); // does not re-render yet
    setDisabled(f => !f); // does not re-render yet
    // React will only re-render once (that's batching!)
  } return (
    <div>
      <h1>{count}</h1>
      <button disabled={disabled} onClick={handleClick}>
        Increment
      </button>
    </div>
  );
}
```

上面，我们有`count`和`disabled`两种反应状态。当我们点击一个按钮时，React 总是批处理并在一次重新渲染中更新它们。

这避免了不必要的重新渲染，提高了我们的性能。异步请求呢？React 17 不进行批处理，而是进行两次独立的更新。

```
function Counter() {
  const [count, setCount] = useState(0);
  const [disabled, setDisabled] = useState(false); function handleClick() {
    doPostRequest()
      .then(() => {
        setCount(c => c + 1); // does re-render
        setDisabled(f => !f); // does re-render
      })
  } return (
    <div>
      <h1>{count}</h1>
      <button disabled={disabled} onClick={handleClick}>
        Increment
      </button>
    </div>
  );
}
```

现在，在 React 18 中，我们可以在`promises`、`setTimeout`、`native event handlers`或任何其他事件中批量更新状态。

# 禁用自动批处理

有时，我们需要在每次状态改变后立即重新呈现组件。在这种情况下，使用 **flushSync** 方法来禁用自动批处理。

```
import { flushSync } from 'react-dom'; // Note: react-dom, not reactfunction handleClick() {
  flushSync(() => {
    setCounter(c => c + 1);
  });
  // React has updated the DOM by now.
  flushSync(() => {
    setFlag(f => !f);
  });
  // React has updated the DOM by now.
}
```

# 服务器端渲染

通过`Suspense`，React 18 通过使异步服务应用程序的各个部分成为可能，对 SSR 进行了重大的性能改进。

服务器端呈现允许您从提供的 React 组件生成 HTML 文本，然后加载 JavaScript 代码并与 HTML 合并(称为水合)。

现在，有了`Suspense`，你可以把你的应用分成小的、独立的单元，这些单元可以独立呈现，不需要应用的其他部分，让你的用户可以比以前更快地获得内容。

假设您有两个组件:一个文本组件和一个图像组件。如果你像这样把它们叠在一起:

```
<Text />
 <Image /&gt;
```

然后，服务器试图立刻呈现它们，减慢了整个页面的速度。如果文本对您的读者更重要，您可以通过将`Image`组件包装在`Suspense`标签中，使其优先于图像:

```
<Text />
    <Suspense fallback={<Spinner />}>
         <Image />   
    </Suspense>
```

这一次，服务器首先为您的文本组件提供服务，当您的图像等待加载时，会显示一个微调器。

# 过渡

React 18 最重要的更新之一是引入了***start transition***API，即使在大屏幕更新期间，也能让你的应用保持响应。
有时在繁重的更新操作过程中，你的应用程序变得没有响应，***start transition***API 对于处理这种情况非常有用。
API 允许用户控制并发方面，以改善用户交互。这是通过将繁重的更新包装为“***start transition***”来完成的，并且只有在启动更紧急的更新时才会被中断。因此，它实际上分类紧急更新和缓慢更新。
如果过渡被用户操作中断，React 将会抛出尚未完成的陈旧渲染工作，并且将只渲染最新的更新。

# 结论

感谢阅读，希望这篇文章对你有用。即将发布的 React 18 稳定版将为开发者社区带来一系列激动人心的新功能。主要重点是并发性和逐步升级到新版本。