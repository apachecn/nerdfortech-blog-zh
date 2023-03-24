# Cypress 页面对象模型

> 原文：<https://medium.com/nerd-for-tech/cypress-page-object-model-953791736972?source=collection_archive---------1----------------------->

![](img/8a9c2c757644bc58e90e7f1a400cdfa8.png)

页面对象模型，也称为 POM，是一种创建用于存储所有 web 元素的对象存储库的设计模式。它有助于减少代码重复和改进测试用例维护。在页面对象模型中，将应用程序的每个网页视为一个类文件。每个类文件将只包含相应的网页元素。使用这些元素，测试人员可以在被测网站上执行操作。

让我们在 cypress 文件夹中创建一个名为 pages 的文件夹。

![](img/5c20ef18e7ee759c194a07251e267e12.png)

让我们为登录和仪表板功能创建一个页面，因此我创建了 Login.js 和 Dashborad.js，并插入以下代码行。

![](img/09b38b33736874c96e646a92e3edd380.png)![](img/51aaad60d0fb8eb7562551e02d770771.png)

之后，我们将这两个类文件导入到我们的测试类中，如下图所示:(现在你可以看到代码的可用性)

![](img/a9fd614695d7d467b6df9f46a1fd963e.png)![](img/b3c8ddf7bc3dc62cf3f8f3b9b64ce0e8.png)

就这些了，现在是运行代码的时候了。

![](img/707934b393a1a488015a29c9e9e97482.png)

我们来做一件事，当前默认的执行顺序是 DashboardTest 和 LoginTest。所以我想改变行刑的顺序。所以我的首要任务是登录测试。因此，我们必须按照下图对 cypress.json 文件中的测试列表进行排序:

![](img/7dbb451129886a64918d7ee5bedbb914.png)

现在看看结果

![](img/de32d7c3e36cb56c91f6435b0c5c5497.png)

还有一点要补充的是，请使用名为 Cypress Support 的插件来获得所有与 API 和自动生成代码帮助相关的建议。

![](img/9b378005aa097449e10f171d5a9f25ea.png)