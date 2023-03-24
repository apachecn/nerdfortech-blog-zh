# 冒泡排序

> 原文：<https://medium.com/nerd-for-tech/bubble-sort-d42a8e12239b?source=collection_archive---------26----------------------->

冒泡排序算法的实现。

![](img/bb4178c2e8a73f8a30602311819958a3.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上[温刚斋](https://unsplash.com/@wgzhai?utm_source=medium&utm_medium=referral)的照片

## 简介:

*   **冒泡排序**用于对给定数组中的元素进行排序。
*   它通常在数组很小的时候使用。
*   当列表部分排序时，它工作得很好。
*   O(n)是冒泡排序的时间复杂度。

## **冒泡排序是如何工作的？？**

让我们明白…

它在每次迭代中比较相邻的值，如果它们的顺序错误(第一个值大于第二个值),则交换它们。

在每次迭代中，较大的值放在列表的末尾，(len(list)-i)。

这里‘I’是第 I 次迭代。

## 实施:

```
def bubble_sort(arr):
    for j in range(len(arr)-1):
        swapped = False
        for i in range(len(arr)-1-j):
            if arr[i]>arr[i+1]:
                arr[i], arr[i+1] = arr[i+1], arr[i]
                swapped=True
        if not swapped:
            breakif __name__ == '__main__':

    array = [3,5,2,7,8,4]
    bubble_sort(array)
    print(array)Output:[2,3,4,5,7,8]
```

*   在上面的 python 代码中，我们使用'**交换的**变量来优化算法。
*   如果我们的算法已经排序，那么交换值为假，它打破了循环。
*   或者在第 n 次迭代中，列表被完全排序，然后在该迭代之后，交换的值为假，并且它中断循环。

## 结论:

*   我们已经了解了冒泡排序的工作原理。
*   我们还看到了冒泡排序的实现。