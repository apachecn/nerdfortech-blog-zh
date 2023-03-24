# Kubernetes 手记

> 原文：<https://medium.com/nerd-for-tech/kubernetes-handnotes-4a3a437428b4?source=collection_archive---------15----------------------->

![](img/379071cc8f3d2f9b895136e5383311aa.png)

这些是我在与 kubernetes 合作的几年中准备的一些笔记。其中许多通常不为人所知，除非一个人深入挖掘了官方的 kubernetes 文档。

这些没有特定的顺序，旨在作为快速和相对详细地参考 kubernetes 的注释。

这些手记的目标读者是已经熟悉 kubernetes 核心概念的初学者，现在希望更深入地理解，以及希望快速和全面地参考 kubernetes 最重要的方面，而不是一遍又一遍地参考官方文档的专业人士。

这些绝不是 kubernetes 官方文件的替代品。无论如何，它们都是学习 kubernetes 的最佳资源。与官方的 kubernetes 文档相比，这些手记只是试图提供一个获得的*知识与花费的*时间的更好比率。

## 涵盖的资源:

1.  分离舱
2.  复制集
3.  控制器
4.  主组件
5.  节点组件
6.  目标
7.  UID 的
8.  名称空间
9.  服务
10.  标签
11.  标签选择器
12.  字段选择器
13.  释文
14.  对象管理
15.  初始化容器
16.  秘密生成器
17.  永久卷(pv)和永久卷声明(pvc)
18.  进入
19.  入口控制器
20.  服务发现
21.  端点
22.  Kube 代理
23.  Kube dns
24.  Etcd

## 分离舱

1.  Kubernetes 集群中的每个 Pod 都有一个唯一的 IP 地址，即使是同一节点上的 Pod 也是如此。
2.  Pod 中的每个容器共享网络名称空间，包括 IP 地址和网络端口。
3.  默认情况下，docker 使用主机私有网络，因此容器只有在同一台机器上才能与其他容器对话。
4.  Pod 内的容器可以使用 localhost 相互通信，并且集群中的所有 Pod 都可以在没有 NAT 的情况下相互查看。
5.  Pod 中的所有容器都可以访问共享卷，从而允许这些容器共享数据。
6.  尽管每个 Pod 都有一个唯一的 IP 地址，但如果没有服务，这些 IP 不会暴露在群集之外。*服务允许您的应用程序接收流量*。

## 复制集

1.  通过更改部署中副本的数量来实现扩展
2.  一个**复制集**可能会通过创建新的 Pods 来动态地驱动集群回到期望的状态，以保持应用程序的运行
3.  Kubernetes 也支持 pod 的自动伸缩，但是这超出了本文的范围。缩放到零也是可能的，它将终止指定部署的所有 pod。
4.  kubernetes 复制集中的状态

*   1.*期望的*:配置的副本数量
*   2.*当前*:显示现在有多少副本正在运行
*   3.*最新*:为匹配所需(已配置)状态而更新的副本数量
*   4.*可用*:显示用户实际可用的副本数量

5.kubernetes 中的更新是有版本的，任何*部署更新*都可以恢复到以前的(稳定的)版本。

## 控制器

1.  一个**控制器**处理 pod 管理的所有方面，包括但不限于 pod 的创建、调度、复制和修复。

## Kubernetes 主组件

1.  **kube-apiserver** :公开 Kubernetes API。它被设计为水平扩展
2.  **etcd** :一致且高度可用的键值存储，用作 Kubernetes 所有集群数据的后备存储。始终为您的 Kubernetes 集群的 etcd 数据制定一个备份计划。
3.  kube-scheduler :根据个人和集体资源需求、硬件/软件/策略约束、相似性和反相似性规范、数据局部性、工作负载间干扰和最后期限，调度新创建的 pods 在节点上运行。
4.  **kube-控制器-管理器**:运行控制器。每个控制器包括:

*   1.**节点控制器**:负责节点宕机时的通知和响应。
*   2.**复制控制器**:负责维护系统中每个复制控制器对象的正确数量。
*   3.**端点控制器**:填充端点对象(即加入服务&窗格)。
*   4.**服务帐户&令牌控制器**:为新的名称空间创建默认帐户和 API 访问令牌。

