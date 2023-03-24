# 采用 Apollo Android 的 GraphQL，为什么值得一看。

> 原文：<https://medium.com/nerd-for-tech/graphql-with-apollo-android-why-its-worth-taking-a-look-f30ceb4a23ae?source=collection_archive---------2----------------------->

![](img/8ae2579325a254977db60c6fbad0cc96.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上[摄影师 ende](https://unsplash.com/@fotografierende?utm_source=medium&utm_medium=referral) 拍摄的照片

# **简介**

不久前，我从我的同事[Ramy](https://twitter.com/ramy17ak)那里听说了在[proxy m](https://www.proxym-group.com/)中工作的 GraphQL，我很想了解这种消费 API 的新方式，因为到目前为止，在 android 俱乐部中，REST 方法是主导者。

这看起来很有趣，我决定研究一下，然后我做了任何好奇的开发者都会做的事情，在互联网上询问这个问题。

事实证明这并不新鲜，GraphQL 是脸书从 2012 年开始开发的，并于 2015 年公开发布，所以让我们先问一些问题。

# **什么是 GraphQL**

*****长答案*** : GraphQL 都是数据通信。您有一个客户端和一个服务器，它们都需要相互通信。客户机需要告诉服务器它需要什么数据，而服务器需要用实际数据来满足客户机的数据需求。GraphQL 介入了这种交流。**

**GraphQL 服务器为客户机提供预定义的*模式*——可以从服务器请求的数据模型。换句话说，模式在定义如何访问数据时，充当了客户机和服务器之间的中间地带。**

*****模式*** 是描述您的数据的东西，它基本上是一个功能文档，列出了客户端可以向 GraphQL 层提出的所有问题。在如何使用模式方面有一些灵活性，因为我们在这里讨论的是节点图。T**

**该模式主要表示 GraphQL 层所能回答的限制。关于 Schema 和 GraphQL 的更多细节可以在这篇详细的文章中找到**

# ****GraphQL 操作****

**GraphQL 有三个主要操作:**

1.  *****查询*** 读取数据**
2.  *****突变*** 为写入数据**
3.  *****订阅*** 用于自动接收一段时间内的实时数据。**

**最常用的操作是查询和变异，你可以把*查询*想象成 GET 请求的等价物，而*变异就像 POST、PUT 和 DELETE。***

**[订阅](https://www.apollographql.com/docs/react/data/subscriptions/)使用 WebSocket 保持与您的 GraphQL 服务器的活动连接，这使您的服务器能够随着时间的推移推送订阅结果的更新**

# ****为什么选择 GraphQL****

**GraphQL 旨在使 API 快速、灵活且对开发人员友好。GraphQL 允许开发人员在单个 API 调用中构造从多个数据源提取数据的请求。换句话说，在一个请求中获得许多资源，并要求得到你所需要的，确切地得到它。**

# ****graph QL 与 REST 的主要区别****

**REST APIs 的最大问题是多端点的性质。一个中等规模的应用程序可以轻松拥有 100 多个端点，这需要客户端进行多次往返以获取数据。**

**为了说明 REST 和 GraphQL 在从 API 获取数据时的主要区别，让我们考虑一个真实的场景，假设我们将在脸书获取用户资料页面，为了便于讨论，我们将保持简单**

**使用 REST API，您通常会通过访问多个端点来收集数据。在本例中，这些可能是获取初始用户数据的`/user/<id>`端点，其次，可能有一个返回朋友列表的`/user/<id>/friends`端点，第三个端点将是返回帖子列表的`/user/<id>/posts`。**

**然而，一个 GraphQL API 只有一个端点，通常是`/graphql`。我们可以轻松地创建一个查询来替换上面所有的三个端点调用，并指定我们想要接收的确切属性。**

# ****采用安卓系统的 GraphQL】****

**为了在 android 应用程序中使用 GraphQL，我们将需要使用[Apollo 库](https://www.apollographql.com/docs/android/)，这是一个强类型客户端，它从 GraphQL 查询中生成 Java 和 Kotlin 模型，所以让我们讨论一下在 android 项目中运行 Apollo 所需的步骤。**

## **阿波罗装置**

**我们将从在项目级别添加这个插件开始**

```
//project dependencies
dependencies **{**
    classpath "com.apollographql.apollo:apollo-gradle-plugin:2.4.6" 
**}**
```

**然后在模块级别添加插件**

```
plugins **{**
    id 'com.apollographql.apollo'
**}**
```

**要让 Apollo 运行，我们还需要实现 Apollo 运行时库，如果我们想用 Apollo 操作支持协程，我们还需要添加 Apollo 协程库，Apollo 还可以支持 RxJava 和缓存。**

```
dependencies **{**
    def apollo = "2.4.6"
    implementation "com.apollographql.apollo:apollo-runtime:$apollo"
    implementation "com.apollographql.apollo:apollo-coroutines-support:$apollo"

**}**
```

## **模型生成**

**Apollo 有一个很好的特性，你可以生成 Kotlin 模型，你可以通过在 gradle 文件的模块层添加下面的代码来实现。**

```
apollo **{** generateKotlinModels.set(true)
**}**
```

## **添加模式**

**为了生成 java 模型，需要一个 GraphQL 模式和一个查询。模式通常可以从你的 [API playground](https://rickandmortyapi.com/graphql) 下载，在 android 应用程序中，模式应该添加在主文件夹下，如下例所示"[https://github . com/HamdiBoumaiza/apolloricandmorty/blob/master/app/src/main/graph QL/com/HB/rickandmortyapollo/Schema . SDL](https://github.com/HamdiBoumaiza/ApolloRickAndMorty/blob/master/app/src/main/graphql/com/hb/rickandmortyapollo/schema.sdl)**

## **添加查询**

**下面是一个 GraphQL 的示例查询，我使用了 [Rick 和 Morty API](https://rickandmortyapi.com/graphql)**

```
query GetCharacters($page: Int) {
    characters(page: $page) {
        info {
            pages, count, next
        }
        results {
            id, name, image,
            episode {
                id, name
            }
        }
    }
}
```

## **构建阿波罗客户端**

**创建一个基本的 Apollo 客户端是非常容易的，正如你在下面看到的，客户端可以用很多方式配置，例如你可以添加一个 *OkHttpClient* 或者添加一个*拦截器*。**

```
private fun apolloClient()=
    ApolloClient.builder().serverUrl(*BASE_URL*).build()
```

## **执行查询**

**设置好客户端后，我们现在可以执行我们的查询，正如您在下面看到的，我们只需使用查询函数，并将查询生成的类作为参数传递给它**

```
apolloClient().query(GetCharactersQuery(Input.optional(page)))
```

**从那以后，剩下的数据流就是你的责任了，我在[中加入了一个小应用](https://github.com/HamdiBoumaiza/ApolloRickAndMorty)，它展示了如何实现 Apollo，并演示了如何添加模式和查询并执行它们，我还用 MVVM 的匕首柄实现了一个干净的架构，请随意查看。**

# ****最终想法****

**读完这篇文章和其他关于 GraphQL 的文章后，你可能会想到一个问题，为什么 REST 仍然是主导者？**

**在这篇文章中，我主要讨论了为什么我们应该考虑 GraphQL，以及如何在 android 应用程序中实现它，但我也应该明确告诉你，GraphQL 有一些弱点，比如缓存和速率限制的复杂性**

**即使我喜欢 GraphQL，并且认为将来会有更多的人使用它，我也不认为 REST 会很快出现，特别是随着 [HTTP/2](https://developers.google.com/web/fundamentals/performance/http2) 的出现，但是对于移动和 web 应用程序来说，它可能是选择加入的更好的解决方案，绝对值得研究和考虑。**

**感谢阅读！**

**特别感谢 [Rim Gazzeh](https://twitter.com/RimGazzeh) 花时间审阅这篇文章并反馈意见，使之成为一个更好的版本。**

**如果你喜欢这篇文章，请随意分享，并在 [LinkedIn](https://www.linkedin.com/in/hamdi-boumaiza) 或 [Twitter](https://twitter.com/HamdiBoumaiza1) 上加我**

**[](https://github.com/HamdiBoumaiza/ApolloRickAndMorty) [## HamdiBoumaiza/ApolloRickAndMorty

### 你好，在这个项目中，我试图展示如何用干净的架构和 MVVM 构建一个 Android 应用程序…

github.com](https://github.com/HamdiBoumaiza/ApolloRickAndMorty)**