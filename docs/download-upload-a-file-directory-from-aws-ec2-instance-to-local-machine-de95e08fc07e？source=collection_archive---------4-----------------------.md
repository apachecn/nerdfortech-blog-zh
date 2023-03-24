# 将文件/目录从 AWS EC2 实例下载/上传到本地计算机。

> 原文：<https://medium.com/nerd-for-tech/download-upload-a-file-directory-from-aws-ec2-instance-to-local-machine-de95e08fc07e?source=collection_archive---------4----------------------->

在这里，我们将了解如何在本地机器和 AWS EC2 实例之间交换文件。

**相关链接**

1.  [https://medium . com/@ Rahul 26021999/how-to-create-a-Ubuntu-20-04-server-on-AWS-ec2-elastic-cloud-computing-5b 423 b5 BF 635](/@rahul26021999/how-to-create-a-ubuntu-20-04-server-on-aws-ec2-elastic-cloud-computing-5b423b5bf635)
2.  [https://medium . com/@ Rahul 26021999/how-to-connect-to-ec2-instance-AWS-from-windows-Ubuntu-da 97 c 0 cc 9 c 8](/@rahul26021999/how-to-connect-to-ec2-instance-aws-from-windows-ubuntu-da97c0cc9c8)

# **从 AWS EC2 实例下载文件**

通过运行这个命令，您可以从 AWS EC2 实例下载一个文件到您的本地机器。

```
$ sudo scp -i path/to/mykey.pem user@ec2–49–252–8–93.ap-south-1.compute.amazonaws.com:path/remote/filename path/local/directory
```

# **从 AWS EC2 实例下载一个目录**

通过运行这个命令，您可以从 AWS EC2 实例下载一个文件夹到您的本地机器。

```
$ sudo scp -i path/to/mykey.pem -r user@ec2–49–252–8–93.ap-south-1.compute.amazonaws.com:path/remote/directory path/local/directory
```

**# Hack 1** :要从 AWS EC2 下载文件夹，可以先存档，然后像下载文件一样下载。

# 从本地将文件上传到 AWS EC2 实例

通过运行这个命令，您可以将文件从本地机器上传到 AWS EC2 实例。

```
$ sudo scp -i path/to/mykey.pem path/local/filename user@ec2–49–252–8–93.ap-south-1.compute.amazonaws.com:path/remote/directory
```

# 从本地将目录上传到 AWS EC2 实例

通过运行这个命令，您可以从本地机器上传一个文件夹到 AWS EC2 实例。

```
$ sudo scp -i path/to/mykey.pem -r path/local/directoryName user@ec2–49–252–8–93.ap-south-1.compute.amazonaws.com:path/remote/directory
```