5.**云控制器管理器**:运行与底层云提供商交互的控制器。云控制器管理器二进制文件是 Kubernetes 版中引入的 alpha 特性。以下控制器具有云提供商依赖性:

*   1.**节点控制器**:用于检查云提供商，以确定某个节点在停止响应后是否已从云中删除
*   2.**路由控制器**:用于在底层云基础设施中设置路由
*   3.**服务控制器**:用于创建、更新和删除云提供商负载平衡器
*   4.**卷控制器**:用于创建、附加和安装卷，并与云提供商交互以协调卷

## Kubernetes 节点组件

1.  kubelet :在集群中的每个节点上运行的代理。它确保容器在 pod 中运行。它不管理不是由 Kubernetes 创建的容器。
2.  **容器运行时**
3.  **插件**:提供集群功能
4.  **DNS** : DNS 服务器。为 Kubernetes 服务提供 DNS 记录。Kubernetes 启动的容器会自动将该 DNS 服务器包含在它们的 DNS 搜索中。
5.  **网络用户界面**
6.  **集装箱资源监控**
7.  **集群级日志**

## Kubernetes 对象

1.  它们是库伯内特系统中的持久实体。Kubernetes 使用这些实体来表示集群的状态。
2.  通过创建一个对象，您可以有效地告诉 Kubernetes 系统您希望集群的工作负载是什么样子；这是您的集群所需的状态。
3.  每个 Kubernetes 对象都包含两个管理对象配置的嵌套对象字段:对象规范和对象状态。

*   1. **Spec** :描述你对对象的期望状态
*   2.**状态**:描述对象的实际状态，由 Kubernetes 系统提供并更新。

4.*对象规格*文件中的必填字段:

*   1. **apiVersion**
*   2.**种类**
*   3.**元数据**
*   1.*名称*:必填项
*   2. *uid* :强制:系统提供
*   3.*名称空间*:可选
*   4.**规格**

5.Kubernetes REST API 中的所有对象都由一个**名称**和一个 **UID** 明确标识。

*6。Name* 是客户端提供的字符串，引用资源 URL 中的对象，比如 */api/v1/pods/some-name* 。这就成了 DNS_NAME？。

7.一次只能有一个给定种类的对象有给定的名称。但是，如果删除该对象，可以创建一个同名的新对象。

## Kubernetes UID 的

1.  它们是 kubernetes *系统生成的唯一标识对象的字符串*。
2.  在 Kubernetes 集群的整个生命周期中创建的每个对象都有一个**独特的 UID** 。它旨在区分相似实体的历史事件。

## 名称空间

1.  Kubernetes 支持由同一个物理集群支持的多个虚拟集群。这些虚拟集群被称为**名称空间**。
2.  名称空间适用于许多用户分布在多个团队或项目中的环境。
3.  命名空间为名称提供了范围。*资源的名称需要在一个名称空间内是唯一的，但不能跨名称空间*。
4.  命名空间不能相互嵌套。
5.  默认名称空间:

*   1.**默认**
*   2. **kube-system** :用于由 Kubernetes 系统创建的对象
*   3. **kube-public** :所有用户(包括未经认证的用户)均可读取。

6.如果想要跨名称空间访问服务，需要使用完全限定的域名(FQDN)。

7.kubernetes 的默认行为是在本地名称空间中查找服务。服务 DNS 条目的格式为:**..svc.cluster.local** 。

*8。名称空间资源*本身不在名称空间中。

*9。低级资源*，如*节点*和*持久卷*，在任何名称空间中都是**而不是**。

## 服务

