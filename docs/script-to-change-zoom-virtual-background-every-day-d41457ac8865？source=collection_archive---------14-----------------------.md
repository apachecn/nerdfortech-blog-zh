# 每天改变缩放虚拟背景的脚本

> 原文：<https://medium.com/nerd-for-tech/script-to-change-zoom-virtual-background-every-day-d41457ac8865?source=collection_archive---------14----------------------->

![](img/90a97107d443ff4ce60f5dc174a7915d.png)

在过去的几个月里，我发现我在徒步旅行时点击的图片有了新的用途。我开始用它们作为缩放虚拟背景。如果你和我一样，拍了很多照片，很难决定哪张好看。然后我决定在不同的日子里把它们都用上。遗憾的是，Zoom 没有这个内置功能。作为一名软件开发人员，我必须每天自动选择一个随机缩放的虚拟背景。

# 这个脚本是做什么的？

Zoom 确实有一个 API，我可以用来每天改变我的背景，但对于这项任务来说，这似乎太费力了。软件开发人员天生懒惰，对吗？

相反，我发现 Zoom 应用程序创建了一个在它的 preferences 文件夹中被选中的背景的副本，并引用它。该脚本只是接收一个随机文件，并用这个后台文件替换它。瞧啊。示出了不同的缩放虚拟背景。然后可以将它放入 cron 作业中，每天(或者您喜欢的任何频率)执行，以定期更改后台。

# 先决条件

我已经把所有我想用作背景的图片放在了我的用户目录的一个文件夹里。我的是在`/zoom/bgpictures/`，这是我在脚本中使用的，但是它是一个变量，你可以把它改成你想要的任何值。

接下来，我们在应用程序中设置一个缩放虚拟背景。哪一个并不重要。我们所需要的是 Zoom 将分配给这个背景的唯一 ID。目录中可能已经有一些文件，但是我们希望选择与我们刚刚上传的图像相对应的文件，以避免替换不同的文件。目录位于:`~/Library/Application Support/zoom.us/data/VirtualBkgnd_Custom`。文件名将类似于:`9WAE197F-90G2-4EL2-9M1F-AP784B4C2FAD`

# 剧本

我们将使用一个 bash 脚本来替换我们刚刚使用的图像。我会把这个脚本放在我为背景图片创建的缩放文件夹中。它可以被命名为任何你喜欢的名字，我把我的命名为`~/zoom/zoombg.sh`。脚本如下:

```
#!/bin/bash# The name of file that we copied before and will be replaced with
OG_BG="9WAE197F-90G2-4EL2-9M1F-AP784B4C2FAD";# Directory where Zoom keeps the backgrounds
ZOOM_DIR="/Users/$USER/Library/Application Support/zoom.us/data/VirtualBkgnd_Custom/";# Directory of our images
BGPATH="/Users/$USER/zoom/bgpictures/";# Picking a random file
NEW_BG=$(find "$BGPATH" -type f | sort -R | head -1);# Replacing the file
cp -R "$NEW_BG" "$ZOOM_DIR/$OG_BG";
```

如果为目录选择不同的路径，请在变量中进行更改。我们需要通过运行以下命令使该脚本可执行:

```
chmod 755 ~/zoom/zoombg.sh
```

# 随机更改缩放虚拟背景

剧本准备好了。我们需要做的就是将它放入 cron 作业中，这是一个内置的基于时间的作业调度程序。我们需要决定一个时间表，我们希望改变虚拟背景的缩放频率。我每天早上 9:55 做作业，因为我的会议从早上 10 点开始。

如果你是 cron jobs 的新手，你可以使用[生成器](https://corntab.com/)来帮助你。我使用:

```
55 9 * * 1-5 /Users/saranshkataria/zoom/zoombg.sh > /dev/null 2>&1
```

第一部分(55 9 * * 1–5)是您需要根据您的时间表定制的内容。第二部分只是告诉操作系统在那个时间点做什么。如果您为 bash 脚本选择了不同的位置，则需要更新路径。

要将其放入 cron 作业中，请键入

它将打开一个使用 vi 的编辑器。敲击键盘上的 I 键进入插入模式，粘贴到行中，按 Escape 后接“:wq”，回车保存并退出。

这就是全部，我们将有一个随机缩放虚拟背景每天/频率你选择！

如果您使用的是 Windows，可以对脚本进行相应的修改，使用起来应该相当简单。

如果你有任何问题，欢迎在下面留言！

*原载于 2021 年 8 月 17 日*[*【https://www.wisdomgeek.com】*](https://www.wisdomgeek.com/general/script-to-change-zoom-virtual-background-every-day/)*。*