# 来自 Google 云存储的 ESP32 OTA 更新

> 原文：<https://medium.com/nerd-for-tech/esp32-ota-update-from-google-cloud-storage-a41a4b69a8a2?source=collection_archive---------6----------------------->

完全免责声明，我是嵌入式系统和 C++的新手，我希望听到任何和所有的反馈。

## 依赖关系:

对于这个项目，我使用的是 VS 代码的 pio 扩展。代码是用 c++写的，使用的是 arduino 框架。Platform.ini 文件:

```
[env:esp32dev]platform = https://github.com/platformio/platform-espressif32.gitboard = esp32devframework = arduino
```

服务器运行的是 express.js，部署到 Google App Engine。

## 服务器:

服务器将处理下载。从 GC-storage 中取出 bin 文件，并将其提供给 esp 设备。我将使用 Node 将我的设置部署到 App Engine，因此我将使用 express.js。服务器将非常简单，因此它应该很容易在许多其他平台上实现。

如果我们从 GET 函数的顶部开始，我们可以看到我将内容类型头设置为“应用程序/八位字节流”。这只是告诉任何浏览器这是代码，而不是应该向用户读出的东西。

在我的用例中，在 url 中编码一个版本，在大多数用例中，不应该由设备来决定运行哪个版本。

我尝试从我的 app.yaml 文件中指定的 google 云存储桶中获取一个文件，并创建一个读取流。

服务器做的最后一件事是通过调用:

```
downloadedFile.pipe(res);
```

要在 App Engine 中指定环境变量，请将其添加到 app.yaml 文件中:

```
env_variables: GCLOUD_STORAGE_BUCKET_OTA_UPDATE: name-of-bucket
```

## 电潜泵装置:

每次在我的 ESP 设备上运行 OTA 更新都需要很长时间。嵌入式系统不是我的强项。最后，我找到了一个可以在我的硬件上工作的设置，所以如果有什么不适合你，看看我在底部链接的文章。

由于所有 Google App 引擎默认使用 https 而不是 http，所以我们必须使用 WiFiClientSecure。如果不需要 ssl，可以将其更改为普通的 wifi 客户端。

稍后，我将制作一个教程，向客户端添加 CA 证书，但现在我将只使用 setInsecure()。这不应该在生产环境中进行，因为它和普通的 http 一样不安全。

```
client.setInsecure(); // Insecure as DUCK - change to use a cert.
```

因为我们使用了 ESP32 Http Updater 库，所以代码的其余部分是不言自明的。

## **上传新版本**

一旦代码启动并运行，你将需要固件文件。如果您使用 Platform.io，很容易找到固件文件，它应该位于您的项目的基础下。pio/build/ *环境名*，应该叫**固件. bin** 。

firmware.bin 应该总是以 **0xe9** 开头，所以上传文件后最好从服务器下载并运行:

```
$ hexdump .pio/build/esp32dev/firmware.bin
```

这应该打印出一大堆数字，你要滚动到顶部，看第一行，确保它像这样

```
0000000 e9 06 02 20 64 25 08 40 ee 00 00 00 00 00 00 00
```

其中 **e9** 是重要部分。如果不在那里，esp 将崩溃，并显示一个错误代码，说明有一个神奇的字节。

更新失败的另一个原因是设备上没有足够的空间。试着看一下分区场景。ESP 需要保留两倍的固件大小，因此，如果您有一个 4 MB 空间的 ESP 设备，您需要保留 2 MB 以使更新工作。您可以在下面的链接中查看 Pio 如何处理分区表。

帮助链接:

**Github 源代码:**【https://github.com/OscarHedeby/esp32-ota-server 

**使用 VS 代码启动并运行 Pio:**[https://randomnerdtutorials . com/VS-code-platform io-ide-esp32-esp8266-arduino/](https://randomnerdtutorials.com/vs-code-platformio-ide-esp32-esp8266-arduino/)

**分区表:**[https://docs . platform io . org/en/latest/platforms/espressif 32 . html # Partition-tables](https://docs.platformio.org/en/latest/platforms/espressif32.html#partition-tables)