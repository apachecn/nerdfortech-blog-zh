# 卷积编码简介—第三部分

> 原文：<https://medium.com/nerd-for-tech/intro-to-convolutional-coding-part-iii-5529fdeebdb6?source=collection_archive---------2----------------------->

![](img/94a3073dd9437d36b4565d0ac705c478.png)

由 [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

这篇文章展示了一个使用 Viterbi 算法解码的非递归卷积码的 Python 实现示例。我之前关于该主题的帖子大致介绍了这些代码，并回顾了[编码](https://yair-mz.medium.com/into-to-convolutional-coding-part-i-d63decab56a0)和[解码](https://yair-mz.medium.com/intro-to-convolutional-coding-part-ii-d289c109ff7a)的过程。我的完整实现可以在专门的 GitHub gist 上找到。然而，我写它是为了好玩，它的目的是有教育意义。更好、更优化(尽管不一定容易阅读)的实现是存在的。

# 卷积码类

## 构造器

`ConvolutionalCode`类实现了前面讨论的编码器和解码器。构造函数接收一个整数元组，每个表示一个生成器。

构造函数从生成器的数量来推断代码的速率(为了简单起见，我们只考虑`k=1`)。约束长度由最大生成器以及 FSM 的状态确定。最后，调用`_build_fsm`方法。

## 有限状态机（Finite State Machine 的缩写）

`_build_fsm`方法将 FSM 指定为两个嵌套字典。根据`state`和`input`，字典`next_states`包含 FSM 中的下一个状态:

```
self.next_states[current_state][current_input]
```

新状态是通过右移旧状态(丢弃 LSB)并添加左移输入来找到的。

```
new_state = (current_input << (self.constraint_length - 1)) + (current_state >> self.k)
```

根据`state`和`input`，字典`out_bits`保存由编码器输出的位值(根据发生器数量的位数):

```
self.out_bits[current_state][current_input]
```

对于每个生成器，获得一个位反转表示(由于用于指定生成器的约定)。

```
bit_reversed_gen = int('{:0{width}b}'.format(gen, width=self.constraint_length+1)[::-1], 2)
```

将左移输入添加到状态以表示 LSR。

```
lsr = (current_input << self.constraint_length) + current_state
```

以发生器为掩码应用按位`&`

```
generator_masked_sum_arg = bit_reversed_gen & (lsr)  # mask input and state with generator
```

对 mod 2 位求和以获得输出位

```
bin(generator_masked_sum_arg).count("1") % 2
```

完整的方法如下所示

## 编码器

为简单起见，我们假设编码器的输入表示为`bytes`。我们将输入转换为二进制形式，将状态机初始化为零状态，然后一次将一位输入 FSM，对序列进行编码。一个额外的步骤是在用“约束长度”比特编码之前用零填充流的结尾。填充确保状态机以确定的状态完成编码过程，帮助解码器选择一条幸存路径。这种填充称为“零尾终止”。

## 解码器

为了解码，我们需要遍历各种网格路径。于是我写了一个`TrellisPath`类。该类将路径保存为状态列表。它还保存了与路径、路径中的最后状态、路径长度和路径度量相对应的输入位列表。它还实现了一个`add_2_path`方法，该方法允许向路径添加一个新的状态、一个位输入和一个分支度量。`duplicate_path`类方法允许复制路径，因为路径必须被复制。

使用`TrellisPath`我们可以实现解码器。首先，将接收到的序列分解成 n 位的组，并初始化幸存路径列表

```
received_codewords = [tuple(data[i: i+self.n]) for i in range(0, len(data), self.n)]    
surviving_paths = [TrellisPath()]
```

然后迭代每个码字。对于每个码字，迭代`surviving_paths`。对于每条路径，找到由`possible_input`指定的`possible_transitions`，并以路径和分支度量为特征。这些`possible_transitions`中的每一个在分支时都会产生 *k* 条潜在路径。然而，网格中的每个节点有多个`entring_paths`。在进入节点的那些路径中，具有最小路径度量的路径被选择以形成`new_path`，在下一个码字的迭代期间被认为是`surviving_paths`。代码如下所示。

# 完整的代码和例子

[要点](https://gist.github.com/YairMZ/b88e594047c7b5366053cd7fb375a94f)还包括作为单个可运行文件的全部代码。在底部，我添加了两个运行它的最小的例子。