# 使用 Google Gson 将 Json 转换成 Kotlin 对象

> 原文：<https://medium.com/nerd-for-tech/using-google-gson-to-convert-json-into-kotlin-objects-3b9f27f8454?source=collection_archive---------8----------------------->

![](img/58c6d55845ae44347955b553f656a4bf.png)

费伦茨·阿尔马西在 [Unsplash](/s/photos/json?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

在开发一些应用程序、API 等时，将 Json 解析成 Kotlin 对象是很常见的事情。在本文中，我将尝试解释如何使用 Google [**Gson**](https://github.com/google/gson) 库将一个简单的 Json 转换成 Kotlin 对象。

## 添加依赖性

要使用 [**Gson**](https://github.com/google/gson) 库，您必须在 app 模块 Gradle 文件(Module: app/build.gradle)中添加依赖项并重新同步。

## 将 Json 转换为 Kotlin 对象

假设我们有下面这个数据对象。

我们可以创建一个函数，使用 **Gson()接收 json 字符串作为参数，并返回我们转换后的 Food 对象。fromJson()** 要转换的函数。

## 反之亦然

您还可以将一个数据对象转换成一个 Json 字符串，这与下面这个函数的想法是一样的。

这只是一个在 Kotlin 中转换 json 的简单方法，希望对你有所帮助。