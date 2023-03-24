# 在 ARM 设备上使用 Nginx、certbot 和 Docker 设置安全反向代理。

> 原文：<https://medium.com/nerd-for-tech/setting-up-a-secure-reverse-proxy-with-nginx-certbot-and-docker-on-arm-devices-6a7bd50c92b2?source=collection_archive---------1----------------------->

## 一个简单而完整的指南，介绍如何设置您的 ARM 设备作为一个完全安全的反向代理。

最近在我的 Raspberry Pi 设备上安装了[家庭助手](https://www.home-assistant.io/)的容器版本，这是一个流行的家庭自动化工具，我希望能够从互联网访问它。对于那些熟悉它的人来说，“监督”版本的配置非常简单，因为它提供了许多插件来无缝设置 nginx、动态 dns 甚至 wireguard vpn 服务器。但是对于它的其他版本，你几乎需要手动完成所有的事情，所以我决定划分责任，在不同的设备上自己配置所有的东西。

因此，让我们看看如何用两个 docker 容器配置我们的设备，一个用于 nginx，一个用于 certbot，以启动和运行我们的 https 服务器。

# 要求

*   本指南假设您的设备上已经安装了 Docker 并且功能齐全。
*   您应该已经订阅了动态 dns 服务，最后但同样重要的是，您的路由器应该已经正确配置为转发端口 80 和 443。
*   熟悉 Nginx 者优先:)

## 文件夹配置

首先，我们需要在 docker 主机上创建一些文件夹，以存储一些配置和 certbot 证书。此外，由于我们想要使用 2 个 docker 容器，我们还需要在容器之间共享这些证书。

> 请注意:我假设你的 arm 设备是基于 linux 的，所以你实际上可以复制粘贴下面的命令，它们将会工作并以你的用户名创建文件夹

```
$ mkdir -p /etc/letsencrypt
$ mkdir -p /nfs/$USER/nginx/www/letsencrypt/.well-known/acme-challenge
```

第一个文件夹是我们存储证书的地方，第二个文件夹有双重功能:它在/nfs/$USER 下创建一个 nginx 文件夹来存储配置，同时创建一个 acme-challenge 文件夹供 certbot 使用。

## 安装 Nginx

现在我们需要启动 nginx 并服务于一个 **http** 位置来完成 *acme-challenge。*

用以下内容创建一个 *proxy.conf* 文件:

```
server {
 listen 80;
 server_name mycustomserver.anydyndns.org;location ^~ /.well-known/acme-challenge/ {
 allow all;
 root /var/www/letsencrypt;
 default_type “text/plain”;
 }
}
```

并将其放入/nfs/$USER/nginx 文件夹中。

> 请注意:您需要更改的唯一部分是 server_name，以及您选择的 dyndns 名称。

现在我们准备启动我们的容器:

```
docker run --name nginx -d -p 7070:80 -p 7443:443 --restart always \
 -v /nfs/$USER/nginx/proxy.conf:/etc/nginx/conf.d/default.conf \
 -v /nfs/$USER/nginx/www/:/var/www/ \
 -v /etc/letsencrypt:/etc/letsencrypt nginx
```

*   “proxy.conf”文件完全取代了 default.conf(我不需要服务任何其他 http 位置，只需要 acme challenge 所需的位置)
*   我们将 www 文件夹本地绑定到先前创建的文件夹
*   我们绑定了 letsencrypt 证书文件夹，以便以后与 certbot 共享
*   我分别在 7080 和 7443 上重新映射了端口 80 和 443，但是可以根据您的需要随意更改。

