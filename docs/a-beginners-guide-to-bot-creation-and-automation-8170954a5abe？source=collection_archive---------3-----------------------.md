# 机器人创建和自动化初学者指南

> 原文：<https://medium.com/nerd-for-tech/a-beginners-guide-to-bot-creation-and-automation-8170954a5abe?source=collection_archive---------3----------------------->

每项工作都有可以自动化的重复性任务和流程，这会占用你大量的时间。在数字世界中，自动化是最重要的技能。在本文中，我将向您介绍为各种应用程序创建机器人的基本技术。我们将一起创建简单的机器人，在谷歌中搜索，并自动喜欢 Instagram 上的随机帖子。所以，没有任何进一步的告别，让我们开始吧！

![](img/7892701c2c25a7e62b649bc429dda9e4.png)

# 谷歌搜索机器人

首先，我们将建立一个简单的搜索机器人，自动打开网页浏览器，加载谷歌页面，并键入一个词进行搜索。一旦下一个页面加载，它会截图并点击图片，然后重定向到搜索的图片。它获取新页面的另一个截图，并点击它找到的第一张图片。它获取最终的屏幕截图并关闭浏览器。有趣不是吗？

创建一个名为*的文件夹。在 VSCode 中打开那个文件夹，创建一个名为 *simple.js* 的文件。现在，用`node init -y`安装一个新的 Nodejs 项目，然后用`npm i playwright`安装**剧作家**库。对于那些想知道这个剧作家库是什么和它做什么的人来说，剧作家基本上是一个 Node.js 库，用一个 API 来自动化 [Chromium](https://www.chromium.org/Home) 、 [Firefox](https://www.mozilla.org/en-US/firefox/new/) 和 [WebKit](https://webkit.org/) 。它旨在实现跨浏览器的 web 自动化，这是一种环保、高效、可靠且快速的技术。你可以在 https://github.com/microsoft/playwright 找到更多关于剧作家的信息。一些替代品是木偶师或硒。*

为了让 async/await 特性正常运行，您需要为 Nodejs 安装 8 的最低版本**。在开始自动化和机器人创建之前，你应该对谷歌 Chrome 提供的 Chrome DevTools 有一些基本的了解。这将使任务变得容易得多。当你加载一个网页，右击并选择检查选项，开发工具窗口出现。您想要自动化的网页部分总是有一个**唯一元素**。你需要寻找那个独特的元素。这可以通过打开 DevTools 并选择您想要自动化的页面部分来完成，然后在 Inspect 窗口的 Elements 选项卡下突出显示相应的 HTML 代码。您需要找到这部分代码中的独特元素。**

simple.js 的代码如下:

```
// npm install playwright// may take a while for downloading binaries// minimum node version 8 for async / await featureconst playwright = require(‘playwright’);const browserType = ‘chromium’; // chrome//const browserType = ‘firefox’; // firefox//const browserType = ‘webkit’; // safariasync function main() {// disable headless to see the browser's actionconst browser = await playwright[browserType].launch({ headless: false });const context = await browser.newContext();const page = await context.newPage(); //wait for the browser to create a new page by putting awaitawait page.goto(‘http://google.com'); //opening Google pageawait page.waitForLoadState(‘load’);const searchTerm = ‘Automation Bots’;//the term to be searched in Google searchbarconst input = await page.$(‘[name=”q”]’);//unique element in the search bar section of inspect sourceawait input.type(searchTerm);await page.waitForTimeout(2000);//wait 2000 ms before pressing enter so that the entire term is typedawait input.press(‘Enter’);await page.waitForLoadState(‘load’);await page.screenshot({ path: ‘result1.png’ });//taking screenshot of the page loadedawait page.waitForTimeout(3000);//wait 3s after taking screenshot const imageClicks = await page.$(‘[data-hveid=”CAEQBA”]’); //clicking the images linkawait imageClicks.click();await page.waitForLoadState(‘load’);await page.waitForTimeout(3000);await page.screenshot({ path: ‘result2.png’ });//taking screenshot of the new page loadedawait page.waitForTimeout(3000);const imageOne = await page.$(‘[alt=”Types of Bots in Automation Anywhere | Robotic Process Automation | justSajid”]’); //clicking the first imageawait imageOne.click();await page.waitForLoadState(‘load’);await page.waitForTimeout(3000);await page.screenshot({ path: ‘result3.png’ });await page.waitForTimeout(3000);await browser.close(); //close the browser once everything is done}main();
```

让我们通过键入`node simple.js`来运行代码。一旦代码开始运行，搜索机器人就会被激活，并开始执行代码中提供的所有任务。它拍摄的**截图**会自动存储在你的 GoogleSearchBot 文件夹中(关注下面视频中 VSCode 的左侧，在 Bot 启动之前和 bot 完成任务之后)。让我们来看看它的功能:

# 类似 Bot 的 Instagram

现在，我们已经收集了创建机器人的基本知识，是时候向前推进一点了。因此，我们将设计一个像 Bot 一样的 Instagram，它可以通过你的电子邮件和密码自动登录到你的 Instagram 帐户，并随机喜欢你 feed 上出现的帖子和评论。

到现在为止，你们一定都知道任何 Nodejs 项目的入门包:创建一个名为 InstaLikeBot 的文件夹→打开 VSCode →打开 InstaLikeBot 文件夹→创建一个名为 instagram.js 的文件→打开终端窗口并键入`npm init -y` →通过键入`npm install dotenv playwright`安装剧作家库和 dotenv 包→开始使用代码。

这里，我们将利用**环境变量**。因此，安装 *dotenv* 包，通过键入`npm install dotenv`为您的项目创建一个虚拟环境。现在，创建一个名为*的文件。env* 存储机密信息，如用户名、电子邮件、密码等。instagram.js 的代码如下所示:

```
// create .env file// EMAIL=// PASSWORD=// npm install dotenv playwrightrequire(‘dotenv’).config() //including the dotenv package files to handle all the bootstrap for usconst playwright = require(‘playwright’);const browserType = ‘chromium’; // chrome//passing in the login credentials by Environment Variablesconst email = process.env.EMAIL;const password = process.env.PASSWORD;async function main() {const browser = await playwright[browserType].launch({ headless: false });const context = await browser.newContext();const page = await context.newPage();await page.goto(‘http://instagram.com');// Opening Instagram pageawait page.waitForLoadState(‘load’);// wait until the page is loaded because Instagram is a single page applicationawait page.waitForTimeout(3000);const inputEmail = await page.$(‘[name=”username”]’); //unique element of the input fieldawait inputEmail.type(email, 2000);const inputPassword = await page.$(‘[name=”password”]’);await inputPassword.type(password, 2000);await inputPassword.press(‘Enter’);//in order to submit our login credentialsawait page.waitForTimeout(5000);// closes the modal by clicking on Not Nowconst notNow = await page.$(‘“Not Now”’);await notNow.click();let current = 0;while (true) {// only get unliked imageslet elements = await page.$$(‘[aria-label=”Like”]’);// stop if there are no new itemsif (elements.length — 1 == current) break;for (; current < elements.length; current++) {const element = elements[current];// scrolls into viewawait element.hover();await page.waitForTimeout(2000);// randomly like every third imageif (Math.random() > .3) {await element.click();}}}console.log(‘Finished the script.’);await browser.close();}main();
```

我们完事了。现在让我们通过键入`node instagram.js`来运行这个项目。机器人被激活并开始工作，如下所示:

所以，这都是我这边的。我希望你会喜欢创建机器人，并有效地利用你的时间。机器人的功能非常简单，易于理解。你可以随时给机器人添加额外的有趣的功能。此外，你现在可以尝试创建 Spotify Listener Bot、Telegram Bot 或 Twitter Follower Bot。感谢阅读。祝你今天开心！玩的开心！