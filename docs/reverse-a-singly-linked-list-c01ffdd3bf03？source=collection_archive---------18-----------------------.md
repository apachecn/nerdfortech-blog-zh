# 反转单向链表

> 原文：<https://medium.com/nerd-for-tech/reverse-a-singly-linked-list-c01ffdd3bf03?source=collection_archive---------18----------------------->

## 给定一个对单向链表头的引用，反转它并返回一个对反转链表头的引用。

![](img/40515c04deef6cf17c9fcf578f9dd5a0.png)

"图片由 [Edge2Edge 媒体](https://unsplash.com/@edge2edgemedia?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/chain?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄"

🔔这篇文章最初发布在我的网站上，[MihaiBojin.com](https://mihaibojin.com/coding-puzzles/linked-list/reverse-singly-linked-list?utm_source=Medium&utm_medium=organic&utm_campaign=top-promo)。🔔

> 给定一个对单向链表头的引用，反转它并返回一个对反转链表头的引用。

这是一个简单的问题，可以用几种方法解决。在本文中，我将用我所知道的最优雅的方式来解决它——在线性时间内就地解决。

注意:如果你不熟悉这个数据结构，[参见维基百科关于链表的文章](https://en.wikipedia.org/wiki/Linked_list#Singly_linked_list)。

## 一个简单的 LinkedList 实现

但是首先，让我们从一个简单的单链表实现开始。

```
import java.util.*;

public class LinkedList<T> implements Iterable<T> {
  private Node head;
  private Node tail;

  public void add(T value) {
    if (this.head != null) {
      // the majority use-case: the head element exists
      this.tail.next = new Node(value);
      this.tail = this.tail.next;
      return;
    }

    initFirstElement(value);
  }

  private void initFirstElement(T value) {
    this.head = new Node(value);
    this.tail = head;
  }

  public void addAll(List<T> values) {
    values.forEach(this::add);
  }

  /**
   * A singly linked list node
   */
  private class Node {
    private T value;
    private Node prev = null;
    private Node next = null;

    private Node(T value) {
      this.value = value;
    }
  }
}
```

上面的代码代表了一个非常简单的链表实现，它构成了我们问题的基础。

在我们继续之前，我们可以让它变得更好一点。首先，我们需要按顺序检索列表的所有元素。

Java 惯用的方法是实现一个`Iterator`。

```
public class LinkedList<T> implements Iterable<T> {
  // ...

  @Override
  public Iterator<T> iterator() {
    return new LinkedListIterator();
  }

  /**
   * Iterate over elements until none are left
   */
  private class LinkedListIterator implements Iterator<T> {
    private Node cursor = LinkedList.this.head;

    @Override
    public boolean hasNext() {
      return cursor != null;
    }

    @Override
    public T next() {
      try {
        return cursor.value;
      } finally {
        cursor = cursor.next;
      }
    }
  }
}
```

另一个好处是覆盖了`toString`方法，这样我们就可以打印出列表的元素:

```
public class LinkedList<T> implements Iterable<T> {
  // ...

  @Override
  public String toString() {
    List<T> results = new ArrayList<>();
    Iterator<T> it = this.iterator();
    while (it.hasNext()) {
      results.add(it.next());
    }

    return results.toString();
  }
}
```

当然，在我们继续之前，让我们确保我们的自定义链表实现按预期工作。

对于下面的例子，我使用了 [JUnit 5](https://junit.org/junit5/docs/current/user-guide/) 和 [Hamcrest](http://hamcrest.org/JavaHamcrest/) ，但是你可以随意替换它们。

```
import java.util.List;

import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.equalTo;

class LinkedListTest {

  @Test
  void testLinkedListImplementation() {
    // ARRANGE
    List<Integer> input = List.of(1, 2, 3);
    LinkedList<Integer> tested = new LinkedList<>();

    // ACT
    tested.addAll(input);
    String output = tested.toString();

    // ASSERT
    assertThat("Expecting the same elements", output.toString(), equalTo(input.toString()));
  }

  @Test
  void testEmptyLinkedList() {
    // ARRANGE
    LinkedList<Integer> tested = new LinkedList<>();

    // ACT
    String output = tested.toString();

    // ASSERT
    assertThat("Expecting an empty list", output.toString(), equalTo(List.of().toString()));
  }
}
```

## 解开谜题

解决这个问题的一个简单方法是遍历列表的所有元素，将它们添加到堆栈中，然后弹出它们。这种方法可行，但是它需要`O(N)`额外的空间。

如果您试图反转一个包含数百万元素的列表，您可能会耗尽堆空间。

我们可以做得更好！

与大多数链表算法一样，您可以依赖两个游标:

*   第一个保持对反向列表中最后一个元素的引用
*   第二个保存了对原始列表的新头部的引用

一步一步地，我们将迭代原始列表，在我们前进的过程中构造它。最后，我们将只剩下对反向列表头部的引用。

让我们看看这是怎么回事！

```
public class LinkedList<T> implements Iterable<T> {
  // ...

  public LinkedList<T> reverse() {
    // initialize references
    Node newHead = null;
    Node oldHead = this.head;
    Node newTail = null;

    // while there are elements we haven't seen in the original list
    while (oldHead != null) {
      // keep a reference to the next element in the original list
      Node next = oldHead.next;

      // change the direction of the elements
      oldHead.next = newHead;

      // oldHead becomes newHead (the reversed list's head)
      newHead = oldHead;

      // at this point, keep a reference to the reversed list's new tail
      // we will need it later
      if (newTail == null) {
        newTail = newHead;
      }

      // advance the read element, in the original list
      oldHead = next;
    }

    // construct the resulting linked list object
    LinkedList<T> result = new LinkedList<>();
    result.head = newHead;
    result.tail = newTail;
    return result;
  }
}
```

当然，我们还需要一个测试来确认解决方案是否有效:

```
class LinkedListTest {
  // ...

  @Test
  void testReverseLinkedList() {
    // ARRANGE
    List<Integer> input = List.of(1, 2, 3);
    LinkedList<Integer> tested = new LinkedList<>();
    tested.addAll(input);

    List<Integer> expected = List.of(3, 2, 1);

    // ACT
    String output = tested.reverse().toString();

    // ASSERT
    assertThat("Expecting the same elements, in reverse order", output.toString(), equalTo(expected.toString()));
  }
}
```

现在你有了:就地(没有额外的空间)、线性(只迭代一次输入)、单链表反转。

我希望你喜欢这个练习；与更简单的替代方案(使用堆栈)相比，这是一个更好的解决方案。

理解双指针概念是有效解决链表编码难题的关键。因此，我建议花点时间学习它，因为它肯定会派上用场！

如果您有任何问题或寻求任何建议，请([在 Twitter 上 DM 我](https://twitter.com/mihaibojin))！

直到下一次…

如果你喜欢这篇文章并想阅读更多类似的文章，[请订阅我的简讯](https://motivated-founder-807.ck.page/db1cf284bf)；我每隔几周就发一封！