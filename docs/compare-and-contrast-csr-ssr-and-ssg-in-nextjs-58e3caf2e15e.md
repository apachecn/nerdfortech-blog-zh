# 比较和对比:下一个 JS 中的 CSR、SSR 和 SSG

> 原文：<https://medium.com/nerd-for-tech/compare-and-contrast-csr-ssr-and-ssg-in-nextjs-58e3caf2e15e?source=collection_archive---------0----------------------->

![](img/d3c0bc2852472b5e8345fb228ebf4201.png)

照片由[克里斯托弗·罗宾·艾宾浩斯](https://unsplash.com/@cebbbinghaus?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

自从 NextJS v9.3 发布以来，我们已经为 React 应用程序提供了三种不同的渲染选项。

*   客户端渲染
*   服务器端渲染
*   静态站点生成

每种方法都有其使用案例和优点。让我们探索一下我们想要选择其中一个而不是另一个的情况，以及如何在我们的代码中实现每一个的一些例子。

## 1)客户端渲染

![](img/375ba2b64980cb8138ed12502f50772c.png)

照片由 [Remotar 乔布斯](https://unsplash.com/@remotarjobs?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

它是如何工作的？

客户端呈现本质上意味着大部分工作(获取数据和创建 HTML)是由浏览器根据用户的需求来完成的。

a)客户端通过 URL 请求页面。

b)服务器用一个空白页响应客户机，并引用 JavaScript 文件。

c)这些由浏览器下载并运行，用于构建页面。

d)根据 JavaScript 的需要，从 API 获取和填充数据。

这有一些好处，但也存在一些问题:

**客户端渲染优点:**

*   好快啊！服务器很容易出售一个空白页面，而且这个方法只生成需要显示的 HTML，所以您的浏览器可以很容易地用 hide 和 show 事件处理一大块元素。虽然要处理的代码比较多，但是渲染的时间并不多。由于延迟加载，客户端渲染可能比服务器端渲染快得多。
*   这是表演！与需要刷新或重新呈现整个页面的传统 HTML 页面不同，客户端呈现模拟不同的页面，但将它们加载到单个页面上。这减轻了内存和处理能力的压力，比服务器端渲染的结果更快。
*   非常适合单页应用程序。没有其他模型支持 SPA，这允许我们在应用程序中重用 UI 组件，而无需再次从服务器请求它们。这与企业社会责任的快速和高效的本质密切相关。

**客户端渲染缺点:**

*   你最初提供的是一个空白页，所以当客户第一次访问你的页面时，他们看到的也是一个空白页。在 JavaScript 应用程序的初始加载和页面构建过程中，等待时间比服务器端呈现要长。
*   搜索引擎优化或 SEO。由于您的元数据是由 JavaScript 在第一次加载时加载的，所以当搜索引擎的网络爬虫访问您的页面时，搜索引擎将看不到这些数据。这对于服务器端呈现来说不是问题，因为提供给客户端的初始页面已经包含了所有这些数据。
*   如今，JavaScript 包可能非常大，所以特别是在慢速连接上，将所有这些代码下载到客户端可能需要一些时间，这又使得页面的初始加载比理想情况慢。

## 履行

我们如何告诉 NextJS 我们想使用 CSR？让我们来看看一些代码。

我们不需要在用于 CSR 的 React 组件中使用任何特殊的 NextJS 函数。我们可以使用 input 或类似于`useEffect`的钩子发出传统的 HTTP 请求。

```
import React, { useState, useEffect } from 'react'

const Home = () => {
  const [data, setData] = useState([])
  useEffect(() => {
    const getData = async () => {
      const response = await fetch('[https://official-joke-api.appspot.com/random_ten](https://official-joke-api.appspot.com/random_ten)')
      const data = await response.json()
      setData(data)
    }
    getData()
  })
  return (
    <main>
      <h1>Here are some Jokes!</h1>
      <ul>
        {data.map(joke => (
          <li key={joke.id}>{joke.setup} - {joke.punchline}</li>
        ))}
      </ul>
    </main>
  )
}

export default Home
```

简单的设置，服务器不做任何繁重的工作。客户端将接收此代码，自己发出请求并构建页面。

## 2)服务器端渲染

![](img/ea94ef15e67f7b63a3381f026027d0fd.png)

泰勒·维克在 Unsplash[上的照片](https://unsplash.com?utm_source=medium&utm_medium=referral)

它是如何工作的？

服务器端渲染允许我们在服务器上运行 React 应用程序，因此在站点到达浏览器之前，我们就完成了繁重的工作。

a)客户端通过 URL 请求页面。

b)服务器接收请求并运行附加的 Javascript，构建自己的 DOM 元素。

c)服务器从 API 请求所需的数据，并将这些数据作为道具提供给 React 组件。

d)服务器构建 HTML 并将完成的页面作为响应发送给客户端。

**服务器端渲染优点:**

*   立竿见影！当客户端收到第一页时，数据已经可用。这是慢速连接的理想选择。
*   没有客户端获取！服务器已经检索并实现了数据，因此客户端不需要自己发出任何 HTTP 请求。
*   伟大的搜索引擎优化！因为当页面被请求时，所有的元数据都已经呈现到 HTML 中，所以网络爬虫将会看到来自应用程序的所有数据，这对于 SEO 可见性是理想的。

