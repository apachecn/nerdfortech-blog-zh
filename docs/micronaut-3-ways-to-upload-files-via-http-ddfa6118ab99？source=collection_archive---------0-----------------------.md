# Micronaut | 3 种通过 HTTP 上传文件的方法以及如何测试。

> 原文：<https://medium.com/nerd-for-tech/micronaut-3-ways-to-upload-files-via-http-ddfa6118ab99?source=collection_archive---------0----------------------->

![](img/3335021d7e9a73c8457a517e9bf31d42.png)

照片由 [Siora 摄影](https://unsplash.com/@siora18?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

几天前，我不得不上传一个压缩文件到服务器上。起初，我很难找到一个好的方法来上传它，但后来我决定用 Micronaut 的方式，因为无论如何我都在使用这个框架。网站上有三种不同的上传文件的方法。

# 使用流文件上传上传

StreamingFileUpload 以小块的形式上传整个文件，而不是一次上传。尤其是对于大文件，这是非常重要的。语法非常简单。首先，您有一个 Post 注释，我们在其中定义了上传链接的位置。重要的是对 consumes 部分使用 MULTIPART_FORM_DATA，对 produces 部分使用 TEXT_PLAIN。上传功能本身会创建一个临时文件，您可以将它更改为您的文件上传位置。它将上传的文件传输到所需的位置，在本例中是 tempFile，如果上传成功，则返回 true 值。如果该值为 true，它将返回一个 HttpResponse.ok，如果为 false，它将返回一个 HttpResponse 冲突，并显示消息“上传失败”。

要使用这种方法上传文件，您可以使用一个简单的 curl 命令:

```
curl -F "file=@YOUR_ZIP_FILE.zip" localhost:8080
```

这非常简单明了。但是，如果您不想直接保存文件并首先对其执行一些操作，该怎么办呢？这很快就会变得非常复杂。

# 上传字节数组

上传 ByteArray 而不是使用 StreamingFileUpload 的好处是，在对文件进行操作之前，您不必先保存文件。例如，如果您想先解压缩上传的文件，然后将已经解压缩的文件保存到文件系统中，这将非常方便。你可以创建一个像这样的函数来处理整个解压过程。

文件名是保存解压缩文件的位置。这个函数用 byteArray 创建一个 inputStream，并使用它生成一系列 zipEntries。它会过滤掉 __MACOSX 文件夹，并将所有反斜杠(如果存在的话)替换为正斜杠。然后，它在 targetFile 的 parentfile 中创建条目。最后，它使用 outputstream 将 zipStream 复制到 outputStream，output stream 的位置与文件名相同。

这已经是一个很好的解决方案，但是在上传文件时传递文件和文件名可能会有点不方便，特别是如果 fielname 和 file 是相同的输入值。

```
curl -F "file=@YOUR_ZIP_FILE.zip;fileName=YOUR_ZIP_ZILE_NAME.ZIP" localhost:8080
```

# 上传完整文件上传

对我来说，这是解决我的问题的最简单和最好的方法。CompletedFileUpload 有 getBytes()和 getFileName()函数，我可以在提取函数中直接使用它们。通过这种方式，我可以防止用户在 curl 命令中两次输入相同的文件名。它看起来会像这样。

```
curl -F "file=@YOUR_ZIP_FILE.zip" localhost:8080
```

请注意，这是与 StreamingFileUpload 完全相同的命令。这是因为两者几乎相同，不同的是，这个函数一次上传整个文件，而 StreamingFileUpload 以小块上传文件。另一个要提到的非常重要的事情是，StreamingFileUpload 没有 getBytes()方法，而这对于我们提取 zip 文件的方式是必不可少的。

# 测试成功上传

我用 AssertJ 来测试我上传的功能。我将只向您展示我的集成测试，因为其他测试是针对您自己的代码的。我们首先要看的是成功与否。为了模拟上传，我们必须首先准备一个 requestBody，其中包含所有需要的内容。在我们的例子中，它需要一个名称、一个文件名、内容类型和文件本身。之后，我们可以用 build()来构建它。然后，我们必须创建一个 HttpRequest 到上传函数正在监听的位置，并提供请求体。在这种情况下，它只是一个斜线。

对于响应，我们使用以下行:

```
client.toBlocking().exchange<MultipartBody, String>(request, String::class.java)
```

这实际上只是捕获请求的生成响应，并将其保存到一个变量中。

这个测试的最后一部分是断言。在这种情况下，我们想要测试状态、内容类型和主体消息。

# 测试失败的上传

我们还必须测试当上传失败时会发生什么。requestBody 实际上是相同的，只是文件不再是 zip 文件。相反，我们使用 txt 格式，这将导致上传时出错。

因为每个不是响应的 HttpResponse。OK 抛出异常，我们要断言它会抛出一个。这种情况下的异常类型是 HttpClientResponseException。因为我们已经断言将抛出异常，所以我们可以使用这个异常来获取响应状态和 contentType。正文与成功上传时略有不同。在这种例外情况下，身体总是空的。正因为如此，我们必须使用消息，它本质上只是正文的内容。

我希望这篇文章对你有用，现在你可以自己使用这些上传方法了。