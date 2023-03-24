# 评估 CSS 评估，第 4 部分

> 原文：<https://medium.com/nerd-for-tech/assessing-the-css-assessment-part-4-b1168fa3db30?source=collection_archive---------17----------------------->

这是我在 LinkedIn 的 CSS 评估的最后一篇文章。在这之后，我计划写关于 JavaScript 的评估。

**卧底人员**

CSS 允许你通过设置*显示*属性为*无*或者设置*可见性*属性为*隐藏*来“隐藏”网页上的元素。评估中的一个问题询问*显示:无*和*可见性:隐藏*之间有什么不同(如果有)。事实上，尽管这两种方法都隐藏了元素，但它们的工作方式并不完全相同。在给出的四个答案中，您需要选择一个说明在元素上使用 *display:none* 不仅会隐藏它，还会将其从文档流中移除，而 *visibility:hidden* 会使元素不可见，但会在页面上留下它所占用的空间。

如果你担心使用这些方法是否会影响你的 SEO 排名，请查看[diib.com](https://diib.com/learn/google-seo-hidden-content/)的这篇文章。

**哪个孩子？**

毫无疑问，你会不时地在你的 CSS 中使用[伪类](https://www.w3schools.com/css/css_pseudo_classes.asp)，所以 LinkedIn 评估中有关于它们的问题也就不足为奇了。回答这个问题你会有困难吗:“使用:n-child 伪类，从第 2 项开始，无论有多少项，对列表中每三个项进行样式化的最有效方法是什么？”对于*n-child*，可以使用关键字*偶数*和*奇数*，一个数字(只对一个元素进行样式化)，或者公式 *(an + b)* 。执行问题中提到的操作，您将需要[公式](https://www.w3schools.com/cssref/sel_nth-child.asp)。

根据 W3Schools.com 的说法，“a 代表一个循环的大小，n 是一个计数器(从 0 开始)，b 是一个偏移值。”在这种情况下，我们需要将 *a* 设置为 3，从而每三个项目设置一个样式。从第二项开始，我们需要将 *b* 设置为 2。因此，正确答案如下:

```
li:nth-child(3n + 2) { margin: 0 5px;}
```

**级联动作**

查看以下样式规则:

```
p:first-of-type { color: red }p { color: blue }.container { color: yellow }p:first-child { color: green }
```

现在，将这些规则应用于以下 HTML:

```
<div class=”container”>
  <h1>Heading</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</div>
```

第一段是绿色、红色、蓝色还是黄色？回答这个问题需要理解级联的原理。当然，CSS 代表“层叠样式表”(希望你已经知道了！)，其中 [*级联*](https://en.wikipedia.org/wiki/CSS#:~:text=The%20name%20cascading%20comes%20from,Wide%20Web%20Consortium%20(W3C).) 指的是“指定的优先级方案，以确定如果有多个规则匹配特定元素，则应用哪个样式规则。”浏览器将应用具有最大特异性的样式规则，或者如果两个样式规则具有相同的特异性，则将最后声明的一个应用于元素。

那么，哪个样式规则将被应用于上面例子中的第一个

元素呢？我们可以很容易地删除第三个，因为它适用于

，作为父元素，它比被包含的

元素具有更少的特异性。我们也可以排除第四个声明，因为两个

元素都不是第一个子元素。(在这种情况下，

# 是元素的第一个子元素。)

问题归结为常规标记选择器是否比包含伪类的标记选择器具有更高的特异性。我认为将伪类添加到选择器中会使它更具体，这是常识，而且……确实如此。第一个

元素中的文本将是红色的。

与 HTML 评估一样，CSS 评估每次都会有不同的问题，我只介绍了一些你可能会遇到的问题和主题。YouTube 上有很多人参加各种 LinkedIn 评估的视频，所以我建议观看这些视频，作为准备的一种方式。如果你决定在 LinkedIn 上申请 CSS 技能徽章，祝你好运！