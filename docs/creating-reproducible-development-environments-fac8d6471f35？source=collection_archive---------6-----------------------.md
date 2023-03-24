# 创建可重复的开发环境

> 原文：<https://medium.com/nerd-for-tech/creating-reproducible-development-environments-fac8d6471f35?source=collection_archive---------6----------------------->

## 使用流浪者，VirtualBox 和 Docker

![](img/2a21fe02b7227e4e70df5fa4f529dcbf.png)

卡斯帕·卡米尔·鲁宾在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

开发团队面临一个古老的问题:如何确保团队中所有开发人员的开发环境是相同的？一个不太常见但同样重要的问题是，一个新的开发人员加入一个项目需要多长时间？几个小时？几天？如果我告诉你你可以在几分钟内完成这件事，那会怎么样？

## 在我的机器\_(ツ)_/上工作

开发环境通常是脆弱的。如果您只是将所有的软件直接安装在您的计算机上，那么您将面临一个项目的升级导致另一个项目停止工作的风险。更糟糕的是，您有时会从团队中的某个人那里得到可怕的“在我的机器上工作”的借口，但它在您的机器上不工作，或者它在您的机器上工作，但在生产中不工作。如果您可以使用虚拟机在您的笔记本电脑或台式机上毫不费力地自动创建一致的本地开发环境，会怎么样？

## 自动化一切:甚至是开发人员

小心翼翼、煞费苦心地创建文档，一步一步地指导如何通过剪切-粘贴来重新创建开发环境，这并不是非常敏捷的做法。记住，比起全面的文档，我们更看重工作软件。DevOps 的宗旨之一是以代码为的*基础设施的形式实现自动化。这种做法不是手工制作服务器和网络等基础设施，而是使用 Terraform 等配置工具和/或 Ansible、Chef 或 Puppet 等配置管理解决方案，使用代码而不是文档来自动部署和配置软件运行所需的基础设施。但是开发者呢？他们的环境不也应该自动化吗？*

我在纽约大学教授一门关于 DevOps 和敏捷方法学的研究生课程，这在学生使用 Windows PCs、MAC 甚至 Linux 笔记本电脑的课堂环境中有着特殊的含义，这门课程的要求可能与学生正在学习的另一门课程不同。他们应该能够同时参与两门课程的项目。这与开发人员同时参与多个项目没有什么不同。这个问题有各种特定于语言的解决方案，但是以一种语言不可知的方式来解决这个问题不是很好吗？

## 可口可乐大战:我再也受不了了

百事可乐——可口可乐，纸——塑料，Windows——Mac，选择太多，生产力却很低。为了在课堂上解决这个问题，我和我的团队一起工作，我使用了三部曲:vagger、VirtualBox 和 Docker，让我所有的开发人员和学生在 Linux 上开发。如果基础设施作为代码有利于生产，那么为什么不使用它来自动配置开发人员笔记本电脑上的环境呢？此外，让开发人员养成在无头服务器环境中工作的习惯也很好，因为当云中的生产工作负载行为不当时，他们在调试时会遇到这种情况。

T2 是一个开发者工具，用于通过命令行界面自动创建轻量级、可复制和可移植的虚拟环境。 **VirtualBox** 是一款运行在 macOS、Windows 和 Linux 上的免费虚拟机管理程序，可以在你的笔记本电脑或台式电脑上托管虚拟机。过去，我为我的开发团队创建了虚拟机，将它们导出为 OVA 文件，并且必须上传巨大的数千兆字节的 OVA 映像，他们必须下载这些映像才能使用。这要花我几个小时。当软件需要更新时，我必须创建一个新的映像并上传，而开发人员需要再次下载它，这需要花费更多的时间。有了 vagger，我只需要在我们的每个 git 存储库中放一个名为`Vagrantfile`的小文件，其他的事情就交给开发人员了。更新就像更新通过版本控制跟踪的文本文件一样简单。

## 从哪里开始？

首先，我让我的开发团队和学生安装两个软件:vagger 和 VirtualBox。就是这样！当然，他们需要像 Visual Studio Code 这样的程序员编辑器，但对于开发环境，他们只需要 vagger 和 VirtualBox。这意味着，新开发人员或学生的入职时间与安装 Vagrant 和 VirtualBox 的时间一样长，并且可以使用 Mac 上的 Homebrew 或 Windows 上的 Chocolatey 这两个简单的命令来自动完成:

