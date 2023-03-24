# Python 中的链表

> 原文：<https://medium.com/nerd-for-tech/linked-list-in-python-e61552fec1a4?source=collection_archive---------4----------------------->

![](img/3a185395074216f2f2a0c9e1fc020ebc.png)

Python 使用 List 作为动态数组来存储数据。列表在连续内存位置存储数据。在列表开头插入和删除元素的复杂度为 O(n)。因为这些操作需要整个元素列表向前移动一个位置。

**列表算法复杂度:**

```
Find by Index : O(1)
Find by Value : O(n)
Traversal : O(n)
Insert Element : O(n)
Delete Element : O(n)
```

为了解决这个问题，有另一种线性数据结构——链表。链表存储分散位置的数据，使用引用指针指向下一个元素。链表的每个节点都包含数据和对下一个节点的引用。头指针用于引用第一个节点。在空链表的情况下，头指针指向 null。

如果我们比较两者，链表比数组有两个主要优点。
1。您不需要预先分配空间。
2。插入/删除更容易

**链表的算法复杂度:**

```
Insert Element at Beginning : O(1)
Delete Element at Beginning : O(1)
Insert/Delete Element at End : O(n)
Traversal : O(n)
Access Element by Value : O(n)
```

现在，让我们跳到编码部分，下面的操作在代码中是可用的。

*   访问第 n 个节点
*   按值搜索元素
*   在开头插入
*   在末尾插入
*   在指定索引处插入
*   在指定索引后插入
*   在指定值后插入
*   将列表转换为链接列表
*   获取链表的长度
*   移除指定索引处的元素
*   按值删除

链表有以下缺点:
1-不允许随机访问，需要顺序遍历才能访问元素。
2-需要额外的内存空间来存储 next_node 指针。
3 阵列具有更好的缓存局部性，性能更高。