# Docker、Heroku Container Registry 和 GitHub Actions:构建快速、高性能的简化应用程序

> 原文：<https://medium.com/nerd-for-tech/docker-heroku-container-registry-and-github-actions-building-fast-performant-streamlit-apps-77ab62f4db75?source=collection_archive---------3----------------------->

*本文假设您熟悉使用 Streamlit 构建您的数据应用程序和仪表板，或者至少以前听说过 Streamlit。*

![](img/348cf317a80d188b4d5d96fe3919782d.png)

[un splash 上的 Siora](https://unsplash.com/photos/DhoCVkssJjs)

不久前，我被分配到一个仪表板上工作，该仪表板将我公司开发工作流程的一部分自动化。作为一个 Streamlit 的非官方支持者——在大约 6 或 7 个国际会议上做了无数次关于 Streamlit 的演讲——我很自然会在这个项目中使用 Streamlit。

Streamlit 使用起来总是简单明了，但是 Heroku 上的性能往往是一个复杂的问题。这让我开始研究如何尽可能快地加载我的 Streamlit 应用程序，而不必牺牲任何东西。

下面列出了我尝试过的所有技术，我会深入研究为什么你会考虑使用它们，这样你就不会遇到我遇到的所有琐碎问题。

*   构建简化的应用程序
*   不要直接推进到 Heroku，用 Docker
*   缩小图像尺寸:如果是 Streamlit，就不要用 Alpine！
*   使用 GitHub Actions 工作流自动化后续部署
*   其他有用的技术(Streamlit 的内部)

*鼓声**

## 构建简化的应用程序

如果您已经熟悉了 Streamlit，那么您会知道使用 Streamlit 创建 web 应用程序就像编写简单的 Python 脚本一样简单，只需要在这里或那里进行一些 API 调用。出于我们的目的，下面是一个示例脚本:

这是一个简单的脚本，它接受用户输入，并根据用户输入满足的条件创建一个折线图。

要在本地服务器上运行它，就像在终端/命令提示符下运行下面的命令一样简单；

```
$ streamlit run app.py
```

## 不要直接推进到 Heroku，用 Docker

一旦我们有了网络应用，下一个合乎逻辑的步骤就是推进到云端，Heroku 是一个很好的免费选择。这就是问题可能开始出现的地方。

通常的技术是创建 requirements.txt、Procfile 和 setup.sh 文件，然后使用 Git 推送到 Heroku 应用程序，该应用程序作为远程应用程序添加到您现有的 Git 存储库中。但是 Streamlit 应用程序在 Heroku 服务器上加载速度非常慢。

一个很好的理论是 Streamlit 有很多包依赖关系(考虑到它抽象了很多东西，这在某种程度上是可以理解的)，这些依赖关系可能会占用 Heroku 的大量内存。因此，如果你的应用程序包含一大堆占用内存的功能，加载速度肯定会变慢。

解决这个问题的一个好方法是使用 Docker。Docker 是一个平台，可以非常快速地构建、发布和运行应用程序，而不用太担心基础设施的依赖性。我的一个队友在她的机器上安装 Streamlit 时遇到了一个问题(怪无数的依赖项吧！)Docker 是避免这种不便的好方法。

除了代码目录之外，您需要的只是 requirements.txt 文件，之后您将创建 Dockerfile。一旦构建了 web 应用程序的 Docker 映像，就可以很容易地推送到 Heroku Container Registry 来开始从外部访问 web 应用程序。

第一行代码使用 Python 图像作为基础图像。然后，我们复制并安装 requirements.txt 中列出的依赖项。之后，我们将目录内容复制到一个名为/app 的新目录中，并将其初始化为我们的工作目录。最后，最后一行代码启动了 Streamlit 应用程序。

为了在本地测试 Docker 映像，下面的两行命令将构建 Docker 映像并运行容器；

```
$ docker build -t app:latest .
$ docker run app:latest
```

这将在本地启动 Docker 容器，您可以通过显示的网络 URL 访问它。

**部署到 Heroku 容器注册中心**

要部署到 Heroku，需要以下命令；

```
$ heroku login
$ heroku container:login
$ heroku create app_name
$ heroku container:push web
$ heroku container:release web
$ heroku open
```

分别登录 Heroku 和 Heroku 容器需要前两行命令。下一行命令使用指定的名称在 Heroku 上创建应用程序。下一行构建图像并将其推送到容器注册表。以下命令行将图像发布到应用程序，而最后一行在浏览器中打开现在部署的应用程序。

但是即使为我的项目做了这些，我的 Docker 图像大小仍然是 3GB+,并且加载部署的 web 应用程序需要花费很长时间。:(

## 缩小图像尺寸:如果是 Streamlit，就不要用 Alpine！

在研究如何解决这个瓶颈时，几乎所有资源都推荐使用 Alpine Linux。所以我用了它，但我的 Docker 形象就是建不起来。它无法安装 Streamlit 的某些依赖项。

我研究了为什么“美国队长，但对于较小的图像尺寸”不能在我最需要它的时候拯救我，我偶然发现了[这篇很棒的文章](https://pythonspeed.com/articles/alpine-docker-python/)，关于为什么 Alpine 不是 Python Docker 的较小图像尺寸的最佳人选。事实证明，标准的 PyPi 车轮在 Alpine 和 Streamlit 上不起作用，它们有很多可爱的车轮。:(

所以我把“Alpine Linux”换成了“Python-slim”作为我的基础镜像。我还在“pip install”中使用了“— no-cache-dir”标志，这样它就不会在安装完软件包后将它们的安装文件存储在缓存中。

这样做极大地减少了我的项目的 docker 图像大小，从将近 4GB 减少到 300MB 多一点。喔喔喔！

## 使用 GitHub 动作自动化后续部署

如果你想不断修改你的 web 应用程序，而不必每次都使用“heroku container:push”和“heroku container:release ”, GitHub Actions workflow 是下一个可以利用的 DevOps 工具。GitHub 动作可以用来自动化开发工作流的部署部分，这意味着在第一次初始推送后，您不必再次手动推送至 Heroku。

您需要做的就是在您的项目目录中创建 CI.yml 文件，格式如下；

*。github - >工作流- > CI.yml*

CI.yml 将包含如下代码行:

“name”键描述 CI.yml 文件的用途。“on”键在主存储库被推送时触发工作流。在“job”键中是 Heroku 命令，用于登录、推送新的构建，以及将新的构建发布到 web 应用程序。

最后，为了允许访问 Heroku，应该在 GitHub Secrets 中添加一个访问密钥。要生成身份验证密钥，下面的代码行将会这样做；

```
$ heroku authorizations:create
```

复制生成的密钥，并通过导航到设置→密码→新建存储库密码将其保存在 GitHub 存储库中。我将密钥命名为“HEROKU_API_KEY ”,以匹配 HEROKU 使用的变量名，尽管您可以将名称改为您喜欢的任何单词。您应该确保在工作流文件中使用相同的名称。

## 其他有用的提示

我不想让这篇文章变得太长，下面的提示与部署没有特别的关系，因为它们是 Streamlit 的内部技术，使应用程序的内存大小更有效。所以我只是在这里提到他们，但我可能会写一篇关于他们的文章。

*   从项目目录中删除静态文件，因为它们会添加到文件中。如果这些静态文件保存在 Amazon s3 上，并通过 URL 加载到你的应用程序中，你的应用程序会更好，这就引出了我们的下一个技巧；
*   尽可能使用**圣缓存**。不经常使用的文件或函数，比如 pd.read_csv，应该虔诚地缓存。这意味着每次与 web 应用程序交互时，不会重新加载缓存的文件。
*   使用**会话状态**在重新运行时存储变量。除了有助于减少重新加载的时间之外，这确实节省了我编写大量函数以确保我的变量被永久存储以供重用所带来的压力。

## 总之；

在这篇文章中，

*   我解释了为什么您应该使用 Docker 在 Heroku 上部署，而不是直接在 Heroku 上部署您的 web 应用程序。
*   我还阐述了如何通过使用 Python-slim 作为基本图像来减小 Docker 图像的大小。
*   我们还探索了如何使用 GitHub 动作来自动化后续部署。
*   最后，我们使用了其他有用的技术来帮助我们缩小应用程序的大小。

感谢你远道而来。快乐流线型！

请随时在 LinkedIn 和 Twitter 上与我联系。我很乐意回答你的任何问题或简化自由职业者的工作:)