1.  Kubernetes 中的一个**服务**是一个抽象，它定义了一组逻辑 pod 和一个访问它们的策略。服务使依赖 pod 之间的松散耦合成为可能。
2.  尽管每个 Pod 都有一个唯一的 IP 地址，但如果没有服务，这些 IP 不会暴露在群集之外。*服务允许您的应用程序接收流量*。
3.  服务被分配一个唯一的 IP 地址(也称为 clusterIP)。
4.  该地址与服务的生命周期相关联，并且在服务存在时不会改变
5.  一个服务由一组 pod 支持，这些 pod 通过端点公开。
6.  服务的选择器将被连续评估，结果将被发布到一个端点对象
7.  当一个 Pod 终止时，它会自动从端点中删除，并且与服务选择器匹配的新 Pod 会自动添加到端点中。
8.  一个服务 IP 是完全虚拟的，它永远不会上网。
9.  Kubernetes 服务提供了一个稳定的，**虚拟 IP (VIP)** 地址。
10.  虚拟 IP(VIP)地址意味着它没有连接到任何网络接口
11.  VIP 的目的是将流量转发到 pod
12.  保持 VIP 和 pod 之间的映射最新是 kube-proxy 的工作，kube-proxy 运行在每个节点上，查询 API 服务器以了解集群中的新服务。
13.  服务的*目标不一定是 pod* 。它可以是外部集群组件、其他名称空间中的组件、非 kubernetes 组件。只需定义没有选择器属性的服务。
14.  如果没有选择器属性，则不会创建端点对象。
15.  对于多端口服务(公开多个端口的服务)，您必须给出所有端口的名称，以便端点能够被区分
16.  通过在 *ServiceSpec* 中指定类型，可以以不同的方式公开服务:

*   1. **ClusterIP(默认)** —在集群中的内部 IP 上公开服务。这种类型使服务只能从群集内部访问。
*   2.**节点端口** —使用 NAT 在集群中每个选定节点的同一端口上公开服务。使用`<NodeIP>:<NodePort>`从集群外部访问服务。这充当 ClusterIP 的超集。
*   3.**负载平衡器** —在当前云中创建一个外部负载平衡器(如果支持)，并为服务分配一个固定的外部 IP。这充当节点端口的超集。
*   4. **ExternalName** —通过返回具有任意名称(由规范中的 ExternalName 指定)的 CNAME 记录来公开服务。不使用代理。这种类型需要 1.7 版或更高版本的 kube-dns。
*   5.综上所述， **ExternalName** = >是**负载均衡器的超集** = >是**节点端口** = >是**集群 IP** 的超集

17.差异黑白端口、目标端口和节点端口:

*   1. **Port** :端口号，使一个服务对同一集群内运行的其他服务可见
*   2.**目标端口**:服务运行的端口
*   3.**节点端口**:外部用户*使用 **Kube-Proxy** 可以访问服务的端口*

18.服务在一组 pod 之间路由流量。

19.关于**节点端口**类型服务的几点说明:

*   1.它*不是*为生产环境设计的。同样使用*负载平衡器*或*入口控制器/资源*
*   2.需要为服务定义指定额外的节点端口属性
*   3.在每个节点上打开指定的(如果未指定，则自动选择)端口
*   4.每个端口只能有一个服务
*   5.您只能使用 30000–32767 端口
*   6.如果您节点/虚拟机 IP 地址发生变化，您需要处理它

20.关于**负载平衡器**类型服务的几点注意事项:

*   1.向外界公开服务的最佳方法，如果您的云提供商支持的话
*   2.没有过滤，没有路由等。这意味着您可以向它发送几乎任何类型的流量，如 HTTP、TCP、UDP、Websockets、gRPC 或其他。
*   3.每个使用 *LoadBalancer* 公开的服务都将获得自己的 IP 地址，并且您必须为每个公开的服务支付一个 LoadBalancer，这可能会非常昂贵。

21.使用服务规范中的 *externalips* 将 IP 地址设置为服务目标。这可以在群集之外

22.服务有一个集成的负载平衡器，可以将网络流量分配给公开部署的所有单元。服务将使用端点持续监控正在运行的 pod，以确保流量仅发送到可用的 pod。

23.如果想要跨名称空间访问服务，需要使用完全限定的域名(FQDN)。

24.kubernetes 的默认行为是在本地名称空间中查找服务。服务 DNS 条目的格式为: *<服务名>。<名称空间名称> .svc.cluster.local* 。

25.Kubernetes 提供了一个 DNS **集群插件服务**，可以自动为其他服务分配 DNS 名称。

26.关于**无头服务**的几点说明:

