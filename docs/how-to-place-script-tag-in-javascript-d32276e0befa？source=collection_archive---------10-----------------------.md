# 如何在 JavaScript 中放置脚本标签？

> 原文：<https://medium.com/nerd-for-tech/how-to-place-script-tag-in-javascript-d32276e0befa?source=collection_archive---------10----------------------->

![](img/54a53a322e691195bc18c3a3c3fc218b.png)

# 介绍

我们在这里了解如何在 javascript 中放置脚本标签，但是首先，我们学习 JavaScript。JavaScript 代码不关心它存在于哪里。它可能与制作网页的 HTML 混合在一起，也就是嵌入其中。或者，类似于外部 CSS 文件，它可能被放置在由 HTML 文件加载的单独文件中。在最好的情况下，最好有一个外部 JavaScript 文件；出于同样的原因，最好有一个外部 CSS 文件。另一方面，我们仍然想知道如何在 HTML 文件中放置 JavaScript。

# 描述

*   在 JavaScript 中，脚本标签可以放在三个地方:

在两个 BODY 标签的中间，
在两个 HEAD 标签之间，
为外部文件的链接，也在 HEAD 部分。

我们知道，HTML 代码的顺序非常重要，因为它控制着页面上元素的顺序。在 HTML 中，如果我们希望标题出现在文本之上，我们就把标题放在文本之上。但是 JavaScript 没有这样做。[例如，当页面加载时，我们所有的 JavaScript 函数都被加载到内存中。](https://www.technologiesinindustry4.com/)在代码可能存在的地方，浏览器会找到它，并在页面加载结束时存储它。加载页面时，只要页面显示，所有的 JavaScript 代码都会保留在内存中，准备执行。

经过授权，我们可以将 JavaScript 代码放在 HTML 文件中的任何地方——头部分、正文部分的开头、正文部分的中间或正文部分的结尾。如果我们愿意，我们可以在 HTML 文件的不同位置散布不同的 JavaScript 函数。浏览器会将其全部分类。

通常，脚本最重要的位置是在主体部分的末尾。这些保证了 CSS 整形和图像显示在脚本加载时不会延迟。当我们在 HTML 中嵌入 JavaScript 代码块时，我们必须将 JavaScript 代码放在和标记之间。下面是被标签包围的两个函数。

1.  李>
2.  函数 sayHi() {
3.  alert ("Hello world！");
4.  李>
5.  函数 sayBye() {
6.  alert ("Buh-bye！");
7.  李>
8.  李>

我们可以在和标签中间包含任意数量的代码——包含任意数量的[函数](https://www.homeandlearn.co.uk/javascript/javascript_tag_placement.html#:~:text=In%20Javascript%2C%20SCRIPT%20tags%20can,also%20in%20the%20HEAD%20section.&text=The%20reason%20to%20do%20it,before%20the%20script%20is%20read.)。我之前提到过，如果我们愿意，我们可以在 HTML 文件中散布不同的代码段。但是每个部分都必须包含在和标记之间。

出于大多数目的，编码人员喜欢将他们的全部或大部分 JavaScript 代码放在一个单独的 JavaScript 文件中，然后让浏览器在加载 HTML 文件的同时加载这个外部文件。

类似于 HTML 和 CSS 文件，JavaScript 文件是一个简单的文本文件。它没有标题或其他特殊部分。它甚至没有和标签。这是公正的纯 JavaScript 代码。在上面的实例中，包含这两个函数的 JavaScript 文件的全部内容是:

1.  函数 sayHi() {
2.  alert ("Hello world！");
3.  李>
4.  函数 sayBye() {
5.  alert ("Buh-bye！");
6.  李>

JavaScript 文件的文件扩展名是:. js。

文件名的前端由我们决定，前提是合法。[其中一些会很好。](https://www.technologiesinindustry4.com/)

scripts . js
corejs . js
main-code . js
main _ code . js
main . code . js

我们对 HTML 文件中的 JavaScript 文件进行解释，就像我们包含外部 CSS 文件一样——带有开始和结束标记。

将包含 JavaScript 文件的标记放在 body 部分的末尾是一个好主意，原因类似于在 body 部分的末尾编写 JavaScript 代码是一个好计划。我们可能会包含您喜欢的任意数量的外部 JavaScript 文件。我们还可以包含 JavaScript 文件，并在 HTML 中嵌入其他 JavaScript 代码。

更多详情请访问:[https://www . technologiesinindustry 4 . com/how-to-place-script-tag-in-JavaScript/](https://www.technologiesinindustry4.com/how-to-place-script-tag-in-javascript/)