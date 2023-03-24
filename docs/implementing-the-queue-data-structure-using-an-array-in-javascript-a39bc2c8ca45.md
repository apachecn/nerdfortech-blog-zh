# 在 Javascript 中使用数组实现队列数据结构

> 原文：<https://medium.com/nerd-for-tech/implementing-the-queue-data-structure-using-an-array-in-javascript-a39bc2c8ca45?source=collection_archive---------13----------------------->

![](img/671ca5f8c4509bac3dc6d9ae824090bc.png)

Melanie Pongratz 在 [Unsplash](https://unsplash.com/s/photos/queue?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 介绍

在本文中，我们将学习如何在 Javascript 中使用数组实现队列！队列数据结构的基本实现将通过以下方法完成:

1.  *enqueue()* —将元素添加到队列中
2.  *dequeue()* —删除并返回队列中输入的第一个项目
3.  *isEmpty()* —根据队列是否为空返回 true 或 false
4.  *front()* —返回队列的前面元素
5.  *print()* —返回队列的所有元素

> 注意:在我以前的文章中，我讨论了什么是队列数据结构。[点击这里](https://meghagarwal.medium.com/the-queue-data-structure-77be9054e67c)阅读文章。

所以我们开始吧！

# 简单的方法

您可以非常容易地用 Javascript 实现队列。首先，让我们创建一个数组。

```
var queue = [];
```

接下来，让我们使用 ***push()*** 方法推送一些值。

```
var queue = [];
queue.push(1);
queue.push(2);
```

开始了。我们已经准备好了一个基本队列，并且已经完成了入队操作。类似地，您可以执行出列操作。

```
queue.shift(); //dequeue operation
```

要检查队列是否为空，可以检查数组的长度是否为空。

```
if(queue.length > 0){
    console.log("The Queue is not empty")
} else { 
    console.log("The Queue is empty") 
}
```

您甚至可以打印前端元素和所有元素。

```
console.log(queue[0]); //prints the front element
console.log(queue); //prints all the elements
```

上面是一个简单的队列方法。您可以使用这种方法，但是更好的方法是基于类的。

# 基于类别的方法

## 节点类

```
class Node {
    constructor(data) {
       this.data = data;
   }
}
```

Node 类将用于创建节点，然后可以将这些节点在队列中 ***入列*** 或 ***出列*** 。该节点是一个基本类，只有一个构造函数来初始化对象和分配数据。这是一个简单的实现，您可以添加一些定制来满足您的需求。

## 队列类

现在，让我们创建主队列类。这个类将拥有这些与队列数据结构相关的方法: *enqueue()、dequeue()、isEmpty()、front()、print()。*

```
class Queue {
   constructor() {
      this.elements = [];
   }

   enqueue(node) {
      this.elements.push(node);
   }

   dequeue() {
      if(this.elements.length > 0) { 
          return this.elements.shift();
      } else {
          return 'Underflow situation';
      }
   } isEmpty() {
      return this.elements.length == 0;
   }

   front() {
      if(this.elements.length > 0) {
          return this.elements[0];
      } else {
          return "The Queue is empty!";
      }
   } print() {
      return this.elements;
   }
}
```

现在，我们来了解一下 ***类*** 队列的各个部分。

```
constructor() {
   this.elements = [];
}
```

该类同样由一个初始化对象的构造函数组成。它还创建一个空数组，用于创建队列。

```
enqueue(node) {
   this.elements.push(node);
}
```

enqueue 方法(不是静态方法)将节点入队(将节点添加到队列的后面)。在基层，节点是数组中的 ***推*** 。将元素推到数组末尾的 javascript 中提供了 ***push()*** 方法。

> 注意: ***enqueue()*** 方法不遵循**溢出条件**。溢出条件检查队列是否已满(有更多的内存可用于添加新元素)。这个溢出条件在我创建的类中没有实现，因为我们假设队列是动态增长的。

```
dequeue() {
  if(this.elements.length > 0) { 
       return this.elements.shift();
  } else {
       return 'Underflow situation';
  }
}
```

dequeue 方法(不是静态方法)将元素从队列的前面出队/移除。该方法检查队列中的元素是否大于 0；否则，会出现下溢的情况。在基础级别，元素从数组中被 ***移位*** 。javascript 中的 **shift()** 方法可以从数组中移除第一项。这就是时间复杂性混乱的地方。我在以前的文章中解释过: ***当使用数组实现队列时，出列操作的时间复杂度是如何变化的。***

> 注意: ***dequeue()*** 方法遵循**下溢条件**。下溢条件检查队列中是否还有元素需要出队。您不能让空队列出列，是吗？

```
isEmpty() {
   return this.elements.length == 0;
}
```

isEmpty 方法(没有一个方法是静态的)检查队列是否为空。在基本级别，它返回数组的长度是否为 0。这个方法不言自明。

```
front() {
  if(this.elements.length > 0) {
      return this.elements[0];
  } else {
      return "The Queue is empty!";
  }
}
```

front 方法返回队列的第一个元素。它检查元素数组的长度是否大于 0，并基于此返回一个值。您可以将 if-else 语句定制为更短的语句，如三元运算符。我喜欢原来的方式！

```
print() {
    return this.elements;
}
```

最后一个方法 print()返回元素数组。同样，这是不言自明的。

# 有用吗？

现在，让我们测试队列是否工作。为此，我创建了一个简单的测试代码。

```
const queue = new Queue();//Logs: true
console.log(queue.isEmpty());queue.enqueue(new Node({"number": 1}));
queue.enqueue(new Node({"number": 2}));
queue.enqueue(new Node({"number": 3}));
queue.enqueue(new Node({"number": 4}));
queue.enqueue(new Node({"number": 5}));//Logs: false
console.log(queue.isEmpty());/* Logs:
[
     Node { data: { number: 1 } },
     Node { data: { number: 2 } },
     Node { data: { number: 3 } },
     Node { data: { number: 4 } },
     Node { data: { number: 5 } }
]
*/
console.log(queue.print());queue.dequeue();//Logs: false
console.log(queue.isEmpty());queue.dequeue();
queue.dequeue();
queue.dequeue();
queue.dequeue();//Logs: true
console.log(queue.isEmpty());//Logs: []
console.log(queue.print());
```

# 最终代码

下面是 GitHub 存储库的最终代码。链接:[https://github.com/Megh-Agarwal/Queue-data-structure](https://github.com/Megh-Agarwal/Queue-data-structure)

我们使用 javascript 中的数组成功实现了队列数据结构！恭喜你！如果你喜欢这篇文章，掌声会激励我继续写这样的文章:)