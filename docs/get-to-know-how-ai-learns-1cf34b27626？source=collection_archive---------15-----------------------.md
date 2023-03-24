# 了解人工智能如何学习

> 原文：<https://medium.com/nerd-for-tech/get-to-know-how-ai-learns-1cf34b27626?source=collection_archive---------15----------------------->

人工智能编程还很新吗？本文将向您展示 AI 如何简单地预测**“这个数字是正数还是负数？”**使用 Python。

![](img/948ea97144a87ab0bae895b962d2d4ec.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral) 拍摄的照片

你需要什么？

*   Python 3.7(或更高版本)
*   张量流(遵循此设置[https://www.tensorflow.org/install](https://www.tensorflow.org/install)或[https://www.tensorflow.org/install/gpu](https://www.tensorflow.org/install/gpu)
*   Numpy
*   激动:)

现在，我们如何定义正数或负数？对于人类来说，这很容易。如果数字小于 0，表示为负，否则为正。

```
# An example set of numbers.
numbers = [-2.2, -1.4, -0.8, 0.2, 0.4, 0.8, 1.2, 2.2, 2.9, 4.6]
```

要教计算机，我们需要说出**【标签】**就像我们上小学时老师告诉我们的一样。我们被告知-1 是负数，1 是正数。那么我们理解或者说**【分类】**所有数字前面带负号的都是负数。计算机试图学习的这种理解。

但是，电脑不会用英语说话，所以我们只是用一个数字来表示正反。

```
# 0 means negative, and 1 is positive 
# -2.2 is 0, -1.4 is 0, -0.8 is 0, 1.2 is 1, and so onnumbers = [-2.2, -1.4, -0.8, 0.2, 0.4, 0.8, 1.2, 2.2, 2.9, 4.6]
label = [0, 0, 0, 1, 1, 1, 1, 1, 1, 1]
```

**现在，如何教计算机？(TL；博士)**

我们将使用 Tensorflow 的内置网络来创建一个简单的神经网络，称为**“Dense”**。还有其他层可以使用，如 MaxPool 等。

```
import tensorflow as tf
import numpy as npdata = [-2.2, -1.4, -0.8, 0.2, 0.4, 0.8, 1.2, 2.2, 2.9, 4.6]
label = [0, 0, 0, 1, 1, 1, 1, 1, 1, 1]# Convert python's array to numpy
data = np.asarray(data)
label = np.asarray(label)# Create the neural-network
model = tf.keras.Sequential([
    tf.keras.layers.Dense(128, activation='relu'),
    tf.keras.layers.Dense(**1**, activation='sigmoid')
])# Compile the model
model.compile(optimizer='adam',
              loss=tf.keras.losses.**BinaryCrossentropy**(),
              metrics=['accuracy'])
```

请注意模型中的最后一个密集，它只有 1 个单位，因为我们的输出只有 1 个东西，要么是“0”，要么是“1”(负的或正的)。因为我们想预测一个“二进制”的东西，我们将使用二进制交叉熵。

**如何开始训练？(TL；博士)**

只需调用 model.fit:)

```
result = model.fit(data, label, epochs=100, validation_split=0.1)
```

**如何预测？(TL；博士)**

只需在 model.fit 之后调用 model.predict

```
# Change the array and see the result
data_test = np.asarray([-2.2, -1.4, -0.8, 0.2, 0.4, 0.8, 1.2, 2.2, 2.9, -4.6])
predictions = model.predict(data_test).round()
print(predictions)
```

这是我们得到的输出:

```
[[0.] # -2.2 is a negative - TRUE
 [0.]
 [0.]
 [1.]
 [1.]
 [1.]
 [1.]
 [1.]
 [1.] # 2.9 is a positive - TRUE
 [0.]]
```

我们得到了 100%的准确率。耶！

**下一步怎么办？**

你可以像本教程中所想的那样继续预测二维图像[https://www.tensorflow.org/tutorials/keras/classification](https://www.tensorflow.org/tutorials/keras/classification)。本教程将教你**“如何猜衣服的名字。这是衬衫、凉鞋等吗？”**

为什么要创造 AI 来预测正数和负数呢？LOL。我个人认为这是最容易进入深度学习的方式。你会惊讶于计算机是如何学习的。