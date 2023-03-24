# NodeJS 中的指数退避算法

> 原文：<https://medium.com/nerd-for-tech/exponential-backoff-algorithm-in-nodejs-40e3d765b183?source=collection_archive---------0----------------------->

![](img/b89f7f09fddebc3ffda80c484fa95a6f.png)

吉姆·威尔逊在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在使用第三方库和 API 时，由于 API 的速率限制策略，我们有时可能会出错。如果发出的请求数量超过了每个时间限制的配额，就会发生这种情况。例如，每分钟 100 个对 POST /example/user 的请求。

我在使用 Google Sheets API 时也遇到过类似的错误。谷歌文档建议在等待一段时间后继续尝试指数补偿算法[https://cloud . Google . com/IOT/docs/how-tos/指数补偿](https://cloud.google.com/iot/docs/how-tos/exponential-backoff)

让我们动手看看代码。

```
const backoff = (fun, successFun, failureFun, exponent) => { console.log('Retrying after' + Math.pow(2, exponent)) setTimeout(async () => { const res = await fun() if (res.data) { console.log('success') successFun() } else if (exponent <= 10) { backoff(fun, successFun, failureFun, exponent + 1) } else { console.log('failure') failureFun() } }, Math.pow(2, exponent) + Math.random() * 1000)}
```

## 论点是:

1.  fun:这是一个进行第三方 API 调用的函数，在我的例子中是 google sheet API。这并不局限于一段特定的代码。`fun`函数可以有任何可能导致成功或失败的代码。
2.  successFun:这是一个帮助器函数，在成功运行`fun`后执行一段代码。这可以像控制台日志或在数据库中输入数据一样简单。
3.  failureFun:类似于 successFun，但是当重试次数超过。在上面的代码中，我们在结束一天的工作之前要重试 10 次。
4.  指数:指数是 2 的当前幂。这指定了重试前等待的时间。每次尝试后，我们都会增加这个值。

## 逻辑是:

退避函数的代码被包装在一个等待`Math.pow(2, exponent) + Math.random() * 1000`秒时间的 setTimeout 中。我们使用随机，以便 2 次或更多次重试同时呼叫的概率更小。
setTimeout 里面的事情很简单。我们只需使用`fun`进行 API 调用，如果成功，就会执行`successFun`块，并停止进一步的执行。
如果 API 调用失败，递增指数后再次调用`backoff`函数。这告诉代码在等待一段时间后重试。
如果 API 调用失败，并且达到了重试限制(上面代码中的 10 ),我们将在那里停止重试和执行，并执行`failureFun`

那都是乡亲们！