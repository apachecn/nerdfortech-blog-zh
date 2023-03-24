# 空值和空值检查——如何在任何代码库中安全地使用空值

> 原文：<https://medium.com/nerd-for-tech/nulls-and-null-checks-how-to-work-safely-with-nulls-in-any-codebase-de9ddc592efe?source=collection_archive---------17----------------------->

![](img/9320388ec9da982bf80251dea54c54dd.png)

[干净代码](https://programmingduck.com/articles/clean-code)的一个重要部分是正确处理空值。

几十年来，空值一直是编程中一个棘手的问题。

发明者东尼·霍尔甚至称之为一个十亿美元的错误。

从语义上讲，空值是必要的。它们代表了价值的缺失。例如，用户可以填写具有可选字段的表单。他们可以将可选字段留空。这是空值的一个原因。

问题是空值很难处理和跟踪。

# 空值的问题是

在代码库中很难跟踪空值。有许多事情:

*   具有属性为`null`
*   可以返回`null`
*   做某事前需要检查`null`

如果你错过了一个“空检查”，你就有一个 bug。你的程序可能会出错，甚至崩溃。

例如，如果您忘记先检查`null`，下面的代码会崩溃:

```
// this function crashes if the argument is null
function foo(arrayOrNull) {
  return arrayOrNull[0];
}
```

代码应该是这样的:

```
function foo(arrayOrNull) {
  if (arrayOrNull === null) {
    return null;
  }
  return arrayOrNull[0];
}
```

问题是 100%彻底地检查你的空支票是非常困难的。跟踪每一个空值即使不是不可能，也是极其困难的。

![](img/5c8941b658de2a8abad3395ba394953d.png)

# 处理空值的解决方案

使用空值很困难。为了使事情变得简单，这里有一些你可以使用的可能的解决方案。有些是坏的，有些是好的。我们会逐一检查。

解决方案是:

*   对周围的一切进行检查
*   使用 try / catch 代替空检查
*   返回默认值，而不是`null`
*   使用空对象模式
*   记得检查每个空值
*   使用一个强制你检查每一个`null`的类型系统
*   使用类似选项类型的东西

以下是每一项的详细信息:

# 在每件事情周围放置一个空检查

处理空值的一个解决方案是总是检查它们，即使在你不需要的时候。勾选“以防万一”。毕竟“拥有它而不需要它，总比需要它而不拥有它要好。”乔治·埃利斯。对吗？

如果这是你确保不遗漏空支票的唯一方法，那么也许…

然而，这不是最佳解决方案。问题是你的代码中的某些东西可能是不应该是`null`的。换句话说，你有一个 bug。

但是，如果您在不需要的地方使用了空检查，您将会默默地忽略这个 bug。它将被一张空头支票吞噬。

例如:

```
// car is never supposed to be null
if (car !== null) {
  car.getWheels();
}
```

在上面的代码中，`car`可能在不应该的时候是`null`。那是一个错误。然而，由于一个不必要的空检查，程序不会崩溃。该错误将被忽略。

但是，如果没有不必要的空检查，程序就会崩溃。

例如:

```
// car is null due to a bug
// the program crashes
car.getWheels();
```

这是一个很好的场景。正如[如何应对错误](https://programmingduck.com/articles/error-responses)中所解释的，最起码，你要知道自己有 bug。崩溃清楚地表明了这一点，但是默默忽略 bug 却没有。

换句话说，您可能应该避免不必要的空检查。

否则，如果你想做[防御性编程](https://programmingduck.com/articles/defensive-programming)，你可以有额外的空检查。然而，如果事情实际上是`null`的话，放入一些记录错误的代码。这样，您可以在以后调试问题。(有关更多信息，请参见[记录错误以便稍后调试](https://programmingduck.com/articles/error-recording)。)

# 使用 try / catch 代替空检查

条件 vs . try/catch 是一场适用于所有可能无效的动作的辩论。为此，在[无效动作控制流程](https://programmingduck.com/articles/control-flow-invalid-actions)中有更详细的解释。

除此之外，试/抓解决不了问题。

您可能会忘记添加 try / catch 块，就像您可能会忘记空检查一样。在这种情况下，您的程序可能会崩溃。

更糟糕的是，不同的 try / catch 块可能会无意中捕获到异常。那是一只无声的虫子。无声的错误往往比崩溃更糟糕。

# 返回默认值而不是 null

另一种选择是避免返回`null`。相反，返回相关类型的默认值。

例如，您可能有一个通常返回字符串或空值的函数。返回空字符串，而不是 null。或者，您可能有一个通常返回正数或 null 的函数。不返回 null，而是返回 0 或-1(如果 0 不是合适的默认值)。

## 默认值的好处

默认值减少了代码中的空值数量。

在某些情况下，它们也减少了条件句的数量。当您可以以同样的方式对待默认值和“正常”值时，就会发生这种情况。

例如，无论`user.name`是一个普通的值还是空字符串，这段代码都有效。

```
function printUserGreeting(user) {
  const name = user.name;
  const formattedName = name.toUppercase();
  const greeting = `Hello ${formattedName}`;
  document.body.append(greeting);
}
```

但是，如果`user.name`有时是`null`，这个函数需要一个空值检查才能工作。

```
function printUserGreeting(user) {
  const name = user.name;
  if (name === null) { // null check
    document.body.append('Hello');
  } else {
    const formattedName = name.toUppercase();
    const greeting = `Hello ${formattedName}`;
    document.body.append(greeting);
  }
}
```

返回默认值可能是好的。然而，也有不利的一面。

## 默认值的缺点

一个不利之处是,`null`的语义没有得到尊重。语义上，`null`表示没有值。它并不意味着一个合法的值。相比之下，空字符串或数字 0 可能是合法的值。0 或-1 可能是数学计算的结果。空字符串可以是提供给函数的分隔符。它们并不意味着没有数据。

与第一个缺点相关的另一个缺点是，您会丢失关于该值是表示空值还是合法值的信息。有时候区分这两者是很重要的。您不会总是能够以相同的方式使用默认值和正常值。

例如，考虑 JavaScript 的`Array.prototype.indexOf()`方法。它返回自然数(0 或正整数)，或者-1 作为默认值(而不是 null)。但是，在大多数情况下，您永远不能使用值-1。您将需要一个条件来查看该方法是返回-1 还是一个正常值。这就失去了意义。从您的代码的角度来看，它可能是空的。

例如:

```
function findUser(userArray, targetUser) {
  const index = userArray.indexOf(targetUser);
  if (index === -1) {
    console.log('Sorry, the user could not be found');
  } else {
    console.log(`The target user is user number ${index + 1}`);
  }
}
```

另一个缺点是你可能有很多函数。每个可能需要不同的默认值。在这种情况下，您将拥有一个对其中一个有效的默认值，但对其他的无效。然后，其他函数将需要条件来检查默认值。这又一次证明了这一点。这实际上使得代码更难处理。检查`null`比检查“魔力值”更容易。

最后，还有其他一些缺点:

*   想出一个默认值可能很困难
*   追踪默认值的来源(在代码中)可能很困难

## 默认值的判定

总结:这是一个有用的解决方案。然而，要小心它的缺点。你需要自己判断何时使用这个选项。

我个人不太常用。

但是，一个经常使用的“缺省”值是一个空集合。例如，空数组或空哈希表。这往往有所有的好处，没有缺点。那是因为说“是的，这个东西*有一个集合*，它只是*恰好是空的*”在语义上是正确的。此外，大多数代码应该能够以与非空集合相同的方式处理空集合。

# 使用空对象模式

空对象模式类似于使用默认值(如上所述)。

不同之处在于，它处理的是类和对象，而不是字符串和数字之类的原始值。它为值(属性)和行为(方法)设置默认值。

您可以通过创建一个 null / empty / default 对象来使用 null 对象模式，该对象具有与普通对象相同的接口。这个对象的属性和方法将有默认值和行为。

例如，下面是一个普通的`User`类，它可能存在于您的代码库中:

```
class User {
  constructor(name, id) {
    this.name = name;
    this.id = id;
  }

  updateName(name) {
    this.name = name;
  }

  doSomething() {
    // code to do something
  }
}
```

下面是一个你可能拥有的`NullUser`类的例子(一个空对象):

```
class NullUser {
  constructor() {
    this.name = 'Guest'; // default value
    this.id = -1; // default value
  }

  updateName() {} // do nothing (default behaviour)

  doSomething() {
    // do nothing, or do some other default behaviour
  }
}
```

代码中的用法大概是这样的:你可能有一些代码通常会返回`null`或者一个普通的对象。不返回`null`，而是返回空对象。这类似于返回默认值。

例如，下面的代码有时会返回`null`:

```
function findUser(userId) {
  const targetUser = users.find(user => user.id === userId);
  if (!targetUser) {
    return null;
  }
  return user;
}
```

相反，您可以使用以下代码，它返回一个空对象而不是`null`:

```
function findUser(userId) {
  const targetUser = users.find(user => user.id === userId);
  if (!targetUser) {
    return new NullUser();
  }
  return user;
}
```

然后，无论何时使用空对象或普通对象，都不需要空检查。

为了说明这一点，这里有一些不带的空对象模式的示例代码**:**

```
// class User is shown above

const users = [new User('Bob', 0), new User('Alice', 1)];

function findUser(userId) {
  const targetUser = users.find(user => user.id === userId);
  if (!targetUser) {
    return null;
  }
  return user;
}

function printName(user) {
  if (user === null) { // null check here
    document.body.append(`Hello Guest`);
  } else {
    document.body.append(`Hello ${user.name}`);
  }
}

function main() {
  const user = findUser(123);
  printName(user);
}
```

下面是相同的代码，只是它使用了空对象模式:

```
// classes User and NullUser are shown above

const users = [new User('Bob', 0), new User('Alice', 1)];

function findUser(userId) {
  const targetUser = users.find(user => user.id === userId);
  if (!targetUser) {
    return new NullUser(); // instead of returning null, return a null object
  }
  return user;
}

function printName(user) {
  // no null check
  document.body.append(`Hello ${user.name}`);
}

function main() {
  const user = findUser(123);
  printName(user);
}
```

至于是否使用空对象模式，与默认值类似。

![](img/3cc44543f5c0f52d495dd793077a5a20.png)

# 记得检查每个空值

彻底检查你的所有检查的一个方法是…彻底检查你的所有检查…

每次你写代码的时候，都要非常小心你的空检查。你应该明白`null`可以出现在哪里，不应该出现在哪里(哪里会是 bug)。

这是非常困难的。有时候可能感觉不可能。但是，如果你不使用其他解决方案，这是你必须做的。

# 使用强制空检查的类型系统

拯救打字系统。一些编程语言，比如带有`strictNullChecks`选项的 TypeScript，能够理解什么时候某个东西可能是`null`。然后，他们强迫你检查。

另一个例子是 C#及其可空的引用类型。这些警告你检查(他们不强迫你)，但是那仍然是有帮助的。

这意味着你永远不能忘记检查`null`。

我个人认为这是一个很好的选择。

# 使用选项类型

最后一个选项(没有双关的意思)是使用类似选项类型的东西(也称为可能类型)。

这并没有完全消除空检查。但是，它减少了很多。此外，剩下的几个空检查位于易于使用的地方。很难忘记把它们放进去。

使用选项类型，您有两个空支票，而不是无数个空支票。

空支票位于:

1.  选项类型本身
2.  第一个返回选项类型的函数

下面是选项类型的一个(非常)简化的实现:

```
class Option {
  constructor(nullOrNormalValue) {
    this._value = nullOrNormalValue;
  }

  map(fn) {
    if (this._value === null) {
      return this;
    }
    const newValue = fn(this._value);
    return new Option(newValue);
  }
}
```

要对选项类型做些什么，您可以使用`map`方法并传入一个函数。如果您曾经对数组等使用过`map`函数，这应该很熟悉。

这里的关键点是空检查在选项类型内部。换句话说，每次你试图使用这个值时，你都会得到一张免费的空支票。这意味着，只要您使用选项类型，就永远不会忘记您的空检查。

在你第一次返回一个选项的地方，你还需要一个空检查，或者其他一些条件。

例如，下面是一个通常会返回 null 或正常值的普通函数:

```
function getNextScheduledEvent(user) {
  if (user.scheduledEvents.length === 0) {
    return null;
  }
  return user.scheduledEvents[0];
}
```

这是同一个函数，但是现在，它返回一个选项。

```
function getNextScheduledEvent(user) {
  if (user.scheduledEvents.length === 0) {
    return new Option(null);
  }
  return new Option(user.scheduledEvents[0]);
}
```

编写完这段代码后，您不再需要对返回值进行任何空检查。

例如，下面是不带选项的代码:

```
function getNextScheduledEvent(user) {
  if (user.scheduledEvents.length === 0) {
    return null;
  }
  return user.scheduledEvents[0];
}

function foo(nextScheduledEvent) {
  if (nextSceduledEvent === null) { // null check
    // do nothing
  } else {
    // stuff
  }
}

function bar(nextScheduledEvent) {
  if (nextSceduledEvent === null) { // null check
    // do nothing
  } else {
    // stuff
  }
}

function baz(nextScheduledEvent) {
  if (nextSceduledEvent === null) { // null check
    // do nothing
  } else {
    // stuff
  }
}

function main() {
  const user = {scheduledEvents: []}
  const nextEventOption = getNextScheduledEvent(user);
  const a = foo(nextScheduledEvent);
  const b = bar(nextScheduledEvent);
  const c = baz(nextScheduledEvent);
}
```

注意，每个函数都需要一个空检查。

下面是使用选项的相同代码:

```
function getNextScheduledEvent(user) {
  if (user.scheduledEvents.length === 0) {
    return new Option();
  }
  return new Option(user.scheduledEvents[0]);
}

function doubleEventPrice(event) {
  // no null check
  return {
    ...event,
    price: event * 2,
  }
}

function foo(event) {
  // stuff, no null check
}

function bar(event) {
  // stuff, no null check
}

function main() {
  const user = {scheduledEvents: []}
  const nextEventOption = getNextScheduledEvent(user);
  const a = nextEventOption.map(doubleEventPrice);
  const b = nextEventOption.map(foo);
  const c = nextEventOption.map(bar);
}
```

请注意缺少空检查。

当然，这是一个非常简化的解释。使用选项类型的意义远不止于此。选项的实际实现也将更加复杂。

![](img/f93b366df1910c3b07249abd02344c86.png)

# 您应该使用哪个选项？

我们讨论了很多处理空值的方法。

为您的代码库选择合适的代码取决于您。你需要权衡各自的利弊。你还需要考虑你的喜好。

就个人而言，我喜欢类型系统强制空检查。除此之外，我有时可能会使用默认值或空对象模式。截至本文撰写之时，我还没怎么用过期权类型。然而，许多人对那一个充满热情。这似乎是一个很好的解决方案。

如果你愿意，可以在下面留下你推荐的方法和原因的评论。

# 最终注释

所以这就是这篇文章。我希望你觉得有用。

一如既往，如果有任何遗漏，或者有任何异议，或者有任何意见或反馈，请在下面留下您的评论。

好的，谢谢，下次见。

# 信用

图像制作者名单:

*   单个盒子 Christopher Bill 在 Unsplash 上的照片
*   两个盒子——照片由 Pexels 的 Karolina Grabowska 拍摄
*   便笺——由 AbsolutVision 在 Unsplash 上拍摄的照片
*   指向笔记本电脑——照片由约翰·施诺布里奇在 Unsplash 上拍摄

*原载于 2021 年 7 月 26 日*[*【https://programmingduck.com】*](https://programmingduck.com/articles/nulls)*。*