# 在 GatsbyJS 中设置路径重定向

> 原文：<https://medium.com/nerd-for-tech/setting-up-path-redirects-in-gatsbyjs-6af3ca7f727b?source=collection_archive---------10----------------------->

## 通过在 _redirect 文件中定义规则，永久地重定向 Gatsby 和 Netlify 中的路径。

![](img/fd90dcf7574cab4eaee9e548fefeb61e.png)

"照片由[朱利安·霍格桑](https://unsplash.com/@julianhochgesang?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/move?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄"

🔔这篇文章最初发布在我的网站[MihaiBojin.com](https://MihaiBojin.com/personal-site/path-redirects-in-gatsbyjs?utm_source=external&utm_medium=organic&utm_campaign=rss)上。🔔

在任何一个网站的生命周期中，都会有一段时间为了更好的 SEO 评级而重新组织内容。

当这种情况发生时，您需要将旧路径重定向到新路径。对于常规网站，您可以轻松定义任何必要的重定向。比如 [Apache](https://httpd.apache.org/) 有[重写引擎](https://httpd.apache.org/docs/2.4/rewrite/remapping.html)， [Nginx](https://www.nginx.com/) 有[重写](https://www.nginx.com/blog/creating-nginx-rewrite-rules/) s，等等！

这通常如下进行:

*   用户在其浏览器中请求`/path/a`
*   响应的 web 服务器检查其重定向表，并确定内容已经移动
*   然后，它用一个`301 Moved Permanently`、`Location: /path/b`报头来响应
*   通知用户的浏览器加载`/path/b`

对于像 Gatsby 这样静态生成的站点，实现这种流量并不容易。首先，Gatsby 所做的只是生成 HTML。其次，web 服务器(不是 HTML)处理[响应头](https://developer.mozilla.org/en-US/docs/Glossary/Response_header)。

一些研究揭示了[盖茨比云如何处理重定向](https://support.gatsbyjs.com/hc/en-us/articles/1500003051241-Working-with-Redirects)。

这种方法是可行的，但是需要您将`gatsby-node.js`中的所有重定向规则定义为 javascript。对我来说，这就像是一种代码的味道...

经过进一步的研究，我发现了【Netlify 如何处理重定向。在文本文件中列出我的重定向规则似乎更合适。作为一个额外的好处，如果我决定把我的网站从盖茨比的云转移到[的网络，我已经领先一步了。](https://www.netlify.com/)

这方面的代码非常简单；看看吧！

## 1.从文本文件生成重定向

```
async function processRedirects({ actions, reporter }) {
  const lineReader = readline.createInterface({
    // using the same file name as Netlify
    input: fs.createReadStream('_redirects'),
  });

  const { createRedirect } = actions;

  // process every line and define the path redirect
  lineReader.on('line', (line) => {
    const [fromPath, toPath] = line.split(/\s+/);
    console.log(`Redirecting ${fromPath} to ${toPath}`);
    createRedirect({ fromPath, toPath });
  });
}
```

## 2.调用盖茨比-node.js 中的逻辑

```
exports.createPages = async ({ graphql, actions, reporter }) => {
  ...
  await processRedirects({ actions, reporter });
};
```

## 3.定义所有重定向规则

例如，这里有一个将`/tag`路径重定向到`/tags`的定义。

```
/tag   /tags
```

## 她就写了这么多，各位。

我在这里没什么可说的了。希望你觉得这个小教程有用！

如果你喜欢这篇文章并想阅读更多类似的文章，请订阅我的简讯。我每隔几周就发一封！