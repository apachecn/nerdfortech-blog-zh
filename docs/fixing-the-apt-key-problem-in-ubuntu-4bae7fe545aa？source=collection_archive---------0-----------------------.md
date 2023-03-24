# 修复 Ubuntu 中的 apt-key 问题。

> 原文：<https://medium.com/nerd-for-tech/fixing-the-apt-key-problem-in-ubuntu-4bae7fe545aa?source=collection_archive---------0----------------------->

![](img/9c5fc61ee92dbf6a79bdf0902d8526d5.png)

凯利·西克玛在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

有时，特别是如果您从事开发工作，您可能需要向包管理器添加一个源代码。虽然你得到的很多. deb 包会自动解决这个问题，但是如果你不能直接下载 deb 文件，你可能会面临这个问题。

# 问题是

问题？apt-key 已被弃用，在 Ubuntu 22.04 和 Debian 11 之后将不再包含，但许多文档尚未针对新方法进行更新。apt-key 中唯一没有被否决的部分是 apt-key del，它有助于移除密钥以迁移到新的密钥存储方式。

因此，当您更新或安装软件包时，您可能已经注意到了这一点，但是您会得到以下警告:

> w:[https://FTP . PostgreSQL . org/pub/pgadmin/pgadmin 4/apt/jammy/dists/pgadmin 4/in release](https://ftp.postgresql.org/pub/pgadmin/pgadmin4/apt/jammy/dists/pgadmin4/InRelease):密钥存储在 legacy trusted . gpg Key ring(/etc/apt/trusted . gpg)中，详见 apt-key(8)中的弃用部分。

这是因为添加了如下的密钥:

```
$ wget — quiet -O — [https://www.postgresql.org/media/keys/ACCC4CF8.asc](https://www.postgresql.org/media/keys/ACCC4CF8.asc) | sudo apt-key add -
```

# 解决方案

为了解决当前的问题，我们需要首先从密匙环中删除您遇到错误的密匙。如果你没有得到一个错误，不要做任何事情，因为它没有坏。也就是说，删除密钥的第一步是通过以下操作获取其 id 的最后 4 位:

```
$ apt-key list
```

你不需要 sudo 来做这个。执行该命令后，您将看到如下输出:

```
 — — — — — — — — — — 
pub rsa4096 2017–06–22 [SC]
 xxxx xxxx xxxx xxxx xxxx xxxx xxxx xxxx C33A 7AFF
uid [ unknown] Pop OS (ISO Signing Key) <[info@system76.com](mailto:info@system76.com)>
sub rsa4096 2017–06–22 [E]/etc/apt/trusted.gpg.d/cubic-wizard-ubuntu-release.gpg
 — — — — — — — — — — — — — — — — — — — — — — — — — — — 
pub rsa4096 2015–11–05 [SC]
 xxxx xxxx xxxx xxxx xxxx xxxx xxxx xxxx B4F1 283B
uid [ unknown] Launchpad PPA for PJ Singh/etc/apt/trusted.gpg.d/microsoft.gpg
 — — — — — — — — — — — — — — — — — — 
```

id 是 10 段长的线，其中有 x(这些是数字，不是 x)

取最后四个数字并将它们连接在一起(因此对于 C33A 7AFF，它将是 C33A7AFF)。现在执行这个命令:

```
$ sudo apt-key del xxxxxxxx
```

同样，x 将是最后一个 8。

现在，最后一步是在新方法中重新添加键。它将与前面的类似，我们将只更改命令的按位 or 运算符(|)之后的部分。

```
$ wget -qO- [https://myrepo.example/myrepo.asc](https://myrepo.example/myrepo.asc) | sudo tee
 /etc/apt/trusted.gpg.d/myrepo.asc
```

密钥应该有 asc 描述。它也可能不允许您首先将它放在 trusted.gpg.d 中，所以如果最终在/etc/apt 中创建它，或者您的 cwd 将它移到 trusted.gpg.d 中:

```
$ sudo mv myrepo.asc /etc/apt/trusted.gpg.d
```

您的问题现在应该已经解决了。