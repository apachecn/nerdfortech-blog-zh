# 如何在 Webview Flutter 中添加内嵌 HTML

> 原文：<https://medium.com/nerd-for-tech/how-to-add-inline-html-in-webview-flutter-fcc7e0f6a3a2?source=collection_archive---------1----------------------->

![](img/53e9de0aa093bdf269bccc1c29e4e2ce.png)

由 [Ahmad Odeh](https://unsplash.com/@aoddeh?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我以为你看了上面的图片后也会跳舞，因为我现在就在跳舞！哈哈…

好了，回到今天的正题……我不打算一一讲述“*创建一个颤动的应用程序，等等等等*”。我相信你已经可以或者已经创建了一个 Flutter 应用程序，我相信你！

现在我有了 dart 文件，名为: *Webview.dart.*

```
import 'package:flutter/material.dart';

class Webview extends StatefulWidget {
  @override
  _WebviewState createState() => _WebviewState();
}

class _WebviewState extends State<Webview> {
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```

这就是我们所拥有的，我们需要将 [Flutter_Webview](https://pub.dev/packages/webview_flutter) 包添加到我们的*中。yaml:*

```
dependencies:
  flutter:
    sdk: flutter

  *# The following adds the Cupertino Icons font to your application.
  # Use with the CupertinoIcons class for iOS style icons.* cupertino_icons: ^1.0.2
  webview_flutter: ^2.0.9

dev_dependencies:
  flutter_test:
    sdk: flutter
```

记得更换你的*minSDKVersion:*Android>app>build . gradle

```
defaultConfig **{** // *TODO: Specify your own unique Application ID (https://developer.android.com/studio/build/application-id.html).* applicationId "connelblaze.cb.ranqz"
    minSdkVersion 20
    targetSdkVersion 30
    versionCode flutterVersionCode.toInteger()
    versionName flutterVersionName
**}**
```

然后将权限添加到您的清单文件中:

```
<manifest...
><uses-permission android:name="android.permission.INTERNET" />
...<application
```

现在回到我们的 dart 文件 main.dart:

```
import 'package:flutter/material.dart';
import 'package:ranqz/screen/Home.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(

        primarySwatch: Colors.*blue*,
      ),
      home: Home(),
    );
  }
}
```

Home.dart:

```
import 'package:flutter/material.dart';
import 'package:ranqz/screen/Webview.dart';

class Home extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Home'),
      ),
      body: Container(
        child: Center(
          child: GestureDetector(
            onTap: () {
              Navigator.*push*(context, MaterialPageRoute(builder: (context) => Webview()));

            },
            child: Text("Go to webview"),
          ),
        ),
      ),
    );
  }
}
```

Webview.dart:

```
import 'dart:async';
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:webview_flutter/webview_flutter.dart';

class Webview extends StatefulWidget {
  @override
  _WebviewState createState() => _WebviewState();
}

class _WebviewState extends State<Webview> {
  final Completer<WebViewController> _controller =
  Completer<WebViewController>();
  WebViewController _con;

  /*
  *
  * Webview
  * */

  String setHTML(String email, String phone, String name) {
    return ('''
    <html>
      <head>
        <!-- Required meta tags -->
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
        <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
      </head>

        <body style="background-color:#fff;height:100vh ">

          <div style="width: 50%; margin: 0 auto;margin-top: 200px">
            <table class="table table-striped">
              <tbody>
                <tr>
                  <th>Name</th>
                  <th>$name</th>
                </tr>
                <tr>
                  <th>Email</th>
                  <td>$email</td>
                </tr>
                <tr>
                  <th>Phone</th>
                  <th>$phone</th>
                </tr>
              </tbody>
            </table>
            <a type="button" class="btn btn-success" style="width: 210px" href="https://connelevalsam.github.io/connelblaze/">
              Submit
            </a>
          </div>
        </body>
      </html>

    ''');
  }

  _printz() => print("Hello");

  _loadHTML() async {
    _con.loadUrl(Uri.dataFromString(
        setHTML(
          "connelblaze@gmil.com",
          "+2347034857296",
          "Connel Asikong"
        ),
        mimeType: 'text/html',
        encoding: Encoding.*getByName*('utf-8')
    ).toString());
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Webview'),
      ),
      body: Builder(builder: (BuildContext context) {
        return WebView(
          initialUrl: 'https://flutter.dev',
          javascriptMode: JavascriptMode.unrestricted,
          onWebViewCreated: (WebViewController webViewController) {
            // _controller.complete(webViewController);
            _con = webViewController;
            _loadHTML();
          },
          onProgress: (int progress) {
            print("WebView is loading (progress : $progress%)");
          },
          navigationDelegate: (NavigationRequest request) {
            if (request.url.startsWith('https://www.youtube.com/')) {
              print('blocking navigation to $request}');
              return NavigationDecision.prevent;
            }
            print('allowing navigation to $request');
            return NavigationDecision.navigate;
          },
          onPageStarted: (String url) {
            print('Page started loading: $url');
          },
          onPageFinished: (String url) {
            print('Page finished loading: $url');
          },
          gestureNavigationEnabled: true,
        );
      }),
    );
  }

}
```

恭喜你！！！你现在可以说:“妈妈，我成功了！

无论如何，你有任何意见或要说的，打电话给我。
或者你想捐赠给这位英俊潇洒的年轻男性……你仍然可以来找我！

这是第 2 部分，使用了 [Javascript](https://connelblaze.medium.com/working-with-javascript-in-webview-flutter-639ee09124af)

干杯，继续编码！

给我买杯咖啡