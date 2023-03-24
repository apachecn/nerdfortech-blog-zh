# 数据科学的 NumPy 完全指南

> 原文：<https://medium.com/nerd-for-tech/a-complete-guide-on-numpy-for-data-science-c54f47dfef8d?source=collection_archive---------1----------------------->

## 从基础到高级学习和实现 NumPy 的指南，用于处理数据时的探索性数据分析。

# 什么是 NumPy？

NumPy 是一个 Python 库，可以帮助分析数据。它由处理数据科学的个人使用。这是一个线性代数库，绑定到 C 库，速度非常快。

# 如何安装 NumPy？

要使用 pip 安装 NumPy:

> pip 安装数量

要使用 Anaconda 安装 Numpy:

> 康达安装数量

# 什么是 NumPy 数组？

在使用 NumPy 进行数据科学研究时，我们通常需要处理 NumPy 数组。这些阵列有两种类型:

1.  矩阵

矩阵通常是二维的，但也可以只有一行或一列。

2.向量

另一方面，向量是严格一维的。

# 如何使用列表创建 NumPy 数组？

**→导入库**

```
**>>> import** numpy **as** np
```

**→创建一个列表，然后将其转换为一维数组。**

```
>>> list1 **=** [11,23,34,56]
>>> list1
[11, 23, 34, 56]>>> np.array(list1)
array([11, 23, 34, 56])>>> array1 **=** np.array(list1)
>>> array1
array([11, 23, 34, 56])
```

**→创建一个列表列表，并将其转换为二维数组。**

```
>>> list2 **=** [[11,22,33],[55,66,77],[88,99,100]]
>>> np.array(list2)
array([[ 11,  22,  33],
       [ 55,  66,  77],
       [ 88,  99, 100]])
```

如上所述，有两个维度，即行和列。维度也用数组所在的括号数来表示。有一个圆括号和一个方括号将数组括起来，因此它是二维的。

# 如何使用内置方法创建 NumPy 数组？

**→使用类似于 python 范围的 arange 方法创建。参数是开始、停止和步长值。第一个值是“start ”,和 range 函数一样上升到(stop-1)。**

```
>>> np.arange(0,10,1)
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])>>> np.arange(0,10,2)
array([0, 2, 4, 6, 8])
```

**→创建全零数组。**

```
>>> np.zeros(5)
array([0., 0., 0., 0., 0.])>>> np.zeros((3,2))
array([[0., 0.],
       [0., 0.],
       [0., 0.]])
```

**→创建全 1 数组。**

```
>>> np.ones(5)
array([1., 1., 1., 1., 1.])>>> np.ones((3,2))
array([[1., 1.],
       [1., 1.],
       [1., 1.]])
```

**→创建一个数组，数组中的值在一个区间内等距分布。它接受参数:开始、停止、值的数量。**

```
>>> np.linspace(1,20,5)
array([ 1\.  ,  5.75, 10.5 , 15.25, 20\.  ])
```

如上所示，它返回间隔 1 到 20 的 5 个数字，这些数字间隔相等。

**→创建单位矩阵。**

```
>>> np.eye(3)
array([[1., 0., 0.],
       [0., 1., 0.],
       [0., 0., 1.]])
```

**→用均匀分布的随机数(0–1)创建一个数组。**

```
>>> np.random.rand(3)
array([0.13426   , 0.22672772, 0.98574852])>>> np.random.rand(3,2)
array([[0.13636649, 0.3366877 ],
       [0.36993761, 0.02392286],
       [0.20869183, 0.59256244]])
```

**→用正态分布的随机数创建数组(以 0 为中心)。**

```
>>> np.random.randn(3)
array([ 0.71105797, -0.33395766,  0.67756835])>>> np.random.randn(4,2)
array([[ 1.21447908,  0.6830743 ],
       [-0.28203856,  0.16459752],
       [-0.32451067, -0.1618622 ],
       [-0.9331776 ,  0.6281955 ]])
```

**→使用 randint()创建一个包含随机整数的数组，其中要传递的参数是 low、high 和 size。低的是包容性的，高的是排他性的。**

```
>>> np.random.randint(1,50)
49>>> np.random.randint(1,50,5)
array([27, 43, 44, 39, 16])
```

# NumPy 数组的属性和方法是什么？

```
>>> arr1 **=** np.arange(10,35)
>>> arr1
array([10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34])
```

