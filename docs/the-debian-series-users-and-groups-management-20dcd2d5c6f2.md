# Debian 系列:用户和组管理

> 原文：<https://medium.com/nerd-for-tech/the-debian-series-users-and-groups-management-20dcd2d5c6f2?source=collection_archive---------0----------------------->

我想创作这个简短的故事，用简单的方式解释如何正确地管理用户和操作系统组及其权限。事实上，这不是代码的意图，但某些片段将与每个解释放在一起。代码总是必要的。

你可能会想，为什么 **Debian** ？有很多 Linux 发行版**存在。很多人使用 Ubuntu(非常流行)、Linux Mint，其他人喜欢使用基于 RedHat 风格的操作系统，如 CentOS 或 Fedora，甚至其他用户更喜欢专用的操作系统，例如，infosec 从业者和专业人员通常使用 Slax、Parrot 或最著名的 Kali Linux。**

Debian 是目前最大的 Linux 发行版之一，被全世界的系统管理员(相关)和程序员广泛使用。 **Debian** 基于 **Ubuntu** ，使用一个名为 **APT** 的包管理器，包格式为 **.deb.**

关于 **dpkg** 、 **apt** 、 **aptitude** 、 **tasksel** 等包管理器，我会在本系列的其他故事中进行更详细的讲解，敬请期待。

为了使事情更简短，有许多与 Linux、GNU 基金会、内核等相关的历史和背景信息。如果你用谷歌搜索，在一堆点击中你会有更好更完整的信息，所以我会跳过这个。

所有的练习都是在一个带有 GNOME 2 图形环境的旧 Debian 7 操作系统上完成的。Debian 7 实际上是一个不太老的操作系统，事实上，下面所有的例子都可以很好地与最新的 Debian 版本兼容。

在今天的节目中，我将首先谈论最基本的东西，用户和组。在继续之前，我们需要明确的是，在全新安装时，我们设置了一个 **su** 用户，代表**超级用户**，也称为 **root。**

## 关键文件

在 Debian 实例中，我们有两个对用户管理很重要的文件，它们负责根据你的兴趣列出和指定用户的目录、密码和权限。

第一个是`/etc/passwd`,它基本上记录了所有系统用户的信息，因为显而易见的原因，在全新安装中，root 用户是第一个列出来的用户，但是这里显示了更多的信息。这里有一个简单的例子。

在终端上使用`su`登录后，您可以执行以下操作

`cat /etc/passwd`

您将看到如下内容:

```
root:x:0:0:root:/root:/bin/bash
david:x:1000:1000:david,,,:/home/david:/bin/bash
mongodb:x:122:65534::/home/mongodb:/usr/sbin/nologin
```

每一行代表系统中的一个用户，有许多参数代表每个用户的一些东西，下面我将以我的用户`david`为例列出，我们有以下信息:

```
:x (shadow file, password storage)
:1000 (created ID user, starting from 1000)
:1000 (created group user, starting from 1000)
:david (description name)
:/home/david (user directory folder)
:/bin/bash (user console type)
```

另一个密钥文件是`/etc/shadow`,它处理密码加密和处理用户相关安全访问的规则等等。这个文件有一个类似于`/etc/passwd`的结构，但是有一堆不同的参数来完成 Debian OSs 中的用户身份

这里有一个以前使用同一个`david`用户的例子。

```
:david (name of the user - login)
:password (encrypted password)
:16016 (UNIX EPOCH date of the changed password since 1970) 
:0 (remaining days of current password before change)
:99999 (remaining days of password usage)
:7 (warning days to change password)
```

简单地说，前两个文件都将数据记录到系统中，以便处理用户登录和终端访问，传递系统上每个用户的密码配置和到期日期。

说了这么多，让我们转到创建用户的基本命令。

# 基本命令

在进入每个命令之前，让我给你一个提示:总是检查手册，有一个直接的命令，称之为`man [command]`

我们继续吧。

## useradd 命令

听起来，它在操作系统上创建了一个用户，但是是不可访问的(暂时)，你很快就会知道我在说什么。

`useradd david`

这将创建一个操作系统用户，但您将无法登录，因为没有相关的密码，也没有个人目录。通过使用`-d`命令标志，你可以提供一个目录路径名。

也就是说，当您键入`useradd -d /home/david david`时，将在`/home/david`目录中为`david`用户创建一个目录。并且用户将被添加到`/etc/passwd`列表中。

在输入过程中，您会看到终端中发生了什么，您会注意到一些文件是从一个`/etc/skel`目录中复制过来的，这最后一个包含了用于配置环境变量的文件和其他与用户 bash 会话相关的内容。这些文件被复制到`/home/[user]`并为每个会话进行管理。

如果您执行了`useradd david`命令，您将需要通过从`/etc/skel`目录本身复制来手动创建文件，然后使用`chown`命令更改权限。

