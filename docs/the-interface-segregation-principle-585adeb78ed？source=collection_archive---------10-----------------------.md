# 界面分离原理

> 原文：<https://medium.com/nerd-for-tech/the-interface-segregation-principle-585adeb78ed?source=collection_archive---------10----------------------->

这是第四个，很直接。它规定任何客户端都不应被迫依赖于不使用。

留下一个空白的方法是一个糟糕的软件设计的标志，当然这可以通过使用接口作为数据结构主角来修复。

这里有一个不好的例子。假设您的办公室里有一堆打印机，要求您对它们进行统一分类。每种类型的打印机都有不同的功能。

分类从使用一个接口开始，该接口以方法的形式列出了每个可能的功能。像这样。

```
public interface IMultiFunction {
  public void print();
  public void getPrintSpoolDetails();
  public void scan();
  public void scanPhoto();
  public void fax();
  public void internetFax();
}
```

然后，你可以有许多类来实现这个接口并使用每个方法，这里有三个例子。

`CanonPrinter`级。

```
public class CanonPrinter implements IMultiFunction {[@Override](http://twitter.com/Override)
  public void print() {
    // capable
  }[@Override](http://twitter.com/Override)
  public void getPrintSpoolDetails() {
    // capable
  }[@Override](http://twitter.com/Override)
  public void scan() {}[@Override](http://twitter.com/Override)
  public void scanPhoto() {}[@Override](http://twitter.com/Override)
  public void fax() {}[@Override](http://twitter.com/Override)
  public void internetFax() {}
}
```

`HPPrinterNScanner`和`XeroxPrinter`是一样的，这些名字只是举几个例子。

接口迫使你实现所有在它里面声明的方法，当不需要的时候产生了许多空方法，但是也实现了。

有了这一切，怎么能解决呢？

诀窍是缩小特定操作的接口范围，这将增加文件，但对于给类提供准确的功能来说是有效的，避免了空方法。

这种新方法通过添加`IFax`、`IPrint`和`IScan`接口来删除`IMultiFunction`接口。类似这样的。

```
public interface IFax {
  public void fax();
  public void internetFax();
}public interface IPrint {
  public void print();
  public void getPrintSpoolDetails();
}public interface IScan {
  public void scan();
  public void scanPhoto();
}
```

这些类将保持不变，但要实现的方法更少。

这是结果。

```
public class CanonPrinter implements IPrint {[@Override](http://twitter.com/Override)
  public void print() {
    // capable
  }[@Override](http://twitter.com/Override)
  public void getPrintSpoolDetails() {
    // capable
  }
}
```

也

```
public class HPPrinterNScanner implements IPrint, IScan {[@Override](http://twitter.com/Override)
  public void print() {
    // capable
  }[@Override](http://twitter.com/Override)
  public void getPrintSpoolDetails() {
    // capable
  }[@Override](http://twitter.com/Override)
  public void scan() {}[@Override](http://twitter.com/Override)
  public void scanPhoto() {}
}
```

看看那里的双接口实现。

最后。

```
public class XeroxPrinter implements IPrint, IScan, IFax {[@Override](http://twitter.com/Override)
  public void print() {
    // capable
  }[@Override](http://twitter.com/Override)
  public void getPrintSpoolDetails() {
    // capable
  }[@Override](http://twitter.com/Override)
  public void scan() {}[@Override](http://twitter.com/Override)
  public void scanPhoto() {}[@Override](http://twitter.com/Override)
  public void fax() {}[@Override](http://twitter.com/Override)
  public void internetFax() {}
}
```

和一个三接口实现。

坚实的原理相辅相成，同心协力，达到设计良好的软件的共同目的。

现在让我们转到最后一个原则，依赖性反转原则。