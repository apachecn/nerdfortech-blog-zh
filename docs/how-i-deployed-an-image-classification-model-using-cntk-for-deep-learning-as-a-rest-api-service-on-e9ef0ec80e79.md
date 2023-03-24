# 我如何在 Azure 上部署一个使用 CNTK 进行深度学习的图像分类模型作为 REST API 服务

> 原文：<https://medium.com/nerd-for-tech/how-i-deployed-an-image-classification-model-using-cntk-for-deep-learning-as-a-rest-api-service-on-e9ef0ec80e79?source=collection_archive---------0----------------------->

# 警告:tldr

我最近写了一些关于 Azure 的文章，分享了我在新工作中获得的宝贵的开发知识，以及我在 1 月 18 日成功部署的第一个应用程序“捐赠范围预测”。本博客是我部署图像分类模型系列的第三部分。

AzureML Workbench CNTK 构建的模型可以回答诸如“图像中存在对象吗？”(其中对象可以是例如“狗”、“汽车”、“船”等。)以及更复杂的问题，如“该患者的视网膜扫描显示了哪种眼病？”— [计数器](https://github.com/Microsoft/CNTK)

![](img/540e8425ca6933cdbd048da407f2833d.png)

[本页](https://docs.microsoft.com/azure/machine-learning/preview/scenario-image-classification-using-cntk%5D(https://docs.microsoft.com/azure/machine-learning/preview/scenario-image-classification-using-cntk))解释了如何训练、测试和部署一个图像分类模型。然而，它没有解释一些事情，比如你需要一些 AzureML 依赖来运行这些脚本。默认情况下，某些 AzureML 功能在您的系统上不可用。

运行以下命令来安装它们:(记得在 Azure Workbench CLI 中运行)

```
pip install -I --index-url [https://azuremldownloads.azureedge.net/python-repository/preview](https://azuremldownloads.azureedge.net/python-repository/preview) --extra-index-url [https://pypi.python.org/simple](https://pypi.python.org/simple) azureml-requirements
```

此外，你需要一个 GPU 来运行和完善模型使用 SVM 和 DNN 在第二步！深度学习尤其需要相当大的硬件才能高效运行。只是一个小小的免责声明；你的系统越小，你就需要越多的时间来得到一个训练有素的模型。

![](img/c43cfc67bae691b306428fe90828785a.png)

预训练的 DNN 保持固定(也就是说，它不是精炼的)。我用的是 Azure 深度学习 VM。深度学习虚拟机(DLVM)是[数据科学虚拟机](http://aka.ms/dsvm) (DSVM)的特殊配置变体，可以更容易地使用基于 GPU 的 VM 实例来训练深度学习模型。

![](img/77a97ef2af8d5e8dea1e53b291c08b4c.png)

在创建项目时，我还指定了一个 git 项目 URL，这样即使我切换机器和 ide，项目也很容易获得。

最后一步，可以将训练好的系统发布为 REST API。好吧，我们开始吧。打开 Deploy Jupyter 笔记本并启动笔记本服务器。把内核改成<projectname>本地。检查第一个笔记本中的工作目录和部署文件夹。应该是这样的:</projectname>

参数:datasetName = fashionTexture
文件复制到部署文件夹:。/deploy
工作目录:

```
C:\Users\<AzureResource Name>\AppData\Local\Temp\2\azureml_runs\ImageClassification_1517210766172
```

要导航到此路径，请使用 cd 命令:

```
cd <Working directory>
cd deploy
```

注册环境提供者:

```
az provider register -n Microsoft.MachineLearningCompute 
az provider register -n Microsoft.ContainerRegistry
```

创建一个 ACS 集群(可能需要 10-20 分钟才能完成配置):

```
az ml env setup --cluster -l eastus2-n acsdeployment
```

指定用于“云”部署的目标上下文(这也将返回 Kubernetes 仪表板的 URL)。请注意，以下命令是在步骤 2 中运行“az ml env setup -cluster”命令后显示的:

```
az ml env set -g acsdeploymentrg -n acsdeployment
```

从本地部署切换到集群部署:

```
az ml env clusteraz ml service create realtime -c ../aml_config/conda_dependencies.yml -f deploymain.py -s deployserviceschema.json -n imgclassapi1 -v -r python -d helpers.py -d helpers_cntk.py -d utilities_CVbasic_v2.py -d utilities_general_v2.py -d svm.np -d lutId2Label.pickle --model -file cntk_fixed.model
```

Rest API 名称应该类似于 imgclassapi1。 <somename>- <someid>.eastus2\.
使用新的评分 URL 和通过运行以下命令获得的服务密钥更新脚本 6:</someid></somename>

```
az ml service keys realtime -i imgclassapi1.<somename>-<someid>.eastus2.
```

通过运行以下命令获取有关 Rest API 的信息:

```
az ml service usage realtime -i imgclassapi1
```

你会收到这样的东西

```
PrimaryKey: xxxxxxxxxxxxxxxxxxxxxxxxx
 SecondaryKey: xxxxxxxxxxxxxxxxxxxxxxxxxScoring URL:
[http://<ipaddress>/api/v1/service/imgclassapi1/score](http://<ipaddress>/api/v1/service/imgclassapi1/score)Headers:
 Content-Type: application/json
 Authorization: Bearer <key>
 (<key> can be found by running ‘az ml service keys realtime -i imgclassapi1.acsdeployment-<id>.eastus2’)
```

招摇网址:

```
http://<ipaddress>/api/v1/service/imgclassapi1/swagger.json
```

cmd 的用法:

```
az ml service run realtime -i imgclassapi1.acsdeployment-<id>.eastus2 -d "{\"input_df\": [{\"image base64 string\": \"iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAIAAAACDbGyAAAAFElEQVR4nGP8//8/AxJgYkAFpPIB6vYDBxf2tWQAAAAASUVORK5CYII=\"}]}"
```

PowerShell 的用法:

```
az ml service run realtime -i imgclassapi1.acsdeployment-<id>.eastus2 --% -d "{\"input_df\": [{\"image base64 string\": \"iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAIAAAACDbGyAAAAFElEQVR4nGP8//8/AxJgYkAFpPIB6vYDBxf2tWQAAAAASUVORK5CYII=\"}]}"
```

通过调用以下命令获取调试日志:

```
az ml service logs realtime -i imgclassapi1.acsdeployment-<id>.eastus2
```

**卷曲:**

![](img/1d1caad212168b49a4724e756921d516.png)

CURL 调用:(接口的 URL 只读属性返回当前服务工作客户端的 URL。)

cURL 是一个使用 URL 语法获取或发送文件的命令行工具。

```
curl -X POST -H "Content-Type:application/json" -H "Authorization:Bearer <key>" --data "{\"input_df\": [{\"image base64 string\": \"iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAIAAAACDbGyAAAAFElEQVR4nGP8//8/AxJgYkAFpPIB6vYDBxf2tWQAAAAASUVORK5CYII=\"}]}" http://<ipaddress>/api/v1/service/imgclassapi1/score
```

我喜欢 cURL 使用 PHP 的方式。PHP cURL 库绝对是异类。不像其他 PHP 库提供了大量的函数，PHP cURL 仅用四个函数就完成了其主要功能。

典型的 PHP cURL 用法遵循以下步骤。

1.  **curl_init** —初始化会话并返回一个 curl 句柄，该句柄可以传递给其他 cURL 函数。
2.  curl_opt —这是 curl 库的主要工具。这个函数被多次调用，并指定我们希望 cURL 库做什么。
3.  **curl_exec** —执行一个 curl 会话。
4.  **curl_close** —关闭当前 curl 会话。

**等价的 PHP 脚本是:**

```
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, "http://<ip>/api/v1/service/imgclassapi1/score");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, "{\"input_df\": [{\"image base64 string\": \"iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAIAAAACDbGyAAAAFElEQVR4nGP8//8/AxJgYkAFpPIB6vYDBxf2tWQAAAAASUVORK5CYII=\"}]}");
curl_setopt($ch, CURLOPT_POST, 1);

$headers = array();
$headers[] = "Content-Type: application/json";
$headers[] = "Authorization: Bearer <key>";
curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);

$result = curl_exec($ch);
if (curl_errno($ch)) {
    echo 'Error:' . curl_error($ch);
} 
curl_close ($ch);
```

**等价的 Python 脚本是:(如果你想用)**

```
import requests

headers = {
 'Content-Type': 'application/json',
 'Authorization': 'Bearer <key>',
}

data = [
 ('{"input_df": [{"image base64 string": "iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAIAAAACDbGyAAAAFElEQVR4nGP8//8/AxJgYkAFpPIB6vYDBxf2tWQAAAAASUVORK5CYII', '"}]}'),
]

response = requests.post('http://<ip>/api/v1/service/imgclassapi1/score', headers=headers, data=data)
```

总之，在 Azure 上部署服务时，创建 acsdeploymentrg 资源组时，创建了一个存储帐户、一个容器服务、一个负载平衡器、一个公共 IP 地址、一个可用性集、两个具有两个网络接口的代理虚拟机、一个具有一个网络接口的主虚拟机、一个网络安全组、一个路由表、一个虚拟网络、一个容器注册表和 25 个类似的东西。

下一个系列—在云上部署服务时到底发生了什么。

*原载于 2018 年 2 月 18 日*[*http://kuharanbhowmik.wordpress.com*](https://kuharanbhowmik.wordpress.com/2018/02/18/how-i-deployed-an-image-classification-model-using-cntk-for-deep-learning-as-rest-api-service-on-azure/)*。*