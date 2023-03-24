# 我的 Ruby on Rails 项目:图像共享应用

> 原文：<https://medium.com/nerd-for-tech/my-ruby-on-rails-project-image-sharer-app-14b93b16642f?source=collection_archive---------15----------------------->

![](img/b151ed4c70d1ac10ae5caf5e30deca4d.png)

我认为作为 Xi 学院软件工程密集项目的一部分，Ruby on Rails 应用程序项目是最具挑战性的，但在许多方面都是值得的。这是课程的最后一个模块，包括我在内的许多人都感到精神疲劳。

该项目的目标是使用 Ruby on Rails 构建一个 CRUD(创建、读取、更新和删除)应用程序，应用我们对 MVC 架构、数据建模的理解，使用外部 Gems，构建登录和注销功能，包括使用第三方应用程序，如脸书。

我的 MacBook Pro M1 在本地使用 Ruby on Rails 的 Visual Studio 代码时遇到了重大问题，所以我改用了 AWS Cloud9 EC2 IDE。令人惊讶的是，在完成这个项目后，我通过某种方式调试了终端的堆栈跟踪，使最新的 Ruby on Rails 版本 6.1.4 成功地在本地使用 Visual Studio 代码。我还解决了如何从使用 SQLite3 转移到更高级、更稳定、性能更高的数据库关系管理系统 PostgreSQL。

Ruby on Rails 项目是一个很好的机会来巩固我的知识和技能，以便在下一个最终的全栈应用项目中构建或开始构建我的初创公司的 MVP(最小可行产品)。我的初创公司 HealthAide 的目的是为健康和福祉建立一个视频内容提要和事件管理平台。这项服务允许观众浏览、推广和分享视频和在线活动。此外，它允许经验证的内容制作者创建、推广视频和虚拟活动并从中获利

因此，我觉得一个务实的起点是构建一个图像共享应用程序，以巩固我的 CRUD 应用程序构建技能，并满足项目范围的要求。

在这个项目中，我使用了 10 个额外的 Ruby Gems，这为我节省了大量的时间来硬编码这些功能，包括 simple-form、omniauth、bootstrap-sass、devise、paperclip、acts_as_votable 和 haml。

我还学会了如何使用 HAML (HTML 抽象标记语言)，这大大加快了编写 HTML/ERB 代码的速度。我强烈建议 Rails 开发人员考虑使用这个 Gem 来节省大量时间。

我发现这个项目最具挑战性的方面是对我的数据进行建模，并需要对其进行修改。这是相当令人沮丧的修复协会和修改源代码，使这一工作。在对数据建模之前仔细思考有助于在您需要添加新的数据类型及其相应的关联时避免事后的挫折。我还发现，对于 AWS EC2 Cloud9 IDE 的使用，在线文档和支持非常有限，这意味着我必须尽最大努力自己解决一些问题，特别是脸书认证，因为出现了以下错误警告消息:

URL 被阻止:此重定向失败，因为重定向 URI 未列入应用的客户端 OAuth 设置的白名单中。请确保客户端和 Web OAuth 登录已打开，并将所有应用程序域添加为有效的 OAuth 重定向 URIs。”

请注意，我还没有解决这个问题，所以任何人有建议，请让我知道！

完成这个项目巩固了我的 Ruby on Rails 技能，增强了使用这个框架构建创业 MVP 的好处(因为执行速度是实现产品市场适应性的关键决定因素)。Ruby on Rails 框架对于资源有限的初创企业来说是一个很好的选择，因为它支持可靠性、可伸缩性、效率和速度，能够快速部署全栈应用程序。例如，AirBnB MVP 是基于 Ruby on Rails 构建的，许多大公司也继续使用 Ruby on Rails 作为其技术堆栈的一部分。

![](img/48a41ceeffd397ddfcc072008efe1a17.png)

# 我期待着下一个全栈项目，其中的任务是构建一个与 Ruby on Rails API 后端通信的 React 前端，同时使用 Redux 进行状态管理。我希望有足够的时间、编程知识和技能，以及精力来成功地构建 HealthAide MVP！