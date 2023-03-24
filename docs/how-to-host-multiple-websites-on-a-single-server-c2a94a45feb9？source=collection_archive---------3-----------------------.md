# 如何在一台服务器上托管多个网站

> 原文：<https://medium.com/nerd-for-tech/how-to-host-multiple-websites-on-a-single-server-c2a94a45feb9?source=collection_archive---------3----------------------->

*一种在一个 VPS 服务器实例上托管几乎无限数量的 web 应用的方法——只需几分钱*

![](img/fac6cc218cf5e7f8ceff5bd48120ed6a.png)

由[卡尔·海尔达尔](https://unsplash.com/@carlheyerdahl?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

作为一个自学成才的全栈开发人员，处理我自己的小规模 DevOps 已经成为我日常工作的一部分，尤其是对于个人项目。我总是有一些需要测试的想法，以及展示给潜在雇主的作品集，更不用说我的个人网站了。但是因为我通常都很穷，而且每个网站的访客都很少(如果有的话)，所以为每个网站设置一个专门的 VPS 是疯狂的。

在本文中，我将向您展示将您的 web 应用程序部署到 VPS 服务器的最简单方法，然后继续添加更多的应用程序。我假设您已经知道什么是 VPS，并且知道如何从云提供商那里获得 VPS 的实例。

## 什么技术？

您可能听说过 Apache、Nginx 甚至 Traefik。这些人都有能力完成这项任务，但学习过程可能会很艰难。

当我发现了 [CaddyServer](https://caddyserver.com/) 之后，我的生活变得轻松了一百倍，我强烈建议你去看看。我们将使用 CaddyServer 创建一个称为反向代理的东西。反向代理是魔术发生的地方，CaddyServer 将在一行代码中执行这一魔术。

在本文中我不会谈论具体的语言或框架。你可能在使用 Laravel/Rails/Wordpress/Ghost/Express/React/Spring/任何东西；但是只要你能把你的 web 应用服务到一个端口，这里的过程或多或少是一样的。

## 什么云提供商？

我的推荐是[数字海洋](https://www.digitalocean.com/)。每月大约 5 美元，你就可以在一个叫做“droplet”的 VPS 上托管所有你喜欢的网站。请记住，很明显，5 美元并不能满足任何大流量的需求。你也可以货比三家，购买其他低价产品，比如 Linode 或 AWS free tier。

## 设置您的 DNS

你以前可能已经购买了域名，但是正确设置它们仍然很棘手。吻(保持简单愚蠢！)它。创建记录。我一直认为这是最简单的方法，而不是搞乱名称服务器设置、别名等。

给你的根域分配一个 A 记录，并给它你的 VPS/droplet 的 IP 地址。你需要等一会儿。如果您不确定更改是否已经传播，请运行

```
dig example.com
```

使用 A 记录的好处是，即使你必须为每个 apex 域名付费，大多数注册商也会允许你添加子域名。将任意数量的子域名分配给同一个 IP 地址，我们的反向代理将使用域名本身作为映射来路由流量。

瞧，多个网站使用一台服务器和一个域名。我发现这对于测试 prod 环境，或者将我的后端托管在一个子域，例如`api.example.com`，非常有用。

# 好的，让我们设置反向代理

我通常在这个阶段使用 Docker，但是为了简单起见，我们暂时不使用它。一旦您的 VPS 通过您选择的云提供商初始化，在中的`ssh`。这将类似于

```
ssh root@<ip-address>
```

注意:最好尽快设置一个非 root 用户。

现在转到 [CaddyServer 安装页面](https://caddyserver.com/docs/install)，按照与您的 VPS 操作系统相关的说明进行操作。

安装后，CaddyServer 应该(希望)正在运行。用检查其状态

```
sudo systemctl status caddy
```

在`/etc/caddy`你会发现神奇的`Caddyfile`。这个非常简单的文本文件将控制我们的反向代理。您将在这个文件中找到一些样板文件，并随意创建一个备份，但您可能不需要它。

用这个非常简单的配置替换整个文件:

```
{
    acme_ca [https://acme-staging-v02.api.letsencrypt.org/directory](https://acme-staging-v02.api.letsencrypt.org/directory)
}example.com {
    reverse_proxy localhost:8000
}
```

很简单，对吧？

Caddy 现在将把`localhost`上的端口`8000`与域`example.com`关联起来。你需要在这里添加你自己的域名来代替`example.com`。您可能对`acme_ca`代码感到疑惑——我们稍后会谈到它。

现在，重新加载球童:

```
sudo systemctl reload caddy
```

## 设置第一个 web 应用程序

现在 Caddy 已经准备好了，并且已经将端口 8000 绑定到 http 端口 80 和 https 端口 443(这发生在引擎盖下)，让我们设置我们的 web 应用程序。我不打算深入讨论如何做到这一点，我要说的是，无论您使用什么语言和框架，您都需要在指定的端口上运行产品代码。例如，如果我想用 Gunicorn 运行一个 Django 应用程序，我会运行如下代码

```
gunicorn config.wsgi:application --bind 0.0.0.0:8000
```

或者，如果我正在运行一个类星体框架应用程序，它可能是

```
cd dist/ssr/ && PORT=8000 npm run start
```

假设您可以启动一个应用程序，并通过一个特定的端口为其提供服务。记住将您使用的任何端口与您在`Caddyfile`中使用的端口相匹配——这是最基本的。

## SSL/TLS/https 注意事项

这是一个简单的问题。CaddyServer 默认做 https。太牛逼了。

在引擎盖下，Caddy 正在使用让我们加密。当您进行设置和调试时，您不希望每次都请求新的证书—您将遇到每日限额。

这就是为什么我们在`Caddyfile`中添加了这一行:

```
acme_ca [https://acme-staging-v02.api.letsencrypt.org/directory](https://acme-staging-v02.api.letsencrypt.org/directory)
```

你需要在浏览器中“接受风险”来访问你的网站。因此，如果你访问你的域名，你击中了这个警告，这是应该发生的。

当你对应用程序的运行感到满意时，完全删除`acme_ca`代码。这就是通过 https 为您的站点提供服务所需要做的一切。

注意:如果您尝试访问您的域时浏览器挂起，需要检查的一件事是您的防火墙是否允许访问 http/s 端口。

## 设置后续 web 应用程序

一旦第一个应用程序开始运行，您就可以对任意多个应用程序做同样的事情。向`Caddyfile`添加一个新条目，给它一个端口，然后向这个新端口提供另一个应用程序。

例如，如果你想在`api.example.com`运行一个后端 Django 应用程序，在`example.com`运行一个前端 Vue 应用程序，你的`Caddyfile`可能是:

```
example.com {
    reverse_proxy localhost:3000
}api.example.com {
    reverse_proxy localhost:8000
}
```

然后在端口 8000 上运行 Django，在端口 3000 上运行 Vue。一旦您重新加载 Caddy，新的应用程序将自动请求一个新的 SSL 证书。

我希望这篇文章能够帮助您使用反向代理在同一台服务器上托管多个应用程序。下次见！

*如果你想更进一步，看看这些想法是如何工作的，请查看我的开源项目，*[*Djengu*](https://github.com/johnckealy/djengu)*。*