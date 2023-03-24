# 为什么训练 K-最近邻的时间复杂度是 O(1)

> 原文：<https://medium.com/nerd-for-tech/why-the-time-complexity-for-training-k-nearest-neighbors-is-o-1-5b8f417104cf?source=collection_archive---------0----------------------->

## 了解如何从头开始实现 K-最近邻，并了解为什么训练 K-最近邻不需要时间

![](img/d577c62a1a7f58655a3a24a41ba2b4bb.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Aron 视觉](https://unsplash.com/@aronvisuals?utm_source=medium&utm_medium=referral)拍摄的照片

# 你有没有想过训练和测试一个算法的时间复杂度？

![](img/31a803dd6b3eb3d770266f3bd9dd2b69.png)

由 [Anthony Tran](https://unsplash.com/@anthonytran?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

与测试相比，大多数算法在训练时花费大量时间。深度学习算法可能需要几个月的时间来训练，但我们有一个经典的机器学习算法，它在测试过程中需要更多的时间，在训练过程中只需要几毫秒。从技术上讲，那些不知道的人，我们称之为时间复杂度，用 O(n)表示。

所以对于 KNN 来说，训练的时间复杂度是 O(1 ),这意味着它是常数，而测试的时间复杂度是 O(n ),这意味着它取决于测试实例的数量。

这篇文章的先决条件是对 KNN 算法以及面向对象编程概念有一个基本的了解，如果你还在为 K-最近邻而苦恼，可以看看我关于 KNN 的文章。

[K-最近邻-第一部分](/analytics-vidhya/k-nearest-neighbors-part-i-9102f8f3173c?source=your_stories_page---------------------------)

首先，我将向您展示分类的 KNN 的实现，然后通过代码，我将向您展示训练和测试的时间复杂性背后的直觉

让我们直接跳到代码。首先，让我们导入我们需要的依赖项:

```
import statistics
import time
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.datasets import load_iris
from scikitplot.metrics import plot_confusion_matrix
```

让我们构建一个 KNearstNeighbor 类，并定义一个构造函数来传递一个参数 **n_neighbors** 来定义一些最近的邻居:

```
class KNearstNeighbor():
    def __init__(self, n_neighbors):
        self.n= n_neighbors
```

让我们构建一个 **fit()** 方法并传递 X 和 y，它们分别是特征张量和目标张量，它们将成为我们的训练数据集:

```
def fit(self,X,y):
        self.feature = X
        self.target = y
```

让我们将 X 和 y 存储到特征和目标变量中，这样就完成了训练部分。

让我们构建一个 **predict()** 方法，它将一个测试特性作为输入:

```
def predict(self, test):
        self.test_feature = test
        self.test_target = np.zeros(len(self.test_feature))

        for i in range(len(self.test_target)):
            distance = np.sum(np.abs((self.test_feature[i]-self.feature)), axis=1)
            distance_sort= distance.argsort()[:self.n]
            y_sample= []
            for j in range(self.n):                        y_sample.append(self.target[distance_sort[j]])

            self.test_target[i]=statistics.mode(y_sample)

        return  self.test_target
```

让我们更深入地研究这段代码。

首先将测试张量存储在 test_feature 变量中，同时构建一个 test_target 数组作为输出目标张量。

现在，让我们运行一个循环来找出单个测试数据点和训练数据点之间的曼哈顿距离。现在对这些距离进行排序，并找到最接近测试点的 n 个训练数据点的索引。

所以我使用了一个 **argsort()** 方法来对距离进行排序，输出对应于这些距离的索引。

现在将最接近测试点的点的“n”个索引存储在 y_sample 列表中。

但是为什么只有 n 个点，既然 n 是我们要考虑的最近邻的个数。现在找到这些最近点的类，并追加到 y_sample 列表中。现在，该测试点的最终标签将是相邻点的大部分类别标签

在统计模块的帮助下，可以使用 **mode()** 方法计算 y_sample 的模式

现在将类标签存储在 test_target 列表中并返回它:

```
class KNN():

    def __init__(self, n_neighbors):

        import numpy as np
        self.n= n_neighbors

    def fit(self, X,y ):
        self.feature = X
        self.target = y

    def predict(self, X):
        self.test_feature= X
        self.test_target=np.zeros(len(self.test_feature))

        for i in range(len(self.test_target)):
            distance = np.sum(np.abs((self.test_feature[i]-self.feature)), axis=1)
            min_values_index = distance.argsort()[:self.n]
            y_sample= []
            for j in range(self.n):
                y_sample.append(self.target[min_values_index[j]])

            self.test_target[i]=statistics.mode(y_sample)

        return  self.test_target
```

现在让我们在 Iris 类分类数据集上测试它，看看训练和测试的时间复杂度:

```
iris= load_iris
X= iris['data']
y= iris['target']
X_train, X_test, y_train, y_test= train_test_split(X,y)
```

为了找到训练和测试的时间复杂度，我们有一个时间模块。

让我们计算一下 knn.fit(X_train，y_train)执行的时间。让我们借助于 **time()** 方法将训练部分的开始时间存储在 start_train 变量中，将结束时间存储在 end_train 中。然后为了找出所用的时间，只需计算两者的差值。

对测试也重复此步骤:

```
knn=KNN(n_neighbors=3)
start_train= time.time()
knn.fit(X_train,y_train)
end_train = time.time()
start_test = time.time()
y_pred_train= knn.predict(X_train)
end_test= time.time()
y_pred_test= knn.predict(X_test)
print(f"Train_time: {start_train-end_train}, Test_time: {end_test-start_test}")
```

结果

```
Train_time: 0.0, Test_time: 0.00498652458190918
```

这是完整的代码。

您可以通过更改测试和定型数据集的大小来试验不同模型的定型和测试时间。此外，尝试实现其他算法，并查看其时间复杂性的变化。