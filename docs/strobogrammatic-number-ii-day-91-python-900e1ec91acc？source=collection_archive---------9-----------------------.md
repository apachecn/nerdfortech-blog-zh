# 频闪信号编号 II —第 91 天(Python)

> 原文：<https://medium.com/nerd-for-tech/strobogrammatic-number-ii-day-91-python-900e1ec91acc?source=collection_archive---------9----------------------->

![](img/b0a6996b156711e5891d869a6965d121.png)

照片由[金智秀](https://unsplash.com/@soologue?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

今天的问题是 leetcode 上一个中等标签的问题。让我们看看问题陈述。

[**247**](https://leetcode.com/problems/strobogrammatic-number-ii/) **。频闪信号编号 II**

给定一个整数`n`，返回所有长度为`n`的**频闪信号数**。你可以按**任何顺序**返回答案。

一个**闪光数字**是一个旋转`180`度后看起来相同的数字(上下颠倒)。

例 1:

```
**Input:** n = 2
**Output:** ["11","69","88","96"]
```

**例 2:**

```
**Input:** n = 1
**Output:** ["0","1","8"]
```

**约束:**

*   `1 <= n <= 14`

在我们开始解决这个问题之前，让我们先了解一下什么是频闪数字。一个**频闪数字**是一个旋转 180 度看起来不变的数字。那么**是哪些频闪号码呢？0，1，8 是频闪的数字。6 和 9 一起使用时，形成频闪数字。**

我们有“n ”,它是我们形成的数字中的位数，如果它们形成一个频闪数字，就必须进行检查。

解决这个问题的一种方法是创建一个字典，存储每个可以形成频闪数字的数字及其相应的值。

```
char_list = ["0", "1", "6", "8", "9"]
strobogrammatic_dict = {"0":"0","1":"1","6":"9","8":"8","9":"6"}
```

我们将使用 2 根弦；一个保存数字形成另一个保存其可能的旋转数。当位数等于 n 时，如果形成的数等于一个旋转数中保存的数，我们会将其保存为结果的一部分。

```
class StrobogrammaticFinder:
  def findStrobogrammatic(self, n: int) -> List[str]:
      output = []
      char_list = ["0", "1", "6", "8", "9"]
      strobo_dict = {"0":"0","1":"1","6":"9","8":"8","9":"6"}
      def dfs(tmp, st_tmp):
          if len(tmp) == n:
             if tmp == st_tmp[::-1]:
                if str(int(tmp)) == tmp:
                   output.append(tmp)
             return
          for i in range(len(char_list)):
              dfs(tmp+char_list[i],st_tmp+strobo_dict[char_list[i]])

      dfs("","")
      return(output)
```

# 复杂性分析。

**时间复杂度**

对于所有的 n 位数，我们有 5 位数可供选择。因此时间复杂性是 O(5^N).

**空间复杂度**

因为我们递归地调用 dfs 函数，并且内部递归调用以堆栈的形式存储。空间复杂性是 O(5^N).

当我们在 leetcode 上运行这段代码时，我们可能会遇到超时错误。有没有减少通话次数的方法？

我们必须找到所有 n 个地方的数字吗？我们可以试着只找到一半吗？如果我们找到刚好到一半的位置，我们可以旋转这个数并将其设置为另一半。或者更好的是，我们从两个极端位置开始向内移动。

让我们看看如何编码。

```
class StrobogrammaticFinder:
  def findStrobogrammatic(self, n: int) -> List[str]:
        output = []
        # Create a dictionary
        strobo_dict = {"0":"0","1":"1","6":"9","8":"8","9":"6"}
        start = 0
        end = n-1
        #Create a list of size n initialised with None
        result = [None]*n
        self.dfs(start, end, result, strobo_dict, output)
        return output

  def dfs(self, start, end, result, strobo_dict, output):
        if start > end:
            output.append(''.join(result))
            return
        #For each number in the dictionary
        for each_num in strobo_dict:
            #if it is single digit number
            if start!= end and each_num == '0' and start == 0:
                continue
            # if we have reached the middle
            if start == end and each_num in ('6','9'):
                continue

            result[start], result[end] = each_num, strobo_dict[each_num]
            self.dfs(start+1, end-1, result, strobo_dict, output)
```

# 复杂性分析。

**时间复杂度**

对于所有的 n 位数，我们有 5 位数可以选择，但是在上面的代码中，我们从两个极端位置开始向内移动。因此时间复杂性是 O(5^(N/2)).

**空间复杂度**

因为我们递归地调用 dfs 函数，并且内部递归调用以堆栈的形式存储。空间复杂性是 O(5^(N/2)).