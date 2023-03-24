# C 语言中的高阶函数

> 原文：<https://medium.com/nerd-for-tech/higher-order-functions-in-c-74f6c4b550ee?source=collection_archive---------1----------------------->

![](img/8a4c735f1cf1134d5242d4651beb0afc.png)

照片由[迪伦·费雷拉](https://unsplash.com/@dylanferreira?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/high?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

高阶函数的概念是函数式编程中的关键，但像递归函数一样，它们也用于实现某些算法、编程模式，甚至编写内核驱动程序。尽管 C 语言不支持许多现成的 FP 概念，但是高阶函数可以使用函数指针和编译器支持来实现。在本文中，我试图用简单的例子来说明这一点。

# 什么动物是高阶函数？

高阶函数是接受其他函数作为参数或返回函数本身的函数。由于 C 严重依赖于指针的使用，它也允许指针引用像函数一样的可运行代码块。事实上，函数本身就是指向它们所包含的代码的指针，所以我们将会看到，将一个函数赋给一个函数指针是完全有效的。然后，这样的指针可以作为参数传递给函数，或者像其他变量一样由函数返回。这就是高阶函数在 c 中的实现方式。

# 指向功能

根据以下内容声明一个函数:

```
**void fun_name(void);**
^    ^        ^
|    |        `----- parameter type
|    `-------------- function name
`------------------- return type
```

这一行声明了一个名为 *fun_name* 的函数，它不接受任何参数，也不返回任何内容。如果它接受了`int`并返回了`double`，那么它将被这样声明:`double fun_name(int param);`其中 *param* 是整数参数的名称(声明中不强制提供)。很简单。

现在我们想要一个指向这样一个函数的指针，因为我们想把它作为参数传递给其他函数，或者从高阶函数返回。这里有一个指向我们最新功能的指针:

```
**double (*fun_ptr)(int param);**
^       ^^        ^   ^
|       ||        |   `------ parameter name
|       ||        `---------- parameter type
|       |`------------------- pointer name
|       `-------------------- pointer marker
`---------------------------- return type
```

这意味着函数指针的声明和它所指向的函数几乎是一样的。唯一的区别是指针名现在被加上了括号，以避免与返回类型(可能是指针本身)混淆。这也意味着我们需要准确地知道被指向函数的类型。这提供了一些类型安全。

# 函数杂耍出

上一节展示了函数指针的类型，现在让我们看看如何将它添加到函数签名中。

```
**void use_fptr(double (*fun_ptr)(int param));**
^    ^        ^        ^        ^   ^
|    |        |        |        |   `------- pointed fn param
|    |        |        |        `----------- pointed fn param type
|    |        |        `-------------------- pointer parameter name
|    |        `----------------------------- pointed fn return type
|    `-------------------------------------- function name
`------------------------------------------- function return type
```

假设我们的参数列表包含两个函数指针，那么它们可能如下所示:

`void use_fptr(void (*fptr1)(void), void (*fptr2)(void));`

现在棘手的部分是:返回一个函数指针。为了让事情更清楚，我假设我们的函数不接受任何参数，而是返回一个指向接受 int 的函数的指针，并返回一个 double。可以这样声明:

```
**double (* fn_returns_fptr(void))(int param);**
^       ^ ^               ^      ^   ^
|       | |               |      |   `------ returned fptr param
|       | |               |      `---------- ret. fptr param type
|       | |               `----------------- function param type
|       | `--------------------------------- function name
|       `----------------------------------- ret. pointer marker
`------------------------------------------- ret. fptr return type
```

在上面的例子中，所有的括号都是强制性的，否则就无法知道星号指的是什么。所以我们不是简单地把返回类型放在函数名的左边，而是因为返回类型是一个指向函数的指针，我们把指向函数的返回类型放在左边，参数列表放在右边，把我们的函数嵌入到它的返回类型中，就像一个洋葱。这已经足以让任何一个刚刚学习函数指针的人感到困惑，但这可能会更复杂。想象一个函数，取一个以上的函数指针作为参数，返回一个函数指针，取两个函数指针作为参数，返回一个函数指针……无限。一个更短的例子可能就足够了:

```
double (* fun(double (*p1)(int), double (*p2)(int)))(int);
```

一个名为`fun`的函数，它接受两个函数指针，每个指针接受一个 int 并返回一个 double，而它自己返回其中一个函数指针。实际上没那么有趣。假设它返回了一个函数指针，这个函数指针带有一个函数指针。这就是詹姆斯·海特菲尔德所说的“绝对恐怖”。

幸运的是，C 提供了简化这一点的方法。键入别名来拯救世界。

# 蒸馏函数传递

因为函数和指针只是类型，所以我们也可以为它们定义别名，然后使用这个名称来引用它们，而不是笨重的类型。事实上，函数指针语法非常容易变得难以忍受的复杂，以至于我在展示它们的时候已经犯了错误也不会感到惊讶。所以要避免这种风险，尽可能使用类型别名。我们可以为我们的`int`取`double`返回函数指针创建一个类型别名，如下所示:

```
**typedef double (*int_to_double)(int);**
^       ^       ^^              ^
|       |       ||              `---- pointed function param type
|       |       |`------------------- type alias
|       |       `-------------------- pointer marker
|       `---------------------------- pointed function return type
`------------------------------------ type aliasing keyword
```

这不是一个新类型，它是一个现有类型的别名`int_to_double`，是一个函数指针。然后我们可以在参数列表中和作为返回类型使用它，这样詹姆斯·海特菲尔德就不需要睁着一只眼睡觉了。同样可怕的函数声明可以简化如下:

```
**int_to_double** fun(**int_to_double** p1, **int_to_double** p2);
```

好多了。然后，我们可以创建更有意义的 *typedef* s like(有意将样式改为其他现代语言中熟悉的样式，但这并没有什么不同):

```
typedef int (***IntBiFunction**)(int, int);
typedef void (***IntConsumer**)(int);
typedef void (***Callback**)(void);
typedef bool (***IntPredicate**)(int);
typedef int (***IntSupplier**)(void);
```

传递它们轻而易举:

```
int **op**(int a, int b, **IntBiFunction** fun) {
    **return** fun(a, b);
}void **iter**(**IntConsumer** fun, int x) {
    **for** (unsigned int i = 0; i < x; ++i) {
        fun(i);
    }
}**callback** writeln(char *str) {
    **return** lambda(void, (void), { printf("%s\n", str); });
}
```

等等！λ？耶普。

# 我们能做什么

我们已经可以用函数和函数指针做很多事情了。

将它们作为参数传递:

```
int add(int a, int b);
int sub(int a, int b);int result = op(2, 1, add); // => 3
result = op(2, 1, sub);     // => 1
```

将它们赋给(指针)变量:

```
IntBiFunction addptr = add;
IntBiFunction subptr = sub;int result = op(2, 1, addptr);  // => 3
result = op(2, 1, subptr);      // => 1
```

从函数中返回它们:

```
IntBiFunction op(char c) {
    if (c == '+') {
        return add;
    } else {
        return sub;
    }
}
```

将它们放入数组:

```
typedef void (*Action)(void);void pull(void);
void push(void);
void idle(void);Action actions[] = { pull, push, idle };
```

比较它们:

```
if (fptr == push) {
```

等等。

但是有一件事我们不能做，匿名定义它们。

# 我们还能做什么

因此，尽管我们仍然不能匿名定义函数，编译器已经向前迈进了一步，并能够做一些神奇的事情，使我们更接近匿名和 lambda 函数。顺便说一下，是时候说几句术语了:

```
Anonymous functions are functions that are defined without a name, often inline, and therefore sometimes called ‘inline functions’ too, however that’s not exactly the point, and may cause confusion with C’s native concept of [inline functions](https://www.drdobbs.com/the-new-c-inline-functions/184401540), which is a different kind of animal. The other name for anonymous functions is *lambda functions*, which comes from the math theory of *lambda calculus*, where functions have no name.Another term, frequently occurs together with the previous ones, is *closure*. These are anonymous functions that also bind the scope, so that they capture the variables from their environment in the function body to be used even when it’s called from outside of this scope.The support for assigning functions to variables, passing them as arguments, returning them from functions, and putting them in data structures, especially using lambdas and closures, is called having *first-class functions*.
```

我们为什么关心？因为这样的函数更容易创建(事实上，它们可以在运行中定义)，并且作为参数传递给更高阶的函数，这使得能够编写一些算法和模式，同时也促进了并行计算。手头有了这些概念，也有助于我们以声明式风格编写代码，说明我们试图实现什么，而不是给出如何实现的指令。如果没有高阶函数，实现一些逻辑会比必要的更复杂。

那么如何才能做到呢？

# **海合会的做法**

使用 GNU C 编译器，当我们要调用函数时，我们可以动态地定义它们。但是在我向您展示如何操作之前，让我告诉您它需要的特性。这些是 GCC 提供的非标准特性:

1.  语句表达式

最简单的语句表达式是这样的:

```
**({** char *s = "Hello"; **s;** **})**
```

其中括号是表达式的容器，最后一个表达式(在我们的例子中只有变量`s`)提供了语句表达式计算的值。重点是，在语句表达式中，我们可以做的不仅仅是初始化变量。请考虑以下情况:

```
**({** char *s; s = calloc(20, sizeof(char)); **if** (argc > 1) **{** strcpy(s, "We take no arguments"); **} else {** strcpy(s, "Welcome to GCC"); **}** **s;** **})**
```

这是同一个表达式，但是它计算的值是动态变化的。和任何表达式一样，它可以赋给变量，或者作为函数参数传递。

2.嵌套函数

"嵌套函数是定义在另一个函数内部的函数."——在 [GNU 编译器文档](https://gcc.gnu.org/onlinedocs/gcc/Nested-Functions.html#Nested-Functions)中说明。我们不需要知道更多。

当我们结合这两个特征时，真正的好处就来了:

```
**({** void **prn**(int x) { printf("x = %d\n", x); } **&prn;** **})
   ^                                        ^ ^
   |                                        | `------- return value
   `----------------------------------------`--------- nested func.**
```

现在，我们的语句表达式的值是一个指针，指向我们刚刚嵌套创建的函数。

c 能提供的比看起来更多。幸运的是，C 支持宏，这是预处理程序的小替换规则，可以将我们更喜欢的语法转换成编译器可以理解的语法。这允许我们编写自己的匿名函数实现。

不难看出上面表达式中的模式，我们可以信赖一个宏，创建一个语句表达式，返回一个指向没有名字的函数的指针。或者它爱怎么叫就怎么叫，我们不在乎。这是我们获得匿名函数的方法。原作者在编写该宏并在本文中介绍其用法方面做得非常好，但他收到的愤怒评论足以写满一整章关于 lambdas 的内容，因此我觉得有必要在这里澄清一些事情，但我仍然欠你一句话，在这里引用该宏及其用法:

```
#define lambda(lambda$_ret, lambda$_args, lambda$_body)\
  ({\
    lambda$_ret lambda$__anon$ lambda$_args\
      lambda$_body\
    &lambda$__anon$;
  \})
```

宏有三个参数，返回类型、参数列表(args)和函数体，然后这些参数被放入一个语句表达式中，其中指向新创建的函数的指针将是表达式的值。我们可以如下使用该宏:

```
iter(**lambda(**void, (int x), { printf("%d\n", x); }**)**, 5);
```

这意味着我们的 lambda 函数只有一个 int 类型的参数`x`，并且不返回任何值。当然，我们的匿名函数的类型需要与我们传递的参数的类型相匹配。这提供了一些类型安全。

现在最难的部分:*捍卫概念*。在原文下面的评论页边上有一些旁注:

*   如果你不喜欢 lambdas，没关系——有些人也不喜欢宏。
*   不喜欢某样东西并不代表它无效。
*   如果你到目前为止还没有用过 lambdas，现在开始也不迟，但是…
*   没有人强迫你使用它们。
*   有没有可能用错？绝对的。
*   我们是否应该隐瞒这种技术，因为它可能会被错误地使用？肯定不是。即使是恶意代码，也最好暴露出来。
*   在 GCC 中实现*语句表达式*和*嵌套函数*是有原因的——如果你觉得它们有用，就使用它们。
*   没有人会因为*不*知道一些事情而成为更好的开发人员。

记住这一点，我们可以进入下一部分…

# **铿锵的接近**

GNU 不会因为实现了可以像 lambda 函数一样使用的编译器特性而被愤怒的开发人员孤立。事实上，Clang 走得更远，它提供了匿名函数，这些函数也能够捕获主体中实际使用的上下文，所以它们可以被认为是闭包！

让我们看看实际情况。

```
typedef void (^Callback)(void);Callback writeln(char *str);
void call(Callback callback);// ...
Callback my_callback = **^ {** printf("I was called!"); **}**;int y = 5;
iter(**^ (int x) {** printf("%d + %d = %d\n", y, x, y + x); **}**, 5);
```

正如我们所看到的，语法比 GCC 要干净得多。关键是`^` (=λ，lambda)，它告诉我们正在处理一个匿名函数。返回类型如果是 void 可以省略，省略的时候参数列表如果是空的也可以省略— `(void)`。所以语法本质上和函数指针是一样的，星号被一个脱字符号代替，但是我们不再受限于首先声明我们的函数，它可以被动态定义。在最后一行中，我们可以看到局部变量`y`是如何在函数体中被捕获的，并且在从`iter`函数中调用我们的闭包时变得可用，即使它不在它的作用域中，这是相当不错的。

这个功能在 Clang 里叫[封杀](https://clang.llvm.org/docs/BlockLanguageSpec.html)，苹果[提倡的](https://web.archive.org/web/20090920043909/http://images.apple.com/macosx/technology/docs/GrandCentral_TB_brief_20090903.pdf)。 *Y tho？*他们称之为“实现多核的更好方式”。这意味着他们看到了将函数式编程的高度要求的特性结合起来的潜力，这些特性促进了并行计算，并具有 c 语言的速度和硬件接近性。

然而，在 macOS 之外使用块并不简单。`BlocksRuntime`通常不是 Clang (LLVM)发行版的一部分，需要调整才能启用。即使这样，它可能会有臃肿的功能。因此，存在库的[解块克隆，可以单独下载和构建。](http://mackyle.github.io/blocksruntime/)

# 挑战极限

这些都是前沿的特性，推动了 C 编程语言的极限。同时，这也意味着它们的可用性受到编译器支持程度的限制。这破坏了可移植性，不应该在可移植性很重要的情况下使用。使用 GCC 的方法需要打开 2 级优化，并启用 C99 标准的扩展。编译方式如下:

```
gcc **-O2** -o fun.exe **--std=gnu99** fun.c
```

Clang 还需要一个语言扩展，这并不容易获得。我们很可能需要自己编译它，然后我们可以这样编译我们的程序:

```
clang **-fblocks** -o fun-clang.exe fun-clang.c **-lBlocksRuntime**
```

如果我们设法让我们的程序编译，如果我们试图在 GCC 中使用局部变量，或者如果我们不小心将函数指针指向`void*`并返回，它仍然会崩溃，这很容易使开发变得痛苦。

然而，即使这些技术的可用性有限，也绝对值得关注它们，因为它们正被用于某些目的，并且预计会被更多地使用。

另一方面，高阶函数并不是什么新东西，在标准库中(`bsearch`，`qsort`)，在 Linux 内核中，驱动程序接口是使用一个一级函数表定义的，在像 GTK 这样的流行库中，都可以找到高阶函数来支持回调。它们在不复杂的情况下解决了某些控制复杂性的问题，有助于应对并行计算的挑战，并且无论我们实际上使用哪种编程语言，它们都绝对值得学习。

尽管 C 并不以支持这些概念而闻名，尽管有更多适用的语言，它们为一级和更高阶的函数提供了更好的支持，但我们仍然可以在 C 中使用它们，如果我们发现它是合理的，并学习了这样做所需的工具。

# 结论

C 语言是一种非常强大的语言，毫不夸张地说，C 语言主宰着世界。它无处不在，但编程范式也是如此。几乎没有任何严肃的编程语言能够成功地将自己局限于一种范式。我们可以学习更多的范例，了解他们擅长的东西。如果我们为来自其他范例的概念找到了有效的用例，不管这种语言最常用的方式是什么，我们都可以从使用它们中受益。幸运的是，C 提供了对关于高阶函数和一级函数的最重要特性的支持，这些特性甚至可以在优秀编译器的帮助下进行扩展。

然而，我们应该记住在 C 中应用这些概念的利弊，并且还有其他的选择。c 互操作性是许多编程语言的关键，这进一步扩大了我们的选择范围。

作为比较，我们可以在 C++、C#和 Java 等语言中找到对这些概念(以及更多)的现成支持。对于 C 中的核心函数式编程，我们可以[将 Lisp](https://common-lisp.net/project/ecl/) 嵌入到 C 中。如果我们愿意离开 C，有一些围绕这些概念从头设计的语言，如 OCaml 和 Haskell。

无论我们把重点放在哪里，我们仍然可以从外国概念中受益，并把它们用在最合适的地方。了解更多只会让我们成为更好的开发者。