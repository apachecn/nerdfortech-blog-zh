# 木偶师最佳实践

> 原文：<https://medium.com/nerd-for-tech/puppeteer-best-practices-3a1a72c912b0?source=collection_archive---------0----------------------->

这些帮助我减少自动化剥落

![](img/e394de7515600002ad29ab5e59592ab7.png)

阿图尔·潘迪在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 介绍

Puppeteer 非常容易使用，因为它提供了许多 API 来与 dom 交互，并执行复杂的自动化和测试。但是对于初学者来说，有一些开发人员可能没有意识到的复杂性。

所以我在这里分享一些我从经验中学到的最佳实践(在我看来)。让我们开始吧！

# 使用文本/内容而不是 CSS 选择器进行搜索

第三方网站使用的 CSS 类或 id 经常变化。大多数知名网站使用随机加密/散列字符串，这可能会妨碍 CSS 选择器的使用。

当开发人员重构一个网站时，CSS 也会发生变化。所以，最好使用 HTML 元素的文本内容作为事实的来源。

例如，您可能需要单击登录按钮。因此，不要使用 CSS 选择器来选择按钮，而是使用按钮文本“Login”。

以下是实现上述目标的一些方法:

```
// using XPath api, the easiest wayconst loginBtn = await page.$x('//button[contains(., "Login")]')await loginBtn[0].click() // using query selector apiconst buttons = await page.$$('button')for(const button of buttons) { const btnText = await (await button.getProperty('textContent')).jsonValue() if(btnText.includes("Login")) { await button.click() }} // using evaluateawait page.evaluate(() => { const buttons = document.querySelectorAll('button')

  buttons.forEach(button => { if(button.innerText.includes("Login")) { button.click() } })})
```

还有其他像`$$eval`和`$eval`这样的方法可以用来代替上面的方法，但是你领会了它的要旨吧？

# 等待元素加载

使用`page.waitForTimeout`方法等待元素加载可能很诱人，但是下面有更好的方法。这不是一个理想的解决方案，因为您不知道加载元素需要多长时间。

在对一个元素执行任何操作之前，总是使用`page.waitForSelector`，尤其是在一个新页面被加载之后。这一点很重要，因为您想要访问的元素可能需要一些时间来加载，或者在经过一些处理后使用 JavaScript 插入到 DOM 中。

你不能猜测什么时候一个元素会在 dom 上可用，如果你试图执行一个操作，比如说`click`，它可能会抛出一个错误，因为这个元素还没有被加载到 DOM 中。

因此，使用如下等待方法几乎总是必不可少的:

```
// for CSS selectorsawait page.waitForSelector('button[id=loginBtn]')await page.click('button[id=loginBtn]')// for XPathawait page.waitForXPath('//button[@id="loginBtn"]')await page.click('button[id=loginBtn]')
```

有时，包含一个自定义超时选项和`visible`属性也很方便。

`timeout`意味着如果在`n`分钟后没有找到元素，抛出一个错误。`visible`意味着该元素在 DOM 中被发现并且可见。否则，它可能隐藏在另一个元素后面，或者可能具有使其不可见的 CSS 属性。

```
// for CSS selectors// throw an error if the element is still not found after 60000 ms
await page.waitForSelector('button[id=loginBtn]', { visible: true, timeout: 60000 }) // timeout is in millisecondsawait page.click('button[id=loginBtn]')// for XPathawait page.waitForXPath('//button[@id="loginBtn"], { visible: true, timeout: 60000 }')await page.click('button[id=loginBtn]')
```

# 等待导航

当浏览器从一个页面导航到另一个页面或重新加载时，总是需要等待导航完成。否则，如果您试图在要导航的页面上执行操作，而它还没有完成加载，它将抛出一个错误。

可能触发导航的事件一般有`page.goto`、`page.click`、`page.reload`、`page.goBack`和`page.goForward`。

等待导航(直到 dom 完全加载)

```
await page.click('button[id=submitForm]') // triggers navigationawait page.waitForNavigation() // wait till the next page has loaded// your operations on the next page...
```

如果您只想等待 DOM 加载，以上内容可能就足够了。如果您还想在后台等待 fetch/XHR 调用完成，那么`page.waitForNavigation`方法允许您传递该选项。

