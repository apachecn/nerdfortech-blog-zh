# 如何在 Docker 容器上配置 HTTPD 服务器和设置 Python 解释器并运行 Python 代码？

> 原文：<https://medium.com/nerd-for-tech/how-to-configure-httpd-server-on-the-docker-container-33584bea3101?source=collection_archive---------22----------------------->

![](img/d757da3b7a661eeb96000f1f0565b9f5.png)

**第一步:-** 首先，我们要安装 docker 软件。为此，我们必须配置 yum 报告列表。

要配置 yum repo，请使用以下命令:-

```
**cd /etc/yum.repo.d/
gedit docker.repo**
```

![](img/f70175d3a2be159c64caa99e9035d3da.png)![](img/7f1386dc5a1d737035cb239b1c2c8796.png)![](img/cc861fa8b3ad1497d9030f6fa4cda364.png)

现在，我们可以安装 docker 软件了。使用以下命令安装 docker 软件。

```
**yum install docker-ce — nobest -y**
```

![](img/460783243445921dc84878908f90ecd9.png)

现在，使用以下命令启动 docker 服务

```
**systemctl start docker**
```

我们还可以使用以下命令来检查 docker 的状态:-

```
**systemctl status docker**
```

![](img/05f5320e05ce3e61dd9c613c2f66190e.png)

现在，启用 docker 服务，以便 docker 服务在系统重新启动后保持运行。使用以下命令进行相同的操作:-

```
**systemctl enable docker**
```

![](img/c50732309745c76d4b1eb547d7646422.png)

接下来，使用以下命令提取 docker 映像

```
**docker pull *image-name***
```

![](img/68bb506e1c71f43b51d1c4dc235cffcd.png)

现在，从映像启动 docker 容器。使用以下命令进行相同的操作:-

```
**docker run -it — name webos centos**
```

![](img/d1f029694c3d94b80a60b4acdf64810f.png)

使用以下命令检查容器是否正在运行:-

```
**docker ps**
```

![](img/96ae89dd80ebae64d5e6416d956c968f.png)

默认情况下，我们拥有的 centos 映像不包含 ifconfig 命令。因此，我们可以安装提供 ifconfig 命令的软件。 ***net-tools*** 是提供 ifconfig 命令的软件。所以，我们可以用 yum 命令安装这个软件。

```
**yum install net-tools -y**
```

![](img/1cefe8c5301dd381951d133762d58e9b.png)

现在，我们可以检查容器的 IP。

![](img/9d30ede53ef08f46efbb2a07092f0407.png)

## 在 Docker 容器上配置 HTTPD 服务器

**步骤 2:-** 现在，安装 apache httpd 软件。使用以下命令进行相同的操作:-

```
**yum install httpd -y**
```

![](img/e3734e26086a9264a17ece660eaaebca.png)

默认情况下，httpd 软件从/var/www/html 目录中读取 HTML 文件。因此，我们必须将 HTML 文件放在/var/www/html 目录中。使用以下命令进行相同的操作:-

```
**cd /var/www/html
cat > index.html
cat index.html**
```

现在，启动 httpd 服务。默认情况下，systemctl 命令不存在。所以，我们可以使用/usr/sbin/httpd 文件，因为 systemctl 内部加载了这个文件来启动服务。

![](img/9208c325257b67513cb35493f4e1d911.png)

现在，我们可以从浏览器访问网页。

## 安装 Python 解释器&在 Docker 容器上运行 Python 代码

**第三步:-** 现在，我们必须安装运行 python 代码的 python 软件。相同操作的命令如下:-

```
**yum install python3** 
```

![](img/3861ac19a2770aeed6e2cc9a1bae6b1c.png)

接下来，创建一个文件，并在其中编写 python 代码。

![](img/45529478f7783db53bc9061b6d27828e.png)![](img/e36204e5e352f8740e6405ab5cae7ade.png)

现在，使用以下命令运行 python 代码:-

```
**python3 *file-name.py***
```

![](img/b869f6f612c3a3db1c7b73be02b56e7f.png)

***感谢阅读:)***