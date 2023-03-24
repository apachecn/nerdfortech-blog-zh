# 统一字典

> 原文：<https://medium.com/nerd-for-tech/dictionaries-in-unity-1ec967a94a47?source=collection_archive---------4----------------------->

![](img/1cf9e1213605dd0e2577686c825ba075.png)

字典是用于存储键值对的一般集合。它们类似于数组，但存储两个组件。第一个是键，第二个是值。如果你把它想象成一本英语词典，关键字就是单词，值就是定义。

使用**系统包含字典。Collections.Generic** 名称空间。要定义一个，你把它写成 Dictionary < keytype，objectType >。

![](img/1ce520caf921640cb6047337a8c4dbb2.png)

要将值添加到字典中，您将使用 **Add()** 方法。

![](img/702c968424b01812ed334933febdfac1.png)

要删除值，您将使用 **Remove()** 方法。

![](img/5937368e0ad6badd2f5d6b4eafed63ac.png)

词典的独特之处在于，当你搜索它们时，你会使用关键字。当找到密钥时，将返回该值。

![](img/6d729e601e4fe95aae146be46574e864.png)![](img/f6d95f0bcd631193c06fd01817f73e3c.png)

打印键和值的一个简单方法是使用 **LINQ** 库。这将允许您使用 **ElementAt()** 方法来打印键和值。

![](img/c3cd3efff01949f7b94e5bda9689437e.png)![](img/d628519ebf58092113ae3d8ab3ed70c9.png)

要打印整个列表，您可以使用一个 **foreach 循环**。

![](img/d9c7acf39e768289a9c60a9339acfa67.png)![](img/c4ddace2905435468e0912d290e3d7b7.png)