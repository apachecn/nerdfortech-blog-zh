# 初级开发人员-编码面试问题和答案—第 1 部分

> 原文：<https://medium.com/nerd-for-tech/junior-dev-coding-interview-questions-answers-part-1-4bfd9e8e9374?source=collection_archive---------7----------------------->

## 面试指南

![](img/8dca6d01288f3b629e3b1160c103a4e3.png)

[www.freepik.com 新闻摄影制作的电脑照片](https://www.freepik.com/photos/computer)

编码挑战是我在初级开发阶段最大的恐惧之一。我是一个普通的程序员。但一旦我知道了它们背后的机制，它就成了我最喜欢的打发时间的方式之一。

> 如果你觉得自己在软件开发的某个领域很弱，并且想做得更好，只有一条路可走，那就是练习！练习！练习！

如果我一段时间不练习，我也会感到虚弱。我会在重新开始练习后的一两天内把它捡起来。

我整理了一些我面临的问题和我提出的一些问题。本系列的编程语言选择是 C++。我没有像往常一样使用 swift 的原因是大多数面试官喜欢看到我们使用原始技术，而不是使用内置功能。而谁不爱 C++，那是传说。

**我们将讨论四个简单的挑战。**

1.  阶乘
2.  回文
3.  沙漏的最大总和
4.  找出数组中最大和最小的数字。

# 阶乘

**问题:写一个函数，从用户那里获取一个正整数，并计算这个数的阶乘。用递归来解决这个问题。**

> 递归-
> 
> 递归过程或定义的重复应用。

考虑输入= *n* 。那么我们需要

> n*(n-1)*(n-2)*(n-3)* ….3*2*1

例: *n=4* 那么其

4*(4–1)*(4–2)*(4–3)

所以 *4 的阶乘是 24* 。

但是我们需要用递归来解决这个问题。所以公式是

> n！或者 n = n*(n-1)的阶乘！
> 
> n！如果 n = 0 或 n = 1，则= 1

我们所需要的是，如果 *n 大于 1* ，那么我们需要*n * n 的阶乘，否则返回 1*

```
#include<iostream>
using namespace std;

int factorial(int n);//Declaring our factorial function

int main()
{
    int n;

    cout << "Enter a positive integer: ";
    cin >> n;

    cout << "Factorial of " << n << " = " << factorial(n); //The call

    return 0;
}//Defining our factorial function
int factorial(int n)
{
    if(n > 1)
        return n * factorial(n - 1);
    else
        return 1;
}
```

# 回文

> 如果一个字符串的倒数与字符串相同，则称该字符串为回文。

**问题:写一个函数检查给定的字符串是否是回文。
比如“阎娜”是回文，但“特里莎”不是回文。**

面试官很少要求用递归来解决这个问题。但是他们会喜欢的，相信我。

**让我们来看看如何解决这个问题。**

假设我们发送一个单词作为输入，那么我们需要这个单词的第一个和最后一个元素的索引。我们会用这个从前到后比较它们是否相等。我们必须增加起始索引变量，减少结束索引变量，直到起始索引变量大于或等于结束索引变量。

例如，如果单词是 Peep，那么

*开始= 0，结束= 3*

运行递归，直到开始大于或等于结束。

```
++start & --end
```

1.  Peep 的第一个元素和最后一个元素相同。- *现在开始=1 &结束= 2*
2.  Peep 的第二要素和第三要素是相同的。- *现在开始=2 &结束* = 1

现在检查成功了，我们有了一个回文。

当比较元素时，如果它们中的任何一个不相等，我们返回 false

```
#include <iostream>using namespace::std;// Palindrome functionbool isPalindrome(const string &input, int start, long end)
{if (start>=end)return true;if (input[end]!=input[start])return false;return isPalindrome(input, ++start, --end);}//Main functionint main(int argc, const char * argv[]) {string input;cout<<"Enter a string to check whether it's palindrome or not: ";cin>>input;cout<<"Is "<<input<<" a Palindrome, say 1 for true and 0 for false: \n Answer is "<<isPalindrome(input, 0, input.length()-1)<<"\n";return 0;}
```

***伙计们，我该走了。***

其余将在[第二部**中讲述。我打算把它作为一个系列来运作。所以期待吧。**](/nerd-for-tech/junior-dev-coding-interview-questions-answers-part-2-b416f956ce5a)

请在这份报告中找到密码-

[](https://github.com/Rajaikumar-iOSDev/Coding-challenges-for-junior-devs) [## rajaikumar-IOs dev/初级开发人员的编码挑战

### ⌨️:我整理了一些我遇到的问题和我提出的一些问题。这种编程语言的选择…

github.com](https://github.com/Rajaikumar-iOSDev/Coding-challenges-for-junior-devs) 

***注意*** :对于不使用 Xcode 的人，使用“main.cpp”文件即可。