# 使用 RabbitMQ 集群操作器在 Kubernetes 上部署 RabbitMQ

> 原文：<https://medium.com/nerd-for-tech/deploying-rabbitmq-on-kubernetes-using-rabbitmq-cluster-operator-ef99f7a4e417?source=collection_archive---------0----------------------->

在 Kubernetes 上运行 RabbitMQ，并使用一个简单的 Golang 应用程序进行测试。

嗯，RabbitMQ 这个词在整个 DevOps 时代都很常见。因此，在我们开始理解什么是 RabbitMQ 之前，让我们先理解什么是消息传递代理。消息代理是一种软件，它使应用程序、服务能够相互通信并交换数据。这些消息传递代理软件的一些例子是

1.  阿帕奇卡夫卡
2.  亚马逊 MQ
3.  Oracle 消息代理
4.  Apache 活动 MQ
5.  Rabbit MQ(我们将在本文中深入探讨 Rabbit MQ)

RabbitMQ 是一个开源的消息代理。它是部署最广泛的开源消息代理。RabbitMQ 是轻量级的，易于在内部和云中部署。让我们看看 RabbitMQ 官方页面上提到的 RabbitMQ 必须提供的各种特性

1.  支持多种消息协议，如 AMQP、STOMP、MQTT 等。
2.  高可用性和高吞吐量的分布式开发。
3.  支持多种工具和插件。
4.  用于管理和监控的交互式 UI 和 HTTP-API。
5.  对各种语言的丰富支持，如(Python、Java、Go、Ruby、C#等)。

![](img/8e7b0e172f4e8fd4ef3f1b46ff66b0f6.png)

图片来源: [RabbitMQ](https://www.rabbitmq.com/)

# 整个故事是关于什么的？(TLDR)

1.  在 Kubernetes 上安装 RabbitMQ 操作器。
2.  使用 CRD(自定义资源定义)创建一个 **RabbitmqCluster** 。
3.  使用舵图部署一个用 Golang 编写的简单 Hello World 应用程序。
4.  通过向队列发送消息并从客户端接收消息来测试设置。

# 先决条件

1.  Kubernetes 集群(可以是本地、AKS、EKS、GKE、Kind)。
2.  头盔 3 安装在你的 Kubernetes 集群中(安装指南:[https://helm.sh/docs/intro/install/](https://helm.sh/docs/intro/install/))。

## 故事资源

1.  GitHub 链接:[https://github.com/pavan-kumar-99/medium-manifests](https://github.com/pavan-kumar-99/medium-manifests)
2.  GitHub 分支:rabbitmq

## 在 Kubernetes 上安装 RabbitMQ 操作符

```
**###Install the RabbitMQ operator** 
kubectl apply -f [https://github.com/rabbitmq/cluster-operator/releases/latest/download/cluster-operator.yml](https://github.com/rabbitmq/cluster-operator/releases/latest/download/cluster-operator.yml)**###Check if the components are healthy in the rabbitmq-system namespace**kubectl get all -o wide -n rabbitmq-system
```

![](img/ae0895da57c600f75f5d434d09c7d851.png)

一旦 rabbitmq-cluster-operator 运行良好，并且创建了所有相关的组件，现在让我们继续在我们的 Kubernetes 中安装 rabbitmq 集群。现在让我们为清单克隆我的回购协议。

```
$ git clone [https://github.com/pavan-kumar-99/medium-manifests.git](https://github.com/pavan-kumar-99/medium-manifests.git) 
-b rabbitmq $ cd medium-manifests/ $ kubectl apply -f rabbitmqcluster.yaml 
```

让我们更多地了解一下我们用来创建 RabbitmqCluster 的清单文件。

1.  kind: RabbitmqCluster(由 RabbitMQ 集群操作器安装的 CRD。)
2.  metadata . name:rabbit MQ cluster 的名称。
3.  spec.replicas:创建 RabbitMQ 集群的 RabbitMQ 副本的数量。
4.  resources . requests/resources . limits:[https://kubernetes . io/docs/concepts/configuration/manage-resources-containers/](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)
5.  rabbit MQ . additional config:rabbit MQ 集群的附加配置。(参考:https://www.rabbitmq.com/configure.html#config-file)
6.  service.type:用于公开 RabbitMQ 集群的 Kubernetes 服务类型。(这里我们使用 LoadBalancer 类型)。

现在让我们检查一下 RabbitmqCluster 的状态。

```
$ kubectl describe RabbitmqCluster production-rabbitmqcluster 
```

![](img/9ef8832794ee75f64adf8e88832ba46f.png)

RabbitMQ 集群的状态

现在，在上面的 RabbitMQ 集群清单中，我们已经将默认用户名和密码指定为 guest/guest。然而，RabbiMQ 集群操作符也为我们创建了一个 Kubernetes 秘密，这样也可以从那里提取用户名和密码。

```
**###Username**$ kubectl get secret production-rabbitmqcluster-default-user -o jsonpath='{.data.username}' | base64 --decode**###Password**$ kubectl get secret production-rabbitmqcluster-default-user -o jsonpath='{.data.password}' | base64 --decode
```

现在让我们探索由 RabbitMqCluster 创建的资源

```
$ kubectl get all -l app.kubernetes.io/part-of=rabbitmq
```

![](img/4d783eaed533c84c1912b0082735906c.png)

Rabbitmq 资源

您现在应该看到一个有状态集(RabbitMQ 总是使用有状态集来创建集群)、一个 headless 服务(用于集群节点发现)和一个 Kubernetes 负载平衡器(用于访问 RabbitMQ WebUI)。

## 复制/集群管理

现在我们已经看到了 RabbitMQ 创建的所有组件，让我们了解 RabbitMQ 集群的一些重要方面。让我们使用检查集群的状态(您可以进入任何有状态的 set pod)

```
$ kubectl exec production-rabbitmqcluster-server-0 -- /bin/sh -c "rabbitmqctl cluster_status --formatter json" | jq
```

![](img/4a8d2d4aa4ae9e302992c7f6db84e1dd.png)

RabbitMQ 集群状态

现在让我们检查集群中的节点。

```
$ kubectl exec production-rabbitmqcluster-server-0 -- /bin/sh -c "rabbitmqctl cluster_status --formatter json" | jq -r .running_nodes
```

![](img/c3ec3e1219807a4bbac23803bd1f08dd.png)

集群节点

好了，够了黑绿色终端。现在让我们转向一些丰富多彩的东西。RabbitMQ 在端口 15672 公开了一个集群管理 Web UI。让我们通过以下方式获得负载平衡器的 IP 地址

```
$ kubectl get svc production-rabbitmqcluster -o jsonpath='{.status.loadBalancer.ingress[0].ip}'192.168.29.166 ( Since I am using a local cluster my LoadBalancer Ip start with 192* ) **###Let us access the WEBUI from** [**http://192.168.29.166:15672**](http://192.168.29.166:15672) **and login using guest/guest.** 
```

![](img/f7f799dcabefc63d98f66a228e32b6be.png)

RabbitMQ Web 用户界面

## 在 Golang 中部署一个简单的 Hello World 应用程序。

为了测试这个设置，我编写了一个简单的 Golang 应用程序，对它进行了 DockerHub 化，并将图像推送到我的 [DockerHub 注册表中。](https://hub.docker.com/repository/docker/greypavan/medium-manifests)

![](img/5531e17f1f91b7ba02d608613030297c.png)

码头枢纽

你可能想知道为什么图像的尺寸这么小。我已经通过在 Docker 中使用[多阶段构建](https://docs.docker.com/develop/develop-images/multistage-build/)减小了尺寸。这个设置的所有代码和 Docker 文件都已经在 GitHub repo 中。现在，让我们从部署 send microservice helm 图表开始(这将为我们创建一个 WebUI 来发布消息)。

```
$ git clone [https://github.com/pavan-kumar-99/medium-manifests.git](https://github.com/pavan-kumar-99/medium-manifests.git) 
-b rabbitmq$ cd medium-manifests/$ helm install send-service rabbitmq-helm/send-service --set configmap.rmq_url="amqp://guest:guest@192.168.29.165:5672/" 
```

现在您已经安装了 send-service，您应该找到部署、服务(类型:LoadBalancer)和一个水平 Pod Autoscaler，并且创建了 RabbitMQ URL 的 configmap 并将其作为环境变量挂载到 Pod。现在让我们导航到端口 8080 的发送服务 WEB UI。请忽略我真的很烂的 HTML 技能。

```
$ kubectl get svc send-service-rabbitmq-helm -o jsonpath='{.status.loadBalancer.ingress[0].ip}'192.168.29.166 
```

![](img/e25ded60a5e7a77134cd8aeedce08bf7.png)

发送服务 Web 用户界面

让我们在消息部分键入一条消息，看看会发生什么。

![](img/500ce5bf1c54dd95ceb60dd19b656849.png)

发送服务日志

我一发送消息，我的发送服务就成功地将消息发送到 RabbitMQ 队列。让我们从 RabbiMQ UI 验证同样的情况。

![](img/28c51f3485360ff74710f2bff3c52a9b.png)

来自用户界面的消息

![](img/34256d23995ea5624086b3936a99ac70.png)

从 RabbitMQ 控制台可以看到该消息

现在让我们安装接收服务微服务，它将接收来自 RabbitMQ 服务器的消息。

```
$ git clone [https://github.com/pavan-kumar-99/medium-manifests.git](https://github.com/pavan-kumar-99/medium-manifests.git) 
-b rabbitmq$ cd medium-manifests/$ helm install receive-service rabbitmq-helm/receive-service --set configmap.rmq_url="amqp://guest:guest@192.168.29.165:5672/"
```

![](img/ebb17ed9065ee9fa5816270fc24c0cae.png)

接收器服务日志

现在，您应该看到接收微服务正在读取消息。

好了，现在让我们发送一些批量请求，看看我们的微服务是否按预期工作。

```
$ git clone [https://github.com/pavan-kumar-99/medium-manifests.git](https://github.com/pavan-kumar-99/medium-manifests.git) 
-b rabbitmq$ cd medium-manifests/$ sh test_rmq.sh ( This will send 10 curl requests with the iterator as the message ). 
```

![](img/0cba2c030b4401ddd8e272bf3dfa6721.png)

发送服务日志

![](img/cd554a43a539eb75aa5d92c74ff4b1e2.png)

接收服务日志

![](img/d6bfdf3dc604a8a7fb44723e897465db.png)

最终部署...

这是一个非常基本的场景。RabbitMQ 支持许多其他场景，如复杂的工作队列、选择性路由、发布/订阅等。可能还有许多其他复杂的用例及场景。请随意在评论区发表它们。

直到下次……

# 结论

感谢阅读我的文章。希望你喜欢它。以下是我的一些其他文章，你可能会感兴趣。

# 被推荐的

[](/swlh/introduction-to-jenkins-operator-f4cb7ebc2e0b) [## 詹金斯算子简介

### Kubernetes 中的 Jenkins 运算符入门

medium.com](/swlh/introduction-to-jenkins-operator-f4cb7ebc2e0b) [](/nerd-for-tech/introduction-to-crossplane-2f873ae0f9f3) [## 交叉平面简介

### 如何使用 Kubernetes 清单和 Crossplane 在云上创建任何资源。

medium.com](/nerd-for-tech/introduction-to-crossplane-2f873ae0f9f3) [](/faun/introduction-to-bitnami-sealed-secrets-bb5ae74d9a25) [## 比特纳米密封秘密介绍

### 如何使用 Sealed Secrets 和 Kubese 在 GitHub 中存储您的秘密

medium.com](/faun/introduction-to-bitnami-sealed-secrets-bb5ae74d9a25) [](/faun/introduction-to-external-dns-in-kubernetes-654aa4cf38e6) [## Kubernetes 中的外部 DNS 简介

### 如何使用外部 DNS 在 Kubernetes 中自动创建 DNS 记录

medium.com](/faun/introduction-to-external-dns-in-kubernetes-654aa4cf38e6) 

# 参考

 [## Kubernetes 的 RabbitMQ 集群运算符

### rabbit MQ Cluster Kubernetes Operator 是一个 Kubernetes 操作员，它可以自动进行配置、管理和操作…

www.rabbitmq.com](https://www.rabbitmq.com/kubernetes/operator/operator-overview.html)