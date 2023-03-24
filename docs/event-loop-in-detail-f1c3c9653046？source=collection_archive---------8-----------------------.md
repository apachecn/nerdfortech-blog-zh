# 详细的事件循环

> 原文：<https://medium.com/nerd-for-tech/event-loop-in-detail-f1c3c9653046?source=collection_archive---------8----------------------->

![](img/5150e2a451e438c75ca11d3a3f099c6c.png)

扎卡里亚·奥西在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Javascript 已经真正征服了开发世界，大多数尖端技术都在其中实现。像 nodeJs、React 和 Angular 这样的项目正在被大量采用，不只是停留在那里，而是讨论哪个更好。

所有此类框架共有的一个关键方面是 javascript。不管这些框架有多复杂，它们都是一个 javascript 库，这篇博客试图揭示 javascript 和事件循环的细节。

> t**L；DR** 事件循环是一个半无限循环，在 OS 上轮询和阻塞，直到一组文件描述符中的一些准备好。

**什么是** **文件描述符**:文件描述符是标识内核上资源的整数。假设你打开一个文件或者创建一个套接字连接，文件描述符标识了内存中的文件内容。所有进程都被称为内存中的文件，因此得名。

**让我们看看一个普通的 Java 服务器如何处理一个请求**:当接收到一个请求时，一个新的线程被创建来处理它。这种方法的问题是，每个新请求都由 CPU 分配一些资源，服务器很快就会因资源短缺而阻塞。通常不会有问题，因为你的服务器功能强大得多，流量也不疯狂。