```
// wait till there have been atmost 0 requests in 500ms timeframe// for surity, use this instead of 'networkidle2'await page.waitForNavigation({ waitFor: 'networkidle0' })// wait till there have been atmost 2 requests in 500ms timeframe
await page.waitForNavigation({ waitFor: 'networkidle2' })
```

如果需要，您还可以延长默认的 30 秒超时时间以等待更长时间

```
await page.waitForNavigation({ waitFor: 'networkidle0', timeout: 60000 })
```

另一个好的做法是在等待导航时使用`Promise.all`。将第一个承诺作为`page.waitForNavigation`传递，将第二个承诺作为可能触发导航的动作传递。

```
await Promise.all([ page.waitForNavigation(), page.click("#loginBtn") // triggers navigation])
```

这是必要的，因为下面的代码可能不会像预期的那样工作，因为`page.waitForNavigation`可能会继续等待并最终超时。

```
await page.click("#loginBtn") // triggers navigationawait page.waitForNavigation() // use the above appraoch instead!
```

# 等待网络呼叫

您可能会遇到等待 API 调用来填充页面内容的常见用例。在这些类型的用例中，`page.waitForNetworkIdle`可能会派上用场。

```
await page.click("#fetchUsers") // makes an API call (no navigation)await page.waitForNetworkIdle() // wait till the page has no more fetch/XHR calls pending
```

有两个可选属性可以传递给这个方法，`idleTime`和`timeout`。

`idleTime`:指定没有 API 调用之前的最小毫秒数

`timeout`:指定等待到没有 API 调用的时间限制(毫秒)

同样，对于`page.waitForNetworkIdle`，我们应该使用`Promise.all`，原因如上所述。

```
await Promise.all([ page.waitForNetworkIdle(), page.click("#loginBtn")])
```

# 尽可能使用评估方法

我们在 Puppeteer 中编写的代码被转换成浏览器可以理解的普通 JavaScript，并在浏览器上下文中执行。如果您正在编写许多本来可以用`evaluate`方法编写的木偶方法，这可能是低效的。

`evaluate`方法提供了更好的性能，因为
1)代码是用普通的 JavaScript 编写的，从木偶师那里得到的处理更少，并且更容易为大多数开发人员编写
2)大多数单独的木偶师调用可以合并到一个`evaluate`脚本中，这意味着浏览器和木偶师之间的调用更少

我这里说的是`evaluate`，但是所有这些点也适用于其他的帮助器方法，比如`evaluateHandle`、`$eval`和`$$eval`。

例如，假设您想阅读每`table`行`tr`的第一个`td`中的文本

```
// with usual Puppeteer methodsconst tdList = []const trs = await page.$('table > tr')for(const tr of trs) { const td = await tr.$('td') const tdValue = await (await td.getProperty('textContent')).jsonValue() tdList.push(tdValue)}
```

正如你在上面看到的，木偶师将在`table`中为每个`tr`调用浏览器，这意味着许多来回。相反，我们可以使用单个`evaluate`脚本和相对较短的代码来实现这一点。

```
// with evaluate methodconst tdList = await page.evaluate(() => { const tempList = [] document.querySelectorAll('table > tr').forEach(tr => { tempList.push(tr.querySelector('td').textContent) }) return tempList})
```

这只需要对浏览器进行一次调用！

# 何时不使用评估

`evaluate`一旦你掌握了窍门，它似乎是任何事情的最佳解决方案。但是，有些时候你应该避免使用`evaulate`，而是最好使用木偶方法。

这通常包括用户交互，如鼠标点击、键盘事件、打字、悬停、聚焦等。这些可以使用`evaulate`实现，但可能不会引发副作用。

所以，作为一个经验法则，当你需要做浏览器交互的时候，总是使用像`click`、`type`、`select`、`hover`这样的方法。

# 不要忘记关闭您的浏览器实例

就这样，这就是建议。记得做`await browser.close().`

那都是乡亲们！我希望这将有助于你做令人敬畏的事情与木偶戏。

下一场见！

# 有用的链接:

[https://pptr.dev/](https://pptr.dev/)

https://stack overflow . com/questions/55664420/page-evaluate-vs-puppet er-methods