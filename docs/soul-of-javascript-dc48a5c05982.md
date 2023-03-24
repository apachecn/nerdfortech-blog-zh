# 对 JavaScript 的片面热爱

> 原文：<https://medium.com/nerd-for-tech/soul-of-javascript-dc48a5c05982?source=collection_archive---------15----------------------->

按照你的说法，`Soul` javascript 是谁？

首先，我们将讨论 javascript 中的函数，然后我们将了解 JavaScript 的灵魂。

![](img/15871358258fef9ebddf9ea648875826.png)

从广义上讲，函数是完成特定任务的代码块。该流程可以接收数据并返回结果。但是用 javascript 的话来说，上面的描述并不适用于 js。

*   我们可以在同一个代码中多次调用一个函数。
*   可以在函数内部调用函数。这个过程被称为递归。
*   块内所有指令从上到下执行。

上面那一段台词对于 C 和 C++来说可能足够了，但是我们是在 Js 时代。

**函数是 JavaScript** 中的一等公民。一等公民意味着我们可以将一个函数表示为一个值或另一种类型的对象。

*   函数可以将函数作为参数。
*   函数可以返回函数。
*   我们可以用一个值来表示一个函数。

# 1.定义一个函数

在 javascript 中定义函数有多种方法。

我们将通过添加一对花括号，使用指定的名称来调用函数。

这里我们使用 function 关键字来声明一个函数。

```
function name () {// operations// statement
}
```

[**Js 引擎**](/nerd-for-tech/crucial-facts-about-javascript-engine-7b264c17f36d) 在编译 javascript 程序时，通过函数名将函数附加到窗口对象上。因此我们可以在程序的任何地方访问一个全局函数。

## 声明函数的方法

*   按功能关键字。
*   箭头函数/函数表达式..
*   立即调用函数表达式。
*   速记法。

## 函数关键字

**函数声明**可以用一个 function 关键字，后面跟一个必需的函数名，一对括号中的参数列表(para1，…，params)，以及一对用来分隔主体代码的花括号{…}。

```
function fun (parameter) { // function with name funconsole.log(parameter)}// Function with Parameterfunction sum(a ,b) { //Parameter a and bconst total = a + b;console.log(total);}
```

函数声明在当前范围内构成一个变量，其标识符等同于函数名。该变量代表或指向函数对象。

函数变量被**提升**到当前作用域的顶部，这意味着函数可以在声明之前被调用。

## 函数表达式/箭头函数

**一个函数表达式**可以由一个变量/常量名称关键字构成，后跟一个必需的等号“=”符号，一对括号中的参数列表，随后类似于大于“= >”，以及一对花括号{…}来结束函数表达式。

*请不要迷惑我，不要担心*，我是来让**一切变得容易**。

```
const fun = (parameter) => {//operation//statement}
```

一个**箭头函数表达式**是编写函数表达式的一种更精确的方式。箭头函数不会创建自己的`this`引用。箭头功能是构成回叫功能的最理想的安排。

## 立即调用函数表达式/life

在整个执行过程中只执行一次，然后从画面中消失的函数。

生活是一种在创造的同时执行功能的方式。

生活对记忆是有益的，因为它不会给 [**范围**](/nerd-for-tech/understand-scopes-and-scope-chain-in-javascript-12ee91161abb) 和上下文造成任何困境。

```
(function (parameter) {//statement//operation})(argument);
```

在这里，我们没有给函数添加任何签名，因为它是一个匿名函数。我们不能从任何代码点调用匿名函数。
最后一对括号用于调用函数。在括号内，我们可以传递参数。

如今，生命不常被使用。

# 速记方法

速记方法使用 arrow function 或 function 关键字在对象文字和 ES2015 类上声明函数。

速记方法的上下文指向对象级上下文。

```
const obj = {languages: [],add(…lang) {this.languages.push(…lang);},get(index) {return this.languages[index];}};obj.add(‘C’, ‘Java’, ‘PHP’);obj.get(1) // => ‘Java’
```

在速记法中，如果使用*箭头*函数，那么`this`指向**未定义。**

```
const obj = {
  languages: [],
  add: (lang) => {
     // this.languages.push(...lang); // throw an error 
     // here, this point to undefined
     console.log("hi");
   }
};
obj.add('C', 'Java', 'PHP'); // hi
```

速记方法非常有价值，实现简单，理解容易，不需要任何外部绑定方法。

# 2.回叫功能

在 JavaScript 中，**回调函数**作为参数传递给另一个函数，该函数将在稍后执行*。在参数中给出一个函数意味着传递对该函数的引用，而不是传递整个函数。*

```
const fun = () => {
 console.log("hello");
}
function foo(call_back) {
 call_back();
}
foo(fun);
```

**异步 API 调用**中使用的频繁回调函数、错误处理等。

# 3.短时回叫功能

设计箭头函数时，括号对和花括号对于单个参数是可选的，单个 body 语句支持简洁的回调函数。

```
const numbers = [1, 5, 10, 0];
numbers.some(item => item === 0); // true
```

速记回调函数写起来简洁合理。该功能看起来简单易懂。

难以执行和读取的嵌套回调函数。简短回调是编写回调函数的一种友好方式。

现在在深入讨论函数之后，希望你能明白谁是 javascript 的灵魂…

*没错，* ***函数*** *是 javascript 的灵魂。*

# 4.结束语

*   没有第一和最后，一切都取决于情况和开发者的心态。
*   但是，我们仍然有很大的关键点来开发漂亮和简短的代码。
*   arrow 函数是定义和声明函数的健壮解决方案。当回调函数只有一个简短的语句时，arrow 函数是一个很好的选择，因为它可以构建简洁优美的代码。

我希望你在这里学到了新东西。

谢谢你的 time☺️

如果您有任何与 JS 相关的疑问，请随时联系社交媒体

[https://instagram.com/rkstarji](https://instagram.com/rkstarji?igshid=j6twh39s09n9)

https://twitter.com/RkStarJi