**→ Reshape 方法把数组改成新的形状。**

```
>>> arr1.reshape(5,5)
array([[10, 11, 12, 13, 14],
       [15, 16, 17, 18, 19],
       [20, 21, 22, 23, 24],
       [25, 26, 27, 28, 29],
       [30, 31, 32, 33, 34]])
```

如果在重塑矩阵时没有填充，那么它将给出一个错误。确保行数乘以列数等于数组中的元素数。

```
>>> arr1.reshape(3,3)
**---------------------------------------------------------------------------**
**ValueError**                                Traceback (most recent call last)
**<ipython-input-26-2c9beb517969>** in <module>
**----> 1** arr1**.**reshape**(3,3)****ValueError**: cannot reshape array of size 25 into shape (3,3)
```

**→查找数组中的最大值和最小值。**

```
>>> arr1
array([10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34])>>> arr1.max()
34>>> arr1.min()
10
```

若要知道最大值或最小值所在的索引，请使用 argmax()或 argmin()。

```
>>> arr1.argmax()
24>>> arr1.argmin()
0
```

**→ shape()方法求数组的形状。**

```
>>> arr1.shape
(25,)
```

这表示该数组是一维的，有 25 个元素。

```
>>> arr2 **=** arr1.reshape(5,5)
>>> arr2.shape
(5, 5)
```

这表示该数组是二维的，有 5 行 5 列。

**→查找数组中元素的数据类型。**

```
>>> arr1.dtype
dtype('int32')
```

# 如何对一维 NumPy 数组中的元素进行索引和选择？

```
>>> arr1
array([10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34])
```

**→使用切片符号从数组中挑选元素。**

```
>>> arr1[3]
13>>> arr1[1:5]
array([11, 12, 13, 14])
```

**→使用切片改变数组中的值，即广播。**

```
>>> arr1[1:5] **=** 50
>>> arr1
array([10, 50, 50, 50, 50, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34])
```

现在，让我们将数组切片并广播。请注意切片数组的值以及原始数组的值是如何变化的。

```
>>> arr2 **=** arr1[10:15]
>>> arr2
array([20, 21, 22, 23, 24])>>> arr2[:] **=** 25
>>> arr2
array([25, 25, 25, 25, 25])>>> arr1
array([10, 50, 50, 50, 50, 15, 16, 17, 18, 19, 25, 25, 25, 25, 25, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34])
```

要避免这种情况，请复制该数组，然后广播。

```
>>> arr_copy **=** arr1.copy()
>>> arr_copy
array([10, 50, 50, 50, 50, 15, 16, 17, 18, 19, 25, 25, 25, 25, 25, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34])>>> arr_copy[:] **=** 1
>>> arr_copy
array([1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1])>>> arr1
array([10, 50, 50, 50, 50, 15, 16, 17, 18, 19, 25, 25, 25, 25, 25, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34])
```

# 如何对二维数组中的元素进行索引和选择？

```
>>> arr2 **=** np.array([[1,2,3],[4,5,6],[7,8,9]])
>>> arr2
array([[1, 2, 3],
       [4, 5, 6],
       [7, 8, 9]])
```

**→使用括号索引**

对于索引，先传递行值，然后传递列值。

```
>>> arr2[0][1]
2
```

您也可以使用下面的符号，其中行和列的值用逗号分隔。

```
>>> arr2[0,1]
2
```

**→得到矩阵的子部分。**

要获得子矩阵，请使用切片。在下面的示例中，选择行 0 和行 1 以及列 1 和列 2。

```
>>> arr2[:2,1:]
array([[2, 3],
       [5, 6]])
```

# 如何使用布尔数组执行条件选择？

```
>>> arr1
array([10, 50, 50, 50, 50, 15, 16, 17, 18, 19, 25, 25, 25, 25, 25, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34])>>> bool_arr **=** arr1 **>** 20
>>> bool_arr
array([False,  True,  True,  True,  True, False, False, False, False, False,  True,  True,  True,  True,  True,  True,  True,  True, True,  True,  True,  True,  True,  True,  True])
```

**→获取布尔值为真的值。**

```
>>> arr1[bool_arr]
array([50, 50, 50, 50, 25, 25, 25, 25, 25, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34])>>> arr1[arr1**<**20]
array([10, 15, 16, 17, 18, 19])
```

# 如何用数组运算进行数组？

