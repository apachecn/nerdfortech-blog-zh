# Magento2 有用的命令列表

> 原文：<https://medium.com/nerd-for-tech/magento2-useful-command-list-l-desssigner-in-992e71cc985b?source=collection_archive---------1----------------------->

![](img/158c73935489e3809a451cfaf0aa55d3.png)

当我们在 Magento2 商店工作时，我们需要执行一些命令。但是记住所有的命令有点困难。在这里，我将为您提供所有有用的万磁王命令列表。

## 1.创建管理员用户:

```
php bin/magento admin:user:create
```

示例:

```
php bin/magento admin:user:create --admin-user='admin' --admin-password='admin123' --admin-email='admin@xyz.com' --admin-firstname='Aryan' --admin-lastname='Srivastava'
```

## 2.解锁管理员用户帐户

```
php bin/magento admin:user:unlock username
```

示例:

```
php bin/magento admin:user:unlock admin
```

# 隐藏物

## 1.清理缓存

```
php bin/magento cache:clean
```

短代码:

```
php bin/magento c:c
```

## 2.刷新缓存存储

```
php bin/magento cache:flush
```

短代码:

```
php bin/magento c:f
```

## 3.禁用缓存

```
php bin/magento cache:disable
```

短代码:

```
php bin/magento c:d
```

## 4.启用缓存

```
php bin/magento cache:enable
```

短代码:

```
php bin/magento c:e
```

## 5.检查缓存状态

```
php bin/magento cache:status
```

# 设置

## 1.安装升级

升级 Magento 应用程序、数据库数据和模式

```
php bin/magento setup:upgrade
```

短代码:

```
php bin/magento s:up
```

## 2.安装编译

生成 DI 配置和所有可以自动生成的缺失类。

```
php bin/magento setup:di:compile
```

短代码:

```
php bin/magento s:d:c
```

## 3.设置部署

部署静态视图文件。

```
php bin/magento setup:static-content:deploy
```

短代码:

```
php bin/magento s:s:d
```

注意:在开发人员/默认模式下，我们需要强制使用部署标志(-f)来部署静态视图文件。

示例:

```
php bin/magento setup:static-content:deploy -f
```

或者

```
php bin/magento s:s:d -f
```

## 4.卸载 Magento 应用程序

```
php bin/magento setup:uninstall
```

## 5.备份 Magento 应用程序代码库、媒体和数据库

```
php bin/magento setup:backup --code --db --media
```

# 部署

## 1.设置部署模式

设置应用模式。

```
php bin/magento deploy:mode:set <value>
```

注意:值可以是:

a.开发者

b.生产

c.系统默认值

示例:

```
php bin/magento deploy:mode:set production
```

## 2.显示部署模式

显示当前应用程序模式。

```
php bin/magento deploy:mode:show
```

# 偏差

## 1.启用探查器

```
php bin/magento dev:profiler:enable
```

## 2.禁用探查器

```
php bin/magento dev:profiler:disable
```

# 维护

## 1.显示维护模式的状态

```
php bin/magento maintenance:status
```

## 2.启用维护模式

```
php bin/magento maintenance:enable
```

启用除特定 IP 之外的维护模式:

```
php bin/magento maintenance:enable --ip=127.0.0.1 --ip=127.0.0.2
```

## 3.允许特定的 IP

设置维护模式豁免 IP。

```
php bin/magento maintenance:allow-ips --ip=127.0.0.1 --ip=127.0.0.2
```

## 4.禁用维护模式

```
php bin/magento maintenance:disable
```

# 目录

## 1.创建调整大小的产品图像

```
php bin/magento catalog:images:resize
```

## 2.使用 CLI 删除 Magento 应用程序中未使用的产品属性

```
php bin/magento catalog:product:attributes:cleanup
```

# 分度器

## 1.显示索引器信息

```
php bin/magento indexer:info
```

短代码:

```
php bin/magento i:info
```

## 2.显示索引器状态

```
php bin/magento indexer:status
```

短代码:

```
php bin/magento i:status
```

## 3.重置索引器

```
php bin/magento indexer:reset
```

短代码:

```
php bin/magento i:reset
```

## 4.重新索引数据

```
php bin/magento indexer:reindex
```

短代码:

```
php bin/magento i:rei
```

# 组件

## 1.显示模块的状态

```
php bin/magento module:status
```

## 2.启用模块

```
php bin/magento module:enable <vendorname_modulename>
```

示例:

```
php bin/magento module:enable Magento_TwoFactorAuth Magento_CmsPageBuilderAnalytics
```

## 3.禁用模块

```
php bin/magento module:disable <vendorname_modulename>
```

示例:

```
php bin/magento module:disable Magento_TwoFactorAuth Magento_CmsPageBuilderAnalytics
```

# 商店

## 1.显示商店列表

```
php bin/magento store:list
```

## 2.显示网站列表

```
php bin/magento store:website:list
```

# 时间单位

## 1.在 Magento 中安装 crontab

```
php bin/magento cron:install
```

## 2.在 Magento 中运行 cron

```
php bin/magento cron:run
```

## 3.删除 Magento 中的 crontab

```
php bin/magento cron:remove
```

# 信息

## 1.显示 Magento 管理 URI

```
php bin/magento info:adminuri
```

## 2.打印可用备份文件的列表

```
php bin/magento info:backups:list
```

## 3.显示可用货币的列表

```
php bin/magento info:currency:list
```

## 4.显示可用语言环境的列表

```
php bin/magento info:language:list
```

## 5.显示可用时区的列表

```
php bin/magento info:timezone:list
```

希望这篇文章能帮助您理解如何向客户帐户仪表板添加链接。如果您有任何疑问，您可以通过电子邮件直接询问我，地址:[](mailto:aryansrivastavadesssigner@gmail.com)**或通过此处 联系我 [**。**](https://desssigner.in/contact/)**

**如果你想要一个现场会议，请直接在 LinkedIn 上联系我，我会在周末安排一个在线会议。**

***原载于 2022 年 3 月 24 日*[*https://desssigner . in*](https://desssigner.in/magento2-useful-command-list/)*。***