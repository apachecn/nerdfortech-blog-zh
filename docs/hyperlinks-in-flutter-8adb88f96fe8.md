# 颤动中的超链接

> 原文：<https://medium.com/nerd-for-tech/hyperlinks-in-flutter-8adb88f96fe8?source=collection_archive---------11----------------------->

![](img/6e7f395c90f8fecf03f7f04ecfdf903f.png)

超链接是对数据的引用，用户可以通过点击它来跟踪。超链接指向整个文档或文档中的特定元素。带有超链接的文本是超文本。

手机应用程序中有链接会将用户重定向到外部网站。

# url _ 启动器

“url_launcher”是一个会启动 url 的 flutter 插件。

在 pubspec.yaml 中添加插件。

```
url_launcher: ^6.0.2
```

然后运行命令激活插件

```
flutter pub get
```

我们将创建一个简单的文本小部件，并用一个手势检测器()包装它

```
Scaffold(
  body: Center(
    child: GestureDetector(
      onTap: ()async{
        var url = "https://flutter.dev/";

        if(await canLaunch(url)){
          await launch(url);
        }else{
          throw 'error launching $url';
        }
      },
      child: Text("To Read more...",style: TextStyle(
        color: Colors.*blue*,
        fontSize: 30,
        fontWeight: FontWeight.*bold* ),),
    ),
  ),
)
```

我们定义了一个变量 URL，它有到相应网站的链接。可以使用 canLaunch()方法检查支持的 URL 方案。

![](img/e2f25aeb2c87e09307c4620b0bf73590.png)![](img/374f751390a74ce1f0ba1fa5b0d1f894.png)

# `flutter_linkify`

将文本 URL 和电子邮件转换为文本中可点击的内嵌链接。建议添加 url_launcher 在浏览器中打开链接。

```
dependencies:
         flutter_linkify: ^3.1.3
         url_launcher: ^6.0.2
```

现在你可以直接使用下面的小工具了

```
Linkify(
            onOpen: (link)  {
             **if** (**await** canLaunch(link.url)) {
                   **await** launch(link.url);
             } **else** {
                  **throw** 'Could not launch $link';
              }  
           },
            text: "Linkify click -  https://www.youtube.com/channel/UCwxiHP2Ryd-aR0SWKjYguxw",
            style: TextStyle(color: Colors.red),
            linkStyle: TextStyle(color: Colors.pink),
            ),
```

onOpen()回调将传递链接，您可以从那里执行任何操作。在这里我们打开了链接。

谢谢大家！