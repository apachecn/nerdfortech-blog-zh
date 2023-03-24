# 用这些问题打败每一次面试！

> 原文：<https://medium.com/nerd-for-tech/beat-every-reactjs-interview-with-these-questions-15971921d9f8?source=collection_archive---------3----------------------->

这是我面对过的一系列面试问题。

![](img/55d9319e076286304970ccd976cf4e75.png)

照片由 [LinkedIn 销售解决方案](https://unsplash.com/@linkedinsalesnavigator?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

我**’**m**给你一个保证，你再也不会在面试中感到羞愧。让我们直截了当地谈吧。我们将这个博客分成两个部分。**

1.  ****JavaScript****
2.  ****反应堆****

# **Java Script 语言**

**最重要的是你非常了解 ***JavaScript*** ，只有这样你才能擅长 ReactJS。因此，让我们深入探讨一些基本的常见问题:**

## ****核心 JavaScript****

**根据面试官进行多次面试的经验，最有可能出现的问题是:**

1.  ****JS 变量****

*****回答*** :变量是存储数据(值)的容器。我们在 JS 中有三种类型的变量，分别是 let、var 和 const。**

**用 **var** 声明的**变量**的**作用域**是其当前的执行上下文及其闭包，它或者是封闭函数以及在其中声明的函数，或者对于在任何函数之外声明的变量，是全局的。**

**2. **JS 功能****

*****答案:*** 可以重用代码:定义一次代码，多次使用。我们可以将参数传递给函数，并多次使用同一个函数。**

```
function *name*(*parameter1, parameter2, parameter3*) {
  // *code to be executed
  return  (parameter1, so on...)* }
```

**3. **JS 事件****

*****答案:*** HTML 事件是发生在 HTML 元素上的**【事情】**。在 HTML 页面中使用 JavaScript 时，JavaScript 可以**【反应】**这些事件。**

1.  ***onchange***
2.  ***加载***
3.  ***onclick***
4.  ***onmouseover***
5.  ***onmouseout***
6.  ***onkeydown***

**示例:**

```
<button onclick="this.innerHTML=Date()">The time is?</button>
```

**4. **JS 操作员****

**在 JavaScript 中，有各种类型的运算符，即:**

**a)。算术运算符**

```
**+  -----------**(Addition)
-  -----------(Subtract)
**/  -----------**(Divide)
***  -----------**(Multiply)
** -----------(Exponentiation)
%  -----------(Modulus)
++ -----------(Increment)
-- -----------(Decrement)
```

**b)。赋值运算符**

**这将提取好像我们已经使用" x **+=y"** 然后它会做 **"x=x+y "。****

```
**Opreator        Example** =               x = y       
+=              x += y
-=              x -= y
*=              x *= y
/=              x /= y
%=              x %= y
**=             x **= y
```

**c)。字符串运算符**

**对于两个字符串的连接，我们有一个“+”运算符。**

```
let text1 = "Arun";
let text2 = "Kashyap";
let text3 = text1 + " " + text2;**Output :**
Arun Kashyap
```

**d)。比较运算符**

```
**Operator                      Description**==                            equal to
===                           equal value and equal type
!=                            not equal
!==                           not equal value or not equal type
>                             greater than
<                             less than
>=                            greater than or equal to
<=                            less than or equal to
?                             ternary operator
```

**e)。逻辑运算符**

```
**Operator               Description            Example** &&                     AND                    1==1 && 2==3 = false
||                     OR                     1==1 || 2==3 = true
!                      NOT                    !(1==1 && 2==3) = true
```

**f)。条件运算符**

**这是 ReactJS/JS 中最常用的著名操作符之一。**

```
condition ? statement1 : statement2 
```

**这个操作符有两个语句，如果我们的条件为真，那么它将执行第一个语句，否则它将只执行最后一个语句。**

**5. **JS 计时功能****

**JavaScript 中有一个双时间函数，用于在给定时间之间或之后执行该函数。**

```
1\. setTimeout(*function, milliseconds*)
2\. setInterval(*function, milliseconds*)
```

**示例:**

```
**const test = () => {
  alert("Test Function");
}
setTimeout(test,10000);**
```

****6。回调函数****

**回调是作为参数传递给另一个函数的函数。这种技术允许一个函数调用另一个函数。回调函数可以在另一个函数完成后运行。基本上，每个函数都按照其编号的顺序执行。**

*****例如:*****

```
<!DOCTYPE html>
<html>
<body>
  <p id="demo"></p>
<script>
  **function myDisplayer(some) {
    document.getElementById("demo").innerHTML = some;
  }
  function myFirst() {
    myDisplayer("Hello");
  }
  function mySecond() {
    myDisplayer("Goodbye");
  }
  mySecond();
  myFirst();**
</script>
</body>
</html>
```

**下面是代码的输出结果:**

```
Hello
```

**如果我们删除 **myFirst** 函数，那么它将打印**和*再见。*****

**7. **JS 数组方法****

**这些是编程中用到的最重要的方法。我们将这些方法应用于数组。这些方法决定了我们最终得到的结果。我们有各种方法:**

```
**1\.  Array.map()
2\.  Array.filter()
3\.  Array.reduce()
4\.  Array.forEach()
5\.  Array.find()**
```

**让我们看看每种方法的示例:**

**a)。 ***Array.map()*****

**这是最常用的方法，它用以前的数组创建一个新数组，并在执行语句后显示项目。**

```
let array = [1,2,3,4,5,6,7]
array.map(item => item*2)Output : [2, 4, 6, 8, 10, 12, 14]
```

**b)。***array . filter()*****

**它用于过滤我们的数组，该数组过滤并给我们一个新的过滤项数组。**

```
let array = [1,2,3,4,5,6,7]
**array.filter(item => item > 5)**Output : **[6,7]**
```

**c)。***array . reduce()*****

**这个 **reduce()** 方法在数组的每个元素上执行一个 **reducer** 函数(您提供的)，产生一个输出值。**

```
**SYNTEX** : reduce((accumulator, currentValue, **index(optional), array(optional)**) => { ... } )
```

**例如:**

```
let array = [1,2,3,4,5,6,7]
**array.reduce((item,value) => item + value)**Output : **28**
```

**d)。 ***阵列。* forEach *()*****

**该方法为每个数组元素执行一次提供的函数。**

```
let array = [1,2,3,4,5,6,7]
array.forEach((item)=> {
   console.log(item)
})**Output:** 
1
2
3
4
5
6
7
```

**d)。 ***阵。*找到 *()*****

**此方法用于从数组中查找值，然后返回元素(single)。尽管我们有更多的元素，它仍然返回一个元素。**

```
let array = [1,2,3,4,5,6,7]
array.find(item=> item > 5)Output : **6**
```

**除此之外，我还有更多关于 JS 的问题，让我知道你是否喜欢这个并想看更多。**

****这些问题仅来自 JavaScript 基础。在下一篇博客中，我们将学习 EcmaScript 面试问题。然后我们将看到 ReactJS 是专家级问题的基础。****

*****让我们一起成长……*****