*   1.当您不需要或不想要负载平衡和单一服务 IP 时。
*   2.指定*无*作为*集群 IP* 值。
*   3.允许开发人员以自己的方式自由发现，从而减少与 Kubernetes 系统的耦合。
*   4.没有分配群集 IP。
*   5.kube-proxy 不处理这些服务。
*   6.平台不会为它们进行负载平衡或代理。
*   7.如果定义了选择器，端点控制器在 API 中创建端点记录，并修改 DNS 配置以返回直接指向支持服务的 Pods 的记录(地址)。
*   8.如果没有定义选择器，就不会创建*端点*对象。但是，为所有其他类型的 *ExternalName* 类型服务创建 *CNAME* 记录，并为与该服务共享名称的任何*端点*创建记录。

## 标签

1.  标签可以在创建时或以后附着到对象上。此外，它们可以随时修改。
2.  标签用于指定对象的识别属性。
3.  *名称段*是**必需的**，必须在 *63 个字符以内*。
4.  前缀必须是一个 DNS 子域:由点(.)，总长度不超过 253 个字符，后跟一个斜杠(/)。
5.  标签用于指定**识别对象**的属性。*非识别信息*应使用**注释**进行记录。
6.  如果省略前缀，则标签密钥被认为是用户私有的。
7.  **kubernetes.io/**和**k8s.io/**前缀是为 Kubernetes 核心组件保留的。
8.  有效标签值必须等于或少于 63 个字符。它们也可能是空的。

## 标签选择器

1.  客户端或用户可以识别一组对象。标签选择器是 Kubernetes 中的核心分组原语。
2.  支持以下*类型的标签选择器*:基于**等式的**和基于**集合的**。

*   1.基于等式的标签选择器:
*   2.基于集合的标签选择器:

3.基于等式的标签选择器:

*   1.允许的操作员:`=,==,!=`
*   这些可以指定为:

— 3.1.新线路终止:

```
environment = production
tier != frontend
```

— 3.2.逗号分隔:

```
environment=production,tier!=frontend
```

4.基于集合的标签选择器:

*   允许的操作员:`in, notin and exists`(仅密钥标识符)
*   示例:

```
environment in (production, qa)
tier notin (frontend, backend)
partition
!partition
partition,environment notin (qa)
>>> Above one selects resources with a partition key(no matter the value) and with environment different than  qa
```

*5。基于集合的*标签选择器可以与基于*等式的*选择器混合使用。示例:

```
partition in (customerA, customerB),environment!=qa
```

6.标签选择器可以与作为查询参数的 API 调用和 kubectl 命令结合使用，以*过滤返回的结果*。示例:

```
partition in (customerA, customerB),environment!=qa
```

*7。服务*和*复制控制器*不支持*基于集合的标签选择器*。

8.较新的资源，如*作业*、*部署*、*副本集*和*守护进程集*，支持基于集合的标签选择器。

## 字段选择器

1.  它们允许您根据一个或多个资源域的值来选择 Kubernetes 资源。示例:

```
status.phase=Pending
kubectl get pods --field-selector status.phase=Running
```

2.受支持的字段选择器因 Kubernetes 资源类型而异。

3.所有资源类型都支持*元数据.名称*和*元数据.名称空间*字段。

4.使用不支持的字段选择器会产生错误。

5.可以一次过滤多个资源。此外，可以使用逗号作为分隔符给出多个字段选择器。示例:

```
kubectl get statefulsets,services --field-selector=status.phase!=Running,spec.restartPolicy=Always
```

## 释文

1.  它们存储*非识别对象数据*。它们*不能*用于根据它们的值定位/选择对象。
2.  注释中的*元数据*可大可小，结构化或非结构化，并且可以包含标签不允许的字符。
3.  注释具有与标签相同的语法。

## Kubernetes 对象管理

以下是与 kubernetes 对象交互的 3 种方式:

1.  使用**命令式命令**:它们作用于*活体*
2.  使用**命令式对象配置**:它们操作*单个文件*
3.  使用**声明性对象配置**:它们对文件的*目录进行操作*

## 命令式命令

1.  与命令中带有资源名称的 kubectl 一起使用

## 命令性对象配置

1.  与 kubectl with 操作(创建、替换等)一起使用。)和单个对象配置文件。
2.  指定的对象配置文件必须包含 YAML 或 JSON 格式的对象的完整定义。
3.  也可以指定多个文件。示例:

```
kubectl delete -f nginx.yaml -f redis.yaml
```

## 声明性对象配置

1.  对本地存储的对象配置文件进行操作。
2.  用户不定义要对文件采取的操作。
3.  kubectl 自动检测每个对象的创建、更新和删除操作。
4.  使用**补丁**操作保存其他编写器所做的更改，同时应用新的更改(*仅与*不同)。
5.  要查看将要进行的更改，请使用:

```
kubectl diff -f configs/
```

6.要应用更改:

```
kubectl apply -f configs/
```

7.使用命令行标志 **-R** 递归处理目录。

8.这些文件被称为资源配置。

## 初始化容器

1.  它们是在应用程序容器之前运行的*专用容器。*
2.  Thwy 可以包含应用程序映像中没有的实用程序或设置脚本。
3.  他们总是运行到完成。
4.  在下一个任务开始之前，每个任务都必须成功完成。
5.  *自定义容器*可以使用 PodSpec 的 *initContainers 字段指定为 **initContainers** 。*
6.  在*规格*对象的所有方面几乎与常规容器完全相同。
7.  不支持就绪探测，因为它们必须在 pod 就绪之前运行完成。
8.  它们在网络和卷初始化后**启动。**
9.  对*初始化容器规范*的更改仅限于容器图像字段。
10.  **activeDeadlineSeconds** 适用于两种类型的容器。
11.  应用程序容器映像更改只会重新启动应用程序容器，而不会重新启动初始化容器。为此，需要更改 init 容器映像。

## 秘密生成器:kustomization.yaml

1.  机密是一种存储敏感数据(如密码或密钥)的对象。
2.  从 1.14 开始，kubectl 支持使用库化文件管理 kubernetes 对象。
3.  您可以在 **kustomization.yaml** 中通过生成器创建一个秘密。

## 永久卷(pv)和永久卷声明(pvc)

1.  使用共享卷时，使用 **ReadWriteMany** 而不是 **ReadWriteOnce** 。
2.  *访问控制* : **gid(组 id)** 可以分配给创建的卷，以*限制对具有相同 gid 的特定 pod 的访问*。
3.  **PersistentVolume (PV)** 是集群中的一块存储，由管理员手动配置，或者由 kubernetes 使用 **StorageClass** 动态配置。
4.  **PersistentVolumeClaim(PVC)**是用户的存储请求，可由*持久卷*满足。
5.  PersistentVolumes 和 PersistentVolumeClaims 独立于 Pod 生命周期，并通过重新启动、重新计划甚至删除 Pod 来保存数据。
6.  许多群集环境都安装了默认的存储类。当 PersistentVolumeClaim 中未指定 StorageClass 时，将使用群集的默认 StorageClass。
7.  在具有默认存储类(hostPath)的本地集群中，数据保存在节点的 **/tmp** 目录中。因此，可能会在重新启动时丢失。

