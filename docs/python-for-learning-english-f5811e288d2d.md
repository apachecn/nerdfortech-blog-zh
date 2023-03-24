# 学习英语的 Python

> 原文：<https://medium.com/nerd-for-tech/python-for-learning-english-f5811e288d2d?source=collection_archive---------1----------------------->

一个学习英语的命令行界面程序

嗨伙计们！距离我上次发帖已经有一段时间了。如今，我正在同时开发多个项目。因此，我不能写一篇关于技术的新文章。然而，我开发了一个辅助项目来提高我的英语学习。实际上，我正在为我的英语课写这篇课文:P 在这篇课文中，我将写我如何使用我的编程技能来提高我的语言学习。

![](img/acb702b36a5e959ef41972532724e16a.png)

[真诚媒体](https://unsplash.com/@sincerelymedia?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/english?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 英语学习的命令行界面

实际上，有一种老生常谈的说法是，如果你想提高你的编码技能，你需要通过编码找到一个现实生活中的问题来解决。为此，我选择了翻译作为我现实生活中的问题。当我用笔记本电脑阅读或学习时，学习我在页面上看到的新单词有点困难。为了解决这个问题，我开发了一个 CLI 程序。

[](https://github.com/mebaysan/Tureng-Translation-CLI) [## GitHub-mebaysan/Tureng-Translation-CLI:学习英语新单词的 CLI。

### 我创建了这个 repo 来轻松地翻译英语和土耳其语单词，使用非常好的项目，Tureng。这个客户端…

github.com](https://github.com/mebaysan/Tureng-Translation-CLI) 

实际上，它最初只是用于英语到土耳其语的翻译。然而，后来我增强了它的功能。现在，它还可以显示搜索到的单词的同义词和用搜索到的单词创建的句子。

```
>>> python -m tureng_cli translate -w Kasırga -n 10

Meanings of "kasırga" in English Turkish Dictionary : 33 result(s)
#       Category        Turkish         English
1       + Common Usage  + kasırga       + hurricane
2       + Common Usage  + kasırga       + whirlwind
3       + General       + kasırga       + wind
4       + General       + kasırga       + cyclone
5       + General       + kasırga       + twist
6       + General       + kasırga       + squall
7       + General       + kasırga       + windstorm
8       + General       + kasırga       + whirlwind
9       + General       + kasırga       + twister
10      + General       + kasırga       + tourbillion>>> python -m tureng_cli sentence -w "Hurricane" -n 10Hurricane sentence example
#       Sentence
1       + Oh, and the tropical storm will become a hurricane late Saturday night.
2       + The town was considerably damaged by the great hurricane of the 8th of August 1899.
3       + He'd managed to miss the hurricane, though the waters were still rough and the waves high.
4       + Everything needs to be ready, especially if the hurricane shifts to make landfall.
5       + In 1907 a hurricane destroyed the greater part of the laurels of the Prado and the royal palms of the Parque de Colon.
6       + He felt like he'd been hit by a hurricane.
7       + In 1090 a tremendous hurricane passed over London, and blew down six hundred houses and many churches.
8       + An open space forming the heart of the square in which the church stands separates the solitary western tower (14th century) from the choir and transept, the nave having been blown down by a violent hurricane in 1674 and never rebuilt.
9       + In July, on the approach of the dangerous hurricane season, Rodney sailed for North America, reaching New York on the 14th of September.
10      + The hurricane, too, was followed by repeated droughts, and the inhabitants of the out-islands were reduced to indigence and want, a condition which is still, in some measure, in evidence.>>> python -m tureng_cli synonym -w "Hurricane" -n 3Hurricane synonyms
#       Synonym         Defination
1       + cyclone       +  (Meteorol.) A system of rotating winds over a vast area, spinning inward to a low pressure center (counterclockwise in the N Hemisphere) and generally causing stormy weather: commonly called a low, since it coexists with low barometric pressure
2       + typhoon       +  A violent cyclonic storm occurring in the western Pacific Ocean.
3       + wind          +  The wind instruments of an orchestra, or the players of these instruments>>> python -m tureng_cli translate -w "excissment" -n 10Maybe the correct one is
excitement
exciseman
excipient
excise
excised
excipients
excising
excimer
excision
excitant
```

希望它能帮助你提高笔记本电脑的阅读能力。

诚挚的问候