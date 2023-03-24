# 在 HOLOLENS 上部署 Unity 项目的分步指南

> 原文：<https://medium.com/nerd-for-tech/step-by-step-guide-to-deploy-an-unity-project-on-hololens-4ef31d879330?source=collection_archive---------2----------------------->

![](img/bc7e56793d21a63a7f21f134f520391e.png)

安装项目所需的编辑器

![](img/a676203d480895a9f0c1ad7283b1367b.png)

点击**安装编辑器**然后点击**下载文件**

![](img/5e3bd9a24211849ee6427ff3f57997cf.png)

对于当前项目，我们使用 **2019.4.18f1**

![](img/1a343bdc7ef0119c231e4d5664405c6c.png)

**构建项目:**打开项目后，选择文件下的**构建设置**选项

![](img/b38296d9b8407b423362cafff1e88b73.png)

选择我们想要构建的场景，取消选中剩余的并通过**通用 Windows 平台**进行最终构建

![](img/01e0f54ba3a6a7d72215fd5fd6e40b4d.png)

在左下角，我们可以看到**播放器设置**选项，在这里我们可以命名我们的应用程序和许可证，应用程序名称在**产品名称**中修改

![](img/a4f103471789a7b02611059241024fb2.png)

之后，点击开关平台，该平台转到右侧底部角落的**构建**选项

![](img/a197f647a3ad83e2e5006fd91f2e8710.png)

选择空文件夹并构建应用程序。一旦构建完成，我们就可以在该文件夹中看到 sln 扩展文件，在 Visual Studios 中打开该文件

![](img/7de5cc2e0941631d2dd4fdf7c3efe809.png)

正在创建**应用**包

![](img/d4b8c3ec662387b7484b300c1bbcbb01.png)![](img/6ecc74b5972ecead7cdf6bf15547fdb6.png)![](img/c9f2e2da1efbebe67f9331d0c1a77f6b.png)![](img/eb94e8f84a79cf6eefe51515815a12ce.png)![](img/b1f3e2b85c3286afabd77609d3f1b09e.png)

然后取消选中所有，留下 ARM64 与主和决定文件夹路径下载应用程序包并创建它。

![](img/7a2d89ccee6e92cfc0b02abd7ce19b3c.png)

完成后，压缩文件并分享给相关人员，以便在 HOLOLENS 中部署

**使用创建的应用包进行部署**

压缩创建的应用程序文件夹，并发送给相应的人，以便在 hololens 中安装应用程序

从**全息透镜和**浏览器中识别 **IP** 地址

一旦门户打开，将有两个选项来上传应用程序和应用程序证书，它们位于压缩文件夹中，解压缩并上传到各自的区域，然后应用程序将安装在各自的 hololens 中

**使用电缆直接在 HOLOLENS 中部署:**

![](img/aa8022600f57156c7016fc184dc4430f.png)

文件打开后，右键单击应用程序名称，并选择选项底部的属性

![](img/94d8ffdde0c68a2b5f78d027952214a7.png)![](img/df7f7cfd607b84cf5b8e66af03d35435.png)![](img/ea9f5e9cde0b6a7954687cad6ce7b298.png)![](img/c548028e04c7b406f0a4e66dced26390.png)

在顶部选择 **Master 和 ARM64** 架构，运行旁边的**远程 Windows 调试器**

![](img/530af95dda6efcbcb1fb0322b56a2fc3.png)