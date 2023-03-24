# 顶级深度学习算法

> 原文：<https://medium.com/nerd-for-tech/top-deep-learning-algorithms-7b1a43a75dbc?source=collection_archive---------7----------------------->

![](img/09dc56b5eedde171c89014b050d92644.png)

[万用眼](https://unsplash.com/@universaleye?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

有很多深度学习算法被使用，但有哪些是最好的呢？我们来看看吧！！

我已经在这里讨论了最基本的神经网络或者感知机[。如果你想知道一个神经网络在最简单的形式下是如何运作的，请看看这个。](https://adityakumawat502.medium.com/the-most-basic-of-the-neural-networks-271bc59f6c61)

# 深度学习是如何工作的？

深度学习算法使用 ann(人工神经网络的缩写)来复制我们大脑的功能。为什么呢？不想成为哲学或历史，但是我们的大脑是我们所知道的最复杂的机器。因此，即使我们能够复制“脑力”,可能性的数量也是深不可测的。

一种算法如何处理和计算输出当然与另一种算法不同。例如，在一种类型的算法 MLPs 中，通过将基本形式的神经网络(也称为感知器)堆叠在彼此之上并处于不同的水平来处理数据。数据在这些部分之间划分、处理，并产生一个输出或多个输出。

# 类型

*   卷积神经网络
*   递归神经网络
*   长短期记忆网络
*   多层感知器
*   径向基函数网络
*   生成对抗网络
*   深度信念网络
*   自组织映射(SOMs)
*   自动编码器
*   受限玻尔兹曼机器

以下是这些算法工作原理的基本描述:

# 卷积神经网络

工作:他们有处理数据特征的多层。

这些网络主要用于检测物体和处理图像。

实现输出的重要层:

*   卷积层:该层有多个执行复杂操作的过滤操作。
*   整流线性单元:简称 ReLU。该单元对元素应用一些操作，并输出校正的特征图。
*   池层:ReLU 的输出进入这个池层。进行汇集是为了减少特征图的维数。
    池层从 ReLU 的二维输出中输出单个长向量。

例如，如果我们使用 CNN 进行图像分类，那么最终，在汇集完成后，来自汇集层的向量进入前馈网络或全连接层，该层将对图像进行分类。

# 递归神经网络

RNNs 将它们的输出反馈为输入。由于这一特性，rnn 可以记住以前的输入，因此它们可以对数据执行非常强大的操作。

由于喂食本身的特性，RNN 可以随时间展开。这意味着 RNN 在时间 t1 的输出将不同于在 t2 的输出。

用于:

*   图像字幕
*   自然语言处理
*   时间序列分析
*   语言翻译

RNNs 是自然语言处理的最大突破之一。

# 长短期记忆网络

这是一种 RNNs，它的默认行为是回忆过去的信息*(听起来很像人类！)*相当长一段时间。

LSTMs 的用例与 rnn 非常相似，但更具体地说，它们可以用于音乐创作、语音识别等。

自动 LSTM 的工作分三步进行:

*   忘记过去状态的不必要信息。
*   更新当前单元格的值。
*   输出单元格的特定状态。

# 多层感知器

如果你知道感知器，那么你只要看标题就能猜到这个算法是如何工作的。

它们包含各种感知器，这些感知器具有垂直堆叠和水平连接的激活功能。

MLP 用于语音识别、翻译和语音识别。

这些是最流行的神经网络算法。如果你想更详细地了解上述网络以及其他网络，以下是相关链接:

*   [卷积神经网络](https://en.wikipedia.org/wiki/Convolutional_neural_network)
*   [递归神经网络](https://en.wikipedia.org/wiki/Recurrent_neural_network)
*   [长短期记忆](https://en.wikipedia.org/wiki/Long_short-term_memory#:~:text=Long%20short%2Dterm%20memory%20(LSTM)%20is%20an%20artificial%20recurrent,the%20field%20of%20deep%20learning.&text=LSTM%20networks%20are%20well%2Dsuited,events%20in%20a%20time%20series.)
*   [多层感知器](https://en.wikipedia.org/wiki/Multilayer_perceptron)
*   [径向基函数网络](https://en.wikipedia.org/wiki/Radial_basis_function_network)
*   [生成对抗网络](https://en.wikipedia.org/wiki/Generative_adversarial_network)
*   [深度信念网络](https://en.wikipedia.org/wiki/Deep_belief_network)
*   [自组织映射](https://en.wikipedia.org/wiki/Self-organizing_map)
*   [自动编码器](https://en.wikipedia.org/wiki/Autoencoder)
*   [受限玻尔兹曼机](https://en.wikipedia.org/wiki/Restricted_Boltzmann_machine)

非常感谢你的阅读！！！在 [Twitter](https://twitter.com/AdityaK50037212) 上关注我，在 [Linkedin](http://www.linkedin.com/in/aad1tya) 上联系我，帮助我拓展我的人脉！