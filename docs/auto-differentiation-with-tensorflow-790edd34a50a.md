# 张量流自动微分

> 原文：<https://medium.com/nerd-for-tech/auto-differentiation-with-tensorflow-790edd34a50a?source=collection_archive---------0----------------------->

## 让我们来解开自动分化的机制！

![](img/6c75f5b1d80ad0a74ab8fd1278ce0a9e.png)

卢卡斯·法夫尔在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

如果读这篇文章的人是神经网络和其他东西的新手，那么这篇博客中的大部分内容将会很难理解。所以我建议在开始之前先阅读一下[神经网络 101](/artificialis/neural-networks-101-88d4b1f5d854) 。

我们知道，在深度学习中，我们往往会有一个损失函数，它会根据我们的模型参数(权重和偏差)进行微分，这种情况会持续下去，直到参数找到最佳值。但是驱动所有这些计算的潜在机制是什么呢？

对你来说是微积分。

但这不是一个关于微积分 101 的博客，相反，我们将研究**自动微分**，它是深度学习框架的基础。因为我们都知道深度学习模型通常基于梯度技术工作，自动微分使我们很容易获得复杂或深度模型的梯度。

# 为什么我们需要汽车差异化？

例如，对于下面的函数，我们可以很容易地计算偏导数，即计算 **w1** 和 **w2** 的梯度，并且方程是偏导数。

```
 **def** f(w1, w2): 
  **return** 3* w1**2 + 2*w1*w2
```

这里我们的函数看起来很简单，但是当我们构建一个深度神经网络的时候，情况就不一样了，我们会有一个 10 倍复杂的函数。更精确地说，用数学术语来说，我们要计算链式法则成千上万次。

此外，我们将在向前传递的过程中计算偏导数，对于一个完整的神经网络，这将是一个巨大的数字，几乎不可能跟踪这些导数，在计算梯度时，我们将需要这些偏导数。

好了，说了这么多术语，让我们花点时间看看这些术语对我们到底意味着什么。

## 解构术语

当我们第一次用参数计算损失函数时，它会返回偏导数，我们称这个过程为向前传递。

正向传递负责用我们的参数计算损失函数。但我们知道，神经网络必须优化其参数，以实现最佳结果，即获得最小的损失误差。

但是，我们如何找到有助于神经网络找到最佳参数的值，以使损失最小化呢？

渐变。

我们必须通过激活**反向传播**(或)反向通道来获取梯度。首先，我们执行一个正向传递，得到我们的偏导数，通过激活反向传播，使用链式法则来计算梯度。

**但是所有这些与汽车差异化有什么关系呢？**

自动微分有助于我们跟踪这些计算，在反向传播过程中，它只需使用这些参数来计算梯度。我们知道，在偏导数的帮助下，我们能够计算可训练变量(权重和偏差)的梯度，并且仍然能够记录成千上万的导数和梯度。

好了，理论到此为止，让我们进入一些代码，把这个东西包起来。让我们用上面的等式来做实验，我们要用张量流来做这个。

```
w1 , w2 = 5 ,3  # params
eps = 1e-6 # learning rate to step the params# Doing the computation by hand 
(f(w1 + eps, w2) - f(w1, w2)) / eps
```

需要为每个参数调用上述**函数 f()** ，对于大型神经网络而言，这将是一个繁琐的过程。因此，让我们通过使用 TensorFlow 的 **GradientTape** 来美化它，它使用自动微分机制。

```
[ <tf.Tensor: id=828234, shape=(), dtype=float32, numpy=36.0>,<tf.Tensor: id=828229, shape=(), dtype=float32, numpy=10.0> ]
```

这是如何工作的？

*   首先，我们定义两个变量`w1`和`w2`。
*   `tf.GradientTape()`将自动记录每一个涉及变量的操作(只有可训练变量，tf.constants)
*   我们要求磁带计算结果(损失)z 关于变量[ `w1`和`w2` ]的梯度。

不仅结果准确，而且精度仅限于浮点误差。我们必须记住的一件重要事情是，`gradient()`无论有多少个变量，该方法只遍历一次记录的计算(反向传递-反向顺序)。

我们也可以通过在`tf.GradientTape()`块中创建`tape.stop_recording()`来暂停记录。

```
# Returns the gradient and tape is erased
dz_dw1 = tape.gradient(z, w1) # Returns error RUNTIMEERROR
dz_dw2 = tape.gradient(z , w2)
```

`tape`在我们调用它的`tape.gradient()`后会立即自动擦除。但是如果我们需要多次调用`gradient()`，我们可以使用持久参数。

磁带持久调用`gradient()`并在每次执行后删除它。

```
# Setting persistent to True with tf.GradientTape(persistent = True) as tape:
   tape.watch(x)
```

当我们实现一个定制的正则化损失时，可以使用`tape.watch()`，当输入变化很小时，惩罚变化很大的激活，但是这个损失不会基于`tf.Variables`，最有可能是基于`tf.constant`张量。

你可以在 TensorFlow 文档中了解更多，我也会在下面附上一些链接。如果有任何问题，欢迎在评论中指出来。

在那之前，

下次吧。

*   汽车差异化简介:[https://www.tensorflow.org/guide/autodiff](https://www.tensorflow.org/guide/autodiff)
*   反向模式自动差异从零开始:[https://sidsite.com/posts/autodiff/](https://sidsite.com/posts/autodiff/)