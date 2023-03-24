# [已解决]安装 SSL 后，WordPress 中的更新失败/发布失败

> 原文：<https://medium.com/nerd-for-tech/resolved-update-failed-publish-failed-in-wordpress-after-ssl-installation-13671e362609?source=collection_archive---------17----------------------->

![](img/cae1ecb5e32b100c8f6641edd1fe0874.png)

你曾经尝试过在 IIS 服务器上建立一个 WordPress 网站吗？

嗯，我尝试过使用 Cloudflare Universal SSL。安装后，我无法保存/发布帖子或页面。每次我尝试，它抛出一个错误“更新失败”或“发布失败”。

在谷歌搜索了很多之后，我试着遵循一些解决方案，其中一个非常有效。我希望它也能帮助你。

*   设置->永久链接->保存(不起作用)
*   移除所有插件并重启 IIS(不起作用)
*   在 php.ini 中添加了`always_populate_raw_post_data = -1`(运气不好)
*   安装[https://wordpress.org/plugins/classic-editor/](https://wordpress.org/plugins/classic-editor/)插件并激活。这一个实际上工作，但只有当我启用经典编辑器，而不是块编辑器，我想利用 WordPress 的块编辑器。(所以对我没用)
*   安装[https://wordpress.org/plugins/cloudflare/](https://wordpress.org/plugins/cloudflare/)插件并激活。(非常有效)。所有跨原点错误都已解决，一切正常。https://WordPress . org/plugins/cloud flare/