# CSS 的类型

> 原文：<https://medium.com/nerd-for-tech/types-of-css-277d8d262781?source=collection_archive---------7----------------------->

[https://youtu.be/0C4iPnaTEGo](https://youtu.be/0C4iPnaTEGo)

**什么是 css(层叠样式表)？**

CSS-是一种样式表语言，用于描述以标记语言(如 HTML)编写的文档的表示

它是一种简单的设计语言，处理网页的外观和感觉部分。

**CSS 类型**

1.  内部/嵌入式 css
2.  内嵌 css
3.  外部 css

**内部/嵌入式 css**

这可以在单个 HTML 文档中使用。CSS 规则集应该在 head 部分的 html 文件中

**代码示例**

*internal.html*

```
<!DOCTYPE html><html><head><title>Internal CSS</title><!--internal css--><style>body{background-color: rgb(118, 141, 141);
}
p{text-align: center;font-size: 50px;color: blue;}.CPG{color: blue;font-size: 50px;text-align: center;}.main{text-align: center;}</style></head><body><p>learn about technology</p><div class = "main"><div class ="CPG">Compgat</div></div></body></html>
```

**内联 css**

内联 css 在附加了元素的正文部分包含 css 属性。这种样式是由一个使用 style 属性的 HTML 标签指定的。

**代码示例**

*inline.html*

```
<!DOCTYPE html><html><head><title>Inline CSS</title></head><body style="background-color: aquamarine;" ><h1 style="text-align: center;font-size: larger;color: blueviolet;">learn about technology</h1><p style="color: rgb(8, 8, 199);text-align: center;font-size: 50px;">Compgat</p></body></html>
```

**外部 css**

外部 css 包含两个不同的文件。css 和. HTML。css 只包含样式属性，比如 id，标题，类等等

**代码示例**

*external.html*

```
<!DOCTYPE html><html><head><link rel="stylesheet" href="external.css"/></head><body>
<p>learn about technology</p><div class = "main"><div class ="CPG">Compgat</div></div></body></html>
```

*external.css*

```
body{
background-color:aqua;
}p{
text-align:center;
color:blue;
font-size:70px;
}.CPG[
color:blue;
font-size:70px;
}.main{
text-align:center;
}
```

快乐学习😎