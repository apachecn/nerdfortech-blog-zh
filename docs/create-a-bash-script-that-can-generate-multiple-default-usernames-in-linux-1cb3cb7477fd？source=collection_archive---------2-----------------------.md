# 创建一个可以在 Linux 中生成多个默认用户名的 Bash 脚本

> 原文：<https://medium.com/nerd-for-tech/create-a-bash-script-that-can-generate-multiple-default-usernames-in-linux-1cb3cb7477fd?source=collection_archive---------2----------------------->

![](img/76c48b69617fcad24bc29f8254720a97.png)

> -这需要在 Linux 终端上完成

![](img/28104b6b7adbc1cfb62bf8cd1bdc1f10.png)

**我在 AWS 中使用一个预配的 EC2 实例**

📝让我们创建一个文件夹来存储所有内容。

```
$ mkdir namescript
```

![](img/59209e39a343358e2cb93b99552f6289.png)

📝访问您的新文件夹

```
$ cd namescript
```

![](img/bcfd94b5268dbd9ccdc39cb68139390b.png)

📝创建一个 shell 文件，并使用 vim 编辑该文件

```
namescript$ vim usernames.sh
```

![](img/bc85aeb7a3f8b0adc8935cca00f2ceae.png)![](img/43c1c15af415726a47ec1d42a106fdd8.png)

输入 VIM 命令后，您应该会看到这个屏幕

📝按下 Insert 键，确认您的 VIM 已准备好进行编辑。

![](img/8253363bd9618df054481118a210ba30.png)

📝从输入一个 shebang 开始

```
#!/bin/bash
```

> **什么是舍邦？**
> 
> shebang 是脚本文件中的一个特殊字符序列，它指定应该调用哪个程序来运行脚本。shebang 总是在文件的第一行，由字符 **#！**后跟解释程序的路径。

![](img/204c732ecf1edddd6d889ec2beb2cd0a.png)

📝显示带有提示输入的消息

```
echo "Please input a new user below:"
read -p 'Username: ' usrname
```

![](img/9ecb43af907e492855938ccdfc813fc3.png)

> echo 命令将显示消息，read 命令和 will '-p '将提供输入提示。usrname 是代表输入的变量。

📝如果用户名存在，创建一个拒绝条件，否则继续添加用户名。

![](img/b0aa3c5b107e8a3de0192922250b8e74.png)

📝按 ESC 键并键入“:wq”

![](img/68afa580bc7aa032dfd92c76f2f4504b.png)

> “w”将写入更改,“q”将退出 VIM

![](img/385fcc094bcb513ace7fe8cced324d83.png)

📝使 usernames.sh 成为可执行文件

```
namescript$ sudo chmod +x usernames.sh
```

![](img/9e29f52b86b4238756f81273b0da3842.png)

> 要运行 usernames.sh，它需要是一个可执行文件

📝运行代码

```
namescript$ sudo ./usernames.sh
```

![](img/f39c4c54c8a6d81832cc811439b42260.png)

📝创建两个用户名并按回车键

![](img/df4ebeacefce9df787e1fb59393e8a5c.png)

📝让我们确认用户名已经创建

```
namescript$ tail /etc/passwd
```

![](img/b5f9f078d4bac2a15e9c04dc858eb2f8.png)![](img/bfdfdeb751f2331e2f4b5c89b8456532.png)

让我们试一试我们是否能输入相同的用户名

📝运行代码并输入相同的用户名

![](img/5c6ae1d81841601c58aa8d1316869c57.png)![](img/bb514ecdeb7785cc38d93f40519d6550.png)

正如你所见，代码不会输入相同的用户名超过一次。

我希望您喜欢这个 bash 脚本教程

[点击这里获取 Github 中 Bash 脚本的副本](https://github.com/HerbyJ3/Mybashscripts/blob/main/multipleusernamescript.sh)

让我们保持联系
[https://www.linkedin/in/herby-jeanty](https://www.linkedin/in/herby-jeanty)