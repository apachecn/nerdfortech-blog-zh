# 75.排序颜色

> 原文：<https://medium.com/nerd-for-tech/75-sort-colors-15768309bf2f?source=collection_archive---------0----------------------->

**(Leetcode: Medium)**

![](img/65d65e8f35c668e6c359b8c4ea0bbbcd.png)

给定一个带有红色、白色或蓝色的`n`对象的数组`nums`，将它们在适当的位置排序，使相同颜色的对象相邻，颜色顺序为红色、白色和蓝色。

我们将使用整数`0`、`1`和`2`分别代表红色、白色和蓝色。

您必须在不使用库的排序函数的情况下解决这个问题。

**例 1:**

```
**Input:** nums = [2,0,2,1,1,0]
**Output:** [0,0,1,1,2,2]
```

**例 2:**

```
**Input:** nums = [2,0,1]
**Output:** [0,1,2]
```

**例 3:**

```
**Input:** nums = [0]
**Output:** [0]
```

**例 4:**

```
**Input:** nums = [1]
**Output:** [1]
```

**约束:**

*   `n == nums.length`
*   `1 <= n <= 300`
*   `nums[i]`是`0`、`1`，还是`2`。

今天我将讨论解决这个问题的三种方法。

1.  第一种方法(使用**分类**

让我们以上面的例子[2 0 2 1 1 0]为例。

```
Step 1 : Sort the array 
         So array becomes [0 0 1 1 2 2] which is the required answer
         **Time Complexity - O(nlogn)
         Space Complexity - O(1)** 
```

2.第二种方法(使用**计数分类**

```
Use three counter variables to keep track of the count of total 0’s , 1’s and 2’s which will take a time complexity of O(n).Now again traverse the array and put the array elements in order by using the counter variable , so again time complexity is O(n)So total **time complexity - O(2n)** which means it is done in two passes.
**Space Complexity - O(1)** The time complexity can further be optimised. Let us look in the third approach, how?Code : **class Solution {
public:
    void sortColors(vector<int>& nums) {
        int count0 =0, count1=0, count2=0;
        for(int i=0;i<nums.size();i++)
        {
            if(nums[i]==0)
                count0++;
            else if(nums[i]==1)
                count1++;
            else
                count2++;
        }

        int i=0;
        while(count0>0)
        {
            nums[i++]=0;
            count0--;
        } 

        while(count1>0)
        {
            nums[i++]=1;
            count1--;
        }

        while(count2>0)
        {
            nums[i++]=2;
            count2--;
        }

    }
};**
```

3.第三种方法(利用**“荷兰国旗”的概念**的问题)

```
Now this approach is basically based on the concept of **The Dutch National Flag Problem.** This is basically an approach where we divide our array into partitions which are :**1\. array[0 to low-1] = 0 is present
2\. array[low to mid-1] = 1 is present
3\. array[mid to high-1] = unknown
4\. array[high to n(size of array)] = 2 is present**I have attached a few links and resources for your better understanding.Code :**class Solution {
public:
    void sortColors(vector<int>& nums) {
        int low = 0, mid = 0, high = nums.size()-1;
        while(mid<=high)
        {
            if(nums[mid]==0)
            {
                swap(nums[mid],nums[low]);
                low++;
                mid++;
            }
            else if(nums[mid]==1)
                mid++;

            else
            {
                swap(nums[mid],nums[high]);
                high--;
            }
        }
    }
};**Whenever I see 0, I swap it with element present at low position.
When I get 1, I just let it be and when i encounter 2 I swap it with element at high position. Similarly I keep incrementing and decrementig the variables - high, low and mid. 
```

希望有帮助！继续编码！

有用的资源:

1.  [https://www . educative . io/edpresso/the-Dutch-national-flag-problem-in-CPP](https://www.educative.io/edpresso/the-dutch-national-flag-problem-in-cpp)
2.  [https://www . geeks forgeeks . org/sort-an-array-of-0s-1s-and-2s/](https://www.geeksforgeeks.org/sort-an-array-of-0s-1s-and-2s/)
3.  [https://www.youtube.com/watch?v=oaVa-9wmpns&list = plguwdvibif 0 RPG 3 ictpu 74 ywbq 1 cabk 2&index = 2](https://www.youtube.com/watch?v=oaVa-9wmpns&list=PLgUwDviBIf0rPG3Ictpu74YWBQ1CaBkm2&index=2)

既然你喜欢看我的博客，为什么不请我喝杯咖啡，支持我的工作呢！！[https://www.buymeacoffee.com/sukanyabharati](https://www.buymeacoffee.com/sukanyabharati)☕