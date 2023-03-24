# C#中的怪异行为

> 原文：<https://medium.com/nerd-for-tech/spooky-behaviors-in-c-16643f422fd9?source=collection_archive---------0----------------------->

![](img/d4d8a8e8830f3c8f75ef5172098ffe14.png)

[Eyestetix 工作室](https://unsplash.com/@eyestetix?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

今天的文章是关于我偶然发现的潜伏在 C#语言及其库中的一些令人惊讶的行为。我不会称之为 bug，但是如果你不小心的话，它们会让你措手不及。

## 可空数学

第一个问题与可空整数有关。C#中的整数是[整数类型](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/integral-numeric-types)，通常它们有一个默认的…