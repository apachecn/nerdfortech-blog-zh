# 评估评估(第 2 部分)

> 原文：<https://medium.com/nerd-for-tech/assessing-the-assessment-part-2-69085189e26c?source=collection_archive---------33----------------------->

在我的[上一篇文章](/nerd-for-tech/assessing-the-assessments-part-1-f69a6441d9de)中，我讨论了参加 LinkedIn 的 HTML 评估。我将继续这个主题，涵盖更多您可能会遇到的问题。

**HTML 的常客和稀罕物**

评估中的一些问题将涉及 HTML 标签。如果你被问及那些你很少使用或者从未听说过的标签，不要惊讶。以下是我在做评估时或观看 YouTube 视频时看到的一些标签。(这些标签要么在问题中被提及，要么出现在可能的答案列表中)。

*   <wbr>
*   <details></details>
*   <summary></summary>

*   `<datalist></datalist>`

*   <main></main>

*   `你能认出多少？你知道它们的确切用途吗？让我们来看看其中的几个。`

    `第一个，T22，对我来说是新的。它代表“断词”根据 Mozilla Developer Network 的说法，这个空的(或 void)元素“代表一个断字的机会——在文本中的一个位置，浏览器可以随意断行，尽管它的断行规则不会在该位置创建一个分隔符。”在关于这个标签的可能答案中，正确的答案是“如果需要正确的页面显示，它为一个很长的单词提供了一个中断的机会。”`

    `我也从未见过`

    <details>`、<summary>、<datalist>或<samp>。 [<细节>标签](https://www.w3schools.com/tags/tag_details.asp)，顾名思义，提供了一种显示附加信息的方式，例如，一个单词或名字。您希望提供详细信息的单词将包含在<摘要>标签中，附加信息将位于其下方，单词和信息都将包含在<详细信息>标签中。这里有一个例子:</samp></datalist></summary>`</details>

    ```
    <details>
      <summary>Day</summary>
      <p>A period of twenty-four hours, mostly misspent. This period is divided into two parts, the day proper and the night, or day improper — the former devoted to sins of business, the latter consecrated to the other sort. These two kinds of social activity overlap.</p>
    </details>
    ```

    `在网页上，你会在单词 *Day* 旁边看到一个小箭头。点击箭头会使箭头指向下方，并且< p >标签中的文本出现在下方。再次点击箭头将使该文本消失，箭头将指向第*天*。(顺便说一下，这个对*日*的尖刻定义来自阿姆罗斯·比尔斯的*魔鬼词典*。)总之，评估中的一个问题显示了一个单词，旁边有一个箭头，单词下面有一段描述，问题是问什么是最好的编码方式。`

    `用过<datalist>还是<samp>？ [<数据列表>标签](https://www.w3schools.com/tags/tag_datalist.asp)与<选择>标签相似，都提供了预定义选项的下拉列表供用户选择。事实上，两者都包含了<选项>元素。然而，<数据表>连接到一个<输入>元素，并具有自动完成功能。 [< samp >标签](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/samp)“代表计算机程序输出的样本(或引用的样本)”。</samp></datalist>`

    ```
    <p><samp>This does not compute.<br> Please recheck your code.</samp></p>
    ```

    `(注:W3Schools.com 建议使用 CSS 这样的文本代替<samp>。)</samp>`

    `如果你已经用 HTML 创建了表单或者在网页上放置了界面元素，你可能已经使用了`

    *   `"它以编程方式将文本标签与界面元素关联起来."`
    *   `"它在视觉上将文本标签与界面元素联系起来."`

    `然而，根据 Mozilla Developer Network 的说法，“标签文本不仅在视觉上与相应的文本输入相关联；它在程序上也与相关联”(强调是后加的)，你应该选择两个答案中的哪一个？所以，我在评估中至少看到了一个缺陷。`

    `有些问题可能需要你知道什么时候使用一个特定的标签，而不是另一个。一个简单的例子是询问何时使用`

    `和`
    `元素的问题。即使是不熟悉 HTML 的人也可能会认为第一个用于有序列表，第二个用于无序列表。换句话说，您将使用哪个标签取决于项目的顺序是否重要。`

    `另一个问题是用哪个标签来标记一小段代码，用哪个标签来标记一段较长的代码。有四双鞋可供选择:`

```
        ；
        ```

```；```
```；```
```；```

    ``这是 HTML 语义发挥作用的另一个例子。因为这个问题是关于标记计算机*代码*的，所以选择带有……<代码>标签的答案是有意义的。我在之前的帖子里提到过< kbd >是做什么用的，<标记>是用来高亮文字的。这给了我们第一个和第三个选择。你知道哪一种更适合“短代码片段”而不是“长代码块”吗？答案是“<码>；<前>”。为什么？ [<代码>标签](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/code)是内联元素；另一方面， [< pre >](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/pre) 用于“预先格式化的文本，该文本将完全按照在 HTML 文件中所写的那样呈现。”``

    ``在我的下一篇文章中，我将更多地讨论这个问题，以及关于 HTML 属性的评估问题。``