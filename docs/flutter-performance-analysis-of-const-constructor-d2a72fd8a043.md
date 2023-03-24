# 颤振:const 构造函数的性能分析

> 原文：<https://medium.com/nerd-for-tech/flutter-performance-analysis-of-const-constructor-d2a72fd8a043?source=collection_archive---------0----------------------->

![](img/97ffea010873175f61113f11b3d71d34.png)

E 每个人都说使用`[const](https://dart.dev/guides/language/language-tour#constant-constructors)` [构造函数](https://dart.dev/guides/language/language-tour#constant-constructors)可以提高你的应用程序的性能，但是能提高多少呢？我在网上没有找到任何分析，所以我决定自己做。

这是我的测试应用程序: