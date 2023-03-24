# 如何绕过 Angular HTTP 拦截器？

> 原文：<https://medium.com/nerd-for-tech/how-to-bypass-angular-http-interceptor-2491afca16a3?source=collection_archive---------2----------------------->

![](img/46f603367b8f109bd4712571732f09e2.png)

是的，您没听错，有时您需要绕过 angular HTTP 拦截器来处理特定的 HTTP 请求。

在我们继续之前，让我们修改一下什么是角拦截器？。根据文件。

> 使用拦截，您可以声明*拦截器*来检查并转换从您的应用程序到服务器的 HTTP 请求。相同的拦截器还可以在返回应用程序的途中检查和转换服务器的响应。多个拦截器形成了一个*向前和向后*的请求/响应处理程序链。

我知道很专业。让我们用简单的话来说。angular interceptor 检查您发送给服务的每个请求。并检查从服务器返回的响应。

下面是这个拦截器的一些用法。

*   添加令牌 HTTP 请求
*   为所有 HTTP 请求添加自定义 HTTP 头
*   自定义错误处理的逻辑
*   记录所有 HTTP 活动

我想我们对角拦截器有了一个基本的概念。

我知道用角拦截器很好。但在某些情况下，我们需要绕过它。这就是解决方案。

# 创建新的 HttpClient

这里我们使用`[HttpBackend](https://angular.io/api/common/http/HttpBackend)`创建一个新的 HttpClient。拦截器位于`[HttpClient](https://angular.io/api/common/http/HttpClient)`接口和`[HttpBackend](https://angular.io/api/common/http/HttpBackend)`之间。所以诀窍是我们需要将`[HttpBackend](https://angular.io/api/common/http/HttpBackend)`直接注入到我们的服务中，并使用`[HttpBackend](https://angular.io/api/common/http/HttpBackend)`创建一个新的 HttpClient。

根据官方文件

> 当被注入时，`[HttpBackend](https://angular.io/api/common/http/HttpBackend)` **将请求直接发送到后端**，而不经过拦截器链。

```
import {HttpBackend, HttpClient} from '@angular/common/http';
import {***Injectable***} from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class AdmissionHttpService { private httpClient: HttpClient; constructor(
    private handler: HttpBackend,
  ) {
    this.httpClient = new HttpClient(handler);
  }

  getData() {
    return this.httpClient.get(url);
  }

}
```

所以今天就到这里。感谢阅读。