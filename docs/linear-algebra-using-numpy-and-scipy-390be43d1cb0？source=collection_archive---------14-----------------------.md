# 使用 NumPy 和 SciPy 的线性代数

> 原文：<https://medium.com/nerd-for-tech/linear-algebra-using-numpy-and-scipy-390be43d1cb0?source=collection_archive---------14----------------------->

# NumPy

```
import numpy as np
```

# 两个向量的点积

![](img/4bd479991589e722495cba6ea5384cc2.png)

点积也可以用“@”来计算

![](img/c882bfecb81c305a6a5cd5ff6e4fe416.png)

# 交叉乘积

![](img/3cb144f3ab605a814c2a35ce3bac420c.png)

# 纯量乘法

![](img/2e36760ce1408bdf38aaed548342c37d.png)

# 矩阵乘法

numpy.matmul()函数用于返回两个矩阵的乘积。如果两个矩阵的形状没有对齐进行乘法运算，就会产生错误。

![](img/16d6a6bf7370c251023899cb938bf51f.png)

# 决定因素

![](img/b9a74ebf3d33ee9b1e3b304846b1686c.png)

# 解线性方程

x + 6y + 15z = 11

3x + 9y + 5z = 9

x + 4y + 7z = 6

可以使用 numpy.linalg.solve()求解上述线性方程的 x、y 和 z 值

![](img/545372d5f9005d20667e4fdf73c282ce.png)

# 矩阵的逆

此函数用于计算矩阵的乘法逆运算

![](img/d8be2699dc768286d0bebd944dc0c906.png)

# 添加

![](img/d4f8e820824aa637718d36b785f955a5.png)

# 减法

![](img/d282a91d46cb9de1930e67bf1a52617f.png)

# 增加

![](img/1a7a2bc287932ffdaeedd14458f4fd97.png)

# 标量除法

![](img/c9533fd84ae473e4c764f16545dc42bb.png)

# 矩阵的转置

![](img/e826a79a43f611960b1cc40690854759.png)

# 使用 SciPy 的线性代数

scipy.linalg 模块包含了线性代数的所有函数。它包含了 numpy.linalg 中的所有功能，但也有一些 numpy.linalg 中没有的高级功能

```
from scipy import linalg
```

# 解线性方程

![](img/7f4fabf18a74bbe737942e09fe035782.png)

# 寻找行列式

![](img/c6a9b05ad16e26b0f62a2a02c34704e7.png)

# 使用 linalg.inv()求矩阵的逆矩阵

![](img/0caa48a7950fe8ce5556d385a2134a16.png)

# 特征值和特征向量

我们可以使用 linalg.eig()函数找到特征值和特征向量。

![](img/9e4e52c6b740deb0a03ba840bf9c73e1.png)

想了解更多线性代数的函数，可以参考 [numpy](https://numpy.org/) 和 [scipy](https://www.scipy.org/) 的官方文档。

快乐学习！