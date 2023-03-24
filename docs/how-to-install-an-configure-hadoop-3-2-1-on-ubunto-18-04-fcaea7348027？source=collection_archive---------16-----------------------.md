# 如何在 Ubunto 18.04 上安装配置 Hadoop 3.2.1

> 原文：<https://medium.com/nerd-for-tech/how-to-install-an-configure-hadoop-3-2-1-on-ubunto-18-04-fcaea7348027?source=collection_archive---------16----------------------->

![](img/1f8863e7bfc65191eb7190ec98b784ab.png)

在本教程中，我们将在带有**单节点的 **ubunto** 上安装和配置 **hadoop** 。**

首先，我们需要:

**虚拟盒子**——[https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

**18.04**——[https://releases.ubuntu.com/18.04/](https://releases.ubuntu.com/18.04/)

之后，我们将在 virtual box 上安装 ubunto 来做这件事。

*   [https://brb.nci.nih.gov/seqtools/installUbuntu.html](https://brb.nci.nih.gov/seqtools/installUbuntu.html)

在我们的设置完成之后，我们将开始打开 ubunto 上的终端:

使用以下命令检查是否安装了 ja:

![](img/0dc91511cf636da4fd65ee263a8dd157.png)

如果没有安装 java，请键入以下命令:

![](img/66de675ef3cf934f0e772b938b53359e.png)![](img/6299644426f4248b861fd4d130234911.png)

之后，我们将创建 haddoop 组用户:

![](img/3c3d994055e42080f0b0560073492122.png)

之后，我们将创建一个名为 hadoopusr 的新用户:

![](img/9a0999d7a0df02ec66c5dfea094162b6.png)

接下来是以下命令:

![](img/1a5d1f272dd6d339a3123b144dab59bb.png)

**下一步是安装和配置 ssh。**

要安装 OpenSSH 服务器，只需键入以下命令:

![](img/488fbb2578b2f775b9f5ae99e50238d1.png)

Hadoop 使用 SSH 来访问节点，在这种情况下，一旦它是单个节点，我们就必须对其进行配置以访问本地主机。

让我们使用之前创建的 hadoop 用户进入:

![](img/883910221df4533ed3eeec8686e754da.png)

之后，我们将为 hadoopusr 生成一个公共 SSH 密钥:

![](img/32b377a28a3da34cf4537b1fdd99f910.png)

现在让我们将生成的密钥添加到 authorized_keys 列表中:

![](img/4e0a897beffa464d434745b5d4209c14.png)

让我们验证 SSH 配置:

![](img/e1c8875a2bb24d492a2e26514996930c.png)

要完成连接，只需键入:

![](img/3436e53ef972b5e1b400c73b82b6177f.png)

下一步是下载 do Hadoop 2.9.1。该文件将放在桌面上:

![](img/31a03df1cffc968b5bf7e60ca37edc17.png)

现在转到 Dektop 并解压缩 hadoop 文件夹:

```
cd /home/pedro/Desktop
sudo tar xvzf hadoop-2.9.1.tar.gz
```

然后将文件夹移动到目录 **/usr/local/hadoop:**

![](img/ebd047606129fe2c328adcb45286d190.png)

下一步是将“hadoop”文件夹的所有权分配给用户 hadupusr:

![](img/05182a3836fc35d9697e80a406fcd8ea.png)

现在让我们配置几个文件来配置 Apache Hadoop，首先在文件 **~/上定义变量。巴沙尔:**

![](img/f2c1a0bc63266df72fd528f1e9a6debc.png)

文档打开后，您应该在最后放置以下代码:

> 导出 HADOOP _ HOME =/usr/local/HADOOP
> 导出路径=$PATH:$HADOOP_HOME/bin
> 导出路径=$PATH:$HADOOP_HOME/sbin
> 导出 HADOOP _ map red _ HOME = $ HADOOP _ HOME
> 导出 HADOOP _ COMMON _ HOME = $ HADOOP _ HOME
> 导出 HADOOP _ HDFS _ HOME = $ HADOOP _ HOME
> 导出 YARN _ HOME = $ HADOOP _ HOME

现在执行以下代码:

![](img/82e7f0297320b5e1287b7f3c56778755.png)

现在让我们编辑文件 hadoop-env.sh 并定义变量 JAVA_HOME:

![](img/4a8b4f5b1ea4e81f7304a663d9f01bfc.png)![](img/7495b7a50638c82e134340d002724ebe.png)

之后，让我们配置 **core-site.xml:**

![](img/9ae860fd24de421edfed4df9e9cf5403.png)

在配置中添加以下代码:

![](img/fe4a9746808accd9d23b56803d2f688d.png)

下一个文件是 **hdfs-site.xml:**

![](img/9d2839ebe3f545bbb9e769f56c4563fc.png)![](img/adefea733aeaef7885f4a595fdd8d2d6.png)

下一个文件是 **Yarn-site.xml:**

![](img/c1fbe14a899ca6c4cacdce0b4d508c10.png)![](img/2685dfb81be5ea093841ece2ec3fcfb0.png)

下一个文件是 **mapred-site.xml.**

为此，我们复制一份文件 **mapred-site.xml.template** ，并将新文件命名为 **mapred-site.xml:**

![](img/121580db10436d9c4e0801b7b24e9c96.png)

创建文件后，让我们编辑它:

![](img/fb43726d22ae0cbd8342cea89f12c847.png)![](img/2353851af8539c4ba24e11468465c2e9.png)

现在让我们创建两个文件夹到 **namenode** 和 **datanode** :

![](img/7666764b1df7c8c40c507c039d7d97c4.png)

现在配置完成后，我们将把文件夹" **hadoop_space** "的所有权分配给用户 **hadoopusr** ，然后使用以下命令格式化 namenode:

![](img/88e459bc6108cc23c3734f0451206918.png)

现在让我们启动 hadoop 服务:

![](img/f97c160412b54aa3097e67b2f6a870e3.png)

为了查看所有服务是否都正确启动，我们运行了以下命令:

![](img/b0e896d561970b1b4f2fa301d7bb1d23.png)

要访问 Apache Hadoop 管理界面，只需打开浏览器并插入以下 url:

![](img/ac1a4d677b6185d0d03364e54b46307d.png)

就是这样，我们创建了一个单节点群集。