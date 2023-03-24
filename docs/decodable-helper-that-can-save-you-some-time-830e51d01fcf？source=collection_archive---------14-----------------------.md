# 可解码的助手，可以节省你一些时间

> 原文：<https://medium.com/nerd-for-tech/decodable-helper-that-can-save-you-some-time-830e51d01fcf?source=collection_archive---------14----------------------->

![](img/39aa9840b79d1bb21e182399d3cbb471.png)

照片由 [Aron 视觉效果](https://unsplash.com/@aronvisuals?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/time?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

编写一个 iOS 应用程序几乎总是涉及到将来自后端响应或 JSON 文件的数据解析成我们领域的结构或类，因此，解码数据应该尽可能地干净、容易出错和透明。

## 问题是

```
// 1\. Parsing the json (wherever it comes from) to data
let jsonData = JSON.data(using: .utf8)!// 2\. Decoding the data to the actual structure
let fact = try! JSONDecoder().decode(CatFact.self, from: jsonData)
```

这是“最”常用的方法，但是，这两行是强制解包值，由于可能发生“意外”的崩溃，不建议在生产中使用。为了避免这种情况，让我们使用可选绑定以更安全的方式来实现。

```
guard let jsonData = JSON.data(JSON.data(using: .utf8)) else {
    return nil
}do {
    let fact = try JSONDecoder().decode(CatFact.self, from: jsonData)
    return fact
}
catch {
    print(error.localizedDescription)
    return nil
}
```

是的，但不是，因为现在在项目中的每个请求或服务中编写这段代码已经变得很乏味了。然而，考虑到所有符合协议解码的结构和类都可以被重构，让我们把这个片段移到协议本身的一个静态函数中。

## 一个可能的解决方案

```
extension Decodable {
    static func parse(from data: Data?) -> Self? {
        // ...
        return nil
    }
}// We can now use it as follows
let fact = CatFat.parse(from: JSON.data)
print(fact)
```

这似乎只适用于数据，为了改进这一点，让我们通过接受任何类型来增加扩展的灵活性，这样我们就可以从几乎任何 JSON 内容到所需的对象解析它。让我们看看`data(from: Any?)`函数的实现，它将检查不同类型的内容，并返回数据中的对等内容。

```
extension Decodable {
    static func parse(from item: Any?) -> Self? {
        // ...
    }

    private static func data(from item: Any?) -> Data? {
        switch item {
        case let data as Data:
            return data
        case let string as String:
            return string.data(using: .utf8)
        case let item?:
            return try? JSONSerialization.data(withJSONObject: item, options: [])
        default:
            return nil
        }
    }
}
```

## 改进解决方案

在将所有内容放在一起之前，还有一件事需要考虑，如果 JSON 的内容中的键是用 snake case 而不是 iOS camel case 约定编写的，会发生什么情况。让我们在默认情况下用 camel case 为 parse 函数添加一个小的`KeyDecodingStrategy`可选参数。

```
extension Decodable {
    static func parse(from item: Any?, strategy: JSONDecoder.KeyDecodingStrategy = .useDefaultKeys) -> Self? {
        // 1\. Sets the decoding strategy to the decoder
        let decoder = JSONDecoder()
        decoder.keyDecodingStrategy = strategy

        // ..
    }

    private static func data(from item: Any?) -> Data? {
        // ...
    }
}
```

## 整个扩展

## 一个简单的例子

1.  结构的定义和符合可解码的协议。
2.  可以有嵌套结构，只要它们也是可解码的。
3.  SwiftUI 是二手的，但也可以是我们很好的老式 UIKit。
4.  数据来自静态字典，但也可能来自文件、后端响应或任何其他 JSON 类型的内容。
5.  这是我们如何使用我们漂亮的扩展，它看起来干净易懂。可解码的数组也可以解析，只是免费的😉

*感谢阅读。我希望你喜欢这段代码，如果它对你有用，不要害羞👏关于这篇文章。下次见。*