**服务器端渲染缺点:**

*   缓慢的页面转换。你基本上要渲染你的应用两次，一次在服务器上，一次在客户端，所以特别是如果你的应用很大，这会影响页面之间的加载时间。
*   潜伏。由于服务器正在执行渲染工作，如果有许多用户同时访问应用程序，他们可能会在加载应用程序时遇到延迟。
*   UI 兼容性。一些 UI 组件可能严重依赖于 window 对象，当服务器呈现页面时，window 对象实际上并不存在。这可能会导致某些库的兼容性问题。

## 履行

让我们看看如何创建一个服务器端呈现的组件。

```
import React from 'react'export async function getServerSideProps(context) {
    const response = await fetch('[https://official-joke-api.appspot.com/random_ten](https://official-joke-api.appspot.com/random_ten)')
    const data = await response.json()return {
    props: { data }
  }
}const Home = ({ data }) => {
  return (
    <main>
      <h1>Here are some Jokes!</h1>
      <ul>
        {data.map(joke => (
          <li key={joke.id}>{joke.setup} - {joke.punchline}</li>
        ))}
      </ul>
    </main>
  )
}

export default Home
```

导出这个 NextJS 函数`getServerSideProps`将指示应用程序在离开服务器之前检索它的数据，并将其作为 props 提供给组件。然后，HTML 被呈现并发送给客户端。

## 2)静态站点生成

![](img/7cba48076d051ec45ae1387d54c2131a.png)

照片由[扎克船只](https://unsplash.com/@zvessels55?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

它是如何工作的？

静态站点生成基于页面所需的数据创建了许多静态路径。在构建时，这些路径被呈现到静态页面中，并以令人难以置信的速度提供给客户端。

a)客户端向服务器发出请求。

b)服务器接收请求，并且已经为静态数据构建了所需的 HTML 页面。

c)服务器基于所请求的 URL 以完整的、预先构建的页面进行响应。

**静态站点生成优点:**

*   就像服务器端呈现一样，静态站点生成是立即可用的，并且不需要从 API 获取额外的数据。
*   伟大的搜索引擎优化！由于 HTML 是在发送给客户端之前构建的，所以 SSG 的 SEO 可见性是理想的。
*   难以置信的快！静态页面的服务时间快得令人难以置信，与我们在客户端渲染中发送的空白页面的出售时间相当。
*   无服务器！提供静态页面不需要服务器来监控，所以您可以充分利用为提供静态页面而设计的服务。

**静态站点生成缺点:**

*   如果你的站点很大很复杂，构建时间会很长。
*   数据在构建时只提取一次，所以不能动态地重新提取和刷新数据。
*   您可能会遇到与服务器端生成相同的 UI 兼容性问题，因为 window 对象在构建时不可用。

## 履行

NextJS 文档描述了静态站点生成的两种场景。首先是创建一个不依赖于任何外部数据的静态页面。在这种情况下，我们只需要使用 NextJS `getStaticProps`函数来加载一些静态资产。

```
function Blog({ posts }) {
  return (
    <ul>
      {posts.map((post) => (
        <li>{post.title}</li>
      ))}
    </ul>
  )
}

export async function getStaticProps() {
  const res = await fetch('https://.../posts')
  const posts = await res.json()

  return {
    props: {
      posts,
    },
  }
}

export default Blog
```

就像`getServerSideProps`一样，我们在构建时将数据作为道具输入到组件中，页面生成为静态页面，并根据客户端的请求提供服务。

如果我们需要为我们的数据创建动态路由，例如，转到博客文章页面查看更多信息。为了处理这个问题，Next.js 允许您使用语法(pages/* resource */[* paramater *]):`pages/posts/[id].js` 从动态页面导出一个名为`getStaticPaths`的异步函数。

这允许您在构建时挑选要预先呈现到静态页面中的路径。

```
export async function getStaticPaths() {
  const res = await fetch('https://.../posts')
  const posts = await res.json() const paths = posts.map((post) => ({
    params: { id: post.id },
  })) return { paths, fallback: false }
}
```

我们还可以设置一个回退，在这个例子中是`false`，所以如果没有找到路径，我们将只呈现一个 404。

# 结论(用例):

*   客户端渲染非常适合登录体验和单页面应用程序。如果您需要在每次交互时对用户进行身份验证，CSR 是一个不错的选择。在性能不重要的情况下，创建概念证明是另一个用例。
*   服务器端呈现非常适合为慢速连接创建页面，并保持 SEO 可见性。
*   如果我需要服务器端呈现的好处，但只依赖于不经常改变的数据集，静态站点生成将是完美的，因为页面需要重新构建以反映新的依赖数据。

NextJS 处于最大化互联网体验的前沿，这些选项为我们的网站生成方案提供了广泛的灵活性。查看文档以了解更多信息！

附加阅读:

[](https://nextjs.org/docs/basic-features/pages) [## 基本功能:Pages | Next.js

### Next.js 页面是在 pages 目录下的文件中导出的 React 组件。了解他们如何在这里工作。

nextjs.org](https://nextjs.org/docs/basic-features/pages)