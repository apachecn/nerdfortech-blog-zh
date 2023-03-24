# 如何以优化的方式调用代码块？

> 原文：<https://medium.com/nerd-for-tech/how-to-call-block-of-code-in-an-optimized-way-46eb4707a87c?source=collection_archive---------5----------------------->

在本教程中，我们将考虑如何以一种“智能”的方式在某个特定队列上执行代码块，即，是同步调用它(作为常规方法)还是必须使用[**dispatch queue . get { set } Specific(key:)**](https://developer.apple.com/documentation/dispatch/dispatchqueue/1780621-getspecific)**方法在自定义队列上调度执行**

**最近我在检查 XMPP 在 iOS 平台上的实现。在 [XMPPFramework](https://github.com/robbiehanson/XMPPFramework) 中，我发现一些 Obj-C 代码允许在一些特定的队列上调度代码的执行:**

```
void *xmppQueueTag; //pointer to anything- (void) init {
   // ......
  xmppQueueTag = &xmppQueueTag;
  xmppQueue = dispatch_queue_create("xmpp", DISPATCH_QUEUE_SERIAL);
  dispatch_queue_set_specific(xmppQueue, xmppQueueTag, xmppQueueTag,
NULL); //mark queue with key
}- (void) runSync {
  // check if code is running on specific queue. If true then run block, otherwise schedule synchronous execution if (dispatch_get_specific(xmppQueueTag))
     block(); // run block
  else // schedule synchronous execution 
     dispatch_sync(xmppQueue, block);
  }
}
```

**在 **runSync** 方法中，如果从预期队列中调用块代码，则执行块代码，否则计划在其上运行。**

**通常我在主线程上安排“智能”执行。有一个线程的类属性: **Thread.isMainThread.****

**记住，主队列是一个串行队列，只有一个线程(主线程)在这个队列上执行。在其他情况下，尽管 queue 是一个串行队列，但这并不意味着同一个线程正在其上运行。这意味着在这样的队列中只有一个操作是活动的，但是操作可以在不同的线程上执行。**

**我们可以在 Swift 中以典型的方式调用上面的代码(Obj-C ),即使用“桥接”。然而，最好不要混合 Swift 和 Obj-C 代码，尽管 UIView 和 UIViewController 是 NSObject 的后代，并且使用 Obj-C 运行时。但是模型、业务逻辑可以变得免费…**

**DispatchQueue 中有适当的通用类方法:**

```
class func getSpecific<T>(key: DispatchSpecificKey<T>) -> T? //read
func setSpecific<T>(key: DispatchSpecificKey<T>, value: T?) // write
```

**我们添加了一些与队列的关联。在 Obj-C 代码中“队列值”是变量( **xmppQueueTag** )的指针(地址)**

**所以我们可以检查线程的调度队列有一些特定的键值(下面代码中的随机固定值):**

**方法 **runOnQueue** 允许在预期队列上运行时立即调用块( **checkIfQueue** 返回 true)，否则，根据 *dispatchSync* 参数，该块将在特定队列上异步或同步运行。方法 **TestQueue** 失败，如果它没有按预期启动，抛出 assert。方法 **getSpecific(key** :)返回固定的随机初始化的构造函数值，如果它是从我们的自定义队列中调度的，否则它返回 nil。**

**请记住，来自另一个线程的同步调用会导致死锁，如果调用线程等待调用者…**

**在本教程中，我们设法创建了一个示例方法，允许以一种优化的、智能的方式调用一些特定队列上的代码块。**