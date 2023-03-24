# 用于图像分割的主色提取

> 原文：<https://medium.com/nerd-for-tech/dominant-color-extraction-for-image-segmentation-2f30e9590889?source=collection_archive---------2----------------------->

![](img/ac3293cfd8cc4e48104c48129fe18a15.png)

图像分割是图像处理中的一个重要步骤，如果我们要分析图像内部的东西，它似乎无处不在。例如，如果我们试图找出室内图像中是否有椅子或人，我们可能需要图像分割来分离对象，并单独分析每个对象以检查它是什么。图像分割通常作为图像的模式识别、特征提取和压缩之前的预处理。

图像分割是将图像分类成不同的组。在使用聚类的图像分割领域中已经进行了多种研究。有不同的方法，最流行的方法之一是 **K 均值聚类算法**。

因此，在本文中，我们将探索一种读取图像并对图像的不同区域进行聚类的方法。但在此之前，我们先来谈谈:

1.  图象分割法
2.  图像分割的工作原理
3.  k-均值聚类 ML 算法

![](img/82d1785a11ab522d60704088ee0e754e.png)![](img/ed60071c7d4695f24c9b591baf6e50cf.png)![](img/60fff638053639076cc7cc9a538c79c3.png)![](img/5043870f943ef82bbf66a28e5be42d6d.png)![](img/b36d6943c813e49e116d33d84deb503a.png)![](img/c877bae68c0fbdb55cbe936cd2e03ebf.png)![](img/322d1f87a0615b4caad6970391a7825f.png)![](img/342deb1737d2dbcb683fbebc548cceb5.png)