这里有一个例子:

```
chown esanquiz:esanquiz /home/esanquiz
chown [name]:[group] [directory]
```

当您使用创建的用户第一次登录时，将会生成包含以下归档的文件。

对于这个复杂的场景，有一个简单的替代方法，那就是`adduser`命令。

## adduser 命令

这是一个 PERL 脚本，它通过向导创建一切。它与`useradd`非常相似，但基本上处理一切都更快。还处理密码创建和将`/etc/skel`文件复制到用户文件夹

`/etc/skel`目录包含`.bash_logout`、`.bashrc`和`.profile`文件，用户可以将这些文件配置为 bash 会话中的特定动作。

## passwd 命令

简单地说，您可以更改特定用户的密码。

通过在用户会话中执行`passwd`或者提供您想要更改的用户名。

像这样:`passwd david`你会按照提示，就是这样。在`/etc/passwd`文件上也发生了很多变化。

## usermod 命令

此命令帮助您指定特定用户的更改。您可以将其视为编辑用户规格的一种方式，以下是一个示例:

`usermod -s /bin/bash david`

在这个[链接](https://www.tecmint.com/usermod-command-examples/)上，你会找到更多关于 **usermod** 标志和可用配置的信息

## whoami 命令

此命令将提示您通过 CLI 登录到操作系统的当前用户

## userdel 命令

让我们使用`userdel`命令从操作系统中删除一个用户，这也删除了它在操作系统中的关系。`-r`标志是完全删除位于其`/home`目录中的用户文档的关键。如果不使用`-r`标志，它将仅在`/etc/passwd`和`/etc/shadow`文件中被删除。

你也可以通过 GUI 为操作系统生成和管理用户，如果你有可能通过 GUI 访问 Debian，并且你不熟悉前面解释的命令，不要担心。你可以进入系统工具->系统->用户帐户，然后开始工作。(可能需要 root 密码)。

到目前为止，从一个高层次和简单的概述来看，用户是非常重要的，但是组也有很高的优先级。在进入文件权限之前先说一下。

## 我们不会忘记团体

让我们稍微回到`/etc/passwd`，在每个用户的第三个参数上，你会看到**组 ID** ，当你创建一个用户时，**用户 ID** 和**组 ID** 是相同的。出于任何特殊原因，您有责任对每个用户进行正确分组。

让我们向系统添加一个组。

我们只是，`groupadd admins`这里的`admins`是这个组的名字。

在`/usr/sbin/directory`上，你会看到一堆其他的命令，这些命令代表了组可以做的其他功能。许多动作由名称本身描述，它与用户的功能保持相同的类比。比如`groupdel`或者`groupmod`

我们可以使用`cat /etc/group`列出所有组，其他命令也适用，如`less`、`tail`、`head`和`nano`。

此处列出的每个组的结构参考了以下信息:

`[group]:[file references]:[id]:[members]`

在将用户添加到组之前，有一个简单但有用的命令，即`groups`命令。如果您只输入`groups`，您将被提示登录用户所属的组。

现在，您可以像这样将用户添加到组中。

`usermod -g [group] [user]`

如果您使用`-g`标志，您将只引用一个组，如果您使用`-G`，您将向一个用户添加多个组。

当然，您可以使用`gnome-system-tools`包通过 GUI 处理组管理，也可以通过 **apt 或 aptitude 包管理器**随意下载

## 最后，我们来谈谈权限

当我们使用`ls -l`命令列出一些东西时。它将显示如下内容:

`drwxr-xr — x 2`

该字符串代表并决定了 el 对某些文件或目录的访问类型，基本上是 10 个线性位，其中第一位代表文件的**类型，其余 9 位与用户和组权限相关。**

如果一个文件没有任何权限，一个`-`将会替换它。

在下面的 9 个字符中，前 **3 位**是当前用户**的权限，**后 **3 位**代表所属组的权限，**最后 3 位**是系统中任何其他用户的权限。

用一种简单的方式，你可以用数字和字母两种方式来表示和分配权限，检查以下不言自明的关系表示法。

`r (read): 4 bits
w (write): 2 bits
x (execute): 1 bits`

要更改权限，您将使用`chmod`命令，在这里您可以使用这两种符号。可以用`chmod 755 [file]`之类的，或者做`chmod +rw [file]`之类的

前面提到的第一个字节代表列表的数据类型。如果你看到一个`-`(破折号)是一个文件，但是如果你看到一个`d`字母，它引用一个目录。

还有更多要谈的，这里一个巨大的缺失概念是`chown`命令，它基本上改变了文件所有者和组。

权限也适用于组。

解释更多的概念将是一个永无止境的故事，我将在本系列中更多地强调通用术语。在下一个故事中，我将谈论包管理器。

敬请关注。

快乐编码。