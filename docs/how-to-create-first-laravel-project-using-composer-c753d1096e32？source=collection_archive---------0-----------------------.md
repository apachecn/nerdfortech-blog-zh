# 如何使用 composer 创建第一个 Laravel 项目！！！

> 原文：<https://medium.com/nerd-for-tech/how-to-create-first-laravel-project-using-composer-c753d1096e32?source=collection_archive---------0----------------------->

![](img/611c21ca0732b2b81697a142d8692ea5.png)

> Laravel 是一个 php 框架，它将加速你的开发。它会对你的前端和后端都有意见。它有一个很酷的特性，叫做 MVC(模型、视图、控制器)。Laravel 易于访问，但功能强大，为大型健壮的应用程序提供了所需的强大工具。一个极好的反转控制容器，富于表现力的迁移系统，以及紧密集成的单元测试支持，为您提供了构建任何应用程序所需的工具。

在进入教程之前，让我先弄清楚一些事情。

1.  我使用的是安装了 Xampp 服务器的 Windows 10 机器。如果你想在你的系统中安装 xampp，就去这个 [**链接**](https://www.apachefriends.org/download.html) 。
2.  你需要安装**Composer**——它是 PHP 的依赖管理器。您必须将它安装在 xampp 路径中。 [**下载这里**](https://getcomposer.org/download/) **。**

因此，如果你已经正确安装了所有需要的步骤，那么开始创建你的第一个 Laravel 项目。

# 通过 Composer 安装:

打开命令提示符，按照命令操作:

```
composer create-project laravel/laravel example-app

cd example-app

php artisan serve
```

它将创建一个 laravel 项目，文件夹名为“example-app”。然后转到正确的目录。并运行最后一个命令。“php artisan serve”命令用于在本地主机上运行项目。

# Laravel 安装程序:

或者，您可以将 Laravel 安装程序作为全局 Composer 依赖项来安装:

```
composer global require laravel/installer

laravel new example-app

cd example-app

php artisan serve
```

> 就是这个！！！您的第一个 Laravel 项目已经成功创建，并准备构建一些有趣的东西。