# 异常与错误值

> 原文：<https://medium.com/nerd-for-tech/exceptions-vs-error-values-cc7f83f4df63?source=collection_archive---------20----------------------->

![](img/e6a66fc4d3074a0d229d2c3f3f65dd5a.png)

异常与错误值一直是[错误处理](https://programmingduck.com/articles/errors)中争论的话题。有些人对他们的立场很坚定。比如在[干净代码](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)这本书里，鲍勃大叔推荐了异常。在他关于[异常](https://www.joelonsoftware.com/2003/10/13/13/)的帖子中，Joel 提到他更喜欢错误值。

编程语言也采取了立场。C#和 Java 等流行语言传统上使用异常。Rust 等语言使用错误值。

在本文中，我们将研究它们的一些相似之处和不同之处。我们还将提供关于何时使用哪个的建议。

# 异常和错误值的基本示例

简单介绍一下，这里有一些异常和错误值的例子。

如果您已经熟悉它们，请跳到下一部分。

下面是一个在 C#中抛出和捕获异常的例子:

```
public class Example
{
    public void Foo()
    {
        try
        {
            Bar();
        }
        catch (IndexOutOfRangeException ex)
        {
            // handle error
        }
    }

    public void Bar()
    {
        if (true /* some condition to check if something went wrong */)
        {
            throw new IndexOutOfRangeException("Some error message");
        }
        else
        {
            // normal program execution
        }
    }
}
```

在上面的代码中，`Bar`抛出一个异常。在`catch`模块的`Foo`中捕捉并处理异常。

JavaScript 中也是如此:

```
function foo() {
  try {
    bar();
  } catch (error) {
    // handle error
  }
}

function bar() {
  if (true /* some condition */) {
      throw new Error("Error message");
  } else {
      // normal program execution
  }
}
```

误差值可以以不同的方式实现。一种方法是函数返回错误值或正常值。

例如:

```
function foo() {
  const result = bar();
  if (result instanceof Error) {
    // handle error
  } else {
    // normal program execution
  }
}

function bar() {
  if (true /* some condition */) {
    return new Error('Error message');
  } else {
    return 42;
  }
}
```

在上面的代码中，`bar`可以返回一个错误值或者一个正常值。`foo`检查返回值。如果是错误，它会处理它。否则，它继续正常的程序执行。

也可以通过返回单个对象来使用错误值。对象应该有错误和正常返回值的字段。例如，您可以使用一个元组，或者一个具有属性的对象。如果有错误，则`value`应该为空。比如`{error: new Error('Message'), value: null}`。如果没有错误，`error`值应该为空。比如`{error: null, value: 42}`。

下面是一个代码示例:

```
function foo() {
  const result = bar();
  if (result.error !== null) {
    // handle error
  } else {
    // normal program execution
  }
}

function bar() {
  if (true /* some condition */) {
    return {error: new Error('Error message.'), value: null};
  } else {
    return {error: null, value: 42};
  }
}
```

在上面的代码中，`bar`总是返回一个对象。如果出错，对象将在`error`字段中有一个值。否则，`error`字段将为`null`。

![](img/181f86bdd19d515d3490e4fc092e6b65.png)

# 异常和错误值之间的相似性

异常和错误值非常相似。事实上，Rust 和 Swift 等一些较新的编程语言消除了它们之间的大部分差异。

这两者最重要的一点是，它们作为函数/方法的不同返回值。不同的返回值会导致不同的代码执行路径。

它们也有一个很大的缺点。两个都很容易搞砸。

除此之外，您可以:

*   忘记抓住它
*   错误地认为调用堆栈中更高层的一些代码会捕获它
*   意外地在调用栈中没有准备好正确处理它的地方捕获它

此外，您可以完全避免检查错误值。

很容易忘记或者搞砸。即使你不知道，别人也会知道。所以，你必须非常勤奋。

或者，您可以使用一种强制您检查所有错误的编程语言。(稍后将详细介绍。)

# 异常和错误值之间的差异

异常和错误值有一些不同:

# 表演

抛出和捕捉异常通常被认为很慢。返回错误值很快。

然而，异常应该是“异常的”(很少抛出)。实际上，这意味着应用程序的性能不会因为使用它们而受到负面影响。

# 崩溃的程序 vs 无声的 bug

未捕获的异常会使程序崩溃。更罕见的是，异常也可能导致无声的错误(如果您无意中在调用栈中捕捉到它们)。

未经检查的错误值会导致无声的错误。

在这种情况下，例外更好。正如在[如何应对错误](https://programmingduck.com/articles/error-responses)中所解释的，让程序崩溃是比沉默的 bug 更好的默认选项。

# 冒泡

异常可以“冒泡”调用堆栈。没有在`catch`块中捕获的异常将在调用者(调用堆栈中的前一个代码)中抛出。如果在那里没有被抓住，这个过程就会重复。如果到达调用堆栈的末尾，程序将会崩溃。

冒泡有好有坏。

好处是很方便。在某个父函数中可以有一个 try / catch 块。异常将传播到它，并在那里被捕获。

缺点是执行流程不明确。你必须自己跟踪它。您还必须记住哪些异常是在调用堆栈中的什么位置捕获的。

这会让你陷入困境。有时您可能不记得或不知道异常是否会被捕获，或者在哪里被捕获，或者被什么捕获。

相比之下，错误值是标准返回值。如果您希望它们传播，您必须手动传播它们。你必须在不同的函数/方法中手工返回它们，直到栈顶。

这样做的好处是非常明确。这很容易追踪和推理。缺点是它非常冗长。您需要跨许多不同的函数/方法调用的许多 return 语句。

请注意，如果您愿意，技术上可以手动传播异常。然而，这并不是常见的做法。有关这方面的更多细节，请参见后面的“检查的异常”。

# 函数式编程的适用性

通常，异常在函数式编程中不太常见。

这是因为函数式编程提倡不变性和纯函数。

除了例外，有时您需要打破不变性。例如，通常需要在 try / catch 块之外声明变量，然后在 try / catch 中对它们进行变异。

下面是一个代码示例:

```
let a;
try {
  a = new Something();
  // do stuff with `a`
} catch (error) {
  // handle error
} finally {
  a.close();
}
```

此外，抛出的异常不是标准的返回值。这打乱了“纯功能”的观点。

# 一些新语言中的异常和错误值

一些较新的语言，如 Rust 和 Swift，稍微改变了一些东西。

最重要的是，它们强迫你检查所有的错误值和抛出的异常。这意味着您永远不会忘记检查错误或处理异常。

在 Swift 的例子中，这也使得异常冒泡更加明显。它仍然允许异常自动传播。然而，它需要用关键字“throws”标记中间函数(异常将通过这些函数传播)。

这种额外的明确性使得在整个代码中跟踪异常变得更加容易。

缺点是它使事情变得更加冗长。

(Rust 使用错误值，无论如何，您都必须显式地传播这些错误值。)

![](img/322b52e32b07d4d6960e5628e71cf04e.png)

# 你应该使用哪一个？

总的来说，这似乎是一个鲁棒性和安全措施的数量与冗长的问题。

实施错误检查和显式错误传播有明显的好处。这使得忘记做错误处理变得更加困难。你必须有意忽略它来避免它。

然而，冗长也有缺点。这会降低代码的可读性。这也使得对代码进行大的修改变得更加困难。如果您手动传播所有内容，这可能会特别突出。

例如，假设您更改了一个低级函数(或添加了一个新函数)有时会返回一个错误值。该错误可能需要在更高级别的功能中处理。这意味着您需要向每个中间函数添加代码来继续传播错误。

这是一个很大的变化。相比之下，如果您添加一个自动冒泡的异常，您只需在高级函数中添加一个 try / catch 块就可以了。

所以这取决于你来决定你在安全措施和冗长尺度上的立场。

为了最大程度的安全措施，您可能应该使用一种语言，强制您检查所有错误并强制显式传播它们。缺点是错误处理会更加冗长。

安全性低一级的是使用误差值。我认为这些比抛出异常更健壮。这是因为传播错误值比冒泡异常更明确。缺点是有更多的冗长。此外，请注意，你需要非常勤奋与这些。如果你忘记检查错误，你会得到无声的错误。未检查的错误值比未捕获的异常更糟糕。

否则，就抛出“普通”异常(比如 Java、C#和 JavaScript 中的异常)。他们是最不啰嗦的。这并不意味着你不能用它们创建健壮的程序。这只是意味着由你来处理错误并跟踪一切。

在您的编程语言中考虑约定可能也是一个好主意。一些编程语言更喜欢异常。有些人更喜欢误差值。

我个人的偏好是，对于更大范围和更关键的项目，倾向于更高的安全性。对于较小范围的项目，我倾向于更少的冗长和更多的便利(例外)。

# 最终注释

所以这就是这篇文章。我希望你觉得有用。

一如既往，如果有任何遗漏，或者有任何异议，或者有任何意见或反馈，请在下面留下您的评论。

对于接下来的步骤，我建议看看[错误处理系列](https://programmingduck.com/articles/errors)中的其他文章。

好的，谢谢，下次见。

# 信用

图像:

*   决斗的乐高——照片由 Unsplash 上的 Stillness InMotion 拍摄
*   打字机和笔记本电脑——格伦·卡斯滕斯-彼得斯在 Unsplash 上的照片
*   便利贴 Will H McMahan 在 Unsplash 上的照片

*原载于 2021 年 7 月 26 日*[*【https://programmingduck.com】*](https://programmingduck.com/articles/exceptions-error-values)*。*