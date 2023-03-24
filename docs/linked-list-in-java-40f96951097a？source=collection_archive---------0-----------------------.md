# JAVA 中的链表

> 原文：<https://medium.com/nerd-for-tech/linked-list-in-java-40f96951097a?source=collection_archive---------0----------------------->

![](img/c4716558cea28242168e4b189a887338.png)

链表是一种线性数据结构，其中的元素不是存储在连续的内存位置。

链表中的每个元素都是一个单独的对象，具有数据部分和地址部分。这些元素中的每一个都被称为**节点**。每个节点包含一个**数据**字段和一个**引用**(链接)到列表中的下一个节点。

它是 java 中集合框架的一部分。util 包。

# **数组列表 VS 链表**

数组列表和链表都有相同的方法，因为它们都是使用 List 接口实现的，并且它们包含相同类型的对象。

![](img/b8457f45e5f6e66a369c483619c75ef8.png)

但是，它们的构建方式不同，有几个不同之处。

**数组列表**:

*   ArrayList 内部使用一个**动态** **数组**来存储元素。
*   ArrayList 更适合**存储**和**访问**数据。
*   一个 ArrayList 类只能作为一个列表，因为它只实现了**列表接口**。
*   使用 ArrayList 的操作很慢**，因为它在内部使用一个数组，如果我们想从数组中移除任何元素，所有的位都会在内存中移位。**
*   **最好是在我们不得不**随机访问项目**时使用，或者在**删除或添加列表末尾的元素**时使用。**

*****链表*** :**

*   **LinkedList 内部使用一个**双向链表**来存储元素。**
*   **链表更适合**操作数据**。**
*   **Linkedlist 类可以充当 List 和 queue，因为它实现了 List 和 Deque 接口。**
*   **LinkedList 的操作比 ArrayList 的**快**，因为它使用双向链表，因此不需要在内存中移位。**
*   **最好在我们需要**从列表的开头或中间添加和删除项目**和**不需要随机访问**时使用。**

# **链表的优点和缺点**

**![](img/c46bd6a5d216f4d5368fcdc8ef5f1815.png)**

*****优点*** :**

*   **通过分配和释放内存，它可以在运行时**增长**和**收缩**。**
*   **节点的**插入**和**删除**确实更加**容易**，因为不需要移动元素。**
*   **由于链表的大小可以在运行时增加或减少，所以没有内存浪费。**

**![](img/86591f5a93e4b693a22077d016850e39.png)**

*****缺点*** :**

*   ****需要更多的内存**来存储链表中的元素，因为每个节点都包含一个指针，它本身需要额外的内存。**
*   **在链表中遍历元素或节点**是困难的**，随机访问元素是不可能的。**
*   **在链表中**反向遍历真的很难**。在双向链表的情况下，这很简单，但是反向指针需要额外的内存，因此浪费了内存。**

# **基本结构**

**链表包含节点的**序列。如前所述，链表由两部分组成:数据部分和引用部分(指向下一个节点)。在双向链表的情况下，还会有一个额外的**前**部分。****

**![](img/3fd30858d8cf456d8e4d78d40aced5cf.png)**

**第一个节点称为头部。最后一个节点叫做 tail，它指向 null。如果链表为空，那么头的值为空。**

# **节点的实现**

**通用类型-**

```
public class ListNode<T>{
       T data;
       ListNode<T> next;
}
```

**现在让我们看看一些基本的实现！！！**

# **Java 中的单链表**

**整数类型的链表:**

# **链表的长度**

**![](img/43dfbf0a623c76ba1128f4fa2c7b7dd4.png)****![](img/66c3bdbc0da531437e4ca87fc7423d3b.png)****![](img/88855d2be96f456cc1efb7667b8ca1f7.png)****![](img/513ecadf833f06e77de922a51bd4bb36.png)****![](img/2880c07e76869168920ac964bcfd69c4.png)****![](img/a43160bf6ccb5a7ee7f1be2c58927191.png)**

# **插入新节点**

**a)在链表的头部:**

**![](img/89659a6f9c263266f9914abdb742f1b5.png)****![](img/9510f02fb468829b72179fc44875e829.png)****![](img/2a0a867199baa08abb29cf846b39a923.png)****![](img/f2be2b5d7d121cc07a38c9dc5823552f.png)**

**b)在链表的末尾:**

**![](img/ccb839faaa5d72341738b926501834fb.png)****![](img/50cfbcf49cce29d5f518c5ad88ad41c1.png)****![](img/707c8b4145bc9e096bae2c3bbb94a0cc.png)****![](img/c2d01b3cc7768ea3e3179b0737f86ce0.png)****![](img/48f13d2b180989968417706b743e931b.png)****![](img/0f0e622641e47fced6941a0c6391ab7e.png)**

**c)在链表的指定位置**

**![](img/bf810e6f895d81916f081040e702a879.png)****![](img/1741e07c6fce45d99c2892326c5dbe3b.png)****![](img/980a1144f29ad3ad179baf3109c7d225.png)****![](img/01a2316b3f3755a235a3dc5411ecc581.png)****![](img/37eabd2a34fbe71eeb6618834ee85acc.png)****![](img/36cec2c82880035ab3855d5c19408ca4.png)****![](img/8d45ab2c7e4ac948802d38ee28fd159f.png)****![](img/d7ae4f88be50c33a75471ea19a533e05.png)**

