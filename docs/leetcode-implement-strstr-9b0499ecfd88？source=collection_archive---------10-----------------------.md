# LeetCode —实现 strStr()

> 原文：<https://medium.com/nerd-for-tech/leetcode-implement-strstr-9b0499ecfd88?source=collection_archive---------10----------------------->

# 问题陈述

实现 **strStr()** 。

返回针在草堆中第一次出现的索引，如果**针**不是**草堆**的一部分，则返回 **-1** 。

**澄清:**

当**针**为空串时我们应该返回什么？这是在面试中问的一个很好的问题。

针对这个问题，当**针**为空字符串时，我们将返回 0。这与 C 的 **strstr()** 和 Java 的 **indexOf()** 是一致的。

问题陈述摘自:[https://leetcode.com/problems/implement-strstr](https://leetcode.com/problems/implement-strstr)

例 1:

```
Input: haystack = "hello", needle = "ll"
Output: 2
```

**例 2:**

```
Input: haystack = "aaaaa", needle = "bba"
Output: -1
```

**例 3:**

```
Input: haystack = "", needle = ""
Output: 0
```

**约束:**

```
- 0 <= haystack.length, needle.length <= 5 * 10^4 
- haystack and needle consist of only lower-case English characters.
```

# 说明

## 迭代实现(暴力)

迭代方法是使用两个嵌套的 for 循环。外部循环将迭代**干草堆**，对于每个索引，我们将**干草堆**串与**针**串匹配。如果我们到达**针**串的末端，我们返回开始索引，否则我们返回-1。

上述逻辑的一个 C++片段如下。

```
for (int i = 0; i <= haystack.length() - needle.length(); i++){
    int j;

    for (j = 0; j < needle.length(); j++) {
        if (needle.charAt(j) != haystack.charAt(i + j)) {
            break;
        }
    }

    if (j == needle.length()) {
        return i;
    }
}
```

上述程序的时间复杂度为 **O(m*n)** 。

## 递归实现

我们可以使用如下递归方法来解决这个问题:

```
int strStr(string haystack, string needle) {
    // ...basic condition check code

    for (int i = 0; i < haystack.length(); i++){
        if (haystack.charAt(i) == needle.charAt(0))
        {
            String s = strStr(haystack.substring(i + 1), needle.substring(1));
            return (s != null) ? haystack.charAt(i) + s : null;
        }
    }

    return null;
}
```

对于非常大的字符串，递归方法是不合适的。

## 使用 KMP 算法

我们可以用 KMP 算法在 **O(m + n)** 时间内解决这个问题。

让我们检查下面的算法:

```
- return 0 if needle.size == 0

- return -1 if haystack.size < needle.size

- set i = 0, j = 0
- initialize index

- loop while i < haystack.size
  - if haystack[i] == needle[j]
    - index = i

    - loop while haystack[i] == needle[j] && j < needle.size()
      - i++
      - j++

    - if j == needle.size
      - return index
    - else
      - j = 0
      - i = index + 1
  - else
    - i++

- return -1
```

**C++解决方案**

```
class Solution {
public:
    int strStr(string haystack, string needle) {
        if(needle.size() == 0){
            return 0;
        }

        if(haystack.size() < needle.size()){
            return -1;
        }

        int i = 0, j = 0;
        int index;
        while(i < haystack.size()){
            if(haystack[i] == needle[j]){
                index = i;
                while(haystack[i] == needle[j] && j < needle.size()){
                    i++;
                    j++;
                }
                if(j == needle.size()){
                    return index;
                } else {
                    j = 0;
                    i = index + 1;
                }
            } else {
                i++;
            }
        }

        return -1;
    }
};
```

**戈朗解**

```
func strStr(haystack string, needle string) int {
    needleLen := len(needle)
    haystackLen := len(haystack)

    if needleLen == 0 {
        return 0
    }

    if haystackLen < needleLen {
        return -1
    }

    i := 0
    j := 0
    index := 0

    for i < haystackLen {
        if haystack[i] == needle[j] {
            index = i

            for j < needleLen && haystack[i] == needle[j] {
                i++
                j++
            }

            if j == needleLen {
                return index
            } else {
                j = 0
                i = index + 1
            }
        } else {
            i++
        }
    }

    return -1
}
```

**Javascript 解决方案**

```
var strStr = function(haystack, needle) {
    if(needle.length == 0){
        return 0;
    }

    if(haystack.length < needle.length){
        return -1;
    }

    let i = 0, j = 0;
    let index;

    while( i < haystack.length ){
        if( haystack[i] == needle[j] ){
            index = i;
            while( haystack[i] == needle[j] && j < needle.length ){
                i++;
                j++;
            }

            if( j == needle.length ){
                return index;
            } else {
                j = 0;
                i = index + 1;
            }
        } else {
            i++;
        }
    }

    return -1;
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: haystack = "hello", needle = "ll"

Step 1: needle.size() == 0
        false

Step 2: haystack.size() < needle.size()
        5 < 2
        false

Step 3: i, j, k = 0, 0, 0

Step 4: while i < haystack.size()
        0 < 5
        true

        haystack[i] == needle[j]
        haystack[0] == needle[0]
        'h' == 'l'
        false

        i++
        i = 1

Step 5: while i < haystack.size()
        1 < 5
        true

        haystack[i] == needle[j]
        haystack[1] == needle[0]
        'e' == 'l'
        false

        i++
        i = 2

Step 6: while i < haystack.size()
        2 < 5
        true

        haystack[i] == needle[j]
        haystack[2] == needle[0]
        'l' == 'l'
        true
        index = i
        index = 2

        j < needle.length && haystack[i] == needle[j]
        0 < 2 && haystack[2] == needle[0]
        true && 'l' == 'l'
        true

        i++;
        j++;

        i = 3
        j = 1

        j < needle.length && haystack[i] == needle[j]
        1 < 2 && haystack[3] == needle[1]
        true && 'l' == 'l'
        true

        i++;
        j++;

        i = 4
        j = 2

        j < needle.length && haystack[i] == needle[j]
        2 < 2
        false

Step 7: j == needle.length
        2 == 2
        true

The answer returned is index: 2
```

*原发布于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-implement-strstr)*。*