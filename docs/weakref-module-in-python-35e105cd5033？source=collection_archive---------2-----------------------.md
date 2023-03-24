# Python 中的 weakref 模块

> 原文：<https://medium.com/nerd-for-tech/weakref-module-in-python-35e105cd5033?source=collection_archive---------2----------------------->

![](img/9725573e8abe145d81545c770081405b.png)

照片由 [Milad Fakurian](https://unsplash.com/@fakurian?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/shadow?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

自从我写了上一篇文章，已经有一段时间了，确切地说是两个月零两周。事不宜迟，我在写关于 Python 中的`weakref`模块。让我们开始吧。

该模块允许 Python 程序员创建对对象的弱引用。弱引用是不保护对象不被垃圾回收的引用。

我们知道，Python 使用引用计数机制进行[垃圾收集](/analytics-vidhya/memory-management-in-python-4332fbf95cd0)，当存在强引用时，引用计数会增加。让我们看一个例子:

运行文件`python weak-ref-eg-0.py`时，输出是:

```
Reference count for yobj: 1
Reference count of w_yobj is 1
Reference count for yobj: 2
```

我们看到，只有在行号为`21`的行中存在强引用时，引用计数才会增加。

我们可以对这个程序进行更多的扩展，删除具有强引用的对象，并看到引用计数的减少。清理完对象后，如果我们调用弱引用对象`w_yobj`，它将返回`None`。

输出是:

```
Reference count for yobj: 1
Reference count of w_yobj: 1
Reference count for yobj: 2Cleaning...After deleting yobj_2, reference count for yobj: 1calling weakref object, w_yobj, after deleting both objects...
Weak ref: None
```

weakref 为什么用于？非常好的问题。它用于

1.  在缓存或映射中，您持有的对象是昂贵的，并且您不希望它们仅仅因为在映射或缓存中就处于活动状态。

2.为了使处理循环引用更容易。

让我们看一个映射的例子:

运行该文件时，输出是:

```
cache type: <class 'dict'>
reference count after initialization gem opal: 1
reference count after caching gem opal: 2
reference count after appending to list gem opal: 3reference count after initialization gem ruby: 1
reference count after caching gem ruby: 2
reference count after appending to list gem ruby: 3reference count after initialization gem fluorite: 1
reference count after caching gem fluorite: 2
reference count after appending to list gem fluorite: 3List length: 3, contents: [PriceyObject(opal), PriceyObject(ruby), PriceyObject(fluorite)]cleaning list...After cleaning list, cache contains: dict_keys(['opal', 'ruby', 'fluorite'])opal=PriceyObject(opal)
ruby=PriceyObject(ruby)
fluorite=PriceyObject(fluorite)demo returningDeleting PriceyObject(opal)
Deleting PriceyObject(ruby)
Deleting PriceyObject(fluorite)cache type: <class 'weakref.WeakValueDictionary'>
reference count after initialization gem opal: 1
reference count after caching gem opal: 1
reference count after appending to list gem opal: 2reference count after initialization gem ruby: 1
reference count after caching gem ruby: 1
reference count after appending to list gem ruby: 2reference count after initialization gem fluorite: 1
reference count after caching gem fluorite: 1
reference count after appending to list gem fluorite: 2List length: 3, contents: [PriceyObject(opal), PriceyObject(ruby), PriceyObject(fluorite)]cleaning list...Deleting PriceyObject(fluorite)
Deleting PriceyObject(ruby)
Deleting PriceyObject(opal)After cleaning, cache contains: <generator object WeakValueDictionary.keys at 0x7f8a700addd0>demo returning
```

标准`dict`和`weakref`模块的`WeakValueDictionary`的区别在于，在删除包含强引用 gem 对象的列表时，`WeakValueDictionary`不包含对 gems 对象的引用。而标准字典仍然有引用，并且能够对它们进行迭代。

现在让我们来看一个处理循环引用的例子。

我从关于对称表亲关系的[内存管理](/analytics-vidhya/memory-management-in-python-4332fbf95cd0)文章中提取了一个例子，这里是猫的系谱。

输出是:

```
servy's refcount: 2
whiky's refcount: 2AFTER DELETION OF servy
whiky's refcount: 2AFTER ASSIGNING whiky's cousin to be None
whiky's refcount: 1
```

我们看到了导致内存泄漏的循环引用，因为在删除了`servy`对象后,`whiky`对象的引用计数仍然为 2。我们通过手动设置`whiky`的表亲为`None`来克服这种循环引用。

我们可以使用弱引用来处理这种类型的关系。

该文件的输出是:

```
servy's refcount: 1
whiky's refcount: 1AFTER DELETION OF servywhiky's refcount: 1
whiky's cousin: None
```

我们看到引用计数并没有因为设置`cousin`属性的值而增加，当`servy`对象被删除时`whiky`的`cousin`属性的值为`None`。

那不是很容易吗？！

总之，`weakref`模块提供了引用对象的方法，而不需要额外的巨大内存。

请浏览官方文档，查看所有方法。

感谢您的阅读。一会儿见。✨

**灵感:**

*   [pymotw](http://pymotw.com/2/weakref/)
*   [官方文件](https://docs.python.org/3/library/weakref.html)

你可以在 [Patreon](https://www.patreon.com/dkhambu) 上支持我！