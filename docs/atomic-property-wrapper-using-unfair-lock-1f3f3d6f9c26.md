# 使用不公平锁的“原子”属性包装

> 原文：<https://medium.com/nerd-for-tech/atomic-property-wrapper-using-unfair-lock-1f3f3d6f9c26?source=collection_archive---------3----------------------->

在这篇文章中，我将描述属性包装器如何帮助我消除 [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) 模式，以及我如何使用[OS _ fair _ lock](https://developer.apple.com/documentation/os/1646466-os_unfair_lock_lock)实现线程安全。

## **从 NSLock 转换**

之前，对于同步机制，我使用了`NSLock`或递归锁，即主要使用来自`NSLocking`协议的方法(锁定、解锁方法)。

如果有任何“本地”swift 替代方案，最好不要使用 Swift 的 Obj-C 功能。

实现 NSLock 的简单方法是使用**OS _ fair _ lock:**

在引擎盖下，使用了不安全的相互指针，并用适当的不公平锁结构上的指针进行初始化。

*未锁定*，确认到 *NSLocking* 协议。(然而，可以创建完全“本地的”、Obj-C 自由的类似协议)。

在最初的要点中，我增加了一个额外的协议`RunInLockTypeProtocol`

所以它增加了 **runInLock** 方法(本质上是将 block 的调用封装在 **lock()** 和 deferred **unlock()** 方法下)

## **返回属性包装**

让我们利用上面的内容创建一个通用的`Amotic`(线程安全)PW。

包装器基于锁(`NSLocking protocol`)的用法

```
lock.lock()// extra code goes heredefer {
    lock.unlock()
 }
```

**调整**方法突变包含值。

这对于引用类型尤其有意义，因为属性包装器返回类型上的指针，并互斥地返回它，但是内容不能以线程安全的方式更改)。下面是一个**原子**使用 **:** 的例子

```
//@Atomic var value = false// value = true //_value.toggle() or _value.adjust( { _ in !$0 })
```