# 加密冒险:保护您的笔记本电脑 Kubernetes 集群

> 原文：<https://medium.com/nerd-for-tech/adventures-in-encryption-securing-your-laptop-kubernetes-cluster-9e032bf77f3e?source=collection_archive---------11----------------------->

![](img/7f9c9a83f1aba171e993d81b1a7b73bd.png)

“是秘密吗？安全吗？”

*使用* [*证书管理器*](https://cert-manager.io/) 更好的安全性和更好的 web 开发

前端和后端开发人员越来越多地生活在 API 服务器、云托管和服务的世界中，所有这些都受到 TLS 证书和其他类型加密的保护。然而，许多开发人员工具使用的是普通的老式本地主机，传统上根本没有加密。

当您在这些开发环境中使用的 API 和服务受到 SSL 和 TLS 的保护时，这是有局限性的。饼干就不一样了。一些操作失败。您会在本地系统上遇到各种与安全相关的错误，这些错误是在笔记本电脑上工作所特有的。不修复与您的本地设置相关的错误，而不是您试图设计的东西，开发就已经够难了。

因此，趋势是在我们的开发环境中也使用适当的加密。如果您直接在运行于 localhost 上的开发服务器上进行开发，有一些很好的教程可以帮助您进行设置。Daksh Shah 的一篇文章值得一读，因为我们将在本文中使用这种方法:创建和信任我们的自签名证书颁发机构，并为我们的测试服务器创建服务器证书。

对于我们这些在 Docker 和 Kubernetes 中开发容器化应用的人来说，我们需要走得更远。这就是这篇文章的内容。

# 假设和前提条件

这应该不是你第一次学习使用证书设置服务器、使用 openssl 之类的工具或者配置 Kubernetes。我假设你至少玩过所有这些东西。

所以:

*   你至少知道一点如何使用 Kubernetes 和 docker，并且不会被在你的个人笔记本电脑上安装这些东西所吓倒。
*   我将很快地介绍 [PKI](https://en.wikipedia.org/wiki/Public_key_infrastructure) 和 [X.509](https://en.wikipedia.org/wiki/X.509) ，但是希望您能对什么是密钥和什么是证书有所了解。
*   你也知道 X.509 不是从未来来拯救你尚未出生的孩子，也不是不会爬楼梯的邪恶机器人。

![](img/b155d5ced956e4b0d23297e2c211b88d.png)

ED-209 不会创建您的证书。你同意吗？

*   对于我们的第一部分，我们将创建我们自己的“超级证书”，或如它所称的那样，一个“认证机构”，或“CA”。为什么我们需要自己的个人 CA？因为您使用一个 CA 证书来制作其他证书。正如我们将要看到的。

# 我的设置和教程所需的软件

我在本教程中描述的可以在 Linux、Windows 或我在这里做的 MacOS 上完成。作为参考，我的设置如下:

1.  MacBook Pro 上的 MacOS Mojave (10.14)
2.  Mac 版 Docker 桌面，已激活 Kubernetes。你也可以用 minikube 来做这件事，尽管最近我发现 DDFM 的环境更稳定了，也不太可能让我变得不稳定。
3.  我已经安装了 Nginx 控制器，它可以让我的容器和 pod 在它们运行的任何端口上运行，但是我的 web 应用程序可以在端口 80 和 443 上使用，就像上帝打算的那样。在本教程中，您需要安装该软件。[这里是请示](https://kubernetes.github.io/ingress-nginx/deploy/)的好地方。
4.  这种设置的真正神奇之处在于使用了名为 c [ert-manager](https://cert-manager.io/) 的软件，一旦我们设置好了，它就会自动为我们创建服务器证书。你还应该安装那个。
5.  为了有一些我们可以看的内容，你会想要下载我的样本文件。您将在本教程的后面找到安装说明。

# 制作 CA

这一节在很大程度上归功于[cert-manager 的赞助公司 JetStack](https://cert-manager-munnerz.readthedocs.io/en/latest/tasks/issuers/setup-ca.html) 的本教程，在一个关于在其产品中使用自签名证书的章节中。一旦你完成了这个教程，给它一个阅读。

要创建您自己的证书颁发机构，您需要在您的系统上安装 openssl。为了让你开始，我在 GitHub 上有[示例文件可以用作模板。](https://github.com/torenware/lets-encrypt-files)

为了使这个过程更容易无误地重复，我们创建了一个配置文件来创建 CA。此配置仅用于创建我们的 CA；我们不会直接签署服务器证书。这将是证书管理器的工作。

我们将调用配置文件`cert-defaults.csr.cnf`，它将填充一些我们想要嵌入到证书中的基本信息。这里有一个例子:当然，您需要更改设置以适合您自己或您的公司:

```
**[req]**
days                   = 180
serial                 = 1
distinguished_name     = req_distinguished_name
x509_extensions        = v3_ca**[req_distinguished_name]**
countryName_default            = US
stateOrProvinceName_default    = CA
localityName_default           = Our Fair City
organizationName_default       = SomeCompany
commonName                     = Common Name
commonName_default             = SomeCompanyCACert
0.emailAddress_default         = moe@somecompany.com
1.emailAddress_default         = curly@somecompany.com
2.emailAddress_default         = larry@somecompany.com**[ v3_ca ]**
# The extentions to add to a self-signed cert
# @see also https://www.freecodecamp.org/news/how-to-get-https-working-on-your-local-development-environment-in-5-minutes-7af615770eec/
subjectKeyIdentifier   = hash
authorityKeyIdentifier = keyid:always,issuer:always
basicConstraints       = critical,CA:TRUE
keyUsage               = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment, keyAgreement, keyCertSign
subjectAltName         = @alt_names
issuerAltName          = issuer:copy**[alt_names]**
DNS.1 = localhost
```

现在我们有了配置文件，让我们创建我们的 CA 证书:

```
openssl req -x509 -days 1024 -newkey rsa:4096 \
    -extensions v3_ca \
    -config ca-self-signed.cnf \
    -keyout our-ca.key \
    -out our-ca.crt
```

这将生成我们的密钥并创建自签名证书文件，该文件现在存储在我们的-ca.crt 中。因为这是一个 ca，所以保证密钥的安全是非常重要的。该命令要求我们输入一个密码，该密码应该很长并且很难猜到。我将我的存储在 1Password 中；你应该用你最喜欢的密码工具做类似的事情。当我们将密钥导入到 Kubernetes 时，您将需要它。

```
Generating a 4096 bit RSA private key
.......................................++
...........................++
writing new private key to 'our-ca.key'
Enter PEM pass phrase:
```

接下来，您需要告诉您的计算机信任您刚刚创建的 CA 证书。对于 Mac 和 Windows，以及您想要使用的浏览器，该过程是不同的。以下是一些适用于 Mac 和 Windows 以及 Chrome 和 Firefox 的参考资料。

*   [将证书导入 Firefox](https://javorszky.co.uk/2019/11/06/get-firefox-to-trust-your-self-signed-certificates/)
*   [将证书导入 Windows 上的 Chrome](https://www.pico.net/kb/how-do-you-get-chrome-to-accept-a-self-signed-certificate)
*   [将证书导入 Windows 和 MacOS 上的 Firefox 和 Chrome 技术版本](https://www.techrepublic.com/article/how-to-add-a-trusted-certificate-authority-certificate-to-chrome-and-firefox/)

# 配置 Kubernetes 和 Nginx

现在，您已经在笔记本电脑上设置了自签名 CA。创建自己的证书并不困难，如果你只是建立一个网络服务器，这就是你需要做的。Daksh Shah 的文章是你的答案。但是为了保护 minikube 或 Docker 桌面上的 Kubernetes 集群，您需要配置 Kubernetes 来使用您的 CA。

要检查我们的进度，请设置您的系统，以便它可以与应用程序/目录中的 [my Github 模板文件一起工作。这些文件将为名为“dev.somecompany.com”的域创建一个非常简单的部署以及入口定义。因此入口定义将起作用，在/etc/hosts 文件中创建一行，如下所示:](https://github.com/torenware/lets-encrypt-files/tree/master/app)

```
127.0.0.1 dev.somecompany.com
```

(注意:如果你使用的是 minikube 而不是 Docker Desktop，你应该用`minikube ip` 获得你的 kube 的 IP 地址，并使用该 IP 地址而不是 127.0.01)

设置/etc/hosts 后，按照指示应用文件:

```
cd app
kubectl apply -f .
```

我们现在准备开始构建 Kubernetes 对象，这将使事情进展顺利。在这里，顺序很重要:

1.  将 ca 证书数据作为 tls 机密加载到 k8s 中。注意，我正在解密 CA 的秘密密钥，将它加载到 k8s，并在完成后立即删除解密的密钥:

```
openssl rsa -in ../cacert/our-ca.key -out temp-decrypted.key

kubectl create secret tls loaded-ca-key-pair \
   --cert=../cacert/our-ca.crt \
   --key=temp-decrypted.key \
   --namespace=default

rm temp-decrypted.key
```

2.现在创建 cert-manager 所谓的“发行者”。这是我们希望 cert-manager 如何为我们创建证书的定义，提供 CM 需要的信息。注意，在这种情况下，发行者只是指向我们刚刚在`loaded-ca-key-pair`中上传的 CA 数据。我们将把它命名为`issuer.yaml`

```
apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: ca-cert-issuer
spec:
  ca:
    secretName: loaded-ca-key-pair
```

我们将`issuer.yaml`导入到 Kubernetes 中

```
kubectl apply -f issuer.yaml
```

此颁发者将只在当前命名空间中工作；如果您需要一个涵盖所有名称空间的发布者，那么您应该创建一个 ClusterIssuer。语法是相同的，只有“种类”不同。

3.最后，我们需要对入口资源进行一些修改。我们需要添加`cert-manager.io/issuer`注释来指向我们的发行者:

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: ingress-somecompany-service
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/use-regex: 'true'
    **cert-manager.io/issuer: ca-cert-issuer**
```

我们还需要在入口资源的规范中添加一个位，以告诉 cert-manager 将我们的秘密放在哪里。当应用入口资源时，将自动创建此秘密:

```
spec:
  **tls:
  - hosts:
      - dev.somecompany.com
     secretName: dev-sc-secret**
  rules:
  - host: dev.somecompany.com
## and so on!
```

该证书以现在创建的秘密`dev-sc-secret`命名，该秘密是 cert-manager 实际上隐藏我们新创建的证书和密钥的地方，我们的入口资源现在将使用它。恭喜你！您应该能够浏览到 https://dev.somecompany.com 的[并从应用程序/目录中看到部署发布的网页。如果你检查页面使用的证书，你会发现你的浏览器要么非常满意(Chrome)，要么基本满意(Firefox)。您已经获得了对本地群集的安全访问。](https://dev.somecompany.com)

— —

现在，您已经大致了解了如何在 Kubernetes 集群上配置 cert-manager，以及如何使用 X.509 证书保护您的本地开发集群。下一步是了解如何使用这些知识来加密公共 Kubernetes 集群中的免费证书。我们将把它留到第二部分。

*Rob Thorne 是一名全栈开发人员，最近他做了越来越多的开发工作。他可以被雇佣。你可以在 Twitter 上找到 Rob 的@torenware。*