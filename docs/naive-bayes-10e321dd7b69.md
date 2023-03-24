# 朴素贝叶斯

> 原文：<https://medium.com/nerd-for-tech/naive-bayes-10e321dd7b69?source=collection_archive---------1----------------------->

# 朴素贝叶斯分类算法

*   朴素贝叶斯算法是一种监督学习算法，基于贝叶斯定理，用于解决分类问题。
*   它主要用于包含高维训练数据集的*文本分类*。
*   朴素贝叶斯分类器是一种简单且最有效的分类算法，有助于建立快速机器学习模型，从而做出快速预测。
*   它是一个概率分类器，这意味着它根据对象的概率进行预测。
*   朴素贝叶斯算法的一些流行例子是垃圾邮件过滤、情感分析和文章分类。

# **#贝叶斯数学**

![](img/828eb8cc6e4c78f23e4ee81b5bfee08a.png)![](img/bd9db841f75ccc94b2600cf97ff764ec.png)![](img/949431cf19c6c18caba27fb346f3711f.png)

#编码

![](img/68b5f295112633bbfcf0c9f63a75a480.png)

# **#将数据转换成数值数据**

![](img/59b768c2f7890e00baa60e51e2842d54.png)![](img/c04f56168a807d49bb135996e2276ff8.png)

# **#转换成 numpy 数组**

![](img/a714536a0ca148b17ca8679a686a0c7d.png)

# **#培训和测试**

![](img/af20d17c785a7e1b022983ea7808c3f5.png)

# **#蘑菇的种类**

![](img/71b051b810d94c4379bf97726cd90582.png)

**#数据框中的 p 型和 e 型**

# **# #构建我们的朴素贝叶斯分类器**

我们需要一个函数来计算数据集中每个类的先验概率

![](img/986287333035bd6c826c5e24d0cf77b9.png)

**#提示**

![](img/8cc222aff0b72fda0fe27c355dfc9db6.png)![](img/dd38f456fa8058dfe388203e368afc3a.png)

**#提示**

# #条件概率

![](img/feebdf4c5074816e17453e860f852138.png)![](img/d2ff7cffc5eca8e2d9b4507ae6f2a5cf.png)![](img/979296d0dc1863eebb752a06c7bdbda2.png)

**#提示**

![](img/93124702eba73a4a43bbf00067cb2681.png)![](img/f3ab01b028d797e7a7c0da2c0c0c5e66.png)

# **#计算每个测试实例的后验概率，并进行预测**

![](img/22469cfbe0c6d3f4d29a168ff7fadecc.png)![](img/f13723313f20636bfdcf0c4a40c45c9c.png)![](img/b67b0b71c55701648f59e2f8df851522.png)