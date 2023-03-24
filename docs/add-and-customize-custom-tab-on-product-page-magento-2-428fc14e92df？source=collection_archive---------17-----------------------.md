# 在 Magento 2 产品页面上添加和自定义自定义选项卡

> 原文：<https://medium.com/nerd-for-tech/add-and-customize-custom-tab-on-product-page-magento-2-428fc14e92df?source=collection_archive---------17----------------------->

![](img/17ad1ee592f05b5c9509a140e830e09f.png)

电子商务在我们的生活中扮演着非常重要的角色，尤其是在新冠肺炎疫情。众所周知，外出购买我们的必需品是不安全的。在这种情况下，许多供应商提供网上购买我们的商誉。

在网上购物中，展示适当的产品规格对于方便顾客来说是非常重要的。

让我们继续自定义产品详细信息页面中的内容选项卡:

**1。移除产品标签**

从产品页面移除任何标签都是非常简单的任务。我们只需要遵循几个步骤。

文件夹路径:

/供应商/magento/模块-目录/视图/前端/布局

我们需要对`catalog_product_view.xml`进行修改

覆盖我们主题中的文件:

/app/design/frontend/<vendor_name>/<theme_name>/Magento _ Catalog/layout/Catalog _ product _ view . XML</theme_name></vendor_name>

```
<?xml version="1.0"?>
<page layout="1column" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance";; xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
  <body>
    <referenceBlock name="product.info.review" remove="true" />
  </body>
</page>
```

在上面的代码中，我们从产品页面中删除了评论标签。如果您想删除其他选项卡，只需针对该元素并删除即可。

**2。添加新的自定义选项卡**

在`catalog_product_view.xml`中定义选项卡

```
<?xml version="1.0"?>
<page layout="1column" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
   <body>
       <referenceBlock name="product.info.details">
           <block class="Magento\Catalog\Block\Product\View" name="custom.tab" template="Magento_Catalog::custom/custom_tab.phtml" group="detailed_info" >
               <arguments>
                   <argument translate="true" name="title" xsi:type="string">Custom Tab</argument>
               </arguments>
           </block>
       </referenceBlock>
   </body>
</page>
```

现在，在中定义自定义选项卡的模板文件

/app/design/frontend/<vendor_name>/<theme_name>/Magento _ Catalog/templates/custom/custom _ tab . phtml</theme_name></vendor_name>

```
<?php

  echo "This is Custom tab in product detail page";

?>
```

***调用自定义产品页签中的静态块*** :

```
<?php echo $this->getLayout()
     ->createBlock('Magento\Cms\Block\Block')
     ->setBlockId('your_block_identifier')
     ->toHtml();?>
```

**3。重命名产品标签**

重命名现有选项卡是非常简单的任务。我们将目标锁定为`Custom Tab`，并将其重命名为`New Custom Tab`

去`catalog_product_view.xml`

```
<?xml version="1.0"?>
<page layout="1column" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance";; xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
  <body>
    <referenceBlock name="product.info.details">                
      <referenceBlock name="custom.tab">
        <arguments>
          <argument name="title" translate="true" xsi:type="string">New Custom Tab</argument>
        </arguments>
      </referenceBlock>
    </referenceBlock>
  </body>
</page>
```

希望这篇文章能帮助你在产品页面上添加自定义标签。请关注我关于 Magento 前端开发的进一步文章。

如果你喜欢这篇文章，你可以请我喝杯咖啡[https://www.buymeacoffee.com/aryansrivastava](https://www.buymeacoffee.com/aryansrivastava)