# **删除节点**

**a)从链表的头部开始:**

**![](img/844b176ef867473f0b328aabcbeb83c1.png)****![](img/cef3bb2cd60d4c56fd0030f1cfbc0d9b.png)****![](img/52efc31c3cab8cf2708126e1ec5389f3.png)****![](img/4c82ac04768c2684a42244b4f04f3053.png)**

**b)从链表的尾部开始:**

**![](img/ebd57c2fc73e5167144b08a56b7e84a0.png)****![](img/c58d48886684e57f930b7441a20224d7.png)****![](img/d4950c4eccd273a881791bd196d198ef.png)****![](img/16287e0fbf8dfcd7eb622dca1c8c0f09.png)****![](img/42adf87d9f128372f80ab56bd8e0b8a0.png)****![](img/05749e3612613f6a1fc66b1049f39cf1.png)**

**c)从链表的指定位置**

**![](img/6172ff8de2fdfc1306deb1b97f794899.png)****![](img/b3190ce4c24eeabdf481499ea62219a0.png)****![](img/068f1f7a93b376f79cc75e9e8f25b4da.png)****![](img/205b0d5e0366652d042bbfaefdb94ff6.png)****![](img/4ae8ccdebeb62b5216148f8ff43a1e43.png)****![](img/3be3665d921fd1a73a0b59b1ca07c31a.png)****![](img/a1779c4f9b6f6ccca8b3712f9ae43f4f.png)**

# **反转链表**

**![](img/781b21bea7f8df638a89cd76ef6a58c9.png)****![](img/6ca957d4331b3784563498e9339042a3.png)****![](img/2be2f9fa5ec5f6ffaac433e4492ca5db.png)****![](img/dc289099bd43c46c35b852049b5fadc4.png)****![](img/6a43909e20a3bf4fa09abd18c6580639.png)****![](img/d3d69a04c30b20f6da3641a9199530c7.png)****![](img/f8894c5d17eed70326510c069263ae29.png)****![](img/0ed900dc7b2bbbd32a40c0ffa72ccd11.png)**

# **打印链接列表的元素**

**![](img/f812e6ee63211a109e96f45617982cfa.png)****![](img/6baf917dbd2d9ce50b7339be59eac210.png)****![](img/c9d7ad52ab0c0919e3e3faf82797aad9.png)****![](img/e323b63ec2cf60bcdd5782513e698e1e.png)****![](img/6eded53d411de2db82fea8e24ff1ad61.png)**

**让我们现在试试这个！！！**

*****输出:*****

```
**Inserting from begining****4 3 2 1****Inserting from end****4 3 2 1 5 6****Insert at position 3****4 3 7 2 1 5 6****Delete from begining****3 7 2 1 5 6****Delete from end****3 7 2 1 5****Delete from position 2****3 2 1 5****Reverse of the Linked List****5 1 2 3****Length of the Linked List:4**
```

**[***完整代码:***](https://gist.github.com/Sapna2001/46ca82dfc78f76295f0a8b428c56d772)**

**更多练习题请查看[黑客排名](https://www.hackerrank.com/domains/data-structures?filters%5Bsubdomains%5D%5B%5D=linked-lists)。**

# **链接列表方法**

**一些基本方法:**

*   **add first()-将项目添加到列表的开头**
*   **Add last()-在列表末尾添加一个项目**
*   **Remove first()-从列表的开头删除一个项目**
*   **Remove last()-从列表末尾删除一个项目**
*   **Get first()-获取列表开头的项目**
*   **Get last()-获取列表末尾的项目**
*   **get(index)-从 LinkedList 中访问元素**
*   **set(index，value)-更改 LinkedList 的元素**

*****输出:*****

```
**Adding elements****[Rita, Gita, Hari, Priya, Siya, Shyam]****Deleting elements****[Gita, Hari, Priya, Siya]****Element at First:Gita****Element at Last:Siya****Element at 2nd index:Priya****[Gita, Hari, Sapna, Siya]**
```

> **我这边就这样。我希望你喜欢阅读这篇文章😊。**

> **如果你想让**和我**连接，请点击这些链接**
> 
> **领英-[https://www.linkedin.com/in/sapna2001/](https://www.linkedin.com/in/sapna2001/)**
> 
> **github-[https://github.com/Sapna2001](https://github.com/Sapna2001)**
> 
> **网站-[https://sapna2001.github.io/Portfolio/](https://sapna2001.github.io/Portfolio/)**
> 
> **quora-【https://www.quora.com/profile/Sapna-191 **

****如果你有任何建议，请随时通过**[**LinkedIn**](https://www.linkedin.com/in/sapna2001/)**&与我联系，评论区也是你的。****

**如果你喜欢这个故事，请点击拍手按钮，因为它激励我写得更多更好。**

*****感谢阅读！！！！*****