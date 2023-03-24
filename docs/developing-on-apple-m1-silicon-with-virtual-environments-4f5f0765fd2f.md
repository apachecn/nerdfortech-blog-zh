# 在苹果 M1 芯片上开发虚拟环境

> 原文：<https://medium.com/nerd-for-tech/developing-on-apple-m1-silicon-with-virtual-environments-4f5f0765fd2f?source=collection_archive---------0----------------------->

## 码头集装箱一路向下

![](img/6828bce9f508cf7e9fd1e679a226f062.png)

在我的文章[创建可重复开发环境](https://johnrofrano.medium.com/creating-reproducible-development-environments-fac8d6471f35)中，我展示了如何使用**流浪者**作为指挥者，使用 **VirtualBox** 作为虚拟机提供者，为我的学生和开发团队创建一致的开发环境。在苹果发布基于 ARM 架构的**苹果 M1 硅**芯片的 2020 年新款 MAC 电脑之前，这种方式一直运转良好。我选择了 VirtualBox，因为它是免费的，支持 Mac、Linux 和 Windows，但它只能在英特尔计算机上运行(x86_64 架构)，而苹果芯片是 ARM 基础的(aarch64 架构)。事实证明，我的 8 名学生在 2021 年春季学期带着苹果 M1 MAC 电脑出现，这意味着我所有基于 VirtualBox 的实验室都不会为他们工作。我需要另一个解决方案，而且要尽快。

## 苹果芯片上的 Docker

我听说 Docker 发布了一个在苹果芯片上运行的 Mac 版 Docker 桌面的技术预览版。我也想起来了，流浪汉是支持 Docker 这个提供商的。这意味着，vagger 可以控制 Docker 容器的供应，就像它控制 VirtualBox 供应虚拟机一样。这在某种程度上是 Docker 的一个独特用例，因为 Docker 的目的是提供一个一致的、不可变的运行时环境；不要被当成虚拟机。与所有技术一样，总会有超出最初意图的用例，我正要了解这个用例是否可行。

使用 Docker 作为开发环境的概念并不新鲜。其实如果你用的是 [Visual Studio 代码](https://code.visualstudio.com)，有一个叫[远程容器](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)的扩展可以让你在容器中开发。我计划在以后的文章中讨论这个问题。这里的区别是，通常你会使用`docker exec`命令在容器内建立一个外壳，但我需要这些容器的行为完全像一个虚拟机(VM ),因为我不希望学生或开发人员在苹果 M1 Mac 电脑上有不同于在英特尔 Mac 或 Windows 电脑上的体验。这意味着我必须让`ssh`与我的容器一起工作，因为这是 vagger 期望在 VM 内部建立一个 shell 的方式。

## 强制容器表现得像一个虚拟机

每个人都会告诉你永远不要在 Docker 容器中安装 ssh 守护进程(`sshd`)。其实我也是那种人。通常，当使用容器来提供不可变的运行时时，您不应该这样做。这样做将使它们变得不稳定和不安全。但这不一样。这些是开发容器。

还有另一个问题需要解决。Docker 容器通常不像虚拟机那样运行 linux *init* 系统，因为容器只打算运行一个进程，但是我需要安装的许多工具都需要一个 init 系统。因为我在使用 Ubuntu，所以我需要让`systemd` (Ubuntu 的 init 系统)作为第一个进程(PID 1)运行，以便让容器真正像虚拟机一样运行。说起来容易做起来难。

Docker 使得在容器中使用 systemd 变得非常困难，但是幸运的是， [Matthew Warman](http://mcwarman.github.io) 已经为 CentOS 解决了这一部分，并且对我使用 Ubuntu 非常有帮助。在他的指导下，我能够构建一个适合我的目的的 Ubuntu 20.04 Docker 映像。你可以使用[这个映像](https://hub.docker.com/r/rofrano/vagrant-provider)作为一个流浪的提供者来启动你自己的 Ubuntu Docker 容器，这些容器的行为就像虚拟机一样进行开发工作，与 init 系统竞争。

## 构建多架构 Docker 映像

最后一个目标是构建一个 Docker 映像，它可以在基于 Intel 和 ARM 的计算机上运行，这样我的学生和开发团队就可以在任一平台上使用它。这比我想象的要容易得多。我甚至能够在我的英特尔 Mac 上建立一个 Docker 映像，通过使用一个名为`buildx`的实验性 Docker 功能，它可以在英特尔和苹果硅 Mac 上运行。

这些是我用来为 amd64 和 arm64 架构构建我的流浪提供者 Docker 映像的命令。

首先你必须准备一个`buildx`生成器来使用:

```
$ export DOCKER_BUILDKIT=1
$ docker buildx create --use --name=qemu
$ docker buildx inspect --bootstrap
```

这就创建了一个`buildx`容器来为您工作。然后，当您构建映像时，您必须指定您想要构建的平台，并将它们推送到 docker hub(或其他注册表)以获得映像:

```
$ docker buildx build --platform linux/amd64,linux/arm64 --push --tag rofrano/vagrant-provider:ubuntu .
```

上面的命令使用 QEMU 构建一个可以在 Intel ( `amd64`)和 ARM ( `arm64`)上运行的多平台映像，并将其推送到我的 docker hub 帐户。如果您需要支持更多的平台，您可以将它们添加到用逗号分隔的`--platform`参数中。

## 最终解决方案

你可以在 Docker Hub 这里找到最终的 Docker 图片:[ROF rano/游民提供者](https://hub.docker.com/r/rofrano/vagrant-provider)。如果你打算和流浪者一起使用它，没有必要从 Docker Hub 上把它拉下来，因为那会在流浪者处理你的`Vagrantfile`时自动发生。

下面是一个多提供者`Vagrantfile`的例子，它可以使用 VirtualBox 或 Docker 作为带有 vagger 的提供者:

```
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.hostname = "ubuntu" # Provider for VirtualBox
  config.vm.provider :virtualbox do |vb|
    vb.memory = "1024"
    vb.cpus = 2
  end# Provider for Docker
  config.vm.provider :docker do |docker, override|
    override.vm.box = nil
    docker.image = "rofrano/vagrant-provider:ubuntu"
    docker.remains_running = true
    docker.has_ssh = true
    docker.privileged = true
    docker.volumes = ["/sys/fs/cgroup:/sys/fs/cgroup:rw"]
    docker.create_args = ["--cgroupns=host"]
  end # Provision Docker Engine and pull down PostgreSQL
  config.vm.provision :docker do |d|
    d.pull_images "postgres:alpine"
    d.run "postgres:alpine",
       args: "-d -p 5432:5432 -e POSTGRES_PASSWORD=postgres"
  endend
```

要用 Docker 代替 VirtualBox 使用 vagger，只需用`docker`指定`--provider`参数作为提供者:

```
$ vagrant up --provider=docker
```

这将打开一个容器，其行为类似于 Intel 或 ARM 计算机上的虚拟机。你可能已经注意到我在`Vagrantfile`中为 Docker 添加了一个供应块。是的，我们在 Docker 容器中运行 Docker，这样学生和开发人员可以在虚拟环境中使用 Docker，就像我们使用真实的虚拟机一样。在这个例子中，我们在 Docker 容器中运行 PostgreSQL 进行开发工作。

要进入这个开发环境，您可以使用与使用 VM 开发时相同的 vagger 命令:

```
$ vagrant ssh
```

这将`ssh`到 Docker 容器中，就像它是一个虚拟机一样。

## 结论

使用 Docker 作为 vagger 的提供商使我能够为我的学生和开发团队创建一致的开发环境，无论他们是使用英特尔计算机还是基于 ARM 的计算机，如新的苹果 M1 硅 MAC。让一个基本 Docker 映像使用`systemd`作为它的入口点允许容器表现得更像一个虚拟机。我们甚至在容器内部运行 Docker，这样我们就可以在开发过程中将其用于数据库和其他软件。其他解决方案允许在容器中进行开发，但我们将把它留到以后的文章中。