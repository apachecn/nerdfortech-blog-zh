# 重新发明轮子:DIOD(一个新的类型脚本依赖注入库)

> 原文：<https://medium.com/nerd-for-tech/reinventing-the-wheel-diod-a-new-typescript-dependency-injection-library-4893066004e2?source=collection_archive---------19----------------------->

依赖注入是复杂应用程序中经常出现的模式，每天都可以看到它与 Typescript 一起应用。但是与 Java 或 C#等经典语言相比，TS 有一些奇怪的地方，我们需要记住。在这篇文章中，我将谈论他们，以及他们如何通过创建一个全新的 DI 库来推动我重新发明轮子: [DIOD](https://github.com/artberri/diod) 。

![](img/a4b33b7c69ec4b9dfe0429ef7bb90be3.png)

帖子标题是告白；我知道还有其他的用 Typescript 进行依赖注入的库；更有甚者，我用了很多，也从中学到了很多。 [Inversify](https://github.com/inversify/InversifyJS) 、[tsyringe](https://github.com/microsoft/tsyringe)……感谢他们所有人让我能够写下这篇文章，但很明显，这背后有一个我自己创作的动机，我会尽力解释它。

> ⚡️，你可能最终只是在寻找一个依赖注入库，而不是愿意阅读我的解释。不要担心； [*此处停止阅读，试试 DIOD*](https://www.npmjs.com/package/diod) *。*

# 寻找不可能

有人可能会说依赖注入“只是‘传递参数’的一种奇特方式”；这是一个有趣的迷因的好句子，但你可能知道它有更多的含义。特别是，你会注意到它和其他原则一起使用的真正好处，例如[固体](https://en.wikipedia.org/wiki/SOLID)原则。

SOLID it 中的 D 与依赖注入密切相关，是指**依赖倒置**；这个原则鼓励开发人员在某些情况下使用抽象来定义依赖关系。声明这些抽象的通常方式是通过接口，这也是我对任何其他语言所做的，但是让我们看看使用 Typescript 会发生什么。

想象一下这个简单的组件:

```
class NewsletterSender {
  public constructor(
    private readonly mailer: Mailer,
    private readonly postRepository: PostRepository
  ) {}

  public sendLastBlogPosts(emailTo: string): void {
    const articles = this.postRepository.getLast(5)
    const content = articles.map(a => a.toString()).join("\n\n")
    this.mailer.send(emailTo, 'My blog newsletter', content)
  }
}
```

正如您所看到的，它依赖于另外两个服务。遵循使用接口来抽象我们的依赖关系的最佳思想，我们可以将它们定义为:

```
interface Mailer {
  send(to: string, subject: string, content: string): void
}

interface PostRepository {
  getLast(limit: number): Post[]
}
```

显然，我们还需要这些抽象的一些实现:

```
class MailMonkeyMailer implements Mailer {
  public send(to: string, subject: string, content: string): void {
    // Some real implementation here
  }
}

class MyDbEnginePostRepository implements PostRepository {
  public getLast(limit: number): Post[] {
    // Some real implementation here
  }
}
```

基于这个例子，我的理想依赖注入库将允许我们以类似的方式注册和使用这些服务:

```
container.register(Mailer).use(MailMonkeyMailer)
container.register(PostRepository).use(MyDbEnginePostRepository)
container.registerAndUse(NewsletterSender) // as an alias of container.register(NewsletterSender).use(NewsletterSender)

const newsletterSender = container.get(NewsletterSender)
newsletterSender.sendLastBlogPosts('foo@bar.com')
```

不幸的是，这是不可能的。

# 设计时间与运行时间

随着时间的推移，如果您使用 Typescript，在运行时搜索使用接口的概率趋于 1；所以，恐怕你已经知道答案了:不能。接口、类型，还有那些有人称之为语法糖的东西，编译成 Javascript 后就没有了；因此，它们在运行时不可用。

上述类和接口将导致以下编译的 JS 代码:

```
class NewsletterSender {
  constructor(mailer, postRepository) {
    this.mailer = mailer;
    this.postRepository = postRepository;
  }
  sendLastBlogPosts(emailTo) {
    const articles = this.postRepository.getLast(5);
    const content = articles.map(a => a.toString()).join("\n\n");
    this.mailer.send(emailTo, 'My blog newsletter', content);
  }
}
class MailMonkeyMailer {
  send(to, subject, content) {
    // Some real implementation here
  }
}
class MyDbEnginePostRepository {
  getLast(limit) {
    // Some real implementation here
  }
}
```

如您所见，没有接口的痕迹，因此，大多数现有的 DI 库提供了基于字符串或基于符号的解决方案，这是我不喜欢的方法。那些符号或字符串和它们相应的接口之间的关系是弱的，并且是基于约定的；因此，它们容易出错并阻碍重构。

最好的选择是使用抽象类。这种高度自以为是的断言大概会引起很多人的不情愿；但是，一个纯粹的 TS 抽象类将在运行时可用，并允许自动连接基于构造函数的依赖注入。尽管您可能认为这种方法会引入继承问题，但它不会；请注意，我建议的是实现而不是扩展这些抽象类。

通过进行这一更改，我们的新抽象将如下所示(代码的其余部分将保持不变):

```
abstract class Mailer {
  abstract send(to: string, subject: string, content: string): void
}

abstract class PostRepository {
  abstract getLast(limit: number): Post[]
}
```

遗憾的是，这还不够。

# 装修工:不可避免的罪恶

通过使用抽象类而不是接口，我们已经设法为我们的类协作者的抽象提供了一些运行时构造；但是，仍然有一个问题:编译后的 Javascript 没有将这些抽象与构造函数参数联系起来。

Typescript 提供了一种[的单一官方方式](https://www.typescriptlang.org/docs/handbook/decorators.html#metadata)来公开关于最终编译代码中类型的数据；它由激活了`experimentalDecorators`和`emitDecoratorMetadata`编译器选项的 decorators 组成。

最后，我们的示例代码将是:

```
@SomeDecorator()
class NewsletterSender {
  // ...
}

// ...
```

最终编译的代码:

```
// here some auto-generated helpers like __decorate or __metadata
class Mailer {
}
class PostRepository {
}
let NewsletterSender = class NewsletterSender {
    constructor(mailer, postRepository) {
        this.mailer = mailer;
        this.postRepository = postRepository;
    }
    sendLastBlogPosts(emailTo) {
        const articles = this.postRepository.getLast(5);
        const content = articles.map(a => a.toString()).join("\n\n");
        this.mailer.send(emailTo, 'My blog newsletter', content);
    }
};
NewsletterSender = __decorate([
    SomeDecorator(),
    __metadata("design:paramtypes", [Mailer,
        PostRepository])
], NewsletterSender);
class MailMonkeyMailer {
    send(to, subject, content) {
        // Some real implementation here
    }
}
class MyDbEnginePostRepository {
    getLast(limit) {
        return [];
    }
}
```

如果您检查上面显示的生成代码，您将看到现在它有足够的信息来实现我在本文第一部分中考虑的理想库。尽管如此，我还是要对一件事进行理论化。

如果您将依赖性反转原则应用于分层架构，您将最终得到所谓的[六边形架构](https://alistair.cockburn.us/hexagonal-architecture/)(或端口&适配器)。这种架构试图保持应用程序的核心与基础设施或具体的依赖关系相分离，这也意味着它不应该知道控制反转容器或依赖注入工具。用现有的库很难做到这一点，因为它们中的大多数都要求你在每个服务上使用它们的装饰器，包括那些在你的应用程序内层的装饰器。下面这个例子是我整个 app 不想看到的:

```
import { Injectable } from 'some-fancy-di-lib'

@Injectable()
export class ThisIsADomainService {
  // ...
}
```

这段代码将把每一个服务与`some-fancy-di-lib`库耦合起来。此外，如果我想在将来停止使用它，我将需要更改每个服务文件来删除或更改它。那么，为什么不呢...

```
export const MyService = (): ClassDecorator => {
  return <TFunction extends Function>(target: TFunction): TFunction => {
    return target
  }
}
```

并且:

```
import { MyService } from '../MyService'

@MyService()
export class ThisIsADomainService {
  // ...
}
```

每个装饰器，包括上面显示的那些什么都不做的装饰器，都会触发元数据生成，这就是我们所需要的。大多数现有的 DI 库的问题是，它们不能与这样的定制装饰器一起工作，因为它们的逻辑的关键部分驻留在它们提供的装饰器中。我理想中的库，也是我构建的库，允许用户使用他们自己创建的装饰器。

# 新的车轮:DIOD

[DIOD](https://github.com/artberri/diod) 是我为 Typescript (Node.js 或 Browser)创建的新依赖注入库，它具有一些在其他库中很难(如果不是不可能)找到的独特特性:

*   依赖项将专门基于构造函数类型自动连接；因此，它只接受抽象或具体的类作为构造函数类型。
*   使用库提供的装饰器的需求绝不会损害功能。使用用户创建的任何装饰器都可以获得主要特性，以避免供应商锁定和耦合。

除了这些特殊功能之外，它还提供了以下开箱即用的功能:

*   轻量级:它将永远是无依赖性的，并且小于 2kB
*   工厂服务:使用工厂来创建服务。
*   用户提供的服务:使用手动创建的实例来定义服务。
*   范围:默认情况下，每个服务都是暂时的，但是它们可以注册为单个服务或“每个请求”(同一个服务实例将在单个请求中使用)。
*   编译器:注册完所有需要的服务后，需要构建容器。在这个构建过程中，DIOD 将检查诸如缺少依赖项、错误配置或循环依赖项之类的错误。
*   可见性:服务可以被标记为私有。私有服务只能作为依赖项使用，并且不能从 IoC 容器中查询。
*   标记:标记容器中的服务并基于标记查询服务的能力。
*   对普通 JS 的支持:通过手动定义服务依赖，普通 Javascript 的使用是可能的。

在 [NPM 注册表](https://www.npmjs.com/package/diod)或 [Github](https://github.com/artberri/diod) 中查看一下，在那里你会找到所有的文档。不要犹豫，告诉我你对它的看法，如果你喜欢或使用它，就在 Github 上开始吧。我将非常感激。

*原载于 2021 年 6 月 3 日 https://www.berriart.com*[](https://www.berriart.com/blog/2021/06/diod-dependency-injection-typescript/)**。**