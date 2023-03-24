# 表明你的意图

> 原文：<https://medium.com/nerd-for-tech/signal-your-intentions-6895cf4cfaea?source=collection_archive---------6----------------------->

## 通过 Dart 使用 Unix 信号

![](img/df2ddb3f29af45e89cb5db95be559e31.png)

# 信号？

到 2021 年的这个时候，Unix 已经有了丰富的半个*世纪*的历史。基于终端的 shells 和应用程序可以利用的 Unix 特性之一是信号。

# 飞镖道

幸运的是，对于使用 Dart 命令行的开发人员来说，早在 2013 年，Dart 程序就获得了全程监听 unix 信号的支持。用于监听信号的 Dart API 通常很容易，只需监听一个普通的 Dart 流即可，不过您需要记住的唯一一件事是，每个信号都由它自己的实例表示为 [ProcessSignal 类](https://api.dart.dev/stable/2.10.0/dart-io/ProcessSignal-class.html)的静态常数，因此要监听一个信号，例如 SIGINT 信号如下所示:

```
// typically ctrl-c in shell will generate a sigint
ProcessSignal.sigint.watch().listen((signal) {
  print('sigint disconnecting');
  // do any required cleanup tasks 
  exit(0);
});
```

不幸的是 [DartVM 为自己保留监听许多 Unix 进程信号](https://api.dart.dev/stable/2.10.0/dart-io/ProcessSignal/watch.html)，允许 Dart 程序只监听:

*   西格胡普
*   信号情报
*   信号术语
*   SIGUSR1
*   SIGUSR2
*   西格温奇

在 Dart 提供给我们听的信号中，我们可能最想听的是`SIGINT`，我在上面演示过，它对应于用户在大多数终端中做 [CRTL-C。](https://unix.stackexchange.com/a/362566/8837)

这就是在 Dart CLI 程序中处理信号的全部内容！

我希望这对你有所帮助，如果有或者你有任何其他方便的技巧可以分享，请在下面的评论中或者通过 [Twitter](https://twitter.com/mklin) 告诉我。

*原载于*[*https://manichord.com*](https://manichord.com/blog/posts/signal-your-intent.html)*。*