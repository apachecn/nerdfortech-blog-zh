# python3 中的 sense2vec

> 原文：<https://medium.com/nerd-for-tech/sense2vec-in-python3-9d3db9557495?source=collection_archive---------2----------------------->

sense2vec 是一个不可思议的工具，可以在给定关键字的情况下获得同义词和相关性。
关键词不一定是一个独立的词，如“医学”，也可以是一个复合词，如“机器学习”。

sense2vec 可以马上使用，不需要训练自己的语料库。
事实上，它附带了两个完全预训练的向量集，即 reddit_2019 和 reddit_2015。这里有一个[链接](https://github.com/explosion/sense2vec#pretrained-vectors)到你可以下载的官方文档。

本文分为:
1 —安装
2 —导入库
3 — sense2vec
4 —常见问题

如果你刚开始，我建议你选择 reddit_2015 系列。它相当小，你甚至可以在一台不太强大的计算机上运行它。否则，你将会有一个 oom-kill——内存不足的错误。

![](img/d6ec983857bcc6d811991ca042058545.png)

来自[像素库](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1836089)的[像素](https://pixabay.com/users/pexels-2286921/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1836089)的图像

## 1 —安装

```
pip install -U spacy
python -m spacy download en_core_web_sm
pip install sense2vec
pip install pathlib# download your pretrained vector set
cd directory/with/pretrained/set
tar -xvf vectors_md.tar.gz # you should see a s2v_old folder now
```

## 2-导入库

```
import spacy
from sense2vec import Sense2VecComponent,Sense2Vec                                         from pathlib import Path
```

## **3 — sense2vec**

```
def get_correlations(KEYWORD):
    nlp = spacy.load(“en_core_web_sm”) 
    s2v = nlp.add_pipe(“sense2vec”) 
    path = Path(__file__).parent.joinpath(‘s2v_old’) 
    s2v.from_disk(path) 
    doc = nlp(KEYWORD) 
    assert doc[:].text == KEYWORD 
    freq = doc[:]._.s2v_freq 
    vector = doc[:]._.s2v_vec 
    try: 
       closest_words = list(doc[:]._.s2v_most_similar(30)) 
    except ValueError: 
       print(“Nothing found in sense2vec.”) 
       return ([])
    correlated = [w[0][0] for w in closest_words if w[1] > 0.70]
    return (list(correlated))
```

。我们加载英语的空间语言模型。

。我们在 sense2vec 中添加了一个管道——sense 2 vec 已经附带了一组通过管道处理文本的操作，例如消除停用词；通过添加管道，我们在管道的*端*添加了一个额外的操作。您可以将管道视为一组相互链接的操作。

。您可以添加 Path()，以便 python 能够找到您的文件夹，无论它在您计算机上的什么位置——否则您可以在 s2v . from _ disk(Path/to/pre trained/vector/folder)中硬编码您的文件夹的相对路径

。`doc[:].text == KEYWORD`在整个预训练文本中搜索关键词，然后我们可以得到它的频率和向量属性

。我们使用 try/except，以防我们的关键字不在 word2vec
With `closest_words = list(doc[:]._.s2v_most_similar(30))`中，我们得到 sense2vec 找到的前 30 个相关单词。
如果找到关键字，我们会创建一个包含所有得分高于 0.70 的相关单词的列表(根据需要更改该值)。

。我们用 w[0][0]和 w[1]是因为 sense2vec 返回的是一个元组列表，其中每一项都是这样结构的元组:
`((word, POS), frequency)` → for ex。(('问题'，'名词')，0，87)。
词性表示词性，如名词、动词、ADJ……

最后，我们返回列表。

## **4 —常见问题**

如果您得到以下错误，
`raise ValueError(f"Can't read file: {location}")`
`ValueError: Can't read file: ../filtering_models/vectors_md/cfg`
删除您的 s2v_old 文件夹并重新解压缩 vectors_md.tar.gz
如果您仍然得到此错误，请检查您的 s2v_old 文件夹的相对路径。

![](img/b6fb3121a83b62733c39a61acf3a69ec.png)

照片由 [**弗拉德·chețan**](https://www.pexels.com/@chetanvlad?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄自 [**Pexels**](https://www.pexels.com/photo/close-up-photo-of-leaves-with-droplets-1626340/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)