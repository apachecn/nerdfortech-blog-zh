# 我对 React JS 的(简要)介绍。

> 原文：<https://medium.com/nerd-for-tech/my-brief-introduction-to-react-js-a13c0580d98?source=collection_archive---------10----------------------->

![](img/84da577986fdac77500dfc8a285f48d1.png)

弗洛里安·奥利佛在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

最近刚开始做前端实习生。ReactJS 主要由团队使用，在本文中，我将尝试总结我在前端训练营的第一周所学到的东西。

# 反应 JS

## React JS 是什么？

React JS 是一个开源库，它使用 Javascript 为网站创建自定义组件。这些组件可以是整个用户界面，也可以是我们希望保持动态的更具体的小功能。

简而言之，网页的动态组件是可以改变其值或状态而不要求用户重新加载整个页面的组件。活动元素的一个例子可以是我们屏幕上的“时钟”组件，因为我们不需要在每次时间改变时都重新加载页面。

## React 的特点:

我在工作中获得的一些 React 的主要概念:

*   反应 JSX
*   组件和属性
*   反应钩

还有其他几个功能我没有提到，比如‘列表’或者‘条件渲染’。但是使用这些特性的基础是相同的。那么就让我们一个一个的简单看一下。

## 反应 JSX

React JSX 是 JS 和 HTML 的中间点。JSX 组件看起来像 HTML，但不完全是 HTML。它充当了语法糖的角色，使阅读和添加 HTML 来反应变得更容易。

例如:

```
const randomVariable = <h1> This is a Header ! </h1>;
```

上面的句子是有效的 JS，但是添加了

# 标签，也就是 HTML 位。另一个很好的例子是:

```
const var1 = ‘General Kenobi’;
const var2 = <p> Hello there, {var1} </p>;
```

这里，var1 是 JavaScript 中的常规字符串变量，但 var2 是我们的 JSX 组件。大括号{}可以存储任何有效的 JS 代码或变量，如上例所示。

但是我们的工作还没有完成。现在我们已经准备好了台词，我们需要渲染它们。为此，我们使用一个名为 **ReactDOM** 的独特调用。

我们使用。render()调用在提供的容器中呈现一个 React 组件，并返回对该组件的引用。

因此，为了呈现我们已经定义的组件，我们将使用以下调用:

```
ReactDOM.render(
var2,
document.getElementById(‘main’)
);
```

document.getElementById('main ')引用 Id 为' main '的 HTML 标记，通常是一个将呈现 React 组件的

标记。

创建 JSX 对象的其他方法是使用 **React.createElement()** 方法。

例如，使用常规 JSX 对象:

```
const expression = (
<h1 classname = “greeting”>
Hello there, General Kenobi.
</h1>
);
```

等效的 React.createElement()代码应该是:

```
const expression = React.createElement(
“h1”,       //the type of tag we want
{classname: ‘greeting’},    // the classname declaration
“Hello there, General Kenobi.”  //message stored between the tags
);
```

## 组件和属性

组件可以被认为是网页中独立呈现的元素。这些组件可以由预定义的 HTML 标签组成，也可以是自定义组件。

我们可以使用函数定义一个 React 组件；

```
function newComponent(){
return <h1> Hello there, General Kenobi </h1>;
}
```

或者使用 JS 类；

```
class newComponent extends React.Component{
render(){
<h1> Hello there, General Kenobi </h1>;
};
}
```

我们还可以在函数被调用进行渲染时，将“参数”(或道具)传递给函数。这些参数可以是任何数据类型，如字符串或整数。

例如:

```
function Greeting(props){
return <h1> Hello there, {props.name} </h1>;
}const element = <Greeting name = ”Kenobi” />ReactDOM.render(
element,
document.getElementById(‘main’)
);
```

在这里，引用了临时调用问候组件的元素常量。Greeting 组件将 name 作为组件的属性传递，值为“Kenobi”。然后，函数 Greeting 通过将道具名称替换为“Kenobi”来呈现组件。最后，完整的句子呈现在 HTML 文件的“main”部分。

其他功能包括嵌套/分层组件。例如，我们可以让一个组件在另一个组件中使用，从而在它们之间建立一个层次结构。

例如:

```
function Greeting(props){
return <h1> Hello there, {props.name} </h1>;
}function Welcome(){
return(
<div>
    <Greeting name=”Kenobi” />
    <Greeting name=”Anakin” />
    <Greeting name=”Leia” />
</div>
);
}
const element = <Welcome />ReactDOM.render(
element,
document.getElementById(‘main’)
);
```

## 反应钩

在我学习 React 的过程中，react 钩子是最难理解的部分。React 挂钩本质上是一种使用 React 功能而无需定义任何类的方式。但是，更重要的是，钩子在类中不起作用。

钩子提供了一种“钩入”一个函数和改变变量值的方法，而不用处理复杂的类和“this”指针。React 提供了一些内置的钩子，但是你也可以创建自己的钩子。

让我们简单看看一些内置的钩子以及如何使用它们。

1.  **useState()**

useState()是一个内置的 React 挂钩，允许使用定义的函数修改变量。它可以按如下方式使用:

```
function Somefunc(){
const [counter, update] = useState(0);return (
<p>You have clicked {counter} times</p>
<button onClick={()=> update(counter+1)}> Button </button>
)
}
```

这里，

*   “计数器”是我们要改变的变量。
*   “更新”是用于更改“计数器”变量的函数(例如，由于某些事件处理程序)。
*   useState 括号中的 0 是赋予“counter”的初始值。

2. **useEffect()**

效果(或副作用)是 UI 中给定组件在任何给定时间点的状态。这个一般分为三类。该组件已经被

*   **挂载**或引入 UI。这只会发生一次。
*   **更新**，或者 UI 中的值不断变化，从而更新 UI。这可能会发生多次。
*   **卸载**或从 UI 中移除。这也只发生一次。

React 让我们使用 useEffect 一次处理所有三种情况，而不是为每种情况定义三个独立的函数。

useEffect 调用的示例如下:

```
useEffect = (() => {
alert(“good evening”);
return () => alert(“alright bye”);
},[count]);
```

这里，

*   每次更改“count”的值时，都会调用 alert()。
*   [count]放在一个名为“dependencies”的区域中。每次因变量发生更改或更新时，都会调用 useEffect。注意:拥有[]意味着没有依赖关系，并且只调用 useEffect 一次(当组件最初安装到 UI 时)。
*   return()是“teardown”调用。当组件从 UI 中移除时，将调用该函数。

3.**创建定制挂钩**

我们可以使用预定义的 React 挂钩和 useDebugValue()挂钩来创建自定义挂钩。

自定义挂钩的示例如下:

```
function getUserName()
{
const [username, updatename] = useState();
useEffect = (() => {
const name = getFromDB(props.id)    //getFromDB is some function
showUser(name.username)
},[]);
useDebugValue(username ?? 'Loading...')
return username;
}function app()
{
const name = getUserName()
return <h1> {name} </h1>
} 
```

我们使用 useDebugValue 来显示使用 React 调试器工具查看时的值。

下面列出了我在撰写本文时使用的一些资料来源；

*   [https://reactjs.org/docs/hello-world.html](https://reactjs.org/docs/hello-world.html)
*   https://www.youtube.com/c/Fireship/search?query=typescript
*   【https://fireship.io/ 

就是这样！经过第一周的实习，我学到了很多关于 React 的知识，我很高兴能够与大家分享我的一小部分知识。我还要感谢我的朋友阿鲁士校对了这篇文章。

下周我会回来拿更多的。干杯！