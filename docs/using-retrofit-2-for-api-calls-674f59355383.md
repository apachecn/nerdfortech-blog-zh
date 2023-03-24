# 对 API 调用使用 refuge-2

> 原文：<https://medium.com/nerd-for-tech/using-retrofit-2-for-api-calls-674f59355383?source=collection_archive---------2----------------------->

![](img/2fc910487c74e0117e09d4949158edc0.png)

照片由[阿列克斯·马林科维奇](https://unsplash.com/@aleks_marinkovic?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

Retrofit 是一个类型安全的网络库，它使得 android 应用程序可以轻松地使用 RESTful web 服务。它由 Square 构建，使得获取 JSON 或 XML 数据变得容易，这些数据被解析成普通的旧 Java 对象(POJOs)。
用更简单的语言来说,“翻新”是一个库，它通过提供一种简单的方式来进行网络交易，从而使我们的生活变得更容易。通过基于 Rest 的 web 服务从服务器上传或检索数据非常快速和容易。

默认情况下，翻新 2 利用 [**OkHttp**](https://www.journaldev.com/13629/okhttp-android-example-tutorial) 作为网络层，并构建于其上。

翻新使用 POJO(Plain Old Java Object)自动序列化 JSON 响应，POJO 必须预先为 JSON 结构定义。为了序列化 JSON，我们首先需要一个转换器将它转换成 Gson。

为了改进，我们需要将以下依赖项添加到我们的`build.grade`文件中。

*   日志拦截器生成返回的整个响应的日志字符串。
*   默认情况下，翻新 2 利用 [**OkHttp**](https://www.journaldev.com/13629/okhttp-android-example-tutorial) 作为网络层，并构建在它的顶部。
*   为了序列化 JSON，我们首先需要一个转换器将它转换成 Gson

**的第一步**是创建一个 POJO 类，它表示您想要从 API 读取或发送到 API 的数据。

假设我们在 API 和 JSON 中获取客户信息，如下所示

因此，现在我们必须创建一个相应的 Pojo 类，它表示这个 JSON 中的数据，定义如下-

第二步是创建我们的接口类，这有助于用方法定义我们的 API 端点

在上面的类中，我们已经定义了一些执行带有注释的 HTTP 请求的方法。每个 HTTP 方法的其他可能的注释列表如下:`@GET, @POST, @PUT, @DELETE, @PATCH or @HEAD`。

`@GET("api/users")`谓`getCustomerList();`。

`getCustomerList()`是方法名。`CustomerDataModel.java`是我们的响应对象的模型 POJO 类，用于将响应参数映射到它们各自的变量。这些 POJO 类充当方法返回类型。

**方法参数**:有各种各样的参数选项可以在方法内部传递:

*   `@Body`–将 Java 对象作为请求体发送。
*   `@Url`–使用动态网址。
*   `@Query`–我们可以简单地用@Query()添加一个方法参数和一个查询参数名，描述类型。要对查询进行 URL 编码，请使用以下格式:
    `@Query(value = "auth_token",encoded = true) auth_token:String`
*   `@Field`–以 form-urlencoded 格式发送数据。这需要方法附带一个`@FormUrlEncoded`注释。
    `@Field`参数仅适用于 POST

**第三步**是创建我们的改造构建器类，用改造创建网络请求 REST API。这是您定义 API 的基本 URL 以及其他配置的地方。

上面代码中的 getAPIInterfaceObject `()`方法在每次设置改造接口时都会被调用。

上面代码片段中的几个重要术语

## 读取超时:

一旦连接建立，读取超时就开始注意，然后观察每个字节传输的快慢。如果两个字节之间的时间大于读取超时，它会将请求视为失败。每输入一个字节后，计数器复位。

## **写入超时:**

写超时与读超时方向相反。它检查你发送字节到服务器的速度。当然，它也会在每个成功字节后复位。但是，如果发送一个字节花费的时间超过配置的写入超时限制，翻新会将请求视为失败。

## 连接超时

连接超时可能是最重要的超时。它是从开始请求到完成与服务器的 TCP 握手之间的时间。换句话说，如果 Retrofit 无法在设置的连接超时限制内建立到服务器的连接，它会将请求视为失败。

## **通话超时**

呼叫超时设置完整呼叫的默认超时。调用超时跨越整个调用:解析 DNS、连接、编写请求正文、服务器处理和读取响应正文。换句话说，如果从开始到结束的整个 API 调用没有在所述时间限制内完成，那么它将把请求计为失败。

现在我们完成了设置，让我们从活动类中调用 API。

异步发送请求，并在响应返回时用回调通知你的应用程序。因为这个请求是异步的，所以 request 在后台线程上处理它，这样主 UI 线程就不会被阻塞或干扰。

要使用`enqueue()`，您必须实现两个回调方法:

*   `onResponse()`
*   `onFailure()`

如果一切顺利，那么回调将落在 OnResponse()方法上，从那里您可以更新您的 API，如果有任何错误或异常，那么 onFailure()方法将被调用。

*暂时就这样吧！！
感谢阅读，别忘了与你的开发伙伴分享:)
这篇文章最初发表在*[*CodeTheraphy.com*](https://www.codetheraphy.com/)*。*

*更多文章关注我上* [*中*](/@nandishswarup) *和*[*CodeTheraphy.com*](https://www.codetheraphy.com/)*你也可以联系我上*[*LinkedIn*](http://www.linkedin.com/in/nandish-swarup)