**nodeJs 多线程解决方案**:在启动时，我们请求一个创建套接字连接的 TCP 服务器，这会产生一个文件描述符。这个文件描述符用来告诉内核我们对某些事情感兴趣，例如，传入的请求。所以当 TCP 客户端请求连接时，内核会唤醒它。因此，假设您一次收到一百万个请求，所有请求都在不使用线程的情况下同步处理(假设您的服务器只有一个内核)。node 不是立刻承担负载，而是在空闲时承担负载。线程方法将很容易阻塞系统，因为内存中加载了大量等待数据的线程，而在 node 中，无论传入请求的数量有多少，它都会一个接一个地响应所有请求，而不会阻塞。
但另一方面，如果您的任何请求需要很长时间来执行，所有后续请求都会受到影响(可能没有机会执行)。简而言之，在大量请求得到答复之前，其他请求将不得不等待或过期。换句话说，nodeJs 不是运行 CPU 密集型工作的最佳位置。你最好把它交给另一个服务或者依靠工人线程，[点击这里](https://udayreddy.medium.com/scaling-nodejs-server-19c5ecddfbfc)了解更多。

**那么，事件循环是如何工作的呢？正如我说过的，这是一个半无限循环，当我们请求的资源准备好时就会运行。假设我们运行了一个`setTimeout`,它会执行以下操作。
1。JS 将该行放到调用栈
2 中。我们要求内核设置一个计时器。
3。内核设置它并给我们一个文件描述符来识别操作。
3。我们将函数放入内存，并忘记调用了 setTimeout。最重要的是，我们释放了最初分配的资源。
…..片刻之后(超时之后)…
4。一旦定时器关闭，内核将调用 nodeJs 进程。
5。事件循环识别计时器已到期，并将其添加到队列中。
6。如果调用堆栈上没有任何东西，事件循环将再次迭代，以检查队列上是否有新的项目。
7。事件循环发现队列有 setTimeout 的回调，因此运行它。**

> **EventLoop** 每当调用堆栈为空时，运行队列中的项目。

我只提到了“队列”，这可能意味着只有一个队列。没有。有多个队列，一共有六个:
1。定时器队列:存储`setTimeout`和`setInterval`
2 的回调。文件操作队列:存储 TCP、fs 等 I/O 相关的回调。
3。setImmediate queue:存储`setImmediate`4 的所有回调。轮询队列:当前正在节点上运行代码。
5。承诺队列:存储`Promise`
6 的回调。NextTick 队列:存储`nextTick`的回调

事件循环以不同的方式管理每个队列的优先级，因此它分阶段工作。

以下是不同的阶段:
1。**定时器**:该阶段执行`setTimeout()`和`setInterval()`安排的回调。
2。**待定回调**:执行推迟到下一次循环迭代的 I/O 回调。
3。**闲置，准备**:仅供内部使用。
4。**轮询**:检索新的 I/O 事件；执行与 I/O 相关的回调(除了关闭回调、计时器调度的回调和`setImmediate()`)之外的几乎所有回调；节点将在适当的时候阻塞这里。
5。**检查**:这里调用`setImmediate()`回调。
6。**关闭回调**:一些关闭回调，例如`socket.on('close', ...)`。

当我说事件循环分阶段运行时，它每次运行时都基于上述优先级执行回调。

**定时器**:事件循环的第一阶段，检查`timer-queue`是否有任何回调排队等待执行。如果是这样，我们就开始运行它们，一旦完成，我们就进入下一步。

**未决回调**:检查`file operations`队列是否有任何准备好执行的任务。如果是这样，我们就开始运行它们，一旦完成，我们就进入下一步。

**轮询阶段**:在这个阶段，实际的 javascript 代码在这里执行。所以，当你运行`node index.js`时，代码就是在这个阶段执行的。根据代码的不同，它可能会立即执行，也可能会将某些内容添加到队列中，以便在事件循环的下一个节拍执行。
该阶段可以阻塞节点流程，因为它是同步执行的。
除了 close-callback(close 有一个单独的阶段)之外，这个进程以及执行代码还会轮询 I/O。我们正在讨论计时器、文件操作、setImmediate。
如果轮询队列(您的常规同步源代码)为空，它可能会执行以下操作:
1 .如果任何`setImmediate`被调度，它立即退出轮询阶段并进入检查阶段。
2。如果没有安排`setImmediate`，它将等待是否有回调被添加到轮询队列中，并立即执行它们。
3。如果轮询队列为空，并且计时器回调到期，它会快速返回到第一阶段，即计时器阶段。

**检查阶段**:执行当前节拍内安排的所有`setImmediate`回调。

**关闭回调**:所有`close`事件，例如 socket 发出一个关闭事件，将在本阶段执行。

**什么是蜱？**
滴答是当前正在执行的事件循环，一个滴答相当于说:所有阶段运行一次。

## `**process.nextTick()**`

这不是事件循环的一部分，从某种意义上说，它有一个单独的队列，只要 event loop 切换阶段就运行。如果回调被安排在`nextTick-queue`，一旦事件循环从一个阶段切换到另一个阶段，它将同步运行项目。这会阻塞事件循环，所以在使用时要小心，因为它可能会使 I/O 饥饿。
你想运行它的唯一原因是当你需要在你正在做的事情之后，事件循环做其他事情之前运行某个东西。

**Promise.resolve()** 这和 event-loop 完全没有关系。当当前堆栈为空时，它运行回调。比方说，您有一个正在运行的函数，它调度了一个`Promise.resolve`，这个解析的回调在函数当前被执行后立即运行。

`process.nextTick`和`Promise.resolve`被称为微任务，以最高优先级运行，但它们之间`process.nextTick`胜出。所有其他的调度程序都被称为宏任务或者只是一般意义上的任务。

任务的优先级:
`process.nextTick`>`Promise.resolve`>`setImmediate`>`setTimeout/setInterval`
注意:虽然`setImmediate`并不总是保证在`setTimeout`之前运行，上面的优先级是一个大概的比较。

快速回顾一下:1。只要调用堆栈为空，事件循环就会运行。
2。事件循环分阶段运行(定时器- > IO - >池- >检查- >退出)
3。每当调用栈为空时，就执行承诺，即在事件循环之前。
4。process.nextTick 在事件循环的阶段之间运行。

简而言之，你的事件循环应该看起来像下面的乒乓球，它应该快速运行你的指令。I/O 密集型不是问题，但 CPU 密集型是问题。
要了解更多关于 nodeJs 性能以及如何优化的信息，[点击这里](https://udayreddy.medium.com/scaling-nodejs-server-19c5ecddfbfc)

![](img/20436e3059aa3a2fa940f28f904c00dc.png)

活动的事件循环

高度鼓励看下面:
[https://www.youtube.com/watch?v=PNa9OMajw9w&t = 12s](https://www.youtube.com/watch?v=PNa9OMajw9w&t=12s)
[https://www.youtube.com/watch?v=P9csgxBgaZ8](https://www.youtube.com/watch?v=P9csgxBgaZ8)

参考资料:
[https://stack overflow . com/questions/49811043/relationship-between-event-loop-libuv-and-V8-engine](https://stackoverflow.com/questions/49811043/relationship-between-event-loop-libuv-and-v8-engine)
[https://dev . to/khaosdoctor/node-js-under-the-hood-3-deep-dive-into-the-event-loop-135d](https://dev.to/khaosdoctor/node-js-under-the-hood-3-deep-dive-into-the-event-loop-135d)
[https://stack overflow . com/questions/5011500](https://stackoverflow.com/questions/50115031/does-v8-have-an-event-loop)

读起来不错:
[https://dev . to/lunatic monk/understanding-the-node-js-event-loop-phases-and-how-it-executes-the-JavaScript-code-1j 9](https://dev.to/lunaticmonk/understanding-the-node-js-event-loop-phases-and-how-it-executes-the-javascript-code-1j9)