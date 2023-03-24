# 车把:部分，助手和更多

> 原文：<https://medium.com/nerd-for-tech/handlebars-partials-registers-much-more-fca3f500d9e5?source=collection_archive---------1----------------------->

![](img/8ac242201786308ac24c39e71fa87e1b.png)

来源:车把官网

Handlebars 是一种模板语言，用于呈现前端和后端的动态数据。HTML 的框架对于每个人来说都是固定的，作为 hbs 模板，在运行时动态数据将创建一个最终的 HTML 文件。只需输入{{}}，就可以多次使用同一个 HTML 模板文件。在运行期间，{{}}被实际数据替换。

Handlebars 是 Node.js 中使用的模板引擎之一，Node.js 生成的 HTML 文件称为“服务器端渲染(SSR)”。Handlebars 是对 [Mustache](https://mustache.github.io/) 模板语言的扩展，它主要关注简单性和最小化模板。

在今天的帖子中，我们将讨论以下内容。

1.  分音
2.  助手
3.  HBS 在线项目的实施

我们将使用两个文件 1。index.hbs & 2。index.js 文件来显示示例。我将向你展示一些最受欢迎的内置助手。

## 分音:

车把中的部件是一些可重用的模板，可以从任何其他模板中访问。必须通过把手注册 Partial。

## 助手:

手柄中的助手是在给定的输入上执行某个动作并返回输出的函数。一些内置的帮助器有#if、# until、#each、#with、log & lookup。

## #除非:

这是一个内置的块助手，用于检查手柄中的条件语句。如果值为 false，则呈现 If 块中的内容。它是#if helper 的反义词。

例如:

```
// Code in index.js
{
  fruits: {
    "Apple": true,
    "Cucumber": false,
    "Banana": true,
  },
}// Code in index.hbs
{{#unless fruits.Cucumber}}
  <li>Cucumber is not fruit</li>
{{/unless}}
```

## #如果:

这是一个内置的块助手，用于检查手柄中的条件语句。如果值为 true，则呈现 If 块中的内容。不像 javascript 代码，不会做≥或===这样的比较。如果要执行这样的操作，要么在渲染前预处理数据，要么创建一个辅助函数。

例如:

```
// Code in index.js
{
  fruits: {
    "Apple": true,
    "Cucumber": false,
    "Banana": true,
  },
}// Code in index.hbs
{{#if fruits.Apple}}
  <li>Apple is fruit</li>
{{/if}}
```

## #每个:

每个都是一个内置的块助手，在手柄中用来呈现数组输入。您可以通过这个从数组中出来来访问父对象../parentObject.property。如果数组包含对象，则可以通过调用对象的键来访问该值

例如:

```
// Code in index.js
{
  type: "Fruits List",
  fruits: [
    "Apple",
    "Mango",
    "Banana",
  ],
}{
  type: "Books List",
  books: [{
    autherName: "John Doe",
    publishedOn: "1998",
    bookName: "Magics of life"
   }, {
    autherName: "Smith Carl",
    publishedOn: "2004",
    bookName: "Black hole: Begining"
   }],
}// Code in index.hbs
<ul class="fruits">
  {{#each fruits}}
    <li>{{this}} is part of {{../type}}</li>
  {{/each}}
</ul><ul class="books">
  {{#each books}}
    <li>{{this.autherName}} published {{this.bookName}} on {{this.publishedOn}}</li>
  {{/each}}
</ul>
```

## 自定义助手:

为了在模板中使用它，您必须用手柄注册辅助对象。它有两个参数:助手名和函数参数

```
// Code in index.js
handlebars.registerHelper(‘calDiscountAmount’, (planValue, discountPer) => ((planValue * discountPer) / 100).toFixed(0))// Code in index.hbs
<p>
Discount of Rs.{{calDiscountAmount 1000 20}}
</p>// On execution it will show
Discount of Rs.200
```

让我们来看一个完整的车把演示。

```
<!DOCTYPE html>
<html>
<head>
 <title>{{title}}</title>
 {{> (whichPartial)}}
</head>
<body>
 {{>nav}}
 <section>
  {{#each products}}
  <div class="product">
   <img src="{{priductUrl}}" alt="{{productAltText}}" />
   <div class="description">
    <h3>{{productName}}</h3>
    <h5>${{productPrice}}</h5>
    {{#if discountPer}}
    <span>Discount of ${{calDiscountAmount productPrice      discountPer}}</span>
    {{#if}}
    {{#if isInInventory}}
    <h4>Qty: {{productQty}}</h4>
    {{else}}
    <h4>Out of stock</h4>
    {{/if}}
    </div>
   </div>
  {{/each}}
  </section>
 </body>
</html>
```

index.js 中的内容

```
const Handlebars = require('handlebars')
const nav = fs.readFileSync('./nav.hbs').toString()
const suppFilesProd = fs.readFileSync('./suppFilesProd.hbs').toString()
const suppFiles = fs.readFileSync('./suppFiles.hbs').toString()const products = [{
 priductUrl: 'https://guesseu.scene7.com/is/image/1234',
 productAltText: 'Sneaker',
 productName: 'Gloria high logo sneaker',
 productPrice: 100,
 isInInventory: true,
 productQty: 200,
 discountPer: 20,
 }, {
 priductUrl: 'https://guesseu.scene7.com/is/image/4567',
 productAltText: 'Shirt',
 productName: 'Check print shirt',
 productPrice: 100,
 isInInventory: true,
 productQty: 20,
 discountPer: null,
 }, {
 priductUrl: 'https://guesseu.scene7.com/is/image/8756',
 productAltText: 'Bag',
 productName: 'Cate rigid bag',
 productPrice: 100,
 isInInventory: false,
 productQty: 35,
 discountPer: null,
}]handlebars.registerHelper(‘calDiscountAmount’, (planValue, discountPer) => ((planValue * discountPer) / 100).toFixed(0))handlebars.registerHelper('whichPartial', () => {
  const returnPartial = process.env.NODE_ENV === 'production' ?   Handlebars.compile(suppFilesProd) : Handlebars.compile(suppFiles)
  return returnPartial
})handlebars.registerHelper('complexDiscount', (...value) => {
 const data = value.filter((i) => isNumber(i))
 return data.length > 0 ? data.reduce((a, b) => a -b) : 0
})handlebars.registerPartial('nav', Handlebars.compile(nav))router.get('/products', (req, res) => {
 res.render('index', {
  title: 'Buy cool new products',
  products
 })
})
```

在上面的代码中，nav partial 是所有网站页面通用的导航菜单代码文件。因此，我们将其注册为部分，并在所有 hbs 文件中使用。所以你不必在所有多余的页面上写同样的代码。

哪个部分用于根据节点环境动态决定将哪些数据注册为部分数据。当您需要分析或其他只在生产环境中需要的代码时，这很有用。

当您想要向函数发送 n 个参数时，使用复杂的折扣寄存器。让我们举个例子，假设你给第一个人 1 个折扣，给另一个人 3 个不同的折扣。“…value”需要任意数量的参数。

## 结论:

车把通过删除多余的代码、制作模板来简化您的生活。条件语法、循环、寄存器和部分使静态页面变成了动态页面。