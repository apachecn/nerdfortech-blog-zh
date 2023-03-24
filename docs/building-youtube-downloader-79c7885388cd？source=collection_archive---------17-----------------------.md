# 构建 YouTube 下载器

> 原文：<https://medium.com/nerd-for-tech/building-youtube-downloader-79c7885388cd?source=collection_archive---------17----------------------->

![](img/7a495d13bc4a3d06413b1f2f7ac6fb0c.png)

Szabo Viktor 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

各位编码员好！

许多人想从 youtube 下载视频，但他们在下载视频或播放列表时遇到了问题。

因此，我建立了一个 [YouTube 下载器](https://github.com/varchasa/Download-YouTube-video)，它可以从 YouTube 下载任何视频或任何播放列表。

为此，我使用了 Python 编程语言，使用的库是 pytube。

让我们抢先一步，看看如何建立它。

首先，您必须下载库“pytube”

为了图书馆的安装—

```
pip install pytube
```

现在打开您的 python IDE 并执行以下代码—

1.  导入 pytube 库

```
from pytube import YouTube
```

2.将 URL 复制到某个变量中—

```
link = 'https://www.youtube.com/watch?v=Id8YjwJDQwg&t=839s'
```

有两种方法可以保存 youtube 视频。

一是在安装 python 的地方保存视频，二是在你想要的位置保存视频。

我将展示这两者的代码。

*   默认位置—

```
YouTube(link).streams.first().download()
```

*   期望位置—

```
save_location = 'E:\\'
YouTube(link).streams.first().download(save_location)
```

执行完以上几行后，转到您保存视频的位置。

> 瞧啊。您的视频会保存在那里😋。

使用同样的方法，你也可以下载任何播放列表。

> 完整的代码，你可以访问我的 GitHub 库[这里](https://github.com/varchasa/Download-YouTube-video)。

外部来源—

*   [Pytube](https://pytube.io/en/latest/)
*   [Pypi — pytube](https://pypi.org/project/pytube/)

> 觉得我的文章有用，鼓掌👏。

编码快乐！