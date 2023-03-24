# 如何在 Magento 2 中创建自定义模块

> 原文：<https://medium.com/nerd-for-tech/how-to-create-custom-module-in-magento-2-a193e6b76eb5?source=collection_archive---------5----------------------->

![](img/a49367b218ca4d5ea6faefac3e3042f5.png)

如何在 Magento 2 中创建自定义模块

在本文中，我将向您解释如何在 Magento 2 中创建自定义模块。Magento 2 商店中的模块基本上是结构元素。

遵循以下步骤:

1.  创建模块文件夹
2.  创造 registration.php
3.  创建 etc/module.xml 文件
4.  创建 etc/frontend/routes.xml
5.  创建控制器
6.  创建模板文件

## 你可能也会喜欢这篇文章:

1.  [Magento 2 中的主题概念](/nerd-for-tech/theme-concept-magento-2-b9a1fafc3590)
2.  [在 Magento 2](/nerd-for-tech/add-and-customize-custom-tab-on-product-page-magento-2-428fc14e92df) 产品页面上添加并定制自定义标签

# 步骤 1:创建模块文件夹

在 Magento 2 中，我们可以用两种方式存储新的模块文件夹。第一个是 **app/code/** 目录，第二个是 **vendor/** 目录。我将创建第一个，即**应用程序/代码/。**

现在创建您的文件夹:

**app/code/ <供应商名称> / <模块名称>**

其中，<vendor_name>是供应商的名称。在我的例子中，卖主的名字是 Aryan。</vendor_name>

<module_name>是模块的名称。在我的情况下，它是 HelloWorld。</module_name>

我的文件夹结构是:

**app/code/Aryan/hello world/**

```
app/code/
 ├── Aryan/
 │   │   ├──HelloWorld/
 │   │   │
```

# 步骤 2:创建 registration.php

我们需要 registration.php 文件在 Magento 系统中注册我们的新模块。模块注册成功后，可以在 **app/etc/config.php** 文件中看到。

这里我将***registration.php***文件添加到**app/code/Aryan/hello world/**下

```
<?php\Magento\Framework\Component\ComponentRegistrar::register(
    \Magento\Framework\Component\ComponentRegistrar::MODULE,
    'Aryan_HelloWorld',
    __DIR__
);
```

# 步骤 3:创建 etc/module.xml 文件

这里我会在**app/code/Aryan/hello world/etc/**下添加 ***module.xml*** 文件

```
<?xml version="1.0"?>
<config xmlns:xsi="[http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)" xsi:noNamespaceSchemaLocation="../../../../../lib/internal/Magento/Framework/Module/etc/module.xsd">
    <module name="Aryan_HelloWorld" setup_version="1.0.0" active="true">
    </module>
</config>
```

# 步骤 4:创建 etc/frontend/routes.xml

这里我将在***app/code/Aryan/hello world/etc/frontend/***下创建 ***routes.xml***

```
<?xml version="1.0"?>
<config xmlns:xsi="[http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)" xsi:noNamespaceSchemaLocation="urn:magento:framework:App/etc/routes.xsd">
 <router id="standard">
  <route id="helloworld" frontName="helloworld">
   <module name="Aryan_HelloWorld" />
  </route>
 </router>
</config>
```

# 步骤 5:创建一个控制器

在这里，我将创建控制器文件夹和控制器类。

1.  在**app/code/Aryan/hello world/**下创建 ***控制器*** 文件夹
2.  创建控制器名，在我的例子中，我把控制器名作为**索引。**在**app/code/Aryan/hello world/Controller/Index 下创建 ***Index*** 文件夹。**
3.  在**app/code/Aryan/hello world/Controller/Index**下创建控制器类***Index.php***

***内容*Index.php*内容***

```
*<?php
namespace Aryan\HelloWorld\Controller\Index;class Index extends \Magento\Framework\App\Action\Action
{
    protected $_pageFactory;
    public function __construct(
        \Magento\Framework\App\Action\Context $context,
        \Magento\Framework\View\Result\PageFactory $pageFactory)
    {
        $this->_pageFactory = $pageFactory;
        return parent::__construct($context);
    }public function execute()
    {
        return $this->_pageFactory->create();
    }
}*
```

# *步骤 5:创建模板文件*

*现在，是时候创建负责在前端呈现内容的**视图**文件夹了。该文件夹包含作为子文件夹的布局、模板和网站。所有布局句柄都在**布局**文件夹下，所有**模板**文件都在 templates 文件夹下，还有 CSS/LESS、images、js 等静态文件。转到 **web** 文件夹下。*

## ***1。为我们的模块创建一个布局句柄***

*我会在**app/code/Aryan/hello world/view/frontend/layout**下创建***hello world _ index _ index . XML****

*这里，*

*helloworld 是我们在***app/code/Aryan/hello world/etc/frontend/routes . XML***中声明的前置名*

*第一个**索引**是**app/code/Aryan/hello world/Controller/**下的控制器名称*

*第二个**索引**是**app/code/Aryan/hello world/Controller/Index 下的类名。***

***helloworld_index_index.xml 内容***

```
*<?xml version="1.0"?>
<page xmlns:xsi="[http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)" layout="1column" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
    <referenceContainer name="content">
        <block class="Magento\Framework\View\Element\Template" name="helloworld.display" template="Aryan_HelloWorld::sayhello.phtml" />
    </referenceContainer>
</page>*
```

## ***2** 。创建模板文件*

*我会在****app/code/Aryan/hello world/view/frontend/templates/***下创建***say hello . phtml*****

```
***<h2>
    <?= __('Hello World')  ?>
</h2>
<p>
    <?= __('Lorem ipsum dolor sit, amet consectetur adipisicing elit.')  ?>
</p>***
```

***现在，***

***需要执行命令:***

```
***$ php bin/magento setup:upgrade
$ php bin/magento setup:static-content:deploy -f 
$ php bin/magento cache:flush***
```

***现在，我们已经成功地在 Magento 中创建了一个新的模块。***

***希望这篇文章能帮助你创建一个自定义模块。如果你有任何疑问，你可以直接发邮件到[**aryansrivastavadesssigner@gmail.com**](mailto:aryansrivastavadesssigner@gmail.com)**问我。*****

***如果你喜欢这篇文章，你可以给我买杯咖啡[给我买杯咖啡](https://www.buymeacoffee.com/aryansrivastava)。***

## ***下载免费源代码:***

***[点击这里](https://desssigner.in/download/how-to-create-custom-module-in-magento-2/)***

## ***跟我来:***

***[领英](https://www.linkedin.com/in/er-aryan-srivastava-0b9576170/)推特***