# 使用代理池的网络抓取(廉价方式)

> 原文：<https://medium.com/nerd-for-tech/web-scraping-with-a-proxy-pool-the-cheap-way-4c7d6fc9f859?source=collection_archive---------6----------------------->

![](img/50e08dd02b150d9cb85198236e598552.png)

从[https://www.qep.com/products/floor-and-wall-scraper/](https://www.qep.com/products/floor-and-wall-scraper/)刮来的

## 有了 AWS Lambda 功能，100 GB 只需 9 美元

数据科学最难的事情可能是获取数据。公司愿意提供“免费”服务，以换取他们收集的关于你如何使用它们的数据，这给了他们相对于竞争对手的优势。因此，毫不奇怪，他们不会对那些无情地从他们的网站上抓取数据的人太友好，并采用许多复杂的检测算法来拒绝来自同一 IP 地址、过于相似或只是看起来可疑的请求。虽然下载不合理数量的数据是不道德的(有时是非法的)，但没有数据，模型的开发就会被垄断，从而被扼杀。我不会宽恕或谴责网络抓取，但我会告诉你一个有用的技巧。

# 代理池

当有人抓取你的数据时，最简单的方法是如果大量的请求来自一个特定的 IP 地址。出于这个原因，大量的服务涌现出来，让你可以访问大量的服务器，你可以用它们作为访问网站的代理。主要缺点是成本。

# 输入 AWS Lambda 函数

Lambda 函数是 FaaS(功能即服务)的一个例子，也是所谓的无服务器架构中最著名的组件之一。以前，您必须启动服务器来托管您的应用程序，包括从数据库返回值等任何功能。使用 Lambda 函数，您可以通过将所有组件(静态网页、数据库和其他功能)与 Lambda 函数连接起来，在不运行任何服务器的情况下构建一个相当复杂的系统。你为什么想这么做？因为通过这种方式，您只需为应用程序的使用付费，而不必通过全天候运行服务器来“开灯”。如果你期望零星或低容量，去无服务器更具成本效益。你可以一个月调用 100 万次 Lambda 函数，而不会产生任何成本，除了你从其中传输出来的数据，这些数据的收费与 EC2 实例的收费相同(目前是每 100 GB 9 美元)。作为比较， [oxylabs.io](https://oxylabs.io/products/residential-proxy-poolç) 每 GB 收费 15 美元。

Lambda 函数可以用最流行的语言编写，但我将使用 Python，因为它仍然是数据科学家的首选语言。幸运的是，Lambda 函数不能保证保留它们的 IP 地址。我发现，如果他们超过 6 分钟没有被访问，他们的 IP 地址就会改变。此外，对你创建的 Lambda 函数的数量没有真正的限制。那么，为什么不创建尽可能多的网页，并以循环的方式浏览它们，以抓取您感兴趣的网页呢？这正是我们现在要做的。

# 将（行星）地球化（以适合人类居住）

使用 AWS 控制台是乏味的，有时令人困惑，所以我们需要一种自动的方法来创建我们需要的 Lambda 函数。Terraform 是一个很好的工具。一旦您安装了 [Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli) 和 [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) 并将其链接到您的 AWS 帐户，您就可以运行以下程序:

这将自动创建 10 个名为 proxy-0 到 proxy-9 的 Lambda 函数，我们可以用它们来访问网页，如下例所示:

(您将需要 *pip 安装 bot O3*——用于编程式 AWS 控制的 Python 包装器。)如果运行此命令，您应该会看到 10 个唯一的 IP 地址在循环中重复出现。只需将 URL 替换为您感兴趣的网页，然后就可以离开了。您可以配置在文件 *variables.tf* 中创建的 Lambda 函数的数量以及区域——这对于只能从世界上某些地方访问的网页非常有用。

然而，Lambda 函数确实有某些限制。特别是，响应有效负载的大小必须小于 6 MB。称 Lambda 函数为“代理”可能有点夸大其词，因为它只能处理 GET 请求，但对于使用 Beautiful soup 之类的东西从 HTML 页面抓取数据来说，这已经足够了。

[](https://github.com/teticio/lambda-scraper) [## teticio/lambda 刮刀

### 使用 AWS Lambda 函数作为代理来获取 HTTP。有助于 teticio/lambda-scraper 的发展，创造一个…

github.com](https://github.com/teticio/lambda-scraper)