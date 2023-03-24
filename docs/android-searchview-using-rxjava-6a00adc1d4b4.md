# Android search view-使用 RxJava

> 原文：<https://medium.com/nerd-for-tech/android-searchview-using-rxjava-6a00adc1d4b4?source=collection_archive---------0----------------------->

![](img/bbd76f280fc8ec05bc9cd5f6dde88a82.png)

Solen Feyissa 在 [Unsplash](/s/photos/google-search?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

你有没有注意到，当我们开始在搜索栏中输入一些字符时，我们会自动看到一个结果列表，它会随着你的输入而更新。如今，我们每天使用的几乎所有应用程序都有这个功能来搜索，这样你就可以根据你的需要得到结果，而不用滚动太多。
现在有不止一种方法来实现相同的功能，但是我遇到的最好的方法之一是使用 Rxjava 操作符。我们将讨论 java 操作符是如何通过实现一些我从未想过会如此容易实现的特性来节省我的时间的。

在实现 SearchView 时，人们希望拥有许多功能，例如

*   仅在文本长度超过 3 个字符时搜索
*   将所有字符转换成小写
*   仅在用户停止键入时搜索。这不仅会大大减少 API 调用的数量，还会为用户提供更好的反馈
*   如果用户更改了查询，则取消从网络获取结果

所有这些都可以通过使用操作符来实现，我们很快就会讲到。

# 包括 RxJava 依赖项

第一步是提到库，以便您的项目支持 RxJava。在应用程序级 Gradle 文件中添加以下行

# 将搜索视图转换为可观察视图

我们可以通过使用 **PublishSubject 使 SearchView 可见。我们这里使用的是 SearchView，但是也可以使用 EditText。对于 EditText，您必须使用文本更改监听器，而不是查询文本监听器。**

# 执行搜索视图

一旦我们使 SearchView 可观察，我们就可以按照我们的要求应用所有的操作符，如下所示。

让我们一个一个地检查每一个操作符-

*   **反跳:** 反跳操作符用于检测用户停止输入并向 API 请求数据时的延迟。例如，用户正在搜索“cat”，并且用户在很短的时间内键入“c”、“ca”、“cat”等等。所以用户每次输入东西都会有太多的网络调用。但是用户最终感兴趣的是搜索“猫”的结果。理想情况下，当用户在很短的时间内输入“c”和“ca”时，应该没有网络调用。因此，谴责将在给定的时间内，将忽略查询，如果用户类型之间的东西，将只执行查询时，用户停止键入
*   **switch map:**switch map 操作符用于避开不再需要的网络调用结果显示给用户。假设最后一个搜索查询是“猫”，并且存在对“ab”的正在进行的网络呼叫，并且用户键入了“猫”。那么你对“ca”的结果就不再感兴趣了。你只对“猫”的结果感兴趣。所以，开关图来拯救。它只提供最后一个搜索查询(最近的)的结果，而忽略其余的。
*   **过滤器:**人们不希望搜索一个空字符串，或者当一个字符串太小而不能产生想要的结果时。过滤器运营商有助于过滤这种不需要的字符串，从而有助于节省网络通话
*   **DistinctUntilChanged:**DistinctUntilChanged 操作符用于避免重复的网络调用。假设最后一个正在进行的搜索查询是“cat ”,用户删除了“t ”,并再次键入“t”。所以又是“猫”。因此，如果网络调用已经使用搜索查询“cat”进行，它将不会使用搜索查询“cat”再次进行重复调用。因此，distinctUntilChanged 会删除源可观测对象发出的重复的连续项目。

如果您的项目没有用 RxJava 实现的 API 改进调用，那么您将无法阻止重复的 API 调用。我发现了一种解决方法，如果 API 调用仍在进行中，就取消它，然后只进行新的调用。

*暂时就这样吧！！
感谢阅读，别忘了与你的开发伙伴分享:)
这篇文章最初发表在*[*code theraphy*](https://www.codetheraphy.com/)

*更多文章关注我关于* [*中*](/@nandishswarup) *和*[*CodeTheraphy.com*](https://www.codetheraphy.com/)*。
也可以在*[*LinkedIn*](http://www.linkedin.com/in/nandish-swarup)*上联系我。！！*