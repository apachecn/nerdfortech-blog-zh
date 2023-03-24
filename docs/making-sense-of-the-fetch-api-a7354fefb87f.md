# 理解获取 API

> 原文：<https://medium.com/nerd-for-tech/making-sense-of-the-fetch-api-a7354fefb87f?source=collection_archive---------20----------------------->

我最近开始走上漫长而曲折的道路，成为一名软件工程师。老实说，这条路有时似乎仍然令人生畏，但我已经爱上了软件工程中包含日常工作的动手解决问题的部分。我也非常喜欢到目前为止我学到的材料。在过去的几天和几周里，我一直在讨论的一个话题是 Fetch API。在这篇博客中，我将回顾 Fetch API 的基础知识。但是，首先我将解释什么是 API，它是如何工作的，以及数据传输过程 pre-fetch()。这将有助于突出 Fetch API 的独特功能，尤其是相对于它的 AJAX 前身 XMLHttpRequest。

# 什么是 API？

API 代表应用程序编程接口。IBM 在其网站上将 API 解释为“一组定义好的规则，解释了计算机或应用程序如何相互通信。API 位于应用程序和 web 服务器之间，充当处理系统间数据传输的中间层”(参见 IBM 网站[这里](https://www.ibm.com/cloud/learn/api))。作为一个初学软件开发的人，这个想法是有意义的 API 作为“中间层”的想法允许数据从一个系统传递到另一个系统。IBM 再一次提供了一个简单而有效的描述，描述了*一个基本 API 是如何工作的。*

> 1.**客户端应用程序启动 API** 调用来检索信息，也称为*请求*。这个请求通过 API 的统一资源标识符(Uniform Resource Identifier，URI)从应用程序发送到 web 服务器，包括请求动词、请求头，有时还包括请求体。
> 
> 2.**收到有效请求**后，API 调用外部程序或 web 服务器。
> 
> 3.**服务器向 API 发送一个*响应*** ，其中包含所请求的信息。
> 
> 4.**API 将数据**传输到最初发出请求的应用程序。
> 
> *(这些步骤摘自*[](https://www.ibm.com/cloud/learn/api)**站点)**

*现在我们对 API 是什么以及它是如何工作的有了一个基本的了解，让我们看看在 fetch()出现之前这种请求-响应风格的数据传输是如何发生的。*

# *互联网预取()*

*Fetch API 及其前身 **XMLHttpRequest** 的创建是基于一个革命性概念[**【AJAX】**](https://www.w3schools.com/whatis/whatis_ajax.asp)(**异步 JavaScript 和** [**XML**](https://www.w3schools.com/xml/xml_whatis.asp) )的开发和实现。本质上，AJAX 允许网页异步更新。这意味着网页的特定部分可以在“幕后”交换数据时更新。在 AJAX 出现之前，数据交换甚至需要对网页进行最小的更新，这就需要刷新整个页面，这使得 AJAX 在增强用户体验和数据传输能力方面都取得了巨大的突破。XMLHttpRequest 对象成为创建 AJAX 请求的第一种方法——从 URL 中检索数据，只更新部分网页而无需刷新。XMLHttpRequest 对象可以检索任何类型的数据(顾名思义，不仅仅是 XML)。*

# *获取 API*

*Fetch 是在 XMLHttpRequest 之后开发的本地 JavaScript API，它利用了 AJAX 编程。以下是取自 [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) 的基本获取请求的结构:*

```
*fetch('http://example.com/movies.json')
  .then(response => response.json())
  .then(data => console.log(data));*
```

*让我们回顾一下这段代码中发生了什么。首先，**获取**正在从 URL `[http://example.com/movies.json](http://example.com/movies.json)`检索数据。然后 fetch 返回一个**承诺**(关于承诺的更多信息，请查看 Jake Archibald 的这篇非常有用的文章，在这里找到)。不管数据是否被成功检索，fetch 都将返回一个 Promise 对象。承诺然后解析为一个**响应**对象。接下来，第一个**。然后**函数将获取这个响应对象，并使用内置的**将其转换为 **JSON** 。json()** 方法(有关 json 的更多信息，请查看[这个](https://www.w3schools.com/whatis/whatis_json.asp)页面)。最后，第二个**。然后**取之前转换成 JSON 的对象。然后 console.log 的那个对象。另外，一个**。catch()** 函数可以添加到上面的代码中，以便在请求失败时接收一个**错误**对象。*

*fetch()方法还接受可选的第二个参数——一个**配置(或 init)对象**。配置对象允许以多种不同的方式检索和操作后端数据。MDN 提供了一个使用配置对象的获取请求的有用示例(在这里可以找到)。然而，在其核心，配置对象不仅允许 fetch()检索数据，还允许**c**create、 **r** ead、 **u** pdate 和**d**delete 数据(简称 CRUD)。我们期望现代 API 能够执行 CRUD 功能。如前所述，fetch()方法通过指定“请求动词、头，有时还有请求体”来满足这些期望。例如，可以使用 fetch()和包含请求动词 **POST** 的配置对象来创建数据，通过包含请求动词 **GET** 来读取数据，通过包含**补丁**来更新数据，或者通过包含 **DELETE** 来删除数据。还有其他类型的请求，但这四种是最基本的。在完成挑战和构建小型项目时，我几乎完全依赖这些请求。*

# *是什么让 Fetch API 比 XMLHttpRequest 好？*

*其他博客已经专门讨论了这个问题，但是我将简要概述使用 fetch()相对于 XMLHttpRequest 的主要优势。首先，Fetch API 比使用 XMLHttpRequest 对象简单。它需要较少的逻辑硬编码到请求中(特别是在处理响应和错误时)。此外，在 Fetch API 中使用承诺和定义良好的请求和响应对象，有助于创建相对于同等的 XMLHttpRequest 代码更干净、更容易理解的代码。总的来说，Fetch API 相对于 XMLHttpRequest“提供了更强大、更灵活的特性集”(参见 [MDN Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) )。如果你对 fetch()和 XMLHttpRequest 有兴趣，请查看 Swapnil Bangare 的博客[*Fetch API*](/beginners-guide-to-mobile-web-development/the-fetch-api-2c962591f5c#:~:text=The%20Fetch%20API%20makes%20it,modern%20JavaScript%20features%20like%20promises%20.)，以及相关的 [Google 开发者文档](https://developers.google.com/web/ilt/pwa/working-with-the-fetch-api)。*

# *结论*

*在这篇博客中，我谈到了几个不同的主题——都围绕着 Fetch API。首先，我提供了对 API 的基本理解。第二，我将 Fetch API 放在请求-响应周期的更广泛的背景中，这个周期描述了我们作为客户端如何与网站进行交互以访问和操作数据。随着 AJAX 的出现，这个循环现在是异步的。第三，我概述了 fetch()的基础知识。最后，我简要比较了 fetch()和它的前身 XMLHttpRequest。我希望你从我的博客中学到了一些关于 Fetch API 的知识。我喜欢学习 API 以及数据如何从一个系统传输到另一个系统，随着我作为软件工程师的不断学习和成长，我期待着定期发布任何启发我的新软件工程想法或主题！*