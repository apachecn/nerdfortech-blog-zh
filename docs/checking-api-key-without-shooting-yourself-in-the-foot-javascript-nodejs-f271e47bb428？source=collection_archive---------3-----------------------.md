# 检查 API 密钥，而不是搬起石头砸自己的脚(JavaScript，NodeJS)

> 原文：<https://medium.com/nerd-for-tech/checking-api-key-without-shooting-yourself-in-the-foot-javascript-nodejs-f271e47bb428?source=collection_archive---------3----------------------->

![](img/f460787798f5aae107e742d45bcbdbda.png)

照片由[阿扎马特 E](https://unsplash.com/@picsaf?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

非常容易搬起石头砸自己的脚，不仅一不小心让黑客*轻松黑掉认证机制*，还让*更容易让他们找出 API 密钥*！

如果 API 密匙控制对一些潜在的昂贵资源的访问，这可能会特别有问题，因此通过获取 API 密匙，黑客可以让你积累相当多的账单。

当后端开发人员编写 API 时，有时他们需要验证请求者。服务器需要找出是谁发出的请求，这样它就可以授权或拒绝访问。有许多方法可以实现这一点，其中之一是通过 API 键。

# API 基于密钥的身份验证的工作原理

在其核心，API 基于密钥的认证机制是这样工作的:
API 消费者发送一个请求，并在其中嵌入一个密钥。如果密钥与服务器预期的密钥匹配，那么用户就通过了身份验证(因为他们有一个共享的秘密)。
天真地说，代码中可能是这样的:

```
if (req.body.apiKey == expectedApiKey) {
  // Authorize access
} else {
  response.status(401).send('unauthorized');
}
```

因此，如果消费者发送的密钥(`req.body.apiKey`)与我们期望的 API 密钥(`expectedApiKey`)相同，那么我们授权访问，否则我们发送一个错误。
不幸的是，这段代码容易受到定时攻击。

# 什么是定时攻击？

黑客可以用来危害系统的一种技术叫做“定时攻击”。其工作方式是通过测量从系统获得响应需要多长时间，并通过从时间中获得洞察力来利用它。
举个例子，假设一个黑客试图从服务器上猜出一个密码。如果黑客猜测的正确密码越接近，服务器的响应时间就越长，那么他可以使用计时信息来缩小可能的密码猜测范围。这有点像“寻找顶针”,服务器告诉黑客他是变得更近(更热)还是更远(更冷)。

# 为什么天真的方法容易受到攻击？

在 JavaScript 字符串比较中，使用`==`和`===`操作符是不安全的。在后台，它遍历操作数，如果一个字符不同，它会提前终止。它看起来会像这样:

```
bool operator==(string op1, string op2) {
  if(op1.length != op2.length) return false;
  for(let i = 0; i < op1.length; ++i) {
    if op1[i] != op2[i] return false;
  }
  return true;
}
```

这里有 2 个问题:
1。如果输入的长度不相同，过程会提前返回。
2。`for`循环经历的迭代次数取决于相等子串的长度。
发生的情况是，它向攻击者泄露了有用的信息:长度是否正确？如果是的话，到目前为止我猜对了多少子串呢？

作为一个攻击者，我可以猜测 API 密钥，并测量我需要多长时间才能得到拒绝消息。如果我注意到对于一些猜测的 API 键，比其他的要花更长的时间被拒绝，我可以推断我可能已经正确地猜测了一部分(一个子串)。然后，我可以根据这个猜测尝试各种变化，以缩小正确的子串或找出更长的正确子串。

# 如何修复漏洞？

NodeJS 有一个内置的加密模块，它实现了[timingsafequal](https://nodejs.org/api/crypto.html#crypto_crypto_timingsafeequal_a_b)。它与简单的相等性检查的不同之处在于它基于一个常数时间算法。在同样长的时间后，不管字符串是否相等，也不管是否有相等的子串，都会得到一个响应。

它的代码可能是这样的:

```
bool timingSafeEqual(string op1, string op2) {
  bool eq = true;
  for(let i = 0; i < Math.min(op1.length, op2.length); ++i) {
    if op1[i] != op2[i] eq = false;
  }
  if op1.length != op2.length eq = false;
  return eq;
}
```

通过将`req.body.apiKey`和`expectedApiKey`作为参数传递给`timingSafeEqual`，您可以在不泄露计时信息的情况下发现它们是否相等。

然而，事情没那么简单！`timingSafeEqual`的文档说参数必须都是`Buffer` s、`TypedArray` s 或`DataView` s。这不是问题。问题是**它们必须有相同的字节长度**。

所以我们处于进退两难的境地。为了修复这个漏洞，我们需要使用`timingSafeEqual`，但是使用`timingSafeEqual`，如果长度不同，我们需要做一些特殊的事情。我们如何做到这一点而不泄露时间信息呢？

# 让我们看看我们的选择

*如果我们这样做:*

```
if (crypto.timingSafeEqual(Buffer.from(req.body.apiKey), Buffer.from(expectedApiKey)) {
  // Authorize access
} else {
  response.status(401).send('unauthorized');
}
```

如果长度不同，将会引发异常。即使我们处理了这个异常，它仍然有一个计时足迹，所以我们会泄漏计时信息。

*如果我们这样做:*

```
if (req.body.apiKey.length === expectedApiKey.length && crypto.timingSafeEqual(Buffer.from(req.body.apiKey), Buffer.from(expectedApiKey)) {
  // Authorize access
} else {
  response.status(401).send('unauthorized');
}
```

如果长度不同，我们就不会到达`if`语句的`timingSafeEqual`部分(因为 JavaScript 的[延迟求值](https://en.wikipedia.org/wiki/Lazy_evaluation))，所以我们又一次泄露了计时信息。

*我们可以这样做:*

```
if (crypto.timingSafeEqual(Buffer.from(req.body.apiKey.padEnd(expectedApiKey.length).slice(0, expectedApiKey.length)), Buffer.from(expectedApiKey)) {
  // Authorize access
} else {
  response.status(401).send('unauthorized');
}
```

它通过填充强制左操作数与右操作数长度相同。这可能是一个可行的解决方案，但是:

1.  它看起来不干净。它可能会在代码审查中引起一些人的注意，并且可能保证一个“WTF”。
2.  我关注的是`String.slice`的实现。是常数时间算法吗？还是取决于输入大小？
    我还没有查看 NodeJS 内部来验证。

# 我提议的解决方案

哈希两者和`timingSafeEqual`的密码。

```
const hash = crypto.createHash('sha512');
if (crypto.timingSafeEqual(
  hash.copy().update(req.body.apiKey).digest(),
  hash.copy().update(expectedApiKey).digest()
)) {
  // Authorize access
} else {
  response.status(401).send('unauthorized');
}
```

我们使用的好的散列函数有一些属性:

*   对于每一对函数调用，如果明文相同，那么密文也相同。这样我们可以比较密码，并且知道明文是相同的当且仅当密码是相同的。
*   该函数的输出总是相同的大小(以位为单位)。
    这样，`timingSafeEqual`的两个输入大小相同。
*   不管输入是什么，函数都做同样多的工作。
    这意味着它不会泄露计时信息。

这里有一个额外的好处:不用在源代码或环境变量中以纯文本的形式存储 API 键，而是可以存储它的散列。即使有人可以访问您的源代码或环境变量，他们也很难弄清楚 API 密钥。这是因为好的散列函数的另一个特性:

*   给定明文，计算它的匹配密文在计算上是便宜的。
    给定密文，计算它的匹配明文的计算代价非常高。