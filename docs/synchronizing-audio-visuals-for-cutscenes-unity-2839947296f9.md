# 同步过场动画的音频和视频| Unity

> 原文：<https://medium.com/nerd-for-tech/synchronizing-audio-visuals-for-cutscenes-unity-2839947296f9?source=collection_archive---------9----------------------->

## 统一指南

## 关于如何在 Unity 中使用时间轴同步视频和音频的快速指南

![](img/61a12b046a021c798dd02459a9ae53f5.png)

**目标**:使用 Unity 中的时间轴同步过场动画的音频和动画。

在上一篇文章中，我介绍了[如何用时间轴](/nerd-for-tech/handle-animations-with-timeline-unity-a49429fa7beb)处理动画。现在，是时候使用时间轴将动画和过场动画的音频与 Unity 同步了。

# 新过场动画

首先，让我们看看我们的新过场动画。这个过场动画展示了我们游戏的介绍。我们有演员的游戏对象，虚拟摄像机(用 Cinemachine)，小车轨道和音频。正如你在下一张 gif 中看到的，我们已经将摄像机放置到位，并为过场动画制作了动画。剩下唯一要做的就是添加和同步各自的音频:

![](img/8f6eaa691a0989d32f505c1df8421ee6.png)![](img/9b2b6798e17b366528d872872e189704.png)

正如我们在以前的帖子中所做的，我们使用了几个 previs 元素来构建过场动画:

![](img/8d40adee356c11184d3ebff9b72f6d80.png)![](img/cb4d86e90fcb8b13969393c55c1e0abc.png)![](img/9fc9104098dc32302debb43b39c56dad.png)![](img/b8ccc7c958290a23c67f8d36ef2f062b.png)

如果你不知道什么是 previs 元素以及它们在过场动画中是如何工作的，你可以查看这些旧帖子:

[](/nerd-for-tech/working-with-previs-elements-unity-89aa8103007c) [## 使用 previs 元素和 Unity

### 关于使用 previs 元素和 Unity 的快速回顾

medium.com](/nerd-for-tech/working-with-previs-elements-unity-89aa8103007c) [](https://fas444.medium.com/composing-a-cutscene-in-unity-330bc8b99d4c) [## 在 Unity 中合成过场动画

### 关于在 Unity 中构建过场动画的快速指南

fas444.medium.com](https://fas444.medium.com/composing-a-cutscene-in-unity-330bc8b99d4c) 

因此，为了同步过场动画的视觉和音频，我们将使用时间轴窗口。现在，我们只能控制摄像机和动画:

![](img/6b879bf6b4f5ec553d4e2d64100695e5.png)

要打开时间线窗口，您可以点击 ***窗口>时序>时间线***

为了开始在我们的过场动画中实现音频，让我们选择将包含音频的游戏对象，并为每个对象添加一个**音频源**组件。我们的过场动画中将播放两个音频:

*   背景音乐。
*   音频上的声音。

![](img/bf43056364538b95ee5b56c0a1f8e81d.png)![](img/8d9ac74e780ebec354d608b6fcacbb46.png)

现在，要添加音频源，让我们将各自的游戏对象拖动到时间线窗口，并选择 ***添加音轨*** :

![](img/de518fb6c84f53c233f3d25c2b5623f8.png)![](img/58e27d7de803ba74cdd96a9dd9a70663.png)![](img/a686583cd262cc1ebcd6e3158604e90c.png)

现在，要分配将在过场动画中播放的音频文件，让我们将相应的文件从项目文件夹中拖到相应的音频轨道中:

![](img/4462d0c4119b59f307c4d3764a38a712.png)![](img/4538d9e4303e54a088b702c06b2e8d84.png)![](img/227435acd3c6a36604c626193455eb1f.png)![](img/a4d51c1fd42d3f456b41cdb09c313925.png)

如果您想要避免时间线混乱，您可以创建一个新的轨道组，并将音频轨道拖到其中:

![](img/9b84b4f0c11fb070ec2484077f5bf726.png)

最后，为了同步音频和动画，我们可以使用白色箭头精确地选择秒或帧，并修改每个摄像机花费的时间。一个简单但有用的技巧是查看音量并修改动画以匹配时间线:

![](img/34f5ce642a74836716a675bf0831ec2c.png)

这样，我们就能够将动画(摄像机和演员)与过场动画中的音频同步:

![](img/a754bbe5c876ac512c869be139f35229.png)

就这样，我们可以在 Unity 中同步我们过场动画的音频和动画！:d .我会在下一篇文章中看到你，在那里我将展示如何通过简单的点击来移动我们的球员。

> *如果你想更多地了解我，欢迎登陆*[***LinkedIn***](https://www.linkedin.com/in/fas444/)**或访问我的* [***网站***](http://fernandoalcasan.com/) *:D**