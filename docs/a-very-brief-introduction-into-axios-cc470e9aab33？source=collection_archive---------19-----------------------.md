# Axios 的简单介绍

> 原文：<https://medium.com/nerd-for-tech/a-very-brief-introduction-into-axios-cc470e9aab33?source=collection_archive---------19----------------------->

![](img/1f548872bb54b98415c968cae2ac403b.png)

这张照片归功于 Pexels 的 Markus Spiske

我一直致力于建立一个电子商务商店。这个个人项目源于学习如何使用 Stripe API 的愿望，因为我最近的许多技术项目都是前端的，使用 CSS、HTML 和 React。我一直对使用基于 FinTech 的 API 感兴趣，并希望继续磨练我的后端技能。对于这个项目，我想在用户点击检查他们的购物车时创建一个结帐表单，但我很快意识到可以使用 Stripe 做很多事情。

最终，我不得不寻求帮助；我不确定如何设置 Stripe API。另外，我*绝对*不希望一个好奇的用户或潜在的雇主和/或招聘人员输入任何真实的信用卡信息——那将是灾难性的。我找到了一个 YouTube 教程，除了 YouTube、 [Reed Barger](https://www.youtube.com/watch?v=lkA4rmo7W6k&ab_channel=ReedBarger) 没有从后端获取，而是导入并使用了一个名为 *Axios* 的包。

有人困惑吗？

## Axios 是什么？

[FlavioCopes](https://flaviocopes.com/axios/) 将 Axios 定义为“一个非常流行的 JavaScript 库，可以用来执行 HTTP 请求，可以在浏览器和 Node.js 平台上工作”。

您第一次接触从后端检索数据可能是通过 fetch，它在 Promise 对象中返回数据。然后将数据转换成 JSON，并在 web 或移动应用程序上呈现或解析信息。

在这个要点中，我们从本地主机获取。一旦获取得到解决，我们就将得到的响应转换到 JSON 中，然后在控制台中记录数据。

类似地，Axios 也在 Promise 对象中返回来自后端的信息。与 fetch()不同的是，来自 Logrocket 的 Faraz Kelhini 说“Axios 在发送请求时会自动将数据字符串化”。也就是说，Axios 返回的数据就是数据，不需要先转换成 JSON。

## 比 fetch()好还是差？

Axios 并不比使用 fetch()更好也不差。这是一个外部库，需要安装并导入到代码中。

除了字符串化数据，Axios 还可以用在后端，特别是 Node.js。因为它确实为我们字符串化了数据，所以可以说它也更干净。与 fetch()相比，Axios 还可以在更多的互联网浏览器上使用，这很有帮助。

相比之下，Fetch()是一个内置方法，所以它不需要任何安装。它确实要求您将信息的主体字符串化，并将其返回到 JSON，您可以用它进行解析。

随着我对 Axios 的写作和研究越来越多，我对这个库受欢迎的原因有了更多的了解。就我个人而言，我很期待用 Axios 构建更多的项目。虽然它可能并不适合每种情况，但是在审查和重构代码时，这个库似乎更容易使用和理解。

如果您想了解更多关于 Axios 的信息，以及使用 Axios 与 fetch 相比的优势，请查看下面的资源。

## 资源

[](/@MinimalGhost/what-is-axios-js-and-why-should-i-care-7eb72b111dc0) [## 什么是 Axios.js，我为什么要关心？

### 作为一名自学成才的开发人员，我已经尝试了免费/付费互联网教程的广阔天地…

medium.com](/@MinimalGhost/what-is-axios-js-and-why-should-i-care-7eb72b111dc0) [](https://dev.to/veewebcode/what-is-axios-and-how-to-use-it-4an1) [## AXIOS 是什么，怎么用！

### Axios 是一个基于 XMLHttpRequests 服务的轻量级 HTTP 客户端。它类似于 Fetch API，用于…

开发到](https://dev.to/veewebcode/what-is-axios-and-how-to-use-it-4an1) [](https://levelup.gitconnected.com/fetch-api-data-with-axios-and-display-it-in-a-react-app-with-hooks-3f9c8fa89e7b) [## 用 Axios 获取 API 数据，并用钩子在 React 应用程序中显示

### 本文将介绍如何在 React 中用 Axios 获取数据，将其保存到 state 中，然后在 React 中显示它…

levelup.gitconnected.com](https://levelup.gitconnected.com/fetch-api-data-with-axios-and-display-it-in-a-react-app-with-hooks-3f9c8fa89e7b) [](https://flaviocopes.com/axios/) [## 使用 Axios 的 HTTP 请求

### Axios 是一个非常流行的 JavaScript 库，您可以使用它来执行 HTTP 请求，它可以在浏览器和 Node.js 中运行

flaviocopes.com](https://flaviocopes.com/axios/) [](https://blog.logrocket.com/axios-or-fetch-api/) [## Axios 和 fetch():应该使用哪个？- LogRocket 博客

### 在我最近的文章“如何像专业人士一样使用 Axios 进行 HTTP 请求”中，我讨论了使用 Axios 的好处…

blog.logrocket.com](https://blog.logrocket.com/axios-or-fetch-api/) [](https://masteringjs.io/tutorials/axios/axios-node) [## 如何在 Node.js 中使用 Axios

### 当发出 http 请求时，用户可以选择使用普通 javascript 库中的 fetch()来在…

masteringjs.io](https://masteringjs.io/tutorials/axios/axios-node) [](https://dev.to/uma/fetch-vs-axios-http-requests-in-javascript-43jl) [## JavaScript 中的 Fetch 与 Axios HTTP 请求

### Fetch: Fetch 是一种较新的发送 HTTP 请求的方式。在 Fetch 之前，XMLHttpRequest 是一个非常...用 javascript 标记。

开发到](https://dev.to/uma/fetch-vs-axios-http-requests-in-javascript-43jl) [](https://www.geeksforgeeks.org/difference-between-fetch-and-axios-js-for-making-http-requests/) [## Fetch 和 Axios.js 发出 http 请求的区别

### 极客的计算机科学门户。它包含写得很好，很好的思想和很好的解释计算机科学和…

www.geeksforgeeks.org](https://www.geeksforgeeks.org/difference-between-fetch-and-axios-js-for-making-http-requests/)