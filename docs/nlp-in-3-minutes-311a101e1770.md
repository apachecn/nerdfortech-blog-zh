# 3 分钟后 NLP

> 原文：<https://medium.com/nerd-for-tech/nlp-in-3-minutes-311a101e1770?source=collection_archive---------9----------------------->

# 什么是 NLP

NLP 是自然处理语言的首字母缩写，是人工智能的一个子领域，它试图通过不同的信息提取技术来解释和理解人类语言，NLP 的目标是使计算机和人类之间的交互更加无缝，达到计算机可以掌握和理解人类语言的程度，NLP 有两个主要领域，NLU(自然语言理解)和 NLG(自然语言生成)，NLU 试图通过从不同来源(博客，文章等)提取数据来理解人类文本和语音..)当 NLG 试图用某种语言生成数据时，想象一下有人试图用他的母语之外的语言(计算机的母语是 0 和 1)写一段话，而不先学习该语言，当然他不能，机器或计算机也是如此，所以为了理解人类语言，NLP 的管道从 NLU 动作和过程开始，然后是 NLG 动作和过程，因为不理解它就不能写一段话或概括一篇文本是正常的。

# 什么是 NLTK

NLTK 是 python 中的自然语言工具包，它是一个 NLP 库，用于在 Python 中执行 NLP 功能，如命名实体识别或语料库训练、POS(词性标注)、词干、词汇化以及许多不同的功能。

# 一些 NLP 定义

1.语料库:
语料库是一个非常简单的术语，它是单词和
句子的集合，它有不同的语言版本，它涵盖了来自电影的
对话、引用、名字等……我工作过的 NLTK 支持的
语言是英语、德语、阿拉伯语
，但我相信它支持更多的语言。

2.POS(词性标注):
词性标注用于识别
句子中的单词，无论它是名词、副词、动词、形容词、代词
还是许多其他状态，当我在 nltk 中看到它时，我就被它迷住了。

3.词汇化:
词汇化是语言学中使用的一项著名技术，它并不局限于自然语言处理。实际上，到目前为止介绍的许多技术
都与语言学有关，而不是与计算机科学或人工智能有关。因为自然语言处理是语言学和机器学习的结合，或者我们可以考虑人工智能应用之一或语言学的领域，什么词汇化基本上是通过删除额外的
字符或把一个单词转换成形容词或

4.词干化:
词干化的一个变种，个人觉得词干化
没什么用(不明白它的用处)，词干化要
好得多，准确得多。

5.命名实体识别:
命名实体识别是将单词或句子
分类为类别、名称、公司、城市等。它确定
句子中的单词是否是人名、公司名称
或城市或其所属的任何类别，命名实体识别算法是在不同语料库上训练的模型，以
学习如何将单词分类为实体。

6.单词标记化:
任何一个熟悉 python 的人都会发现这个概念相当简单
，一个标记化器将句子分解成单词，nltk 有一个
的惊人特性，有两种类型的标记化器，Word 和
句子，其中单词标记化器将一个段落分解成单词
，而句子标记化器将一个段落分解成更小的
句子。

我们将编写两个小程序来实际演示 nltk 如何有用，试着猜测一下在 T10 程序中使用的概念。

趣味阅读:
[https://www . data camp . com/community/tutorials/text-analytics-初学者-nltk](https://www.datacamp.com/community/tutorials/text-analytics-beginners-nltk)

[https://python programming . net/lemmatizing-nltk-tutorialhttps://www . data camp . com/community/tutorials/text-analytics-初学者-nltk](https://pythonprogramming.net/lemmatizing-nltk-tutorialhttps://www.datacamp.com/community/tutorials/text-analytics-beginners-nltk)

[https://medium.com/m/global-identity?redirect URL = https % 3A % 2F % 2ftowardsdatascience . com % 2f 自然语言处理-NLP-top-10-应用已知-b2c80bd428cb](https://medium.com/m/global-identity?redirectUrl=https%3A%2F%2Ftowardsdatascience.com%2Fnatural-language-processing-nlp-top-10-applications-to-know-b2c80bd428cb)