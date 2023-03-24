# LeetCode —评估反向波兰符号

> 原文：<https://medium.com/nerd-for-tech/leetcode-evaluate-reverse-polish-notation-21f8c1301b22?source=collection_archive---------6----------------------->

# 问题陈述

评估*反向波兰符号*中算术表达式的值。

有效的运算符有 *+* 、 *-* 、*、 */* 。每个操作数可以是整数或其他表达式。

**注意**两个整数之间的除法应该向零截断。

保证给定的 RPN 表达式总是有效的。这意味着表达式将始终计算结果，并且不会有任何被零除的操作。

问题陈述摘自:[https://leet code . com/problems/evaluate-reverse-polish-notation](https://leetcode.com/problems/evaluate-reverse-polish-notation)

**例 1:**

```
Input: tokens = ["2", "1", "+", "3", "*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```

**例 2:**

```
Input: tokens = ["4", "13", "5", "/", "+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
```

**例 3:**

```
Input: tokens = ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
Output: 22
Explanation: ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

**约束:**

```
- 1 <= tokens.length <= 10^4
- tokens[i] is either an operator: "+", "-", "*", or "/", or an integer in the range [-200, 200]
```

## 说明

[逆波兰符号](https://en.wikipedia.org/wiki/Reverse_Polish_notation)是一种数学符号，其中运算符跟随其操作数。

基本方法是使用堆栈。让我们直接检查算法:

```
- create stack st
  initialize int op1, op2- loop for i = 0; i < tokens.size; i++
  - if tokens[i] != "+" && tokens[i] != "-" && tokens[i] != "*" && tokens[i] != "/"
    - push to stack st.push(tokens[i])
  - else
    - set op2 = st.pop()
      set op1 = st.pop() - if tokens[i] == "+"
      - st.push(op1 + op2)
    - else if tokens[i] == "-"
      - st.push(op1 - op2)
    - else if tokens[i] == "*"
      - st.push(op1 * op2)
    - else
      - st.push(op1 / op2)
- for end- return st.top()
```

这个函数的时间复杂度为 **O(N)** ，空间复杂度为 **O(N)** 。

让我们检查一下我们在 **C++** 、 **Golang** 和 **Javascript** 中的解决方案。

## C++解决方案

```
typedef long long ll;class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<ll> st;
        ll op1, op2; for(int i = 0; i < tokens.size(); i++) {
            if(tokens[i] != "+" && tokens[i] != "-" && tokens[i] != "*" && tokens[i] != "/") {
                st.push(stol(tokens[i]));
            } else {
                op2 = st.top();
                st.pop();
                op1 = st.top();
                st.pop(); if(tokens[i] == "+")
                    st.push(op1 + op2);
                else if(tokens[i] == "-")
                    st.push(op1 - op2);
                else if(tokens[i] == "*")
                    st.push(op1 * op2);
                else
                    st.push(op1 / op2);
            }
        } return (int) st.top();
    }
};
```

## 戈朗溶液

```
func evalRPN(tokens []string) int {
    st := []int{} for _, token := range tokens {
        if token == "+" {
            st[len(st) - 2] += st[len(st) - 1]
            st = st[:len(st) - 1]
        } else if token == "-" {
            st[len(st) - 2] -= st[len(st) - 1]
            st = st[:len(st) - 1]
        } else if token == "*" {
            st[len(st) - 2] *= st[len(st) - 1]
            st = st[:len(st) - 1]
        } else if token == "/" {
            st[len(st) - 2] /= st[len(st) - 1]
            st = st[:len(st) - 1]
        } else {
            num, _ := strconv.Atoi(token)
            st = append(st, num)
        }
    } return st[0]
}
```

## Javascript 解决方案

```
var evalRPN = function(tokens) {
    let st = [];
    let op1, op2; for(let i = 0; i < tokens.length; i++) {
        if(tokens[i] != "+" && tokens[i] != "-" && tokens[i] != "*" && tokens[i] != "/") {
                st.push(parseInt(tokens[i]));
        } else {
            op2 = st.pop();
            op1 = st.pop(); if(tokens[i] === "+")
                st.push(op1 + op2);
            else if(tokens[i] === "-")
                st.push(op1 - op2);
            else if(tokens[i] === "*")
                st.push(op1 * op2);
            else
                st.push(op1 / op2 | 0);
        }
    } return st[0];
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: tokens = ["2", "1", "+", "3", "*"]Step 1: stack<ll> st
        int op1, op2Step 2: loop for int i = 0; i < tokens.size()
          i < tokens.size()
          0 < 5
          true if tokens[i] != "+" && tokens[i] != "-" && tokens[i] != "*" && tokens[i] != "/"
             tokens[0] is "2"
             true st.push(stoi(tokens[i]))
             st.push(stoi(tokens[0]))
             st.push(2) st = [2] i++
          i = 1Step 3: loop for i < tokens.size()
          1 < 5
          true if tokens[i] != "+" && tokens[i] != "-" && tokens[i] != "*" && tokens[i] != "/"
             tokens[1] is "1"
             true st.push(stoi(tokens[i]))
             st.push(stoi(tokens[1]))
             st.push(1) st = [2, 1] i++
          i = 2Step 4: loop for i < tokens.size()
          2 < 5
          true if tokens[i] != "+" && tokens[i] != "-" && tokens[i] != "*" && tokens[i] != "/"
             tokens[1] is "+"
             false else
            op2 = st.top()
                = 1 st.pop()
            st = [2] op1 = st.top()
                = 2 st.pop()
            st = [] if tokens[i] == "+"
               true st.push(op1 + op2)
               st.push(2 + 1)
               st.push(3) st = [3] i++
          i = 3Step 5: loop for i < tokens.size()
          3 < 5
          true if tokens[i] != "+" && tokens[i] != "-" && tokens[i] != "*" && tokens[i] != "/"
             tokens[3] is "3"
             true st.push(stoi(tokens[i]))
             st.push(stoi(tokens[3]))
             st.push(3) st = [3, 3] i++
          i = 4Step 6: loop for i < tokens.size()
          4 < 5
          true if tokens[i] != "+" && tokens[i] != "-" && tokens[i] != "*" && tokens[i] != "/"
             tokens[4] is "*"
             false else
            op2 = st.top()
                = 3 st.pop()
            st = [3] op1 = st.top()
                = 3 st.pop()
            st = [] if tokens[i] == "+"
              false
            else if tokens[i] == "-"
              false
            else if tokens[i] == "*"
               st.push(op1 * op2)
               st.push(3 * 3)
               st.push(9) st = [9] i++
          i = 5Step 7: loop for i < tokens.size()
          5 < 5
          falseStep 8: return st[0]We return the answer as 9.
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-evaluate-reverse-polish-notation)*。*