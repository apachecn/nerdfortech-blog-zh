# 往前走，拉姆达…

> 原文：<https://medium.com/nerd-for-tech/go-forth-and-lambda-2a341bcc22f8?source=collection_archive---------5----------------------->

![](img/48d96c0f5b65a4d54511bff03b448ea0.png)

在假期期间，我终于静下心来花些时间建立一个供个人使用的照片存档服务——在接下来的日子里，你会听到更多的消息。这也意味着我开始认真编写一些严肃的 Go 代码，而不是在最初的教程和课程中玩耍。事情是这样展开的。

所以，首先，有些警告。这不是一篇*如何*的文章。它甚至不是一个*这是如何做 AWS Lambda 与 Go* 。这是对我学到的一些东西和我经历的旅程的一个快速调查。那段旅程的一部分是约翰·阿伦德尔([bitfieldconsulting.com/books](https://t.co/9AQ6jp1DeS))的一些非常棒的小书，它们是专注于编写*好的*代码的优秀入门书。作为最后的警告，我还没有到编写好的*惯用 Go 代码的阶段。那需要一些练习。*

好吧，让我们设置一些背景。我的照片存档解决方案是一个数据管道——我会将 JPEGs 文件扔进 AWS 上的摄取 S3 桶，一些 AWS Lambda 代码会注意到这一点，将它们按年/月/日组织到其他地方，然后创建图像的较小缩略图版本。我以前使用 Java 和 Python 构建过这种管道，所以我对完成这项工作所需的所有细节都很满意，这让我可以专注于 Lambda 代码。

了解情况后，我沿着一条逻辑路线前进:我知道 Lambda 将通过某个方法钩子被调用，并且该方法将被传递一大块描述要处理的事件的 JSON。很公平，第一步，让我们找到一种解析 JSON 的方法。

我的研究导致了两种可能性。首先，提供的 Go 库非常擅长于将 JSON 解组到 map 。我从 AWS 获取了一个 JSON 消息的例子，并从那里开始。嗯。是的，我*可以*那样去序列化，但是代码有点难看。一个更干净的解决方案是来自 Josh Baker 的 GJson 库。学习曲线有点陡峭，但是代码看起来更好，并且它具有你可能已经习惯的来自 [jq](https://stedolan.github.io/jq/) 的语义——赢了！

好了，这就是 JSON 的分类，现在我该如何与 S3 对话来读写对象…进入用于 Go 的 [AWS SDK。这是一款*真的*好看的 SDK。它有很好的文档记录，很好地与不断发展的 API 保持一致，并且具有经过深思熟虑的整合的特征，目的是使开发人员的生活更加轻松。嗯，是的，完全摆脱了 JSON 解析的繁琐。](https://aws.amazon.com/sdk-for-go/)

我们首先看到的是在 *main()* 函数中有一块锅炉板:

```
// main function is invoked when the lambda is launched
func main() {
    lambda.Start(HandleLambdaEvent)
}
```

哦，是的，需要一些导入才能使用 SDK——您可以看到 SDK 中的模块被智能地捆绑在一起，因此您可以轻松地挑选所需的服务。

```
import (
   "github.com/aws/aws-lambda-go/events"
   "github.com/aws/aws-lambda-go/lambda"
   "github.com/aws/aws-sdk-go/aws"
   "github.com/aws/aws-sdk-go/aws/session"
   "github.com/aws/aws-sdk-go/service/s3"
   .
   .
   .
)
```

因此，当周围的 Lambda 基础设施运行该函数的实例时，就会调用这个 *main()* 。它的唯一目的是注册实际的事件处理程序，尽管它可以用于您的代码需要的任何设置。我选择将大部分设置放入一个 *init()* 函数中，因为这看起来更像是一件要做的事情，但它的功能是相同的:

```
func init() {
   params = &runtimeParameters{
      SourcePrefix:      validatePrefix(os.Getenv("SOURCE_PREFIX"), *DefaultSrcPrefix*),
      DestinationPrefix: validatePrefix(os.Getenv("DESTINATION_PREFIX"), *DefaultDestPrefix*),
      DestinationBucket: validateDestination(os.Getenv("DESTINATION_BUCKET")),
      Region:            validateRegion(os.Getenv("AWS_REGION")),
   }

   sess, err := session.NewSession(&aws.Config{
      Region: aws.String(params.Region),
   })
   if err != nil {
      log.Fatal("Error starting session", err)
   }

   params.S3service = s3.New(sess)
}
```

没有什么特别的——我从环境中抓取了一些变量，强迫它们使用合理的默认值，并启动了一个 AWS 会话。您可能会注意到，该会话用于创建一个服务，它是我们在 S3 的门面。浏览文档，这很大程度上是 SDK 的模式:每个 AWS 服务都有一个专用的代理服务，允许针对 AWS 帐户的操作整齐地映射到 AWS 提供的相同服务组织。

对，有趣的部分是处理程序本身:

```
func HandleLambdaEvent(request events.S3Event) (int, error) {
```

哎呀。应该先阅读文档。Lambda for Go 中的外围基础设施意味着处理程序被赋予了一个完全格式良好的结构。不需要 JSON 解析。我们学到了什么？*阅读(精)手册*。

我还建议，一个像样的 IDE 是一个帮助，这听起来相当明显，但有时需要说出来。我目前正在使用 IntelliJ，它对 Go 的支持非常好，尽管配置测试运行的界面目前有点笨拙。对我来说，关键的部分是 IDE 使得跳转到 SDK 代码变得非常容易，并且容易跳转到任何可用的文档。

这种对围棋界可见性的奉献是一种极大的快乐。对于 AWS SDK，这意味着我们拥有代码本身，并且:

*   在[https://pkg.go.dev/github.com/aws/aws-sdk-go-v2](https://pkg.go.dev/github.com/aws/aws-sdk-go-v2)的标准形式的 Go 文档
*   位于[https://github.com/aws/aws-sdk-go-v2](https://github.com/aws/aws-sdk-go-v2)的源代码

到目前为止，Go 被证明是 AWS Lambda 的理想对手。它消除了 Python 的缺点，即捆绑所有必需的依赖项是一件非常痛苦的事情，并且它消除了 Java 的膨胀和大量内存占用。代码被编译成一个二进制文件(尽管比我以前用 C 语言编写的要大)，你可以把它压缩，这就是你的可部署文件。非常有趣的是，只需指定目标运行时间，就可以在开发人员的机器上构建 Lambda deploy:

```
$ GOOS=linux go build main.go
$ zip function.zip main
$ aws lambda create-function \
    --function-name my-function \
    --runtime --zipfile files://function.zip \
    --handler main \
    --role arn:aws:iam::1234567890:role/my-lambda-role
```

虽然我实际上并不是这样做的…

到目前为止，我的主要收获是:

1.  Go 使得编写 Lambda 函数变得非常简单
2.  AWS SDK 是我们的朋友
3.  阅读文档！