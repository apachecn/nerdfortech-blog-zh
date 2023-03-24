# Magento 2 中的主题概念

> 原文：<https://medium.com/nerd-for-tech/theme-concept-magento-2-b9a1fafc3590?source=collection_archive---------28----------------------->

![](img/3b08cd0069e1f168a9adaf14dd4ac8cf.png)

电子商务在我们的生活中扮演着非常重要的角色，尤其是在新冠肺炎疫情。众所周知，外出购买我们的必需品是不安全的。在这种情况下，许多供应商提供网上购买我们的商誉。

Magento 是最好的电子商务开源平台之一。这给了用户一个很好的机会来控制店面的外观和功能。这提供了许多工具和功能，如营销、搜索引擎优化(SEO)、目录管理等。

Magento 2 是 Magento 更好的增强版。

让我们继续定制我们的店面:

## Magento 2 中的主题是什么？

Magento 2 中的主题负责前端和后端的外观和感觉。他们使用 PHP、XML、HTML、CSS 和 JavaScript 的组合来实现想要的外观。主题覆盖或扩展了现有的 PHP、HTML、CSS 和 JavaScript，同时提供了额外的功能。

Magento 2 为我们提供了预定义的主题，即“亮度”和“空白”。Magento 为我们的商店提供了创建新主题的选项，或者我们也可以从 Luma 和 Blank 中继承一个主题。

## 如何在 Magento 2 中创建主题？

我们将学习如何基于“Magento Luma”创建新主题。

为了创建基于“Magento Blank”或“Magento Luma”的新主题，我们必须为主题创建文件夹。让我们讨论一下文件夹结构:

```
app/design/frontend/
 ├── <Vendor>/
 │   │   ├──...<theme>/
 │   │   │   ├── ...
 │   │   │   ├── ...
```

这里，

<vendor>是提供特定主题的供应商或公司的名称。</vendor>

<theme>是主题的名称。</theme>

例如:

<vendor>= >教程</vendor>

<theme>= >简单</theme>

现在，文件夹将是:

```
app/design/frontend/
 ├── Tutorial/
 │   │   ├──...Simple/
 │   │   │   ├── ...
 │   │   │   ├── ...
```

在上述文件夹中，我们将创建所需的文件:

1.  theme.xml
2.  registration.php

这些是创建主题的必需文件，还有一个可选文件，即 composer.json 文件。

## 在 theme.xml 文件中声明主题:

```
<theme xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Config/etc/theme.xsd">
      <title>Tutorial Simple</title>
      <parent>Magento/luma</parent> 
</theme>
```

这里，

<title>是主题的名称</title>

<parent>是父主题。</parent>

我们还可以在`theme.xml`文件中添加主题预览图像。

```
<theme xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Config/etc/theme.xsd">
   <title>Tutorial Simple</title>
   <parent>Magento/luma</parent>
   <media>
      <preview_image>media/preview.jpg</preview_image> 
   </media>
</theme>
```

## 在 Magento 系统中注册主题:

要在系统中注册我们的主题，我们必须在主题目录中添加一个`registration.php`文件，代码如下:

```
<?php
/**
 * Copyright © Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */

use \Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(ComponentRegistrar::THEME, 'frontend/Tutorial/Simple, __DIR__);
```

## 创建 composer.json(可选) :

Composer 是 PHP 中的依赖管理工具。它允许你声明你的项目所依赖的库，它将为你管理(安装/更新)它们。

```
{
    "name": "Tutorial/Simple",
    "description": "N/A",
    "config": {
        "sort-packages": true
    },
    "require": {
        "php": "~7.2.0||~7.3.0",
        "magento/framework": "*",
        "magento/theme-frontend-blank": "*"
    },
    "type": "magento2-theme",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "autoload": {
        "files": [
            "registration.php"
        ]
    }
}
```

## 配置店面的图像:

店面上使用的产品图像大小和其他属性在一个`view.xml`配置文件中进行配置。它对于主题是必需的，但如果存在于父主题中，则是可选的。

在`view.xml`文件中配置所有店面产品图像尺寸。例如，通过指定 250 x 250 像素的大小，可以使类别网格视图产品图像成为正方形:

```
<image id="category_page_grid" type="small_image">
    <width>250</width>
    <height>250</height>
</image>
```

更多详情，[点击此处](https://devdocs.magento.com/guides/v2.4/frontend-dev-guide/themes/theme-images.html)

## 为静态文件创建目录:

主题可能包含几种类型的静态文件:

*   风格
*   字体
*   Java Script 语言
*   形象

```
app/design/frontend/Tutorial/Simple/
├── web/
│ ├── css/
│ │ ├── source/ 
│ ├── fonts/
│ ├── images/
│ ├── js/
```

## 主题的结构如下:

```
app/design/frontend/Tutorial/
├── Simple/
│   ├── etc/
│   │   ├── view.xml
│   ├── web/
│   │   ├── css/
│   │     ├── source/ 
│   |   ├── fonts/
│   |   ├── images/
│   |   ├── js/
│   ├── registration.php
│   ├── theme.xml
│   ├── composer.json
```

一旦以上步骤完成，我们需要执行安装:升级命令。

```
$ php bin/magento setup:upgrade
```

执行升级命令后，主题将在我们的数据库中注册。你可以在数据库的“主题表”下查看你的主题。

在上述步骤中，我们已经成功地为我们的店面创建了主题，但我们必须从 Magento Admin 配置店面的主题:

登录 Magento 管理>内容>设计>配置>点击编辑>默认主题>在应用主题下拉菜单中选择你的主题>保存配置

之后，我们需要清除 magento 缓存

```
$ php bin/magento cache:flush
```

现在，我们已经成功地在我们的店面上应用了新创建的主题。

希望这篇文章能帮助你创建一个主题。

接下来的教程题目是[**Magento 2**中的布局概述](https://aryansrivastavadesssigner.medium.com/layout-overview-in-magento-2-9d4739ee4254)

如果你喜欢这篇文章，你可以给我买杯咖啡【https://www.buymeacoffee.com/aryansrivastava 

## 跟我来:

[领英](https://www.linkedin.com/in/er-aryan-srivastava-0b9576170/) [推特](https://twitter.com/AryanSr11861551)

**参考链接:**

1.  [https://devdocs.magento.com](https://devdocs.magento.com/)