```
>>> arr3 **=** np.arange(0,5)
>>> arr3
array([0, 1, 2, 3, 4])
```

**→基本操作**

```
>>> arr3 **+** arr3
array([0, 2, 4, 6, 8])>>> arr3 **-** arr3
array([0, 0, 0, 0, 0])>>> arr3 ***** arr3
array([ 0,  1,  4,  9, 16])
```

# 如何用标量运算执行数组？

标量意味着只是一个单一的数字。因此，在处理标量和数组操作时，NumPy 将标量传播到数组中，并执行元素操作。

```
>>> arr3
array([0, 1, 2, 3, 4])>>> arr3 **+** 5
array([5, 6, 7, 8, 9])>>> arr3 **-** 2
array([-2, -1,  0,  1,  2])>>> arr3 ***** 3
array([ 0,  3,  6,  9, 12])>>> arr3 **/** 6
array([0\.        , 0.16666667, 0.33333333, 0.5       , 0.66666667])>>> arr3 ****** 3
array([ 0,  1,  8, 27, 64], dtype=int32)
```

在 python 中，0/0 给出一个错误，但是在 NumPy 中，当执行 0/0 时，它给出一个警告并返回一个 NAN (Null)值。

```
>>> arr3
array([0, 1, 2, 3, 4])>>> arr3**/**arr3
D:\Anaconda3\lib\site-packages\ipykernel_launcher.py:1: RuntimeWarning: invalid value encountered in true_divide
  """Entry point for launching an IPython kernel.
array([nan,  1.,  1.,  1.,  1.])>>> 1**/**arr3
D:\Anaconda3\lib\site-packages\ipykernel_launcher.py:1: RuntimeWarning: divide by zero encountered in true_divide
  """Entry point for launching an IPython kernel.
array([       inf, 1\.        , 0.5       , 0.33333333, 0.25      ])
```

# 如何执行 NumPy 数组通用函数？

**→求数组中每个元素的平方根。**

```
>>> np.sqrt(arr3)
array([0\.        , 1\.        , 1.41421356, 1.73205081, 2\.        ])
```

**→求数组中每个元素的指数。**

```
>>> np.exp(arr3)
array([ 1\.        ,  2.71828183,  7.3890561 , 20.08553692, 54.59815003])
```

**→寻找数组的最大值。**

```
>>> arr3.max()
4
```

**→三角函数**

```
>>> np.sin(arr3)
array([ 0\.        ,  0.84147098,  0.90929743,  0.14112001, -0.7568025 ])>>> np.cos(arr3)
array([ 1\.        ,  0.54030231, -0.41614684, -0.9899925 , -0.65364362])
```

**→对数函数**

```
>>> np.log(arr3)
D:\Anaconda3\lib\site-packages\ipykernel_launcher.py:1: RuntimeWarning: divide by zero encountered in log
  """Entry point for launching an IPython kernel.
array([      -inf, 0\.        , 0.69314718, 1.09861229, 1.38629436])
```

> 有关 NumPy 的各种方法和功能的更多详细信息，请查看官方文档[此处](https://numpy.org/doc/stable/)。
> 
> 在此查阅笔记本代码[。](https://github.com/jayashree8/Machine_learning_EDA/blob/master/Guide/NumPy/NumPy.ipynb)

# 可供参考的书籍:

[](https://amzn.to/34ma5Pt) [## Python 数据分析:使用 Python、NumPy 和…开始数据分析的终极指南

### 你是否完全是编程新手，想学习如何编码，但不知道从哪里开始？你想……](https://amzn.to/34ma5Pt) [](https://amzn.to/3fMFkbD) [## 用于数据分析的 Python，2e:与 Pandas、Numpy 和 Ipython 的数据争论

### 用于数据分析的 Python，2e:与熊猫的数据争论，Numpy 和 Ipython: Amazon.in: Mckinney，Wes: Books](https://amzn.to/3fMFkbD) [](https://amzn.to/3usg0wO) [## Python 数据分析:使用 Pandas、NumPy 和 Matplotlib

### 探索最新的 Python 工具和技术，帮助您应对数据采集和分析领域。你会…](https://amzn.to/3usg0wO) 

> *联系我:* [*LinkedIn*](https://www.linkedin.com/in/jayashree-domala8/)
> 
> *查看我的其他作品:* [*GitHub*](https://github.com/jayashree8)