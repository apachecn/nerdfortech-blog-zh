# async/Await:JavaScript 中的现代并发

> 原文：<https://medium.com/nerd-for-tech/async-await-modern-concurrency-in-javascript-e8e1a2d9e861?source=collection_archive---------11----------------------->

传统的 CPU 以同步顺序一个接一个地处理任务，多线程 CPU 增强了这一点，如今大多数电脑都有多个核心 CPU。量子计算是一个例外，但这不在本文的讨论范围之内，而且到目前为止还没有工作。

多线程 CPU 使 CPU 能够同时处理多个问题，从而提高电脑的性能。

由于这种“代码”本质上是同步的，因为它是由 CPU 运行的，所以在 JavaScript 中，你给出一个指令，该指令就进入“调用堆栈”。调用栈是等待执行的指令队列，所有的请求按照它们被调用的顺序排列。

比如说。

```
**CODE**
console.log("Task1");
console.log("Task2");
console.log("Task3");**RESULT**
Task1
Task2
Task3
```

注意执行的顺序。

问题是，如果事情需要很长时间才能完成，会发生什么？

假设您正在查询一个繁忙的数据库，而响应需要一段时间才能返回？这将是一场灾难，因为它会阻塞队列，直到它完成执行。

**这个问题是用异步函数解决**

异步允许我们将一条指令移到一边，直到调用堆栈清空，然后将它取回并重新排队。

见下文

```
**CODE**
console.log(“**Task1**”);
setTimeout(function(){ console.log(“**Task2**”); }, 3000);
console.log(“**Task3**”);**RESULT**
Task**1**
Task**3**
Task**2**
```

因此，您可以在这里看到，我们运行了与前面相同的代码，只是我们给了“Task 2”一个异步函数，在本例中是“setTimeout ”,当解释器看到它时，它将它放在一边(它将它放在 Webapis 中)。然后继续执行下一条指令，并在完成其他任务后调用“任务 2”。

它们是多种可用的异步函数，其中许多你可能已经使用过了。例如“fetch”方法。

```
fetch(file)
.then(x => x.text())
.then(y => myDisplay(y));
```

由于 Fetch 是基于 async 和 await 的，上面的例子可能更容易理解:

```
async function getText(file) {
  let x = await fetch(file);
  let y = await x.text();
  myDisplay(y);
}
```

# 事件循环

要完全掌握 async 的威力，最好的方法是你需要很好地掌握 JavaScript 事件循环是如何工作的，这个视频对我来说非常重要，它彻底改变了我编写代码的方式。

理解异步的下一步是使用它，测试不同的东西。[这里有一个沙盒](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)可以让你这么做。尝试各种场景(在尝试查看输出时，不要忘记打开控制台。)

![](img/aaec7317adf64b6e46f9bd925de8bf6a.png)

[来源](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)

资源:

[W3School](https://www.w3schools.com/js/js_api_fetch.asp)