## Kubernetes 秘密对象类型

1.  存储一段敏感数据，如密码或密钥。

## 域名服务器(Domain Name Server)

1.  Kubernetes 提供了一个 dns 集群插件服务,可以自动将 DNS 名称分配给其他服务。
2.  要检查集群上是否正在运行相同的程序，请使用以下命令:

```
kubectl get services kube-dns --namespace=kube-system
```

## 进入

1.  管理对群集中服务的外部访问的 API 对象，通常是 HTTP。
2.  Ingress 可以提供负载平衡、SSL 终止和基于名称的虚拟主机。
3.  有点像反向代理
4.  入口可以配置为向服务提供外部可达的 URL、负载平衡流量、终止 SSL 以及提供基于名称的虚拟主机。
5.  入口不会暴露任意端口或协议。向互联网公开除 HTTP 和 HTTPS 之外的服务通常使用类型为`Service.Type=NodePort`或`Service.Type=LoadBalancer`的服务。
6.  你必须有一个**入口控制器**来满足一个入口。*只创建入口资源没有效果*。
7.  请注意，并非所有入口控制器都支持全部规格。选择时要小心。
8.  它支持 TLS。
9.  支持负载平衡。支持一些常用算法。对于其他情况，可以使用服务负载平衡器。
10.  默认情况下不公开运行状况检查。但是，可以使用就绪探测器。
11.  可以进行跨可用性区域部署，但这取决于云提供商的支持。有关在联合集群中部署*入口的详细信息，请参考**联合文档**。*
12.  入口类型:

