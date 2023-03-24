# 服务器端与客户端渲染

> 原文：<https://medium.com/nerd-for-tech/server-side-vs-client-side-rendering-d6773ad896a7?source=collection_archive---------5----------------------->

![](img/4811842e60f0a26342fbd6e42b3c63fc.png)

在 Pexels 的图片归功于[安东尼奥·巴蒂尼奇](https://www.pexels.com/photo/black-screen-with-code-4164418/)

最近，有人问我关于 React 框架，以及选择和使用这个特定框架的好处。说实话，我不确定:我知道这个框架比同一领域的其他框架加载得更快，但目前无法解释得更深入。

在这次特别的谈话以及更多关于前端开发的谈话之后，我想把我的注意力集中在服务器端渲染和客户端渲染之间的差异，以及使用或的好处。

## 什么是服务器端渲染？

服务器端呈现是显示网页的一种方式。这就是直到最近网络的工作方式。这种类型的呈现利用 HTML 和 CSS，转换它以便 Web 浏览器能够理解和显示它。很长一段时间以来——事实上，自从网络被创建以来——网页就是这样显示的。

## 什么是客户端渲染？

客户端呈现是显示网页的另一种方式。客户端渲染不使用静态的 HTML 和 CSS，而是使用 JavaScript 来运行和显示网页。使用客户端渲染相对来说是最近的事情，并且在前端开发中得到了普及。

## 服务器端渲染的优点/缺点

赞成的意见

1.  更好的搜索引擎优化——网络爬虫在服务器端渲染更容易，这可以改善你的网站在搜索结果中的外观
2.  非常适合静态网站——不是每个网站都需要交互式媒体或动画。如果你是这种情况，也许有一个由 HTML 和 CSS 组成的网站就可以了。
3.  与客户端渲染相比，初始页面加载速度更快

骗局

1.  不适合动态网站
2.  不是登录页面的页面可能会加载较慢
3.  多次重新加载—使用服务器端呈现，您必须频繁请求服务器显示新的和/或不同的页面。

## 客户端渲染的优点/缺点

赞成的意见

1.  动态互动网站的理想选择
2.  更快的加载时间——如果你希望你的用户被定向到不同的页面，它的加载速度会比服务器端框架快得多
3.  各种 JavaScript 库的广泛选择——稍后将详细介绍！

骗局

1.  对 SEO 来说不太好——网络爬虫会有更困难的时候，这可能会导致你的网站出现在搜索结果中
2.  登录页面的初始加载时间较慢
3.  可能需要额外的库

关于这方面的更多内容，我推荐[克里斯蒂安·维加的博客](https://www.freecodecamp.org/news/what-exactly-is-client-side-rendering-and-hows-it-different-from-server-side-rendering-bd5c786b340d/)——克里斯蒂安在他们自己的作品中阐明了许多观点。

## 有哪些客户端框架的例子？

有很多客户端框架，包括:

1.  [Angular.js](https://angularjs.org/)

3.  [React.js](https://reactjs.org/)
4.  [Svelte.js](https://svelte.dev/)

## 有哪些服务器端框架的例子？

还有各种各样的服务器端框架可供选择，包括:

1.  Next.js: Next.js 构建在 React 库之上。鉴于其快速的渲染时间，这是开发人员的普遍选择。它也为你的网站提供了更好的搜索引擎优化。
2.  Nuxt.js: Nuxt.js 构建在 Vue 库之上。
3.  Nest.js: Nest.js 是一个前端框架，构建在 Angular 库之上。

客户端和服务器端渲染都有许多优点和缺点。随着前端开发的继续发展，不难想象会产生什么样的新框架，以及它会如何改变我们目前对前端的理解和交互方式。

如需更多阅读资料，请查阅下面的参考资料。

## 资源

[](https://www.freecodecamp.org/news/what-exactly-is-client-side-rendering-and-hows-it-different-from-server-side-rendering-bd5c786b340d/) [## 客户端与服务器端渲染:为什么不全是黑白的

### 自古以来，将 HTML 放到屏幕上的传统方法是使用服务器端…

www.freecodecamp.org](https://www.freecodecamp.org/news/what-exactly-is-client-side-rendering-and-hows-it-different-from-server-side-rendering-bd5c786b340d/) [](https://dev.to/seal125/what-is-server-side-rendering-22ik) [## 什么是服务器端渲染？

### 多年来，在页面上呈现内容已经发展到比仅仅从一个…

开发到](https://dev.to/seal125/what-is-server-side-rendering-22ik) [](https://javascript.plainenglish.io/top-10-javascript-frameworks-for-server-side-development-in-2020-6d265016c02) [## 2020 年服务器端开发的十大 JavaScript 框架

### 最重要的后端 JavaScript 框架的精选列表

javascript.plainenglish.io](https://javascript.plainenglish.io/top-10-javascript-frameworks-for-server-side-development-in-2020-6d265016c02) [](https://yudhajitadhikary.medium.com/client-side-rendering-vs-server-side-rendering-in-react-js-next-js-b74b909c7c51) [## React js 和 Next js 中客户端渲染与服务器端渲染的比较

### 嗨，在这篇文章中，我想和你们分享我开始使用 Next.js 的经历。我们将讨论…

yudhajitadhikary.medium.com](https://yudhajitadhikary.medium.com/client-side-rendering-vs-server-side-rendering-in-react-js-next-js-b74b909c7c51) [](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks) [## 了解客户端 JavaScript 框架-学习 web 开发| MDN

### JavaScript 框架是现代前端 web 开发的重要组成部分，它为开发人员提供了经过尝试的…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks)