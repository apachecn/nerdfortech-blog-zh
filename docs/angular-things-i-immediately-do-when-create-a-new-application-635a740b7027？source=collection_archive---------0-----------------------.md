# angular——当我创建一个新的应用程序时，我会立即做的事情

> 原文：<https://medium.com/nerd-for-tech/angular-things-i-immediately-do-when-create-a-new-application-635a740b7027?source=collection_archive---------0----------------------->

…您可能会考虑做类似的事情

![](img/0dc749431e5b9db51edd15cbd2b523cd.png)

# 将 Angular 升级至最新版本

**原因**:在我的组织中，开发人员被鼓励从初学者工具包中生成应用程序，平台团队已经在该工具包中包含了必要的特性，例如构建和部署管道。目前 frontend seed 项目带有 Angular v.11，它落后了一个主要版本。我相信让 Angular 应用程序保持最新将使团队能够利用领先的新功能，以及优化和错误修复。重要的是，它非常简单——考虑到还没有添加任何功能。
**如何**:跟随[棱角更新指南](https://update.angular.io/)

# 将 TSLint 迁移到 ESLint

**原因** : TSLint 从 2019 年开始弃用。现在，Angular 还没有包括默认的 linter。如前所述——我的应用程序是用 Angular v.11 生成的，我按照下面的步骤从 TSLint 迁移到 ESLint。
**如何**:跟随[迁移到 ESLint](https://www.youtube.com/watch?v=IDBdtQlugtw&t=108s)

# 用 Jest 和 Angular 测试库替换 Karma 和 Jasmine

**原因**:这个是可选的，是我个人的选择。在使用 Nx workspace 时，我有使用 Jest 和 Angular Testing 库进行测试的经验，我对它提供的性能和信心感到满意。
**注**:由于 Karma 和 Jasmine 默认包含在 Angular project 中，希望迁移到 Jest 的开发者要评估利弊。
**如何**:遵循 [jest-preset-angular 文档](https://thymikee.github.io/jest-preset-angular/docs/getting-started/installation/)和[角度测试库](https://testing-library.com/docs/angular-testing-library/intro/)

# 添加 sonar cube-scanner

**原因**:在我的构建管道中，有一个使用 SonarQube 进行静态代码分析的步骤。由于这一步可能会中断管道，如果开发人员能够在他们的本地环境中运行这种静态代码分析，以获得更快的反馈并尽可能保持管道绿色，那就更好了。
**如何**:按照[Sonar cube 文档](https://docs.sonarqube.org/latest/setup/get-started-2-minutes/)安装 Sonar cube 的本地实例，按照[Sonar cube-scanner](https://github.com/bellingard/sonar-scanner-npm)安装 npm 依赖项以触发本地分析，并考虑安装插件以将测试输出转换为 Sonar 的数据格式，例如 [jest-sonar-reporter](https://github.com/3dmind/jest-sonar-reporter) ，[karma-Sonar cube-unit-reporter](https://github.com/tornaia/karma-sonarqube-unit-reporter)。

# 添加哈士奇

**原因**:同样，这是为了保证您团队中的所有开发人员在将代码推送到远程存储库之前执行所需的步骤，例如静态代码分析、单元测试和 sonar 扫描器。
**如何**:跟随[哈士奇](https://github.com/typicode/husky)

# 考虑

# 添加 Cypress 作为端到端测试

**原因** : Angular 不再提供默认的端到端测试——从 Angular v.12 开始，它的项目中不再包括量角器。我已经看到 Cypress 的采用作为替代。当我需要进行端到端测试时，我可能会考虑使用它。
**如何**:跟随[柏树](https://docs.cypress.io/guides/migrating-to-cypress/protractor#Introduction)

# 添加模拟服务人员(MSW)

**原因**:这个库似乎使我能够模拟一个 REST API，它可以在两种场景中使用，即(1)在单元测试/集成测试中模拟——而不是模拟 httpClient，拦截所有请求并返回模拟数据——以及(2)在开发期间模拟——当后端 API 尚未准备好时。
**如何**:关注此[博客](https://timdeschryver.dev/blog/using-msw-in-an-angular-project#jest-tests)