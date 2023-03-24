# 从排序数组中删除重复项— LeetCode

> 原文：<https://medium.com/nerd-for-tech/remove-duplicates-from-sorted-array-leetcode-3b96b15c3f44?source=collection_archive---------4----------------------->

![](img/dd1e7fb14ae3ab4ef8469cacc0d1ce5b.png)

由 [Roman Synkevych](https://unsplash.com/@synkevych?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

给定一个按**非降序**排序的整数数组`nums`，就地 移除重复的 [**，使得每个唯一元素只出现**一次**。要素的**相对顺序**应与**保持一致**。**](https://en.wikipedia.org/wiki/In-place_algorithm)

因为在某些语言中不可能改变数组的长度，所以您必须将结果放入数组`nums`的**第一部分**。更正式的说法是，如果删除重复项后还有`k`元素，那么`nums`的第一个`k`元素应该保存最终结果。除了第一个`k`元素之外，你留下什么并不重要。

将最终结果放入 `nums`的第一个 `k` *槽后，返回`k` *。**

不要**而不要**为另一个数组分配额外的空间。你必须用 O(1)个额外内存通过**就地修改输入数组**[](https://en.wikipedia.org/wiki/In-place_algorithm)**来做到这一点。**

****约束:****

*   **`0 <= nums.length <= 3 * 104`**
*   **`-100 <= nums[i] <= 100`**
*   **`nums`按**非递减**顺序排序**

****自定义判断:****

**法官将使用以下代码测试您的解决方案:**

```
int[] nums = [...]; // Input array
int[] expectedNums = [...]; // The expected answer with correct lengthint k = removeDuplicates(nums); // Calls your implementationassert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```

**如果所有的断言都通过了，那么你的解决方案就会被接受。**

## **我的解决方案—**

```
**class solution** {
  **public:** 
   **int removeDuplicates**(vector<**int**>& nums)//given parameter's vector
   {
 **unordered_set**<**int**> s; // taken a set as a variable
      **for**(**int** i:nums) // mapped through all the elements of nums
        s.**insert**(i); //inserted the non duplicate elements in s
      nums.**clear**() // emptied the vector nums
      **for**(**int** i:s)
        nums.**push_back**(i) //appended the elements from s to nums
      **sort**(nums.**begin**(), nums.**end**()) //sorted nums
      **return** s.**size**()
   }
}
```

> **运行时间: **20 毫秒****
> 
> **内存使用: **18.9 MB****

## **其他解决方案—**

```
**class** **Solution** { 
  **public**: 
  **int** **removeDuplicates**(vector<**int**>& nums) 
  { 
    **int** j = 0; 
    **int** n = nums.size(); 
    **if**(n <= 1) **return** n; //return the size of nums if n<=1
    **for**(**int** i = 1; i<n; i++)
    { 
      **if**(nums[i] != nums[j]) 
      { 
         j++; 
         nums[j] = nums[i]; //
      } 
    } 
    **return** j+1; 
  } 
};
```

> **运行时间: **16 毫秒****
> 
> **内存使用: **18.2 MB****