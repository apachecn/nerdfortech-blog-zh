# 掌握降价

> 原文：<https://medium.com/nerd-for-tech/mastering-markdown-32d4cb2c4091?source=collection_archive---------2----------------------->

![](img/47746e7ef27c666f0e73534f58843bea.png)

Markdown 基本上是一种轻量级的标记语言，我们可以用它来为纯文本文档添加格式元素。它们存储为。md 或. markdown。

它通常用于格式化自述文件，在在线论坛中撰写消息，以及使用纯文本编辑器创建富文本。使用 Markdown 的热门网站包括 [GitHub](https://github.com/) 、 [Trello](https://trello.com/en) 、 [Reddit](https://www.reddit.com/) 等。

# 为什么降价？

*   简单的语法。
*   与其他文字处理器或所见即所得编辑器相比，它的速度很快。
*   易于携带，即传输文件不需要压缩或存档，文件大小也很小。
*   Markdown 很快转化为完美的 HTML 格式。
*   可以以多种格式获得输出。比如我们可以转换成 HTML 来发布到网上，转换成富文本来发送电子邮件，等等。
*   它提供了比传统出版系统更顺畅的工作流程。

# 减价风味

有许多 markdown 风格为我们提供了原始语法中没有的额外功能，比如表格、引用和语法高亮显示。

GitHub 风格的 Markdown (GFM)是最受欢迎的 Markdown 风格之一，包括额外的功能，如表格、编程语言选择和语法着色。

查看[完整列表](https://github.com/commonmark/commonmark-spec/wiki/markdown-flavors)。

# 降价编辑

有些编辑器具有内置的基本降价功能，而其他编辑器则是为了扩展降价语法而设计的，比如 Ghost。也有针对不同操作系统的降价编辑器。

这里有一些列表，

# 马科斯

*   [麦克唐](https://macdown.uranusjr.com/)
*   [iA 作家](https://ia.net/writer)
*   [尤利西斯](https://ulysses.app/)

# Windows 操作系统

*   [iA 作家](https://ia.net/writer)
*   [代笔人](https://wereturtle.github.io/ghostwriter/)
*   乔普林

# Linux 操作系统

*   [重新文本](https://github.com/retext-project/retext)
*   [代笔人](https://wereturtle.github.io/ghostwriter/)
*   [乔普林](https://joplinapp.org/)

# iOS/Android

*   [iA 作家](https://ia.net/writer)
*   [尤里西斯](https://ulysses.app/)
*   [乔普林](https://joplinapp.org/)

# 网

*   [Blot.im](https://blot.im/)
*   [小胜利](https://www.smallvictori.es/)
*   [幽灵](http://ghost-official.com/)

# 静态站点生成器

*   [杰基尔](https://jekyllrb.com/)
*   [雨果](https://gohugo.io/)
*   [Docusaurus](https://docusaurus.io/)

# 现在让我们开始吧！！！

检查代码以添加以下元素，

*   段落

```
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
```

为了在 markdown 文件中有两个单独的段落，我们必须提供一个行间距，否则它将被组合成一个单独的段落。

*   强调

```
*Italic*

**Bold**

***Italic and bold***

~~Strike through~~
```

*   标题

```
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6
```

*   链接

```
<https://github.com/Sapna2001>

[My Github Account](https://github.com/Sapna2001)

[My Github Account](https://github.com/Sapna2001 "Hover and see")

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor [incididunt][1] ut labore et dolore magna aliqua. Ut enim ad 
minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in 
voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat [cupidatat][here] non proident, sunt in culpa qui officia 
deserunt mollit anim id est laborum.

[1]: https://github.com/Sapna2001 
[here]: https://github.com/Sapna2001
```

*   形象

```
![Image](https://source.unsplash.com/random/100X100)

![Image][1]
![Image][img]

[1]: https://source.unsplash.com/random/100X100
[img]: https://source.unsplash.com/random/100X100

[A link](https://source.unsplash.com/random/100X100)

[![](https://source.unsplash.com/random/100X100)](https://source.unsplash.com/random/500X500")

<img src="https://source.unsplash.com/random/" width="100" height="100" alt="img">
```

*   目录

```
## Unordered List

* Item 1 using star
* Item 2 using star
* Item 3 using star

- Item 1 using dash
- Item 2 using dash
- Item 3 using dash

+ Item 1 using plus
+ Item 2 using plus
+ Item 3 using plus

## Ordered List

1\. Item 1
  * SubItem
    * Subitem
      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut
      enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in 
      reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, 
      sunt in culpa qui officia deserunt mollit anim id est laborum.
2\. Item 2
3\. Item 3
```

*   换行符、水平线和块引号

```
## Line Break
Use br tag <br>
to have a line break.

## Horizontal Rules

Have space between text and the dashes.

---

Otherwise it will appear as heading, like this
---

## Block Quotes

> Block quote 1
>
> Block quote 2

> Block quote 3
```

*   代码块和语法突出显示

```
Here is the code block:

    var x = 10;

Or

```
var x =10;
```

For inline block

Set x as 10, `var x=10;`

Highlighting code to be added and removed

```diff
var x = 10;
- var y = 20;
+ var y = 30;
```

*   桌子

```
## Text Center

| Index | Value |
| :------: | :------: |
|1|10|
|2|20|
|3|30|

## Text Left Align

| Index | Value |
| :------ | :------ |
|1|10|
|2|20|
|3|30|

## Text Right Align

| Index | Value |
| ------: | ------: |
|1|10|
|2|20|
|3|30|
```

*   复选框

```
## Checkbox

* [X] Item 1
* [ ] Item 2
* [X] Item 3
```

# 总结词

> 这就是我的观点，希望这篇文章对你有用😊。

> 如果你想和我联系，请点击这些链接
> 
> github-[https://github.com/Sapna2001](https://github.com/Sapna2001)
> 
> 领英-[https://www.linkedin.com/in/sapna2001/](https://www.linkedin.com/in/sapna2001/)
> 
> 网站-[https://sapna2001.github.io/Portfolio/](https://sapna2001.github.io/Portfolio/)
> 
> quora-[https://www.quora.com/profile/Sapna-191](https://www.quora.com/profile/Sapna-191)

**如果你有任何建议，请随时通过**[**LinkedIn**](https://www.linkedin.com/in/sapna2001/)**&与我联系，评论区也是你的。**

如果你喜欢这个故事，点击鼓掌按钮，因为它激励我写更多更好的东西。

***感谢阅读！！！！***