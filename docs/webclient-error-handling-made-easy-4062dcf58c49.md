# WebClient 错误处理变得简单

> 原文：<https://medium.com/nerd-for-tech/webclient-error-handling-made-easy-4062dcf58c49?source=collection_archive---------0----------------------->

许多读者可能熟悉 WebClient 及其各种用法，但是为了便于解释，让我重申一下显而易见的内容；).

> **什么是 WebClient？**

它是作为 Spring Reactive web 模块的一部分引入的，因此在使用反应堆栈的场景中，它为 RestTemplate 提供了一个替代方案。但是不要搞错了！。对于正常的功能，我们也可以使用相同的方法，您需要等待服务呼叫结束，才能继续执行剩余的功能。这被称为*阻塞操作。* 因此 WebClient 可以支持同步和非同步操作。

![](img/c00532b5545e91df6ca06ada7a4583b4.png)

> **如何处理错误？**

这里的假设是，读者了解 webclient 的基础知识以及如何使用它，因此我将直接进入错误处理部分。如果你们需要知道如何开始使用 webclient，请告诉我。好的，我们开始吧。使用 webclient 时，有多种方法可以处理错误。我将解释最简单的方法，让你们去探索其他可能的方法。

![](img/4814aad6b86e8d2bcc3a7d0a78d8c40e.png)

处理错误，真的

但是在我们进入方法之前，*总是记得扩展****runtime exception****到你正在定义的异常，具体到 WebClient* 中的用法。这让事情变得简单多了。

```
public class UserDefinedException extends RuntimeException {//your class definition which includes error attributes... etc}
```

*   ***初始化网络客户端***

我们可以定义一个 ExchangeFilterFunction，它将根据相关的错误状态代码封装错误。

```
public static ExchangeFilterFunction errorHandler() {
    return ExchangeFilterFunction.*ofResponseProcessor*(clientResponse -> {
        if (clientResponse.statusCode().is5xxServerError()) {
            return clientResponse.bodyToMono(String.class)
                    .flatMap(errorBody -> Mono.*error*(new UserDefinedException1(errorBody)));
        } else if (clientResponse.statusCode().is4xxClientError()) {
            return clientResponse.bodyToMono(String.class)
                    .flatMap(errorBody -> Mono.*error*(new UserDefinedException2(errorBody)));
        } else {
            return Mono.*just*(clientResponse);
        }
    });
}
```

正如代码块中提到的，每当发生 5XX/4XX 错误时，我们可以抛出一个用户定义的异常，然后根据这些用户定义的异常执行错误处理逻辑。一旦定义了这个错误处理程序，我们就可以在 WebClient 初始化中添加它。

```
WebClient webClient() {
    return WebClient
            .*builder*()
            .filter(errorHandler())
            .baseUrl("baseURL")
            .build();
}
```

因此，现在无论在哪里使用这个 webclient，所有的错误都会自动用定制的异常进行包装，这是我们在前面的步骤中声明的。

*   ***在执行服务时调用***

前面的方法更像是一种通用的方法，但是，可能会有我们只需要处理特定错误代码的场景。在这种情况下，我们可以在执行 webclient 时实现错误处理逻辑，如下所述

```
try {
String response = webClient.get()
        .retrieve()
        .onStatus(httpStatus -> httpStatus.value() == <desired Status code>,
                error -> Mono.*error*(new UserDefinedException("error Body")))
        .bodyToMono(String.class)
        .block();
} catch(UserDefinedException userDefinedException) {
  //exception handling logic
}
```

在上面提到的代码片段中，我们可以替换任何我们想要的状态，并相应地处理错误。好的一面是您可以有多个 onStatus()，因此您可以根据您的业务需求处理多个状态代码。

*   让我们再试一次；)

到目前为止，涵盖的场景都是基本和直接的场景，但实际上，当出现错误时，通常遵循的流程是

1.  根据错误，进行特定次数的重试
2.  重试次数用尽后，抛出特定错误。

```
try {
UserDefinedResponse response = webClient
        .post()
        .body(Mono.*just*(userDefinedRequest), Map.class)
        .retrieve()
        .bodyToMono(UserDefinedResponse.class)
        .onErrorResume(Mono::*error*)
        .retryWhen(Retry.*backoff*(3, Duration.*of*(2, ChronoUnit.*SECONDS*))
                .onRetryExhaustedThrow((retryBackoffSpec, retrySignal) ->
new UserDefinedException(retrySignal.failure())))
        .block();
} catch(UserDefinedException userDefinedException){
//Error Handling logic
}
```

上面提到的代码片段显示了如何使用 webclient 实现指数重试。 ***Retry.backoff(3，Duration.of(2，ChronoUnit。秒)，*，**这一行封装了重试所需的逻辑。

*   **3** 表示在执行耗尽逻辑之前要执行的最大重试次数
*   **2 秒**表示第一次回退开始前的最小时间。

一旦重试次数用完 **onRetryExhaustedThrow()** 就会被执行，这样我们就可以按照我们的业务逻辑来处理错误。

> **结尾**

这只是各种各样的概述，还有各种其他的可能性可以探索。在从事 spring boot API 开发时，我在使用 WebClient 处理错误时遇到了困难，因此我想记录我在错误处理中采用的所有方法。建议和反馈总是受欢迎的，所以请添加您的方法或见解，因为与您已经知道的方法相比，您是如何找到这些方法的。:).编码人快乐！！！。