> **提示**:如果你愿意，你可以通过在 acme-challenge 文件夹中添加一个包含任何内容的【test.html】文件并在[http://mycustomserver.anydyndns.org](http://mycustomserver.anydyndns.org)打开一个浏览器来测试 nginx 是否工作。如果一切正常，您应该会看到文件的内容。

## 使用 Certbot 生成证书

现在我们需要用 letsencrypt 生成证书。让我们运行 certbot:

```
docker run -it --name certbot \
 -v "/etc/letsencrypt:/etc/letsencrypt" \
 -v "/nfs/$USER/nginx/www/letsencrypt/:/webroot/" \
 certbot/certbot:arm32v6-latest certonly \
 -d mycustomserver.anyddns.org --verbose --keep-until-expiring \
 --agree-tos --email [myemail@provider.com](mailto:m.arciprete@gmail.com) \
 --preferred-challenges=http \
 --webroot --webroot-path=/webroot
```

在本例中，我们为 certbot 创建了一个容器，它:

*   使用共享的/etc/letsencrypt 文件夹
*   与 nginx 共享“acme-challenge”文件夹，存储挑战并通过 http 提供服务
*   使用 arm32 版本的图像

> 请注意:我们将所有参数都传递给了命令行，因为我们希望使用同一个容器在证书过期时自动更新证书。只是记得用你的动态域名和电子邮件来替换 mycustomserver.anyddns.org。

如果 certbot 正常工作，您应该会收到如下消息:

```
Requesting a certificate for mycustomserver.anydyndns.orgSuccessfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/mycustomserver.anydyndns.org/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/mycustomserver.anydyndns.org/privkey.pem
```

是时候完成 nginx 的配置了，创建一个 https 服务器。

## 将 Nginx 更新为代理 https 请求

让我们打开我们的 *proxy.conf* 文件，并添加以下内容:

1.  添加/位置，并重定向到 https
2.  设置 https 部件为您的新位置服务(在我的例子中是 HomeAssistant)

```
map $http_upgrade $connection_upgrade {
    default upgrade;
    ''      close;
}server {
   listen 80;
   location / {
        return 301 [https://$host$request_uri](https://$host$request_uri);
   }location ^~ /.well-known/acme-challenge/ {
        allow all;
        root /var/www/letsencrypt;
        default_type "text/plain";
   }
}server { listen 443 ssl default_server ipv6only=off;
    server_name mycustomserver.anydyndns.org; ssl_certificate  /etc/letsencrypt/live/mycustomserver.anydyndns.org/fullchain.pem;
    ssl_certificate_key  /etc/letsencrypt/live/mycustomserver.anydyndns.org/privkey.pem; ssl_protocols TLSv1.2;                                                     
    ssl_ciphers "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH:!aNULL:!eNULL:!EXPORT:!DES:!MD5:!PSK:!RC4";
    ssl_prefer_server_ciphers on;                                            
    ssl_session_cache shared:SSL:10m;                                                                       

    proxy_buffering off;access_log            /var/log/nginx/access.log; location / {
      proxy_pass              http://localhost:8080;                      
      proxy_set_header        Host $host;
      proxy_set_header        X-Real-IP $remote_addr;
      proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header        X-Forwarded-Proto $scheme;
      proxy_read_timeout  90;
      proxy_set_header Upgrade $http_upgrade;                                                             
      proxy_set_header Connection $connection_upgrade;
    }
  }
```

proxy_pass 应该包含您想要到达的服务器的地址，例如[http://home assistant . local:8123](http://homeassistant.local:8123)或您想要的任何内容。

> 请注意:我还添加了升级和连接头，以及 nginx 中的“map”部分。只有当你打算像 HomeAssistant 那样使用 Websocket 时，才需要这样做。

# 证书的自动更新

Letsencrypt 证书将在 90 天后过期。为了自动更新它们，我使用一个 cron 任务来调用 certbot 容器，然后重新加载 nginx 来激活更改。如果您注意到了，certbot 容器是用“保留到过期”标志创建的，所以证书实际上只在需要时才更新。

首先，我们通过输入以下命令编辑 crontab

```
$ crontab -e
```

添加以下几行:

```
30 2 * * * docker start certbot
35 2 * * * docker exec nginx nginx -s reload
```

这指示 crontab 在每晚凌晨 2:30 运行“docker start certbot ”,然后在 5 分钟后的 2.35 重新加载 nginx 配置，以确保 certbot 过程完成。