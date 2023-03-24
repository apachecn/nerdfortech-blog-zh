# 有拥抱脸变形金刚的 NLP

> 原文：<https://medium.com/nerd-for-tech/nlp-with-hugging-face-transformers-a41caadf6f2?source=collection_archive---------0----------------------->

![](img/2bf5c673110c775157fe808f8f364348.png)

如果您曾经经历过任何自然语言处理项目或教程，您一定已经学会了这些东西:

1.  分类者
2.  编码器-解码器
3.  LSTM
4.  手套或 Word2Vec
5.  嵌入层

但就这样吗？虽然我们大多数人都使用 rnn 处理文本，但我们很少注意到它们无法在 GPU 上训练得更快。这是由于大多数顺序处理文本的 NLP 模型的架构。我们能即兴创作这个模型吗？

在过去的几年里，自然语言处理模型有所增加。从 2015 年 [**关注**](https://arxiv.org/abs/1409.0473) 机制、 [**变形金刚**](https://papers.nips.cc/paper/7181-attention-is-all-you-need.pdf) 的发布，再加上即将发布的[**【GPT-3】**](https://github.com/openai/gpt-3)，相信计算机对人类语音的处理能力将远远好于人类自己。

从 LSTMs 中的**监督序列**学习到拥有 3.4 亿参数的 [Bert](https://ai.googleblog.com/2018/11/open-sourcing-bert-state-of-art-pre.html) 中的**半监督** **序列**学习，NLP 的需求和应用暴涨。事实上，GPT 3 号有 1750 亿个参数！作为一个初学者，我所学的只有 LSTMs。

为了在研究热潮中保持优势，你需要保持更新。

拥抱脸的变形金刚简化了这些巨大模型的应用。而这种独特的能力并不是以学习任何新的深度学习库为代价的。它与 TensorFlow 和 PyTorch 都集成得很好。

让我们先来看看这个包的一些直接应用。所有的代码也可以在这里正式获得[。](https://huggingface.co/transformers/task_summary.html)

# **1。情绪分析**

情感分析是对情感(积极、消极和中性)的解释和分类。对于这个任务，大多数人会创建一个单词包，并将其传递给逻辑回归(可选的降维)。但是为什么要训练，当我们有了允许我们直接应用它的功能时。

```
from transformers import *nlp = pipeline('sentiment-analysis')print(nlp('I love my country.'))
```

# **2。**命名实体识别

NER 方法也在 NLTK 这样的包中广泛使用。这有助于我们将单词分为不同的类别，如组织名称、人名等

```
nlp_token_class = pipeline('ner')nlp_token_class('FAANG  stands for Facebook, Apple, Amazon, Netflix and Google.')
```

# **3。文本总结**

文本摘要有两种类型:抽取型和抽象型。

提取摘要从文本中选择具有最有价值的上下文的句子，而抽象被明确地训练以从文本中创建摘要。

```
TEXT = """100+ Days of ML Code is a commitment to better your understanding of this powerful tool by dedicating at least 1 hour of your time every day to studying and/or coding machine learning for at-least 100 days.Everyone, beginners as well as professionals are welcome to take up the challenge, join, contribute and collaborate.At the end of this 100 Days Of ML journey, all the members will be able to showcase a rich portfolio of code, analysis and narrative, treating all the above topics and models plus all the additional content, that as a member you will invariably experience and explore yourself, throughout this learning journey."""summarizer = pipeline('summarization')print(summarizer(TEXT))
```

# 4.问题回答

```
qa = pipeline('question-answering')print(qa(context=TEXT, question='How many days is the challenge for? '))print(qa(context=TEXT, question='What will I get at the end?'))
```

# 5.翻译

如果你知道的话，翻译是顺序学习最流行的例子之一。但是有了拥抱脸，你就不需要训练自己的脸了。(对于大部分的流行语言。有了迁移学习，你当然也可以建立你的客户模型。)

```
translator = pipeline('translation_en_to_fr')translator("100 Days of ML Code is a commitment to better your understanding of this powerful tool by dedicating at least 1 hour of your time every day to studying and/or coding machine learning for at-least 100 days.")
```

这些只是一些主要的应用，但还有更多像文本生成，寻找丢失的单词等。但是像我们之前讨论的伯特和 GPT 这样的模型呢？

好吧，拥抱脸为我们提供了使用那些预先训练好的模型以及 PyTorch 和 Tensorflow 的灵活性。

如果你是这个领域的新手，请记住几个关键词:

1.  **标记化**:任何 ML/神经模型都只是一堆公式，不能识别字符串作为输入。所以我们需要将每个单词/字符转换成一些唯一的数字。
2.  **填充和截断**:模型需要有一些固定大小的输入或批量输入大小，因此，小句必须用一些独特的字符连接起来，而长句必须在一定长度上截断。
3.  **型号**:只是参考我们将要尝试的不同型号。
4.  解码:如果一个模型只理解数字，它也会生成数字。这个函数帮助我们解码数字。

现在让我们来看看同样的应用程序，但是有更好的模型。此处提供了所有集成型号的列表[。](https://huggingface.co/transformers/model_summary.html)

# 6.掩蔽语言建模

这种模式有助于填补句子中的某些空白。请注意，在预测之前，您必须在句子中添加一个特殊的屏蔽字符，如下所示。

# 7.文本生成

文本生成是我们学习 LSTMs 的一个很好的例子。但是这种方法(以及许多其他模型)的缺点是，在几次预测之后，预测开始重复自己。因此，为了阻止这一点，我们需要添加一些特殊的东西。类似于<sep>、<end>等特殊标记通常用于此目的。</end></sep>

更多型号可在官方文档中查看，并在 Pipeline 和 Pretrained 模块中提供。

既然您现在已经熟悉了这些转换器的应用和灵活性，我希望您能很快熟悉 NLP。

学习新事物的荣誉。如果你愿意，请留下一些掌声和评论。