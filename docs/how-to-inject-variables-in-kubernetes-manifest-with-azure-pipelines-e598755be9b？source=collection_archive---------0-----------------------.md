# 如何用 Azure 管道在 Kubernetes manifest 中注入变量？

> 原文：<https://medium.com/nerd-for-tech/how-to-inject-variables-in-kubernetes-manifest-with-azure-pipelines-e598755be9b?source=collection_archive---------0----------------------->

最近，我接下了为 Juntoz.com 平台第 3 版创建基础的任务，包括使用 nodejs、docker 容器和 kubernetes 实现完全的云原生。

而且，作为我最初开发的一部分，我发现自己遇到了 K8S 清单的一个常见问题:将同一个清单应用于两个环境(暂存和生产)，但使用不同的设置(例如，在暂存中，我只需要一个副本，而在生产中，我需要两个副本)。

最常用的解决方案之一是使用`Helm`，我还不想去那里，因为我刚刚进入这个容器和编排器的全新世界。

在阅读了关于`Kustomize`的文章后，我决定使用一个对我非常有效的老方法:替换文本变量。它易于理解，易于实施，并且非常有效。

这是可能的，因为我们的构建和部署都在 Azure 管道中运行，所以我们有一个先前的层，在将清单应用到集群之前，我们可以在那里进行这些替换。

# 步伐

每个管道都有一个文件`azure-pipelines.yml`，它定义了管道将要执行的步骤。下面是我们将使用的示例。

```
 variables:
- group: Docker Vars
- name: image-name
  value: my-image
- name: image-tag
  value: $(Build.BuildNumber)stages:
- stage: Build
  jobs:
  - job: Build
    pool:
      vmImage: Ubuntu-16.04
    steps:
    - task: Docker@2
      inputs:
        command: login
        containerRegistry: $(docker-container-registry)
    - task: Docker@2
      inputs:
        command: buildAndPush
        repository: $(image-name)
        tags: $(image-tag)
    - task: CopyFiles@2
      inputs:
        contents: $(build.sourcesDirectory)/k8s*.*
        targetFolder: $(build.artifactStagingDirectory)
    - task: PublishBuildArtifacts@1
      inputs:
        pathtoPublish: $(build.artifactStagingDirectory)
        artifactName: drop
- stage: Staging
  jobs:
  - deployment: Staging
    variables:
    - name: kub-pod-instancecount
      value: 1
    - name: env-name
      value: Staging
    - name: log-level
      value: info
    pool:
      vmImage: Ubuntu-16.04
    environment: STG
    strategy:
      runOnce:
        deploy:
          steps:
          - task: qetza.replacetokens.replacetokens-task.replacetokens@3
            displayName: Replace tokens in **/*
            inputs:
              rootDirectory: $(Pipeline.Workspace)/drop
              targetFiles: '**/*.yml'
              keepToken: true
              tokenPrefix: __
              tokenSuffix: __
          - task: Kubernetes@1
            displayName: kubectl apply
            inputs:
              connectionType: Azure Resource Manager
              azureSubscriptionEndpoint: my-azure
              azureResourceGroup: my-resource-group
              kubernetesCluster: my-cluster
              command: apply
              arguments: -f $(Pipeline.Workspace)/drop
- stage: Production
  jobs:
  - deployment: Production
    variables:
    - name: kub-pod-instancecount
      value: 2
    - name: env-name
      value: Production
    - name: log-level
      value: info
    pool:
      vmImage: Ubuntu-16.04
    environment: PROD
    strategy:
      runOnce:
        deploy:
          steps:
          - task: qetza.replacetokens.replacetokens-task.replacetokens@3
            displayName: Replace tokens in **/*
            inputs:
              rootDirectory: $(Pipeline.Workspace)/drop
              targetFiles: '**/*.yml'
              keepToken: true
              tokenPrefix: __
              tokenSuffix: __
          - task: Kubernetes@1
            displayName: kubectl apply
            inputs:
              connectionType: Azure Resource Manager
              azureSubscriptionEndpoint: my-azure
              azureResourceGroup: my-resource-group
              kubernetesCluster: my-cluster
              command: apply
              arguments: -f $(Pipeline.Workspace)/drop
```

如您所见，管道在所有三个阶段都非常简单:构建、试运行和生产。

`Build`阶段创建 Docker 映像并将其推送到注册中心，它还将发布`k8s-apply.yml`文件作为管道工件，因此它在部署阶段是可见的。

`Staging`阶段执行部署到暂存环境中，而`Production`阶段在其对应的环境中执行相同的操作。

这是我们想要应用的 kubernetes 清单(`k8s-apply.yml`)。

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deploy
spec:
  replicas: __kub-pod-instancecount__
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: __docker-container-url__/__image-name__:__image-tag__
        ports:
        - containerPort: 3000
        env:
        - name: API_PORT
          value: "3000"
        - name: LOG_LEVEL
          value: __log-level__
        - name: RUN_ENV
          value: __env-name__
---
apiVersion: v1
kind: Service
metadata:
  name: my-svc
spec:
  selector:
    app: my-app
  ports:
    - port: 3000
      targetPort: 3000
```

清单是一个常规清单，其特殊性在于它包含几个用“__”括起来的变量名。

神奇的事情发生在任务`qetza.replacetokens.replacetokens-task.replacetokens@3`的每个环境的部署阶段。该任务将扫描所有的`targetFiles`，并验证 __(如 __log-level__)包含的任何文本是否解析为变量名。如果它作为变量名匹配，那么它将把它的值写入文件，然后文件被覆盖。

因此，在临时部署时，清单应该以如下形式结束:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deploy
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: my-registry/my-image:my-tag
        ports:
        - containerPort: 3000
        env:
        - name: API_PORT
          value: "3000"
        - name: LOG_LEVEL
          value: info
        - name: RUN_ENV
          value: Staging
```

在生产中，像这样:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deploy
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: my-registry/my-image:my-tag
        ports:
        - containerPort: 3000
        env:
        - name: API_PORT
          value: "3000"
        - name: LOG_LEVEL
          value: info
        - name: RUN_ENV
          value: Production
```

## 变量在哪里？

大概你在问自己，`__docker-container-url__`怎么变成了`my-registry`？因为`docker-container-url`是一个注册在变量组`Docker Vars`中的变量，所以这个问题得到了解决。

变量可以在几个范围级别上定义，其中更直接的级别将覆盖最远的级别。更多信息，请查看[这个](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/variables?view=azure-devops&tabs=yaml%2Cbatch)。

额外收获:变量的一个很酷的地方是它们也可以内联扩展。如果我定义了一个变量`my-image-prefix = jz`，然后定义了第二个变量`my-image = $(my-image-prefix)-user`，那么任务将生成文本`my-image = jz-user`。