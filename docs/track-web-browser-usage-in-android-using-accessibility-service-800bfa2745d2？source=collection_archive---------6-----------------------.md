# 使用辅助功能服务跟踪 Android 中网络浏览器的使用情况

> 原文：<https://medium.com/nerd-for-tech/track-web-browser-usage-in-android-using-accessibility-service-800bfa2745d2?source=collection_archive---------6----------------------->

![](img/ca092f9c6ffc05f204c59e08cf8b324a.png)

虽然无障碍服务旨在帮助残疾用户，但它是跟踪用户行为的强大工具。它让我们能够跟踪用户手势，监控应用程序的使用，还可以从各种应用程序中读取文本等。

我们可以通过以下方式实现可访问性服务监听器来跟踪用户访问的 URL。它首先以下面的方式在 AndroidManifest.xml 文件中定义我们的服务。

```
<service android:name=".LogUrlService"
    android:permission="android.permission.BIND_ACCESSIBILITY_SERVICE">
    <intent-filter>
        <action android:name="android.accessibilityservice.AccessibilityService" />
    </intent-filter>
    <meta-data android:name="android.accessibilityservice" android:resource="@xml/accessibilityservice" />

</service>
```

我们想要监听的事件类型的详细信息将保存在一个名为 accessibilityservice 的 xml 文件中。这是那个文件的内容。我们感兴趣的事件是 typeWindowsChanged、typeWindowStateChanged 和 typeWindowContentChanged。另一个重要的属性是 canRetrieveWindowContent，它被设置为 true。

```
<?xml version="1.0" encoding="utf-8"?>
<accessibility-service xmlns:android="http://schemas.android.com/apk/res/android"
    android:accessibilityEventTypes = "typeWindowsChanged|typeWindowStateChanged|typeWindowContentChanged"
    android:accessibilityFeedbackType="feedbackVisual"
    android:notificationTimeout="300"
    android:accessibilityFlags="flagDefault|flagIncludeNotImportantViews|flagRequestTouchExplorationMode|flagRequestEnhancedWebAccessibility|flagReportViewIds|flagRetrieveInteractiveWindows"
    android:canRetrieveWindowContent="true"

    >

</accessibility-service>
```

访问此服务的权限需要由用户明确授予您的应用程序，方法是导航至设置->应用程序与通知>高级>特殊应用程序用法>辅助功能，或者我们可以通过调用辅助功能设置意图以编程方式完成此操作

```
Intent intent = new Intent(Settings.*ACTION_ACCESSIBILITY_SETTINGS*);
startActivity(intent);
```

一旦许可被授予，每当可访问性事件被触发时，我们的服务就开始接收回调。由于该事件将由所有应用程序触发，我们需要对其进行过滤，以确保我们只处理与 web 浏览器相关的事件。我们需要预先知道我们要跟踪的浏览器的包名。每次事件被触发时，我们都会看到一个 AccessibilityNodeInfo 对象，它包括窗口内容以及这个父对象的子 AccessibilityNodeInfo 的树。我们寻找一个特定的节点，它代表浏览器的地址栏。这对于每个浏览器都是唯一的。这是一个一次性的活动，我们通过遍历节点，找出哪个节点包含数据，并获得相关的节点 id 名称。

```
private void getChild(AccessibilityNodeInfo info)
{
    int i=info.getChildCount();
    for(int p=0;p<i;p++)
    {
        AccessibilityNodeInfo n=info.getChild(p);
        if(n!=null) {
            String strres = n.getViewIdResourceName();
            if (n.getText() != null) {
                String txt = n.getText().toString();
                Log.*d*("Track", strres + "  :  " + txt);
            }
            getChild(n);
        }
    }
}
```

一旦完成，我们就可以用包名和惟一的节点 id 名构建一个列表。以下是 android 上一些流行浏览器的列表

```
private static List<SupportedBrowserConfig> getSupportedBrowsers() {
    List<SupportedBrowserConfig> browsers = new ArrayList<>();

 browsers.add( new SupportedBrowserConfig("com.android.chrome", "com.android.chrome:id/url_bar"));

   browsers.add( new SupportedBrowserConfig("org.mozilla.firefox", "org.mozilla.firefox:id/mozac_browser_toolbar_url_view"));

   browsers.add( new SupportedBrowserConfig("com.opera.browser", "com.opera.browser:id/url_field"));

   browsers.add( new SupportedBrowserConfig("com.opera.mini.native", "com.opera.mini.native:id/url_field"));

   browsers.add( new SupportedBrowserConfig("com.duckduckgo.mobile.android", "com.duckduckgo.mobile.android:id/omnibarTextInput"));

   browsers.add( new SupportedBrowserConfig("com.microsoft.emmx", "com.microsoft.emmx:id/url_bar"));

    return browsers;
}
```

现在剩下的工作就是在浏览器窗口激活或改变时查询特定节点的内容。

```
case AccessibilityEvent.*TYPE_WINDOW_STATE_CHANGED*:

case AccessibilityEvent.*TYPE_WINDOWS_CHANGED*:
case AccessibilityEvent.*TYPE_WINDOW_CONTENT_CHANGED*: {
    AccessibilityNodeInfo parentNodeInfo = event.getSource();
    if (parentNodeInfo == null) {
        return;
    }

    String packageName = event.getPackageName().toString();
    SupportedBrowserConfig browserConfig = null;
    for (SupportedBrowserConfig supportedConfig: *getSupportedBrowsers*()) {
        if (supportedConfig.packageName.equals(packageName)) {
            browserConfig = supportedConfig;
        }
    } if (browserConfig == null) {
        return;
    }

    String capturedUrl = captureUrl(parentNodeInfo, browserConfig);
    parentNodeInfo.recycle();

    if (capturedUrl == null) {
        return;
    }
```

现在，我们可以在后台持续监控用户访问的所有 URL，即使浏览器以匿名模式运行。

你可以在这里找到这个项目的源文件