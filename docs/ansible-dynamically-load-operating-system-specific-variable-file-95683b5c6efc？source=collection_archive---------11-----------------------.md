# Ansible 动态加载操作系统特定的变量文件

> 原文：<https://medium.com/nerd-for-tech/ansible-dynamically-load-operating-system-specific-variable-file-95683b5c6efc?source=collection_archive---------11----------------------->

![](img/dc59e66f43fe1f3eb2e26bc1c4d17023.png)

在这篇博客中，我将演示我如何创建一个可行的剧本*，它将* ***动态加载与被管理节点的操作系统同名的变量文件，并且仅仅通过使用变量名我们就可以配置我们的被管理节点。***

# 可行的目录结构。

下面是我的 ansible 工作空间的目录结构:

![](img/1782792ab0acf95004039ebac78a50f1.png)

可行的配置文件:

**$ vim ansible.cfg**

![](img/0e94282a48ae4b637aad31583ebc1f68.png)

在这个演示中，我使用 AWS cloud 启动了 3 个托管节点，它们具有 3 个不同的操作系统向导:

1.  Redhat Linux 8
2.  Ubuntu 20.04
3.  亚马逊 Linux 2

可行的库存文件:

**$ vim 库存**

![](img/4b41e082d2f12621f6126402be7058de.png)

让我们检查每个受管节点上的可回答事实:

**$ ansible -m 安装 amazon2 | grep 发行版**

![](img/fc6e23e51695a84e4720de318f8fdb66.png)

**$ ansible -m 安装 ubuntu| grep 发行版**

![](img/f0bb08cd88e2efdfaa27cc3f7a4fb1df.png)

**$ ansible -m 安装 redhat| grep 分发**

![](img/97cdea5910bfb20d2268ae041b213cad.png)

下一步是创建**变量文件**，其名称与我们将在剧本中使用的变量分布相同。

**$ vim vars/Redhat.yml**

![](img/d05a60dbb6602ce0b707c867c616c957.png)

**$ vim vars/Ubuntu.yml**

![](img/aef9f3f073232ccb044a283024f915cb.png)

**$ vim vars/Amazon.yml**

![](img/d05a60dbb6602ce0b707c867c616c957.png)

最后一步是创建一个**剧本**并运行剧本。在行动手册中，我们可以使用 vars_file 关键字和 ansible 事实来导入特定于操作系统的文件。

**$ vim setup.yml**

**$ ansi ble-playbook setup . yml**

![](img/0448a437e7311865705f3b0f44834aee.png)

让我们在浏览器上查看网页。

![](img/f30bb4d4d5f2b46ab577a33763f2daef.png)![](img/43c6ccf842972aef7611e9d731c82ef1.png)![](img/53e11067306cd3b26c54b27b602d1f0b.png)

***瞧，我们已经成功地创建了一个剧本，它动态地加载了与被管理节点的操作系统同名的变量文件，只需使用变量名，我们就可以配置我们的被管理节点。***

**GitHub:**[https://GitHub . com/mtabishk/ansi ble-playbooks/tree/main/load _ OS _ specific _ files](https://github.com/mtabishk/ansible-playbooks/tree/main/load_os_specific_files)

**这篇文章就到这里，希望你能从这里学到一些东西。**

## 谢谢大家的阅读。我很快会带一些新文章回来，谢谢！

穆罕默德·塔比什·坎戴

领英:[https://www.linkedin.com/in/mtabishk/](https://www.linkedin.com/in/mtabishk/)