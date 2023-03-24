# 使用 GitHub 动作自动化您的工作流程

> 原文：<https://medium.com/nerd-for-tech/automate-your-workflow-with-github-actions-7b1c9573f6c8?source=collection_archive---------19----------------------->

![](img/919e54942a352137ac66643b432d3118.png)

在本文中，我们将向您介绍 GitHub Actions。去年，GitHub 推出了一些新功能。其中包括 GitHub Actions。

GitHub Actions 支持**持续集成**和广泛的**自动化**从您的存储库中配置。您可以创建自己的动作，使用和定制 GitHub 社区共享的动作，或者编写和共享您构建的动作。要共享您构建的操作，您的存储库必须是公共的。

GitHub Actions 支持 Windows、Linux 和 MacOS，你可以运行这些操作系统支持的任何编程语言。

让我们来看看 GitHub Actions 的核心概念和一些创建自己的定制工作流的例子。

# 自动化工作流程

自动化是通过定义工作流实现的。工作流由一个或多个称为操作的任务组成。这些操作可以在某些事件上自动运行。

您可以通过编写与您的存储库交互的定制代码以及与 GitHub 的 API 和任何公开可用的第三方 API 集成来创建操作。例如，一个动作可以运行 [npm](https://www.npmjs.com/) 模块，在出现关键问题时发送短信提醒，或者部署生产就绪代码。

动作可以直接在机器上或 Docker 容器中运行。您可以定义操作的输入、输出和环境变量。

# 利益

GitHub 操作:

*   可以直接在 runner 机器或 Docker 容器中运行。
*   可以包括对存储库克隆的访问，允许部署和发布工具、代码格式化程序和命令行工具访问您的代码。
*   提供可以完成持续集成和持续部署的自动化。
*   不需要您部署代码或服务应用程序。
*   有一个简单的接口来生成和使用机密，允许操作与第三方服务进行交互，而无需存储使用该操作的人的凭据。

# GitHub 操作的类型

动作需要元数据文件来定义动作的输入、输出和主入口点。元数据文件名必须是`action.yml`或`action.yaml`。您可以构建 Docker 容器和 JavaScript 动作。

Docker 容器用 GitHub 动作代码打包环境。这创建了一个更加可靠和一致的工作单元，因为动作的消费者不需要关心工具或依赖关系。Docker 容器动作只能在 GitHub 托管的 Linux 环境中执行。

JavaScript 动作可以直接在运行机器上运行，并将动作代码与用于运行代码的环境分开。与 Docker 容器动作相比，使用 JavaScript 动作可以使动作代码更简单，执行速度更快。

# 核心概念

下面给出了对 GitHub 动作中使用的一些重要核心概念的总体理解。

## 行动

操作是工作流的最小可移植构建块，可以组合成创建作业的步骤。您可以创建自己的操作，或者使用和定制 GitHub 社区公开共享的操作。

## 事件

事件是触发工作流运行的特定活动。例如，当有人推送到存储库或创建拉取请求时，就会触发工作流。事件也可以配置为使用 Webhooks 监听外部事件。

## 跑步者

runner 是安装了 GitHub Actions runner 应用程序的机器。运行程序等待可以执行的可用作业。在获得一个作业后，它运行该作业的动作，并向 GitHub 报告进度和结果。Runners 可以托管在 GitHub 上，也可以自托管在自己的机器/服务器上。

## 职位

作业由多个步骤组成，在虚拟环境的一个实例中运行。如果当前作业依赖于前一个作业才能成功，则作业可以彼此独立运行，也可以按顺序运行。

## 步骤

步骤是可以由作业执行的一组任务。步骤可以运行命令或操作。

## 工作流程

工作流是定制的自动化过程，您可以在存储库中设置它，以便在 GitHub 上构建、测试、打包、发布或部署任何项目。工作流是使用存储库中的`.github/workflows`目录中的`yaml`文件定义的。

# GitHub 操作示例

GitHub Actions 使得部署 DevOps 自动化任务的广泛变化变得简单。下面给出的例子描述了使用 GitHub 动作的 CI/CD 和自动化。

## 示例 1:持续集成

持续集成(CI)在合并代码之前，自动化测试和构建代码的过程。作为一种实践，开发人员必须每天一次或多次(如果需要的话)将他们的更改提交或集成到主共享存储库中。

**。github/workflows/workflow . yml**

```
name: Node Continuous Integration on: pull_request: branches: [ master ] jobs: test_pull_request: runs-on: ubuntu-latest steps: - uses: actions/[[email protected]](https://www.partech.nl/cdn-cgi/l/email-protection) - uses: actions/[[email protected]](https://www.partech.nl/cdn-cgi/l/email-protection) with: node-version: 12 - run: npm ci - run: npm test - run: npm run build
```

*来源:*[*https://fireship . io/lessons/five-useful-github-actions-examples/*](https://fireship.io/lessons/five-useful-github-actions-examples/)

## 示例 2:在发布时将包发布到 NPM

如果你维护一个开源包，那么在发布一个新版本后重新发布你的包是单调的。您可以使用下面给出的工作流程来自动发布一个包到 NPM 或 GitHub 包注册表。

**。github/workflows/workflow . yml**

```
name: Sveltefire Package on: release: types: [created] jobs: build: runs-on: ubuntu-latest steps: - uses: actions/[[email protected]](https://www.partech.nl/cdn-cgi/l/email-protection) - uses: actions/[[email protected]](https://www.partech.nl/cdn-cgi/l/email-protection) with: node-version: 12 - run: npm ci - run: npm test publish-npm: needs: build runs-on: ubuntu-latest steps: - uses: actions/[[email protected]](https://www.partech.nl/cdn-cgi/l/email-protection) - uses: actions/[[email protected]](https://www.partech.nl/cdn-cgi/l/email-protection) with: node-version: 12 registry-url: https://registry.npmjs.org/ - run: npm ci - run: npm build - run: npm publish env: NODE_AUTH_TOKEN: ${{secrets.npm_token}} publish-gpr: needs: build runs-on: ubuntu-lates steps: - uses: actions/[[email protected]](https://www.partech.nl/cdn-cgi/l/email-protection) - uses: actions/[[email protected]](https://www.partech.nl/cdn-cgi/l/email-protection) with: node-version: 12 registry-url: https://npm.pkg.github.com/ scope: '@your-github-username' - run: npm ci - run: npm publish env: NODE_AUTH_TOKEN: ${{secrets.GITHUB_TOKEN}}
```

*来源:*[*https://fireship . io/lessons/five-useful-github-actions-examples/*](https://fireship.io/lessons/five-useful-github-actions-examples/)

## 示例 3:发送电子邮件或聊天信息

将 GitHub 中的数据发布到聊天或电子邮件服务中，如 Slack。

**。github/workflows/workflow . yml**

```
name: Slack Issues on: issues: types: [opened] jobs: post_slack_message: runs-on: ubuntu-latest steps: - uses: actions/[[email protected]](https://www.partech.nl/cdn-cgi/l/email-protection) - uses: rtCamp/[[email protected]](https://www.partech.nl/cdn-cgi/l/email-protection) env: SLACK_WEBHOOK: ${{ secrets.SLACK_WEBHOOK }} SLACK_USERNAME: CyberJeff SLACK_CHANNEL: gh-issues
```

*来源:*[*https://fireship . io/lessons/five-useful-github-actions-examples/*](https://fireship.io/lessons/five-useful-github-actions-examples/)

# 结论

您可以使用 GitHub 操作来自动化您的 GitHub 工作流程。例如，GitHub 操作只需要很少的配置即可使用——GitHub 存储库页面上的一个简单按钮就可以触发 CI/CD 管道的配置，这使得开发人员可以专注于开发，而不是为 Jenkins 设置服务器或与 CircleCI 等其他供应商签约。

*原发布于*[*https://www . partech . nl*](https://www.partech.nl/nl/publicaties/2020/04/automate-your-workflow-with-github-actions)*。*