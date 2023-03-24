# 使用 async 和 await JavaScript 的异步结果

> 原文：<https://medium.com/nerd-for-tech/asynchronous-fruits-with-async-and-await-javascript-d8c4f2c91e8a?source=collection_archive---------22----------------------->

![](img/c7b7d85f40efcd25e9f697d929d431f9.png)

照片由 [Reiseuhu](https://unsplash.com/@reiseuhu?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

`async`和`await`是两个关键词，可以帮助异步代码读起来更像同步代码。这可以帮助代码看起来更整洁，同时保留异步代码的优点。异步意味着事情可以独立于主程序流发生。大多数程序都是异步的，在需要的时候才会暂停执行，让计算机同时执行其他事情。

`async`和`await`是建立在承诺上的功能。一个承诺通常被定义为一个价值的代理，这个价值最终将变得可用。承诺是处理异步代码的一种方式。

# 承诺是如何起作用的？

一个`Promise`是一个有状态和值的对象。一旦`Promise`被调用，它将在挂起状态下启动。调用函数继续执行，而`Promise`是挂起的，直到它被解决或拒绝，给调用函数任何被请求的数据。

创建的`Promise`将最终以已解决状态或已拒绝状态结束。resolved 状态意味着一切都按预期运行，而 rejected 状态意味着有错误。

# 异步ˌ非同步(asynchronous)

`async`关键字放在函数声明的前面，把它变成一个异步函数。当用`async`声明一个函数时，它会自动返回一个`Promise`。返回函数与解析`Promise`相同，同样，抛出错误的函数将拒绝`Promise`。

由于关键字`async`，在`getFruit()`中返回的任何内容都将是该值的`Promise`。用户以水果的名称传入。该函数将从该对象解析出水果表情符号。为了实际使用`Promise`实现时返回的值，我们可以使用`.then()`块。

# 等待

当您将`async`函数与`await`关键字结合起来暂停函数的执行时，它的真正威力就显现出来了。`await`关键字告诉 JavaScript 暂停异步函数，直到 await 值返回一个解析的`Promise`。不使用`.then()`回调，我们可以将一个变量赋给`await` `Promise`值来获得解析后的`Promise`的值。

`makeSmoothie()`函数将抓取多个水果，并将它们组合成一个数组。我们可以指定一个变量作为已解析的`Promise`的值。`await`将暂停`makeSmoothie()`，直到`getFruit()` `Promise`解析出一个值。我们可以将该值用作变量 fruit1。我们可以再次做同样的事情来获得另一个水果并返回一个数组。

# 错误处理

我们可以使用 try/catch 块来处理`async`函数中的错误。try 块中的代码将正常运行，直到抛出错误。catch-block 包含指定在 try-block 中引发异常时如何处理的语句。如果 try 块的任何部分抛出异常，它将转到 catch 块，承诺被拒绝。如果 try 块中没有抛出异常，则跳过 catch 块。

可以添加一个 finally-block，它将始终在 try-block 和 catch-block 完成执行后执行。无论是否抛出异常，finally-block 都会执行。如果抛出了异常，并且没有处理该异常的 catch-block，finally-block 中的语句仍会执行。

我们从 fruit1 和 fruit2 中为`Promise`创建了一个 smoothie 变量`await`,而不是在它们上面都有`await`关键字。这将告诉数组中的所有`Promises`同时运行。这将提高制作思慕雪的速度。我们在 try-block 内部有一个错误，这将导致`Promise`被拒绝并立即移动到 catch-block。通过在 catch 块中抛出一个错误，它将破坏承诺链，并由 catch 回调处理。

# 资源

关于 async 和 await 的更多资源，我建议看看这些有用的链接。

[https://nodejs.dev/learn/understanding-javascript-promises](https://nodejs.dev/learn/understanding-javascript-promises)

[https://developer . Mozilla . org/en-US/docs/Learn/JavaScript/Asynchronous/Async _ await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)

[https://medium . com/@ lydiahallie/JavaScript-visualized-promises-async-await-a 3 f1 aad 8 a 943](/@lydiahallie/javascript-visualized-promises-async-await-a3f1aad8a943)

[https://www . theodinproject . com/paths/full-stack-JavaScript/courses/JavaScript/lessons/async-and-await](https://www.theodinproject.com/paths/full-stack-javascript/courses/javascript/lessons/async-and-await)

[https://www.youtube.com/watch?v=vn3tm0quoqE](https://www.youtube.com/watch?v=vn3tm0quoqE)