*   1.*单一服务入口*:公开单一服务。没有主机和路径映射
*   2.*样本扇出*:使用仅路径映射公开多个服务
*   3.*基于名称的虚拟主机*:使用域和路径映射

13.至少在 GKE 上，旋转出 L7 层 HTTP 负载平衡器，因此是非协议不可知的。

14.许多可用的入口控制器:谷歌云负载平衡器、Nginx、Contour、Istio 等等。

## 入口控制器

1.  入口控制器监听 Kubernetes API 获取*入口资源*，然后处理与之匹配的请求。
2.  技术上可以是任何能够反向代理的系统，但最常见的是 Nginx。
3.  Nginx 控制器需要一个后端。其他控制器*可能*不需要。
4.  没有规则的入口将所有流量发送到单个默认后端。
5.  默认后端通常是入口控制器的一个配置选项，不在入口资源中指定

## 服务发现

以下是可以提供服务发现的两种方式:

1.  使用环境变量
2.  DNS(推荐)

## 环境变量—用于服务发现

1.  kubelet 以{SVCNAME} *{SOME_NAME}的形式公开环境变量。例如，对于集群 ip 为 _10.0.0.11* 的 redis 服务:

```
REDIS_MASTER_SERVICE_HOST=10.0.0.11
REDIS_MASTER_SERVICE_PORT=6379
REDIS_MASTER_PORT=tcp://10.0.0.11:6379
REDIS_MASTER_PORT_6379_TCP=tcp://10.0.0.11:6379
REDIS_MASTER_PORT_6379_TCP_PROTO=tcp
REDIS_MASTER_PORT_6379_TCP_PORT=6379
REDIS_MASTER_PORT_6379_TCP_ADDR=10.0.0.11
```

## DNS —用于服务发现

1.  这是一个集群附件。
2.  DNS 服务器*为新的*服务*监视 Kubernetes API* ，并为每个服务创建一组 DNS 记录。
3.  对于命名空间 *my-ns* 中的服务 *my-service* ，为 *my-service.my-ns* 创建一个 DNS 记录。
4.  如果 pod 属于同一个名称空间，则不需要指定名称空间。
5.  Kubernetes DNS 服务器是访问 ExternalName 类型服务的唯一方式。

## 端点

