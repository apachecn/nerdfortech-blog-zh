# Angular 11:第 2 部分在自己的网站中插入外部 HTML 网络抓取

> 原文：<https://medium.com/nerd-for-tech/angular-11-part-2-insert-external-html-in-your-own-website-web-scraping-acbf73d51fda?source=collection_archive---------15----------------------->

在第一部分中，我向你展示了我是如何在我的网站上将一个网页从一个外部源移动到一个本地模态的(你没有阅读它😳头那边马上 [Part 1](https://johanfaerch.medium.com/angular-11-insert-external-html-in-your-own-website-web-scraping-ff78f2540c4b) )。

![](img/9db1181d3cd561836c63ed45e1063c60.png)

伊恩·施耐德在 [Unsplash](https://unsplash.com/s/photos/web-modal?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

你看了吗？好吧，我们可以继续了🤓

这一次，我将扩展原来的解决方案，进一步操纵 HTML 和错误处理。

在我收集了外部 web 页面的 HTML 字符串之后，我发现与 web 站点的其他部分相比，有些样式已经过时了。

```
...
const originText = await response.text();
```

包含我们可以操作的字符串形式的 HTML。我们当然可以根据外部 HTML 的元素和现有类添加一些样式，但在这种情况下，我更喜欢改变 HTML 字符串。

首先，段落是用 p 标签包装的，我想把它换成 span 标签，因为这是我在我的网页上其他地方使用的。为此，我使用带有 regex 的字符串“replace()”方法在同一个方法中执行多次替换，即 p -> span。这并没有削减它，因为这将取代文本中的任何“p”。相反我们这样替换:

```
const text1 = originText.replace(/<p|<\/p>/gi, (x) => {
  if (x === '<p') {
    return x = '<span';
  } else if (x === '</p>') {
     return x = '</span>';
  }
});
```

这应该可以解决段落问题。此外，没有 header 标记，所以我将整个“header”替换为具有相同内容的 h1 标记:

```
const text = text1.replace('<span>Nutzungsbedingungen</span>', '<h1>Nutzungsbedingungen</h1>');
```

页脚变得很奇怪，因为它有来自不再需要的外部站点的支持信息。页面的最后一部分看起来像这样:

```
...
<span>Yadayada</span><hr><span class="text-center"><strong>Contact Yada Support</strong>
<br>Need help?</span>
<span class="text-center"><a class="btn btn-primary" href="[yada@yada.com](mailto:unzerdirect@unzer.com)" role="button"><i class="fa fa-envelope-o"></i> [yada@yada.com](mailto:unzerdirect@unzer.com)</a></span>
```

所以我认为这将是更容易做的 HTML 操作，所以 p 的替换为 span 的，我回到了 HTML

```
const terms = parse(text);
const htmlTerms1 = terms?.querySelector('#sidebar')?.nextElementSibling;
```

并删除了 hr 标签后的两个兄弟

```
htmlTerms1?.querySelector('hr')?.nextElementSibling?.remove();
htmlTerms1?.querySelector('hr')?.nextElementSibling?.remove();
this.htmlTerms = htmlTerms1?.innerHTML;
```

对于这个结果:

```
...
<span>Yadayada</span><hr>
```

所以现在我有了我想要的标题和段落格式，并且我已经移除了页脚。下面是属性“htmlTerms”中的抓取、格式化和最终 HTML 代码:

```
(async () => {
  const response = await fetch('[https://your/external/terms/'](https://www.unzerdirect.com/terms/'));
  const originText = await response.text();
  const text1 = originText.replace(/<p|<\/p>/gi, (x) => {
    if (x === '<p') {
      return x = '<span';
    } else if (x === '</p>') {
      return x = '</span>';
    }
  });
  const text = text1.replace('<span>Nutzungsbedingungen</span>', '<h1>Nutzungsbedingungen</h1>');
  const terms = parse(text);
  const htmlTerms1 = terms?.querySelector('#sidebar')?.nextElementSibling;
  htmlTerms1?.querySelector('hr')?.nextElementSibling?.remove();
  htmlTerms1?.querySelector('hr')?.nextElementSibling?.remove();
  this.htmlTerms = htmlTerms1?.innerHTML;
})();
```

注意可选的链接？这是避免在我向用户交付模型的过程中出现问题时破坏一切的类型脚本部分。但是如果能处理这样的错误就更好了，所以我想我会采用模态…

```
openTerms() {
  if (this.htmlTerms) { // Only needed for the error handling
    this.dialogService.confirm(
      '',
      this.htmlTerms,
      this.translate.instant('Accept'),
      this.translate.instant('Cancel')
    )
    .afterClosed()
    .subscribe((confirmed: boolean) => {
      if (confirmed) {
        this.accept = true;
      }
    });
  } // end of if
}
```

…并添加一个 else 子句，至少给用户一些半有用的信息…

```
...
  } else {
    this.dialogService.alert(
      `Sorry, we could not get the Terms at this point`,
      `Please go to [https://your/external/terms/](https://www.unzerdirect.com/terms/) (copy the URL and paste it in a new tab)`,
      'warning_amber'
    );
  }
}
```

这只是一个带有 URL 的警告，以防出现未定义的术语。

我希望这能给你自己的项目提供一些思路。

快乐编码🤗