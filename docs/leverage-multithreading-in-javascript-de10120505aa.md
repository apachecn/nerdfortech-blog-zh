# 利用 Javascript 中的多线程

> 原文：<https://medium.com/nerd-for-tech/leverage-multithreading-in-javascript-de10120505aa?source=collection_archive---------24----------------------->

![](img/023db5cc69eb71840dd6c8e3e1c03092.png)

我们知道 Javascript 是单线程的。单线程语言是一种只有一个调用栈和一个内存堆的语言。这意味着单个执行线程处理整个事件循环。尽管是单线程的，但是 Javascript 是异步的，到目前为止已经能够很好地解决复杂的现代 web 应用程序问题。然而，随着计算系统的复杂性成倍增加，在单个执行上下文上执行复杂算法变得更加麻烦，并导致网站加载缓慢，转化为糟糕的用户体验。

Javascript 有一个事件循环，它为我们的 web 应用程序提供了异步性，但在不阻塞单线程执行的情况下，它不足以允许前端的大量计算，从而导致浏览器完全崩溃。然而，有人可能会说，现在的计算机速度非常快，现代浏览器已经通过使用[每个站点实例的进程](https://www.chromium.org/developers/design-documents/process-models)或每个标签的不同线程进行了即兴创作，因此每个标签使用自己的线程，网页应该没有问题。对于大多数用例来说都是如此，直到您的浏览器运行复杂的动画或呈现复杂的可视化。这些费力的过程最终会阻塞主线程，进而降低页面渲染速度，导致点击/滚动事件触发缓慢，加载状态冗长，以及网页无响应。

在这种可怕的情况下，网络工作者前来救援。Web Workers 是由 Javascript 在 2009 年引入的，在一些浏览器中仍然是一个实验性的特性。Mozilla Firefox。但是使用 Web Workers 可以完全改变你的网页性能，特别是当客户端涉及大量计算时。

# 那么，什么是网络工作者呢？

Web Worker 是在不同于主线程的其他线程上执行的脚本。运行 javascript 的网页可以触发 Web Worker 脚本。然后，被调用的脚本在一个独立的线程上运行，该线程从主线程派生出来，从而减轻主线程的负担。这两个线程可以使用 postMessage API 通过消息进行通信。
根据个人的使用情况，也有两种类型的 Web Workers 可用，即-

1.**专用 Web Worker** —专用 Worker 只能由调用它的脚本访问。
2。共享 Web Worker —共享 Worker 可由多个脚本访问，即使它们被不同的窗口、iframes 或其他 Web Worker 调用，这也是适用的。

如果需要的话，Web Workers 可以产生更多的工作线程。但是它们都必须与父页面位于同一原点。

# 我用在哪里了？

我在一个应用程序中解决一个众所周知的复杂问题时遇到了 Web 工作者，这个应用程序使用了 [Tensorflow.js](https://www.tensorflow.org/js) 。我们使用基于 Tensorflow.js 构建的人脸检测人工智能库来开发监考应用程序。我们在集成库时遇到了一些问题，因为模型很大，并且在浏览器中加载需要很长时间。此外，计算检测值的算法是一个长时间运行的资源密集型过程，因此会使浏览器崩溃或导致系统完全挂起。网络工作者是我们困境的答案。我们使用 Web Workers 生成一个单独的执行上下文，并在其中运行人脸检测算法。这使得主线程从所有繁重的计算中解脱出来，并无缝地处理其他事件和进程。

# 我们如何实现 Web Workers？

好了，关于 Web Workers 的解释已经足够了，让我们开始实际实现一个 Web Workers。Web Workers 实现起来非常简单。您将需要至少 2 个文件，一个是将在主线程上运行的主脚本。另一个是在专用 Web worker 内部运行的 Worker 脚本。理想的做法是将 worker 脚本保存在一个单独的文件中，因为它将在一个不同的、隔离的执行上下文中运行。

让我们分别将文件命名为 mainFile.js 和 webWorker.js。

在 mainFile.js 中初始化一个运行在主线程上的专用 web worker。

共享的 Web Worker 可以用几乎相同的方式创建，唯一的区别是在实例化过程中使用的构造函数。我们使用`SharedWorker()`构造函数来产生一个共享的 Web Worker。

初始化共享 web 工作线程。

如果指定的文件存在，那么它将被异步下载，如果不存在，那么 Worker 将会无声地失败，即使在 404 错误的情况下，您的 web 应用程序也将工作。

现在我们已经有了专门的 Web Worker，让我们通过从主线程发送消息来与它通信。Web Worker 和主线程使用 postMessage() API 进行通信。

使用浏览器的 postMessage() API 从主线程向工作线程发送消息。

现在，我们的 Web Worker 必须监听来自主线程的消息，以便能够响应它们。我们现在将设置一个监听器，如下所示-

通过 onmessage() API 监听工作线程，以接收和响应来自主线程的消息。

上面的例子展示了 Web Worker 如何通过 onmessage() API 监听来自主线程的消息。

然而，我们从主线程与共享 Web Worker 通信的方式有所不同。

通过端口对象建立连接，并向共享工作者发送消息。

对于共享的 Web worker，我们需要通过`port`对象进行通信——打开一个显式端口，脚本可以通过该端口与 Worker 实例进行通信(在专用 Worker 的情况下，这是隐式完成的)。我们还需要显式调用`start()`函数来建立通信。

在共享工作线程上设置侦听器也略有不同。考虑以下情况-

共享工作线程上的侦听器，用于侦听和响应来自主线程的消息。

从上面的例子可以推断，一个共享的 Web Worker 脚本需要监听`connect`事件，这样无论何时任何外部上下文请求连接，Worker 脚本都可以做出适当的响应。

事件对象让我们能够访问`ports`数组，这有助于我们获得打开连接请求的确切端口。请注意，除了监听消息事件，我们还可以维护一个保存的端口数组，并使用该数组将消息发送到多个上下文，这些上下文调用共享工作器并与之通信。

我们还可以通过调用 terminate()函数从主线程中终止一个正在运行的专用 Web Worker 实例

从主线程终止工作线程。

Web Worker 实例及其所有正在进行的操作(如果有的话)会立即被终止。

类似地，在共享 Web 工作线程的情况下，我们可以通过调用`port.close()`函数来关闭连接，然后工作线程将被立即终止，没有机会完成其操作或清理自身。

通过关闭端口连接来终止共享 web worker。

对于共享 Web Worker 的多个连接，端口连接将只对调用`close()`函数的那个特定实例关闭。所有其他连接将保持打开。您可以通过在不同的选项卡中运行您的应用程序进行检查，然后尝试关闭一些随机端口，其他端口将保持活动状态，因此不受影响。

上面的例子实现了一个简单的 Web Worker，并演示了它与主线程的通信。需要使用 Web worker 的真实场景很难轻松实现，因此我们必须了解 Web worker 实现中的注意事项。

# 有什么条件？

## Web 工作人员无法访问 DOM -

由于 Web Workers 运行在一个单独的执行上下文中，所以它们不能访问 DOM 元素。人们将不能操纵 DOM，不能从主线程访问全局变量，也不能触发它的函数。人们也不能访问`[window](https://developer.mozilla.org/en-US/docs/Web/API/Window)`对象的默认方法和属性。但是，您可以访问 setTimeout()和 setInterval()等函数。您也可以使用 XMLHttpRequest 对象向服务器发出 Ajax 请求。你也可以使用[网络套接字](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API)和数据存储机制，如[索引数据库](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)。根据所使用的工作线程的种类，可以通过[***deditedworkerglobalscope***](https://developer.mozilla.org/en-US/docs/Web/API/DedicatedWorkerGlobalScope)*或*[***SharedWorkerGlobalScope***](https://developer.mozilla.org/en-US/docs/Web/API/SharedWorkerGlobalScope)*来访问工作线程内的上下文。前者分别与一个专用的 Web Worker 一起工作，后者与一个共享的 Web Worker 一起工作。***

## ***内容安全政策-***

***Web Workers 有自己的执行上下文，独立于产生它们的文档，因此它们不受父线程/worker 的内容安全策略的控制。例外情况是工作脚本的来源是一个全局唯一标识符(例如，如果它的 URL 有一个数据或 blob 方案)。在这种情况下，Worker 继承调用它的文档或 Worker 的内容安全策略。***

## ***传递给 Web 工作者的数据量会影响性能。此外，支持的类型是有限的-***

***您传递给 Web Worker 的数据量直接影响到您的网页的性能。正如已经讨论过的，Web Workers 有一个单独的执行上下文，这意味着它也有一个单独的内存和调用堆栈，并且不能从主线程访问变量。因此，传递到工作线程上的所有信息都使用[结构化克隆算法](http://w3c.github.io/html/infrastructure.html#safe-passing-of-structured-data)进行克隆或复制。该算法递归地遍历数据(通常作为对象传递)。虽然这可能不会对小型数据集造成影响，但是将一个包含大量深度嵌套的键值对的大型对象传递给一个 worker 会显著降低线程间通信的速度，并且还会消耗所有的计算资源，从而在无意中造成弊大于利。网络工作者接受的数据类型也有限制。不能使用结构化克隆算法克隆的东西不能传递给 Web Worker ex。函数、DOM 节点和一些属性在克隆时也会丢失。克隆对象的一种方法是使用可转移的对象，如 [SharedArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer) 。***

## ***确保对 SharedArrayBuffer 这样的可传输对象进行原子更新***

***如果不提供共享内存，多线程就无法真正实现。如果每次需要更改时都要复制粘贴整个数据结构，那么在单独的线程上运行代码的想法就毫无意义。对此也有一个变通方法，叫做 [SharedArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer) 。如 MDN 所解释的，SharedArrayBuffer 是一个用于表示通用的、固定长度的原始二进制数据缓冲区的对象。这意味着我们可以在两个线程上更新相同的共享内存，而不是发送数据并从主线程中克隆它。这允许两个线程都引用共享内存，从而完全消除了克隆数据的需要。像这样-***

***在主线程上创建 sharedArrayBuffer。***

***当 worker 收到这个缓冲区时，我们可以使用相同的缓冲区创建另一个数组，类似于上面的方法***

***从主线程传递的缓冲区创建一个数组。***

***现在，我们可以在任一线程上引用同一个共享内存实例，剩下的唯一问题是跨线程无缝地更新这些值。***

***当在多个线程间共享内存时，我们可能会遇到一种叫做[竞争条件](https://en.wikipedia.org/wiki/Race_condition)的情况。当多个线程试图同时修改相同的数据时，可能会发生争用情况。这导致陈旧和不一致的数据。这带来了[原子](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Atomics)对象的概念。***

> ***原子对象提供了在共享存储器阵列单元上不可分割地(原子地)操作的功能，以及让代理等待和分派原始事件的功能。当与规程一起使用时，原子函数允许通过共享内存进行通信的多代理程序以一种很好理解的顺序执行，即使是在并行 CPU 上。***

***简而言之，Atomics 允许您对共享数据进行可预测的操作。原子不仅限于读/写操作。有时候多线程操作需要你监听并响应变化。Atomics 提供了所有上述需求。Atomics 提供了对一组静态方法的访问，这些方法有助于处理和减轻多线程的危险，如死锁、不可预测的读写操作顺序以及数据碎片。***

# ***结论***

***JavaScript 中的多线程正在慢慢获得动力和流行。能够将工作分流到多个线程并在它们之间共享内存的前景有着巨大的潜力。在我看来，`SharedArrayBuffers`和多线程的想法是有益的，尤其是对于 NodeJS 服务。就跨浏览器支持而言，它仍处于初级阶段。对[工作线程](https://nodejs.org/api/worker_threads.html)的支持在大多数浏览器中仍处于试验阶段，谷歌 Chrome 是兼容的浏览器之一。Javascript 多线程很有前途，可以改变传统的 web 应用程序功能。我们可以通过 web workers 将复杂的动画、人工智能集成和更复杂的计算能力带到客户端应用程序中。随着采用的逐渐增加，我们可能会在网络上看到更多这样的东西，直到那时，学习和尝试新事物永远不会是一种损失。***

***编码快乐！！***