# 在颤动中容易地显示进度指示器

> 原文：<https://medium.com/nerd-for-tech/showing-a-progress-indicator-easily-in-flutter-aa564eb0df5c?source=collection_archive---------7----------------------->

![](img/a2260c351c05970da4942423cce5bcf4.png)

迈克·范·登博斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

事实上，异步操作对于我们编写的几乎每个应用程序都是必要的。有时，用户必须等到操作完成才能再次与应用程序交互，这正是进度指示器的目的，向用户提供反馈，以便他们可以知道正在处理的事情。

防止用户在异步操作加载时与应用程序进行交互，方法是使用对话框。在 Flutter 中显示对话框最简单的方法是调用函数 showDialog()，它接收一个上下文和一个生成器:

```
show(BuildContext context, {String text = 'Loading...'}) {
  showDialog<void>(
      context: context,
      barrierDismissible: false,
      builder: (BuildContext context) {
        _context = context;
        isDisplayed = true;
        return WillPopScope(
          onWillPop: () async => false,
          child: SimpleDialog(
            backgroundColor: Colors.white,
            children: [
              Center(
                child: Column(
                  mainAxisSize: MainAxisSize.min,
                  children: [
                    Padding(
                      padding: const EdgeInsets.only(left: 16, top: 16, right: 16),
                      child: CircularProgressIndicator(),
                    ),
                    Padding(
                      padding: const EdgeInsets.all(16),
                      child: Text(text),
                    )
                  ],
                ),
              )
            ] ,
          ),
        );
      }
  );
}
```

上下文对于告诉 flutter 哪个小部件被调用为对话框是必要的，而构建器是我们为对话框定义 UI 的地方。我们还有参数 barrierDismissible，当我们将这个参数定义为 false 时，我们可以防止用户通过触摸对话框外的屏幕区域来关闭对话框。但是这个参数并不阻止用户通过点击 android 设备中的硬件后退按钮来关闭对话框，为了避免这种情况，我们用 WillPopScope 包装了 SimpleDialog，当调用回调 onWillPop 时，它返回 false。

好了，我们已经定义了 showDialog 函数来显示进度对话框，现在我们只需要一个简单的方法来在代码中的任何地方重用它。为了实现这一点，我们将创建一个类来封装我们的函数，并且我们将使用 singleton 模式来避免每次我们想要调用函数时创建一个新的对象。我们的类将看起来像这样:

```
import 'package:flutter/material.dart';
import 'package:flutter/widgets.dart';

class LoadingIndicatorDialog {
  static final LoadingIndicatorDialog _singleton = LoadingIndicatorDialog._internal();
  late BuildContext _context;
  bool isDisplayed = false;

  factory LoadingIndicatorDialog() {
    return _singleton;
  }

  LoadingIndicatorDialog._internal();

  show(BuildContext context, {String text = 'Loading...'}) {
    if(isDisplayed) {
      return;
    }
    showDialog<void>(
        context: context,
        barrierDismissible: false,
        builder: (BuildContext context) {
          _context = context;
          isDisplayed = true;
          return WillPopScope(
            onWillPop: () async => false,
            child: SimpleDialog(
              backgroundColor: Colors.white,
              children: [
                Center(
                  child: Column(
                    mainAxisSize: MainAxisSize.min,
                    children: [
                      Padding(
                        padding: const EdgeInsets.only(left: 16, top: 16, right: 16),
                        child: CircularProgressIndicator(),
                      ),
                      Padding(
                        padding: const EdgeInsets.all(16),
                        child: Text(text),
                      )
                    ],
                  ),
                )
              ] ,
            ),
          );
        }
    );
  }

  dismiss() {
    if(isDisplayed) {
      Navigator.of(_context).pop();
      isDisplayed = false;
    }
  }
}
```

如果您阅读代码，您会发现在 show()方法中，我们将对话框生成器上下文的值赋给了一个类成员私有变量 _context。这个变量的目的是保存调用对话框的上下文。然后，在 dissolve()方法中，我们可以使用该上下文弹出()对话框。此外，我们还显示了变量 is。这个变量将阻止我们显示对话框两次，并帮助我们避免试图关闭一个不存在的对话框。

现在，我们准备在代码中的任何位置显示进度指示器，如下所示:

```
LoadingIndicatorDialog().show(context);
await asynchronousOperation();
LoadingIndicatorDialog().dismiss();
```

就是这样！现在，我们可以在代码中的任何地方轻松地显示进度指示器。希望这些信息对你有所帮助。另一个故事再见。