1.  一个**端点**是一个*REST API 端点*的面向对象表示，它被填充在 Kubernates API 服务器上。因此，就 Kubernetes 而言,*端点*是访问其资源(如 Pod)的方式，即“端点”后面的资源。
2.  包含**端点子集**数组。
3.  *端点子集*是一组具有一组公共端口的地址。扩展的端点集是地址 x 端口的*笛卡尔乘积。*
4.  **endpoint subset 的 EndpointAddress** 可能**不是**是环回(127.0.0.0/8)、链路本地(169.254.0.0/16)或链路本地组播(224.0.0.0/24)。
5.  IPv6 也被接受，但并非所有平台都完全支持。
6.  服务的选择器被连续评估，结果被*发送到一个端点对象*
7.  当一个 Pod 终止时，它会自动从端点中删除，并且与服务选择器匹配的新 Pod 会自动添加到端点中。
8.  *端点*跟踪服务向其发送流量的对象的 IP 地址。
9.  并且通过保持服务和端点的名称相同，端点可以与服务松散耦合。详情请见[https://kubernetes . io/docs/concepts/services-networking/service/# services-without-selectors](https://kubernetes.io/docs/concepts/services-networking/service/#services-without-selectors)。
10.  如果没有提到选择器属性(是一个服务)，则不会创建端点对象。

## Kube 代理

1.  它是一个运行在每个工作节点上的特殊守护程序(应用程序)。
2.  可以在两种模式下运行，可通过`--proxy-mode`命令行开关进行配置:

*   1.*用户空间*
*   2. *iptables*

3.为了获得更高的吞吐量和更好的延迟，请使用 *iptables* 代理模式。

*4。未准备好 IPv6。*

5.维护网络规则并执行连接转发。

6.这有助于:

*   1.调试您的服务，或者出于某种原因从您的笔记本电脑直接连接到它们
*   2.允许内部流量、显示内部仪表盘等。

## Kube dns

1.  这允许直接使用名称访问 K8s 服务，而不是使用 *VIP:PORT* 的组合。
2.  当您使用 kube-dns 时，K8s *将*特定的名称服务查找配置注入到新的 pods 中，允许您查询集群中的 dns 记录。
3.  kube-dns 创建一个*内部集群 dns 区域*，用于 DNS 和服务发现。这意味着我们可以通过服务名直接从 pod 内部访问服务。示例:

```
curl -I nginx-svc:8080
```

4.您可以使用以下命令查看 kube-dns 设置的节点 dns 配置:

```
cat /etc/resolv.conf
> search default.svc.cluster.local svc.cluster.local cluster.local google.internal c.kube-blog.internal
  nameserver 10.32.0.10
  options ndots:5
```

5.如果服务是在默认名称空间中创建的，也可以使用*集群内部 DNS 名称*来访问它:

```
curl -I nginx-svc.default.svc.cluster.local:8080
```

## Etcd

1.  这是一个一致且高度可用的键值存储，用作 kubernetes 所有集群数据的后备存储。
2.  确保始终为您的 kubernetes 集群的 etcd 数据制定备份计划。
3.  所有数据都作为注册表保存在 etc 中。示例:

```
/registry/services
/registry/events
/registry/secrets
/registry/minions
```

4.以下命令可用于查询 etcd 中的数据:

```
etcdctl --ca-file=/etc/etcd/ca.pem get /registry/services/endpoints/default/kubernetes
```

就这些了，各位\ *(ツ)* /

如果您在软件开发方面有任何建议或需要任何帮助，请给我发邮件至 [contact@nitinbansal.dev](https://mailto:contact@nitinbansal.dev/) 或在 [twitter](https://twitter.com/9wxg1) 上给我发短信。

# 参考

1.[https://kubernetes . io/docs/tutorials/kubernetes-basics/expose/expose-intro/](https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/)
2 .[https://kubernetes . io/docs/reference/generated/kubectl/kubectl-commands](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)
3 .[https://kubernetes . io/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume/](https://kubernetes.io/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume/)
4 .[https://kubernetes . io/docs/concepts/workloads/pods/pod-life cycle/](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/)
5。[https://github . com/kubernetes/website/blob/master/content/en/examples/service/nginx-service . YAML](https://github.com/kubernetes/website/blob/master/content/en/examples/service/nginx-service.yaml)
6 .[https://github . com/kubernetes/website/blob/master/content/en/examples/service/networking/run-my-nginx . YAML](https://github.com/kubernetes/website/blob/master/content/en/examples/service/networking/run-my-nginx.yaml)
7 .[https://github . com/kubernetes/examples/blob/master/MySQL-WordPress-PD/WordPress-deployment . YAML](https://github.com/kubernetes/examples/blob/master/mysql-wordpress-pd/wordpress-deployment.yaml)
8。[https://stack overflow . com/questions/54923806/why-do-I-get-unbound-immediate-persistentvolumeclaims-on-minikube](https://stackoverflow.com/questions/54923806/why-do-i-get-unbound-immediate-persistentvolumeclaims-on-minikube)
9。[https://kubernetes . io/docs/tutorials/stateful-application/MySQL-WordPress-persistent-volume/](https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/)
10 .[https://github.com/containous/traefik](https://github.com/containous/traefik)11。[https://blog.openshift.com/kubernetes-services-by-example/](https://blog.openshift.com/kubernetes-services-by-example/)12。[http://container ops . org/2017/01/30/kubernetes-services-and-ingress-under-x-ray/](http://containerops.org/2017/01/30/kubernetes-services-and-ingress-under-x-ray/)
13 .[https://Matthew palm er . net/kubernetes-app-developer/articles/kubernetes-ingress-guide-nginx-example . html](https://matthewpalmer.net/kubernetes-app-developer/articles/kubernetes-ingress-guide-nginx-example.html)
14 .[https://medium . com/@ cash sclay/kubernetes-ingress-82aa 960 f 658 e](/@cashisclay/kubernetes-ingress-82aa960f658e)
15 .【https://github.com/kubernetes/kops/tree/master/addons 