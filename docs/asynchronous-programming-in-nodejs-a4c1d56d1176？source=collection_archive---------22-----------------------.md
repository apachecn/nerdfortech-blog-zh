# NodeJS 异步编程

> 原文：<https://medium.com/nerd-for-tech/asynchronous-programming-in-nodejs-a4c1d56d1176?source=collection_archive---------22----------------------->

![](img/f92f215fcd0313b28531f404f4d6389d.png)

照片由 [Fotis Fotopoulos](https://unsplash.com/@ffstop?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 编写异步代码的不同解决方案

在 NodeJS 中，异步代码主要可以用三种方式编写:

1.  使用回调
2.  利用承诺
3.  使用异步/等待

## 复试

回调是传递给一个函数的函数，这个函数需要时间来执行。这些回调函数只在它被传递到的函数执行之后才被执行。回调也必须等待所有同步代码被执行。直到它在回调队列中等待。

例如定时器、API 调用等。

```
// will be printed after two seconds
const afterTwo = () => { console.log("Printed after two seconds");}; // passing "afterTwo" as a callback function
setTimeout(afterTwo, 2000);// passed an anonymous function as a callback on request
const req = https.request(options, (res) => { res.on("data", (data) => { console.log(data); });});req.end();
```

## 承诺

ES6 中引入了处理异步代码的承诺。每当请求或函数调用返回一个承诺时，必须使用接受函数作为参数的“then”关键字进行解析。该功能仅在承诺返回后执行。执行不会等待调用函数之外的其他代码被执行。

为优先级高于回调队列的承诺引入了一个特殊的作业队列。

```
// a function that mocks api call and returns a promise
const fecthData = () => { return new Promise((resolve, reject) => {

    setTimeout(() => resolve("data fetched"), 3000); });};// resolving the promise after it is receivedfecthData().then((data) => { // logs "data fetched"
  console.log(data);}).catch((err) => { console.log(err);});
```

## 异步等待

关键字 async-await 是在 ES8 中引入的。使用这种语法，可以用看起来像同步的代码来编写异步代码。这个特性的工作原理与承诺完全一样，并且是建立在承诺之上的。async-await 的语法很有意义，感觉很容易使用。请注意，在“async”函数之外使用“await”将会引发错误。

例如，上面的代码可以写成:

```
const fetchData = () => { return new Promise((resolve, reject) => { setTimeout(() => resolve("data fetched"), 3000); });};// use keyword async before function definitionasync function loadData() { // use keyword await before the function call const data = await fetchData(); console.log(data);}loadData();
```

这是所有的乡亲。

如果有帮助，请留下掌声。

下一集见。