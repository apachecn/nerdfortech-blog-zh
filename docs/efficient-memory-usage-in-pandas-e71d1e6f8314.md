# 熊猫的有效内存使用

> 原文：<https://medium.com/nerd-for-tech/efficient-memory-usage-in-pandas-e71d1e6f8314?source=collection_archive---------1----------------------->

当你加载一个大文件到 pandas Dataframe，object 中时，pandas 会消耗比预期更多的内存。在这篇博客中，我们将尝试通过一些最佳实践来缓解这一问题

![](img/b537aa1a80d22f9c8c9fd53f0311e403.png)

由 [Austin Distel](https://unsplash.com/@austindistel?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

**类型转换**

将数据类型视为多个用例

*   减少内存使用
*   不匹配类型
*   特定类型

让我们关注一下内存管理`int64`比`int8`占用更多的内存。当我们从 CSV 文件加载数据时，如果我们没有为每一列指定`dtype`，默认的`dtype`将是`int64`、`float64`或`object`。这些都是最大的数据类型。如果一列只存储从 0 到 20 的数字，`int8`就足够了。对于`float64`，可以用`float32`代替。

看看能省多少内存。检查用法的示例代码片段

```
import numpy as np
import pandas as pd
data = np.random.randint(-100, 100, size=(50000, 10))
df = pd.DataFrame(d)
print(“The total information about original object”)
print(df.info())
print(“The memory usage of all columns”)
print(df.memory_usage())
print('--------------')
intCols = df.select_dtypes(include=[‘int64’]).columns.tolist()
df[intCols] = df[intCols].apply(pd.to_numeric, downcast=’integer’)
print(“The total information about modified object”)
print(df.info()
print(“The memory usage of all columns”)
print(df.memory_usage())
```

这里有个结论，优化效果非常明显。总内存使用量从`1.5MB`下降到了`195KB`，非常可观。内存使用量减少了近`90 percent`。我们唯一做的事情是将数据类型从`int64`改为`int8`。

**使用类别类型代替对象**

在现实世界中，一些列的对象类型选择有限。例如，在一个有 1 亿行的数据集中，有一个名为`Continent`的列标识了七大洲之一，虽然这个数据集有 1 亿行，但我们知道世界上只有几百个地区和国家。如果每行存储一个对象，这是对内存的巨大浪费。在 pandas 中，我们可以使用`category`类型而不是`object`用于此列。

```
import pandas as pd
import string
import random
l = [''.join(random.choice(string.ascii_uppercase + string.digits) for _ in range(2))
for i in range(2000000)]
   s = pd.Series(l)
print("The original series memory usage")
print(s.memory_usage())
s = s.astype("category")
print("The modified series memory usage")
print(s.memory_usage())
```

从结果来看，category 显然表现得更好，一定要试一试！

**加载 CSV 文件时指定列类型**

这是我们已经知道的更多的提示，您可以在加载 CSV 文件时指定列类型。此外，您不需要加载所有列，您可以只加载列的子集。在现实世界中，大多数数据都存储在文件中。最常见的文件类型是 CSV。

希望这篇文章对你有所帮助，请在评论中分享你对改进这篇文章的想法，干杯！