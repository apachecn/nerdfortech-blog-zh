# 生锈的路上发生了一件有趣的事

> 原文：<https://medium.com/nerd-for-tech/a-funny-thing-happened-on-the-way-to-rust-75379c74e99a?source=collection_archive---------5----------------------->

![](img/2afe7fc15e49a9e9d705cbd0a3e0419a.png)

我最近开始花时间学习 Rust，因为它有很多我想要的 Dart 之外的语言的特性。对我来说，Rust 提供了一种编译到本机的语言，也可以在没有大型标准库/运行时的情况下使用，支持简洁的二进制文件，并能够在“裸机”环境中运行，既可以在大型设备上运行，如 Raspberry Pi，也可以在更小的微控制器上运行，如 ARM cortex-M 和 ESP32 等。它也是一种现代语言，具有函数特性、不可变数据、没有 GC 的良好内存管理，同样重要的是，它不是极其复杂的 C++。除此之外，它还使得与现有 C 库的互操作变得容易，并且同样允许容易地创建公开 C APIs 的库。最后，易于交叉编译、丰富的包生态系统和优秀的工具是程序员学习和使用的非常有吸引力的选择。

然而，尽管如此，Rust 并不是一门容易掌握并开始使用的语言，所以我很惊喜地偶然发现了另一种与 Rust 类似的新语言[叫做“V”](https://vlang.io/)。

然而，与 Rust 不同，我能够在一个小时左右的时间里学会使用 V 的核心概念，这似乎是该语言的主要目标之一。它实际上让我想起了 Dart，它的目标是让已经知道另一种类似 C 语言的程序员变得容易和熟悉。

# 安装和使用 V

V 的另一个关键卖点是它简单的安装方法和极快的编译时间。这种说法至少被证明是正确的，按照它的 [Linux 安装指令](https://github.com/vlang/v/blob/master/doc/docs.md#linux-macos-freebsd-etc)在 Debian 上安装它是非常容易的，我发现 V 使用自己的单个 C 文件实现来启动自己的编译器给我留下了非常深刻的印象。

编写 hello world 示例非常简单，V 工具提供了搭建新项目的功能、在工作时运行代码的简单命令等，Dart 程序员(甚至 Node、Deno、Rust 用户等)也会非常熟悉这些功能，并期望将其作为标准。

# FFI 或半身像

在学习了这门语言的基础知识之后，我决定尝试快速创建一个简单的动态库，我可以通过 FFI 从 Dart 应用程序中调用它。

正是在这一点上，我碰到了 V 的一个粗糙的边缘，仍然是一个相当新的语言和小项目。文档中没有提到如何创建动态库，甚至在 v tooling 帮助中也没有。我最终找到了[一个 GitHub 问题](https://github.com/vlang/v/issues/2379)，它碰巧有所需的信息，你需要执行以下命令:

```
v -shared -prod fib.v
```

那期杂志还评价了 V 为其 C abi 函数命名所使用的相当“简单”的名称处理方案，以及 V 中现在可用的新功能，即使用 V 所谓的函数属性来设置函数的 C ABI 名称:

```
[export: 'fibv'] pub fn fib(n int) int { ...
```

从 Dart 调用该函数就是标准的 Dart FFI 机制:

```
import 'dart:ffi'; // Fib function from V 
typedef FibFunc = Int32 Function(Int32 a); typedef Fib = int Function(int a); void main() async { // expect this being run from same dir that contains the dyn shared-lib file   final dylib = DynamicLibrary.open('./fibrec.so');   final fibPointer = dylib.lookup<NativeFunction<FibFunc>>('fibv');     
  final fib = fibPointer.asFunction<Fib>(); 
  print('Fib 7 = ${fib(7)}'); 
}
```

包含上述代码样本的完整示例可以在 [my github repo](https://github.com/maks/v_dart_ffi_example) 中找到。

# 没有独角兽

和所有的语言和技术一样，使用 V 也有很多缺点和优点。虽然它很简单，并且有很好的文档记录，但是我还是设法在编译器文档中找到了使用 V 的漏洞。也因为它相对较新，社区和生态系统才刚刚开始发展，所以，例如，虽然有一个跨平台音频库的绑定，但它本身非常不成熟，非常不完整(没有音频输入)，对我当前项目所需的其他东西的支持，如 Midi 和有机发光二极管 I2C 显示器，是不存在的。

另一个问题是，虽然 V 很自豪它生成的二进制文件比同类语言 Go(和 Dart)的要小得多，但它似乎仍然需要一个相当大的标准库/运行时来包含在它的二进制文件中。即使使用了`-prod`标志，我发现我的几乎像 hello-world 一样的代码产生了一个大约 300kb 的共享动态库，一个大约 100kb 的可执行文件。虽然这些非常小，但它们远没有达到使用其`#![no_std]`时 Rust 可能产生的微小尺寸，在使用小型微控制器时，即使几 kb 也是至关重要的。

所以最后我会用 V 代替 Rust 吗？嗯，没有。

虽然我对 V 印象非常深刻，并将在未来继续检查它的进展，但需要为 ARM 和嵌入式设备提供成熟且经过充分测试的交叉编译工具链，以及 Rust 现在拥有的巨大社区和软件包生态系统，这意味着尽管 V 的简单性有巨大的吸引力，但目前我仍在学习 Rust，并计划在桌面、移动、wasm 和嵌入式微控制器的许多未来项目中使用它。

如果您有兴趣阅读更多关于 Dart FFI、在 Flutter 应用程序中使用 FFI 或在使用和不使用 Dart 的情况下使用 Rust 的信息，请关注我，了解我即将发布的新文章！

*最初发表于*[*【https://manichord.com】*](https://manichord.com/blog/posts/on-way-to-rust.html)*。*