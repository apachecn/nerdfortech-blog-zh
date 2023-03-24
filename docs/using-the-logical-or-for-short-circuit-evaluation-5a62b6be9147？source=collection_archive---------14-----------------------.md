# 使用逻辑 OR 进行短路评估

> 原文：<https://medium.com/nerd-for-tech/using-the-logical-or-for-short-circuit-evaluation-5a62b6be9147?source=collection_archive---------14----------------------->

![](img/9f80f3a8ced25ebfadd61747f0c15da7.png)

罗布·兰伯特在 [Unsplash](https://unsplash.com/s/photos/technology?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

这周我在做一个项目，这个小技巧派上了用场。我认为这真的很酷，值得加入你的锦囊妙计，所以看看这个吧。

在我接触过的其他语言中，布尔 OR 运算符(通常表示为||)用于计算条件语句的布尔表达式。

```
if( friday || birthday ) {
  throwPizzaParty();
}do{
  washLaundry();
  vacuumHouse();
} while ( sunday || messyInside )
```

如果`friday`或`birthday`是真实的，举办一个披萨派对。如果`sunday`或`messyInside`是真的，该做家务了。谢谢你容忍我的愚蠢的例子:)

但是有了 JavaScript，我看到了使用逻辑 OR 的新方法。它可用于以非常简洁的条件返回两个值之一。这给我们带来了一个非常酷的方法，我学会了使用双管*短路评估*。

假设你正在写一个函数，对于一个特定的变量，你不确定它在被调用时是否是未定义的。这是我在本周处理一些异步 API 调用时想到的。我的结局是这样的:

```
const processUsersLocationData(apiResponse) {
  doAllTheThings(apiResponse)
}
```

…但是由于应用程序的异步特性，我不能指望这个函数严格地在 API 响应之后被调用。如果它在收到 API 响应之前被调用，这个函数将试图作用于`undefined`，我们将得到一个错误，杀死服务器。我需要这个函数即使在`apiResponse`还没有被设置，仍然是`undefined`的时候被调用。我最初写了一个条件`if`，但是最后简化成这样:

```
const processUsersLocationData(apiResponse) {
  apiResponse = apiResponse || "still waiting on API";  
  doAllTheThings(apiResponse);
}
```

…第二行，`apiResponse = apiResponse || “still waiting on API”`，当当前为`undefined`时，将的`apiResponse`设置为`"still waiting on API"`。它去掉了设置`if`条件带来的额外代码，并在一行代码中快速工作，*使条件*短路。

当然，and 和 or 似乎总是在一起，以互补的方式工作。而这种情况也没什么不同。你可以用& amp；

```
if(friday){
  pizzaParty();
}
```

…可以简化为:

```
friday && pizzaParty();
```

总结一下，我们可以利用||和&&来简化条件和求值。这可以为你提供一个快速的方法来用一个`truthy`值替换一个`falsey`值，并避免你的应用程序出轨！