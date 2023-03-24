# Swift Leetcode 系列:删除字符串 II 中所有相邻的重复项

> 原文：<https://medium.com/nerd-for-tech/swift-leetcode-series-remove-all-adjacent-duplicates-in-string-ii-ef696ef909e4?source=collection_archive---------9----------------------->

## 了解堆栈如何用于复制

![](img/b45aa049f34b29f7a745cbe5e0c2d615.png)[](https://theswiftnerd.com/remove-all-adjacent-duplicates-in-string-ii-leetcode-1209/) [## 删除字符串 II 中所有相邻的重复项(Leetcode 1209)

### 难度:链接:April Leetcoding 挑战:第 16 天给定一个字符串 s，一个 k 去重包括选择 k…

theswiftnerd.com](https://theswiftnerd.com/remove-all-adjacent-duplicates-in-string-ii-leetcode-1209/) 

你也可以通过上面的链接在 Swift Nerd 博客上阅读完整的故事。

# 问题

给定一个字符串`s`，一个 *k* *重复删除*包括从`s`中选择`k`个相邻且相等的字母，并删除它们，使得被删除的子字符串的左侧和右侧连接在一起。

我们在`s`上重复删除`k`，直到我们再也做不到。在所有这样的重复删除完成后，返回最后一个字符串。

保证答案是唯一的。

## 例子

```
**Input:** s = "abcd", k = 2
**Output:** "abcd"
**Explanation:** There's nothing to delete.**Input:** s = "deeedbbcccbdaa", k = 3
**Output:** "aa"
**Explanation:** First delete "eee" and "ccc", get "ddbbbdaa"
Then delete "bbb", get "dddaa"
Finally delete "ddd", get "aa"**Input:** s = "pbbcggttciiippooaais", k = 2
**Output:** "ps"
```

## 限制

*   `1 <= s.length <= 10^5`
*   `2 <= k <= 10^4`
*   `s`只包含小写英文字母。

# 解决办法

该问题是删除字符串中所有相邻重复项问题的修改版本。这里的问题是重复移除与相邻的**和与**相等的**的字符串元素(重点是这两个)。我们可以提出一种强力逻辑来重复检查长度为 k 的相等子串，并将其从串中移除。然而，时间复杂度将非常高。如果我们使用额外的空间，我们可以很容易地在 O(N)中做到这一点。如果你知道一种无需额外空间和 O(N)时间就能解决它的方法，请在下面评论:)。**

## 堆

堆栈是检查最后遍历的元素并删除子串(如果子串的长度等于 k)的完美工具。由于我们需要有一个字符和连续运行长度的映射，我们可以有一个类对，并在其中保存字母和计数的映射(或者也可以使用元组，但可读性较差)。这样，当字母数变为 k 时，就很容易删除子串。最后一步仍然是从堆栈中构造回字符串。因为堆栈可以有计数为 O(N) 的重复元素

## 空间= **O(N)**

# 感谢您的阅读。如果你喜欢这篇文章，并发现它很有用，请分享并像野火一样传播它！

你可以在[swift 网站](https://theswiftnerd.com/)|[LinkedIn](https://www.linkedin.com/in/varunrathi28/)|[Github](https://github.com/varunrathi28)上找到我

Time = **O(N)**

Space = **O(N)**

Thank you for reading. If you liked this article and found it useful, share and spread it like wildfire!

You can find me on [TheSwiftNerd](https://theswiftnerd.com/) | [LinkedIn](https://www.linkedin.com/in/varunrathi28/) | [Github](https://github.com/varunrathi28)