在使用自制软件的 macOS 上:

```
$ brew install --cask vagrant
$ brew install --cask virtualbox
```

在使用 Chocolatey 的 Windows 上:

```
C:\> choco install vagrant
C:\> choco install virtualbox
```

当然，你需要安装 Mac 版的 Homebrew 或者 Windows 版的 Chocolatey，但是如果你没有的话，你会喜欢它们的，因为它们可以自动更新你的软件。依我看，每个开发人员都应该使用这些命令行工具或类似的工具来管理软件安装并提高他们的工作效率。如果你不想使用这些工具，你可以随时从各自的网站下载[流浪者](https://www.vagrantup.com/downloads)和 [VirtualBox](https://www.virtualbox.org/wiki/Downloads) 并手动安装。

## 流浪汉是傀儡师

**流浪者**提供了拉动控制一切的绳子的自动化。它用于快速一致地配置复杂的配置，并具有可重复性。 **VirtualBox** 为开发者提供虚拟机(VM ),而 **Docker** 处理所有的中间件，无需任何安装。我已经用这个三连胜很长一段时间了，取得了巨大的成功。不管你的笔记本电脑运行的是 Windows、macOS 还是 Linux。每个人都在使用一致的 Linux 环境，他们不会出错，因为这都是自动化的。没有混乱，没有大惊小怪，没有任何错误的版本。每个人都在完全相同的环境中工作。

一旦安装了 vagger 和 VirtualBox，为项目或实验室创建一个完整的开发环境就像以下命令一样简单:

```
$ git clone [https://github.com/rofrano/lab-vagrant.git](https://github.com/rofrano/lab-vagrant.git)
$ cd lab-vagrant
$ vagrant up
$ vagrant ssh
```

就是这样！一个项目需要安装什么软件并不重要，它对开发人员是隐藏的。不同的项目有不同的、有时是冲突的需求，这没有关系，这都是由虚拟机隔离的。开发人员需要的一切都已经被一致地、可重复地安装和配置了。您可以亲自尝试上面的命令。这是我在几个会议上运行的教程的存储库，比如这次在波士顿举行的 Lisa16 会议，主题是[让开发人员更高效地使用 vagger、VirtualBox 和 Docker](https://www.usenix.org/conference/lisa16/conference-program/presentation/rofrano)

要从头开始您自己的空开发环境，请为您的工作创建一个文件夹并使用:

```
$ vagrant init ubuntu/bionic64
$ vagrant up
$ vagrant ssh
```

这将使用框`ubuntu/bionic64`初始化一个新的`Vagrantfile`，并使用 Ubuntu 20.10 Bionic64 映像调出一个虚拟机。`Vagrantfile`是一个 ruby 文件，它包含了如何供应和配置虚拟机的指令。最初，它只包含用于示例代码的框，您可以取消注释或添加附加命令。你可以从流浪者[文档](https://www.vagrantup.com/docs/vagrantfile)中了解更多关于这些命令的信息。该框是开始时的虚拟机映像。流浪者有很多图片可以选择，你可以在[流浪者云](https://app.vagrantup.com/boxes/search)浏览

## 一个文件来统治他们所有人

真正的魔力来自于编辑`Vagrantfile`并添加命令来安装、配置和变出你自己的环境。您可以使用`shell`命令，但 vagger 也可以使用来自 Ansible、Chef 和 Puppet 等流行配置管理系统的文件。想一想。供应生产服务器的相同文件也可以配置开发人员的环境。这就是如何获得 12 因素应用程序[开发/生产奇偶校验](https://12factor.net/dev-prod-parity)。

下面是一个简单的`Vagrantfile`，它将使用`git`和一个使用 Docker 的 PostgreSQL 数据库创建一个 Python 3 开发环境。

```
Vagrant.configure(2) do |config|
  config.vm.box = “ubuntu/bionic64” config.vm.network “forwarded_port”, guest: 8080, host: 8080
  config.vm.network “private_network”, ip: “192.168.33.10” config.vm.provider “virtualbox” do |vb|
    vb.memory = “1024”
    vb.cpus = 2
  end if File.exists?(File.expand_path("~/.gitconfig"))
    config.vm.provision "file", source: "~/.gitconfig", destination: "~/.gitconfig"
  end config.vm.provision “shell”, inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y git python3-dev python3-pip
  SHELL config.vm.provision :docker do |d|
    d.pull_images “postgres:alpine”
    d.run “postgres:alpine”,
      args: “-d — name postgres -p 5432:5432 -v  postgresql:/var/lib/postgresql/data -e POSTGRES_PASSWORD=postgres”
  endend
```

这个`Vagrantfile`提供了一个 Ubuntu 20.10 虚拟机，将虚拟机上的端口`8080`转发到您主机上的`8080`，这样您就可以使用`localhost:8080`来访问您的应用程序。替换应用程序监听的任何端口。它还为虚拟机分配了一个 IP 地址，并分配了 1GB 的内存和 2 个 CPU。然后，它将您的`.gitconfig`文件复制到 VM 中，以便 git 知道您是谁，然后安装 git 和一个 Python 3 开发环境。最后，它使用 Docker 在 Docker 容器中提供 PostgreSQL 数据库。Docker 的支持是原生的，这就是为什么我们不需要在我们的笔记本电脑上下载和安装 Docker。我们从虚拟机内部使用 Docker，因此我们的每个 Docker 环境也相互隔离。

如果您需要更多软件，只需将命令添加到您的`Vagrantfile`中，然后使用:

```
$ vagrant reload --provision
```

这将再次运行配置块并重新安装软件。应该注意的是，一旦您配置了一个虚拟机，那么在您下次使用`vagrant up`时，vagger 将忽略配置块，因此现有的虚拟机会很快启动，因为所有的东西都已经安装好了。

每次 git repo 发生变化时，将`Vagrantfile`提交给 git repo 可以确保团队中的每个开发人员每次都能带来相同的一致环境。

当您想要降低环境时，只需使用:

```
$ vagrant halt
```

这将停止虚拟机。然后`vagrant up`会在你需要的时候再次启动。您也可以使用`vagrant suspend`和`vagrant resume`来保存当前运行状态，而不是重新启动。

当不再需要开发环境时，或者如果一切都出了问题，您可以使用以下命令删除虚拟机:

```
$ vagrant destroy
```

这就是基础设施作为代码的美妙之处。当某件事情出错了，并且“昨天有效，今天无效”时，你不必浪费时间去解决它。只需删除虚拟机，然后配置一个包含以下内容的新虚拟机:

```
$ vagrant destroy
$ vagrant up
```

这会给你一个新的开发环境，就像你刚开始的那一天一样清新。

## 我的文件怎么办？

这才是流浪者真正闪光的地方。现在你可能会想，“难道我不需要把我的文件复制到虚拟机上吗？”或者“如果我删除虚拟机，我不会丢失所有工作吗？”这两个问题的答案都是“不”。原因如下:

在挂载点`/vagrant`上的虚拟机中，vagger 共享你当前的工作目录。您的项目文件永远不会复制到虚拟机中。它们与虚拟机共享，因此您在虚拟机外部或虚拟机内部所做的任何更改都可以在两个地方看到，因为文件实际上只驻留在一个地方，即您的笔记本电脑或台式机上。

一旦你进入虚拟机，你可以在`/vagrant`文件夹下找到你的项目文件。在本例中，您可以从虚拟机中看到您在计算机上创建的`Vagrantfile`。

```
$ vagrant ssh
$ cd /vagrant
$ ls -l
-rw-r—r-- 1 vagrant vagrant  4715 Apr  7 12:52 Vagrantfile
```

因为这些文件是在虚拟机内部共享的，所以当从虚拟机内部执行程序时，您可以在计算机上使用您喜欢的开发编辑器(如 Visual Studio 代码)来编辑这些文件。这为您提供了两全其美的环境，一个运行代码的可移植开发环境，它类似于生产环境，使用您最喜欢的 GUI 编辑器既熟悉又方便。

## 结论

希望我已经为您的开发团队建立了使用基础设施作为代码解决方案的案例，例如 vagger，以便不仅自动化，而且拥有可重复的开发环境，消除“在我的机器上工作”综合症。当您考虑到每个开发人员花费在管理他们的开发环境上的时间时，生产率的提高是累积显著的。这也减少了新员工入职时或人们更换团队时的摩擦。

除了使用 VirtualBox 作为提供者，vagger 还支持 VMware、SoftLayer、Amazon AWS 和 Digital Ocean。这意味着您的开发人员也可以立即为自己构建基于云的开发环境。如果你使用 Visual Studio 代码[远程开发](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)扩展，你实际上可以在云虚拟机内部开发。我将把它留给另一篇博文。