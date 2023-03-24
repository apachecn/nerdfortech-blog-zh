# 如何解决电容器错误:没有这样的模块' FirebaseInstanceID '

> 原文：<https://medium.com/nerd-for-tech/how-to-solve-capacitor-error-no-such-module-firebaseinstanceid-9c142933b589?source=collection_archive---------1----------------------->

如果您在 Capacitor 或带 Capacitor 的 Ionic 框架中构建您的 iOS 应用程序，您可能会遇到以下错误:

```
ios/App/App/AppDelegate.swift:4:8: error: no such module 'FirebaseInstanceID'
```

# 问题是

如果您已经为通知实现了 Firebase Cloud Messaging，那么您必须添加`FirebaseInstanceID`包来接收 FCM 令牌。您还必须将以下代码添加到您的`AppDelegate.swift`文件中:

问题是`FirebaseInstanceID`最近被弃用了，所以上面的代码在新版本中不再有效。

# 解决方案

FCM 令牌现在直接在`Messaging`实例上提供。在你的`AppDelegate.swift`中修改上面的代码以匹配下面的代码。

最后一件事是从你的`AppDelegate.swift`中移除`FirebaseInstanceID`导入。只需**移除**以下线路:

```
import FirebaseInstanceID
```

另外，确保您已经导入了`FirebaseMessaging`。

如果你有任何问题，请在下面的评论中提问。

# 想和我去喝杯咖啡吗？

这篇文章把你从一整天的谷歌搜索中解救出来了吗？厉害！你可以在这里给我买杯咖啡:[https://www.buymeacoffee.com/goranlisak](https://www.buymeacoffee.com/goranlisak)

还需要帮助吗？你可以在这里和我预约会面:[https://www.buymeacoffee.com/goranlisak/extras](https://www.buymeacoffee.com/goranlisak/extras)