# 脚步声——或者说我是如何让 Unity 播放脚步声的

> 原文：<https://medium.com/nerd-for-tech/footstep-sound-or-how-i-tricked-unity-into-playing-footstep-sounds-24830f981562?source=collection_archive---------0----------------------->

**目标**:在达伦和警卫行走时，尽可能准确地播放脚步声

脚步的声音对于一个充分的**沉浸式**游戏来说是必不可少的。但是我们怎么玩呢？我们如何在每一步重复它们？

起初，我想尝试修改*行走控制器*脚本中的 di 行走路线。我想到了一个子协程，它可以循环播放声音，并对等待时间进行微调。