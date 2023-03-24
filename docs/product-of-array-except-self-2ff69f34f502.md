# 除自身以外的数组乘积

> 原文：<https://medium.com/nerd-for-tech/product-of-array-except-self-2ff69f34f502?source=collection_archive---------2----------------------->

(Leetcode)

给定一个整数数组`nums`，返回*一个数组* `answer` *使得* `answer[i]` *等于* `nums` *中除* `nums[i]`之外的所有元素的乘积。

`nums`的任何前缀或后缀的乘积**保证**适合一个 **32 位**整数。

你必须写一个在`O(n)`时间内运行并且不使用除法运算的算法。

**例 1:**

```
**Input:** nums = [1,2,3,4]
**Output:** [24,12,8,6]
```

**例二:**

```
**Input:** nums = [-1,1,0,-3,3]
**Output:** [0,0,9,0,0]
```

# **接近**

1.  使用除法运算符

时间复杂度= O(n*n)其中 n 是给定数组/向量的大小

空间复杂度= O(n ),在这里你将值存储在一个向量中

```
**LOGIC** : Multiply the other elements except nums[i], for eg : arr = [1,2,3] so for 
ans[0] = 2*3 = 6
ans[1] = 1*3 = 3 
ans[2] = 1*2 = 2 that is multiply the other elements except that element , so use the concept of using two loops. PS : Try to write on your own :) 
```

2.不使用除法运算符

时间复杂度= O(n)其中 n 是给定数组或向量的大小

空间复杂度= O(n)

```
**LOGIC INVOLVED :**1\. I am using two vectors - one to store the left product and the other to store the right product. 
For eg :Taking the example [1,2,3]
Left Product is the product of all elements to the left of the given number , leftProduct[0]= 1 **(initialise all elements with 1)**
leftProduct[1]=leftProduct[0]*nums[0] = 1*1 = 1
leftProduct[2]=leftProduct[1]*nums[1] = 1*2 = 2Similarly , rightProd[2] = 1 **(initialise all elements with 1)**
rightProduct[1]=rightProduct[2]*nums[2] = 1*3 = 3
rightProduct[0]=rightProduct[1]*nums[1] = 3*2 = 6nums[0] = leftProduct[0]*rightProd[0] = 1*6 = 6
nums[1] = leftProduct[1]*rightProd[1] = 1*3 = 3
nums[2] = leftProduct[2]*rightProd[2] = 2*1 = 2**CODE :****class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {

        vector<int>leftProd(nums.size(),1);
        vector<int>rightProd(nums.size(),1);

        for(int i=1;i<nums.size();i++)
            leftProd[i]=leftProd[i-1]*nums[i-1];

        for(int i=nums.size()-1;i>=1;i--)
            rightProd[i-1] = rightProd[i]*nums[i];

        for(int i=0;i<nums.size();i++)
            nums[i] = leftProd[i]*rightProd[i];

        return nums;
    }
};**
```

希望这有所帮助！继续编码！

既然你喜欢看我的博客，为什么不请我喝杯咖啡，支持我的工作呢！！[https://www.buymeacoffee.com/sukanyabharati](https://www.buymeacoffee.com/sukanyabharati)☕