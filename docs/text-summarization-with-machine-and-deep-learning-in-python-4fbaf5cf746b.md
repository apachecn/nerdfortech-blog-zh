# python 中机器学习和深度学习的文本摘要

> 原文：<https://medium.com/nerd-for-tech/text-summarization-with-machine-and-deep-learning-in-python-4fbaf5cf746b?source=collection_archive---------10----------------------->

![](img/2b977b1644e82df5e5133b3ec4d95457.png)

Medium 是一个很棒的网站，你可以在那里找到大量的帖子和文章，但是如果你像我一样，你可能没有时间阅读所有的帖子和文章。不要担心我的朋友，我创建了一个小工具来做文本摘要，以帮助您获得每个帖子的关键笔记。

## 文本摘要概述

这是 NLP 的任务之一，目前可以使用提取模型或抽象模型来完成。

1.  提取模型:浏览文本并选择代表原文的最佳句子(选择最佳代表句子子集):莱克兰克、卢恩、LSA…
2.  抽象模型:多做一点，分析上下文并产生新的句子:T5 变压器，GPT2，巴特

## 我在哪里可以找到这些模型？

如果您需要在工作中使用文本摘要，python 中有两个很棒的包:

1.  Sumy:包含已经实现并随时可以使用的提取模型([https://pypi.org/project/sumy/](https://pypi.org/project/sumy/))
2.  变形金刚:包含抽象模型，尽管你可能需要微调它们(【https://pypi.org/project/transformers/】)。

## 你不知道用什么

如果您在所有这些模型中感到迷茫，并且不知道使用哪一个，请不要担心，我已经创建了一个简单的 web 应用程序来测试这些模型。到目前为止，已有 6 种型号可供选择，更多型号即将推出。

你可以通过这个链接尝试每一种算法:【https://SQ4YI25VJOXHOR53.anvil.app/7YFOUTRU6G6WQMI6TDJGP3DM 

## 寻找密码？

这段代码包含每个模型的使用示例，我假设您已经安装了所需的包:sumy、transformers、torch、sentencepiece 和 nltk tokenizer。

我希望这篇文章是简短的，你不需要用上面的算法来总结它。