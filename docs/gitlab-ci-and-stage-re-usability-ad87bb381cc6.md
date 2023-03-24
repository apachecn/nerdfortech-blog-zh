# Gitlab CI 和 stage 可重用性

> 原文：<https://medium.com/nerd-for-tech/gitlab-ci-and-stage-re-usability-ad87bb381cc6?source=collection_archive---------2----------------------->

在任何应用程序中创建可重用的方法或函数都是管理常用代码的有效方法。当您考虑多分支管道时，CICD 管道以及许多重复使用的级引用都是这种情况。

以下面这个为例:

*   **开发分支** : **构建，测试**，*部署到沙盒*，*部署后测试*

*   **主分支** : **构建，测试**，上传到 artifactory，*部署到开发环境，发布测试，部署到测试环境*
*   **发布标签**:从 artifactory 下载构建工件，*部署到生产环境，部署后测试*

构建和测试阶段可能有也可能没有驱动它们的环境特定变量。另一方面，“*部署到< env >环境*”&“*部署后测试*”将有关于您所针对的环境的细节。举个例子。让我们看看如何使用 Gitlab CI，以及如何使它成为一个相当好的模块化。

一个简单的 Gitlab yaml 可能如下所示(是的，它很长)

```
build:
  stage: build
  only:
    - develop
    - master
  script: <build steps>
  artifacts:
    paths:
      - build_artifacts/

test:
  stage: test
  only:
    - develop
    - master
  script: <test steps>

deploy to sandbox:
  stage: deploy
  only:
    - develop
  script: <deploy steps>
  variables:
    ENVIRONMENT: sbox
    PROFILE: default

post deploy tests sandbox: 
  stage: post deploy tests
  only:
    - develop
  script: <post deploy test steps>deploy to dev:
  stage: deploy
  only:
    - master
  script: <deploy steps>
  variables:
    ENVIRONMENT: dev
    PROFILE: default

post deploy tests dev:
  stage: post deploy tests
  only:
    - master
  script: <post deploy test steps>
  variables:
    ENVIRONMENT: dev
    PROFILE: defaultdeploy to test:
  stage: deploy
  only:
    - master
  script: <deploy steps>
  variables:
    ENVIRONMENT: test
    PROFILE: defaultupload to artifactory:
  stage: upload to artifactory
  only:
    - master
  script: <upload to rt steps>
  variables:
    ENVIRONMENT: dev
    PROFILE: defaultpost deploy tests:
  stage: post deploy tests
  only:
    - master
  script: <deploy steps>
  variables:
    ENVIRONMENT: dev
    PROFILE: default      

dl from artifactory:
  stage: deploy
  only:
    - tags
  script: <artifactory download steps>

 deploy to prod:
  stage: deploy
  only:
    - tags
  script: <deploy steps>
  variables:
    ENVIRONMENT: prod
    PROFILE: default

 post deploy tests prod:
  stage: post deploy tests
  only:
    - tags
  script: <post deploy test steps>
  variables:
    ENVIRONMENT: prod
    PROFILE: default
```

# 缺点:

*   《YAML》只有 100 行，仅包含舞台背景。
*   许多可以重复使用的常用参考资料
*   如果需要，很难维护和更新。
*   YAML 最终可能会被应用程序锁定，尽管并不需要如此。

# 第一步:

将 YAML 划分为多个 YAML，并使用 Gitlab 提供的“include”语句。将舞台上聪明的 YAML 台词移到他们自己的 YAML 中，并如下引用它们。

```
include: '/templates/build.yml'
include: '/templates/test.yml'
include: '/templates/deploy.yml'
include: '/templates/post_deploy_tests.yml'
include: '/templates/artifactory_upload.yml'
include: '/templates/artifactory_download.yml'
```

# 隐藏的工作& YAML 主播。

一旦阶段被划分到他们自己的 YAMLs 中，让我们来看看如何减少维护。由于我们已经将阶段关注点分开，它们看起来更容易维护，只需添加或更新一个阶段。

Gitlab 特有的 yaml 行为之一:

*   gitlab 里任何带“.”的工作默认情况下，它前面是一个隐藏的未执行作业。我们可以将它与“锚”的 YAML 解析器功能结合使用，带来一些非常需要的可重用性。让我们看看部署阶段(开发、测试和生产)。

**无 yaml 锚: (移除测试和戳以保持 yaml 较小)**

```
deploy to sandbox:
  stage: deploy
  only:
    - develop
  script: <deploy steps>
  variables:
    ENVIRONMENT: sbox
    PROFILE: default

deploy to dev:
  stage: deploy
  only:
    - master
  script: <deploy steps>
  variables:
    ENVIRONMENT: dev
    PROFILE: default
```

**带 yaml 锚**

```
.deploy_template: &deploy_template
  stage: deploy
  script: <deploy steps>
  artifacts:
    paths:
      - binaries/deploy to sandbox:
  <<: *deploy_template
  only:
    - develop
  variables:
    ENVIRONMENT: sbox
    PROFILE: default

deploy to dev:
  <<: *deploy_template
  only:
    - master
  variables:
    ENVIRONMENT: dev
    PROFILE: default
```

# 常见引用和扩展关键字

但这看起来比我们开始的要大。我们刚才是不是走了几步回到这里？

如果你还记得其他的阶段，有跨阶段的公共引用，比如变量，阶段运行的分支。让我们看看如何以模块化的方式使用它们。

这可以通过两个选项有效地实现:

*   创建一个单独的 yaml 来存放公共元素，如变量、分支引用或跨多个阶段使用的任何 yaml 元素。
*   在 Gitlab CI 中使用 **extends** 关键字。

```
include: '/templates/build.yml'
include: '/templates/test.yml'
include: '/templates/deploy.yml'
include: '/templates/post_deploy_tests.yml'
include: '/templates/artifactory_upload.yml'
include: '/templates/common.yml'
```

“extends”关键字帮助您使用来自另一个 yaml 的附加元素或引用来扩展作业模板。可以把它想象成 Gitlab CI 将所有的模板合并在一起，然后重用“.”你做的任何参考中的隐藏工作。

**Sample common.yml(下面有三个参考..)**

```
.artifact_template: &artifact_template
  artifacts:
    paths:
      - binaries/
.when_develop: &when_develop
  only:
    - develop

 .variables_sbox: &variables_sbox
  variables:
    ENVIRONMENT: sbox
    PROFILE: default
```

重温最初的部署 yaml，包括来自 common.yml 的引用

```
.deploy_template: &deploy_template
  extends:
    - .artifact_template  
  stage: deploy
  script: <deploy steps>deploy to sandbox:
  extends:
    - .deploy_template
    - .when_develop
    - .variables_sbox

deploy to dev:
  extends:
    - .deploy_template
    - .when_develop
    - .variables_dev
```

这可以通过使用 Gitlab 的 stage yaml 的**远程加载来进一步扩展，git lab 允许您将这些可重用的模板作为 git 存储库的一部分，供应用程序团队使用。**

# 这种方法的优点:

*   在一个地方定义变量和公共元素，并在任何地方重用它的扩展。
*   单独阶段的维护更容易执行。
*   如果您希望对每个阶段中存在的内容进行集中的管道控制，请实施标准或阶段特定的步骤。

1.  混合远程加载和本地存在的 YAML 的加载，以使用应用程序的变量引用，同时从中央 repo 使用 stage yamls。