# 和盖茨比一起写博客

> 原文：<https://medium.com/nerd-for-tech/blogging-with-gatsbyjs-b6fbafc198c5?source=collection_archive---------14----------------------->

## 如何使用 Remark、Prism 和其他插件为任何读者在 GatsbyJS 上写博客

![](img/113a04ea70a3ad806d3f669de0c35030.png)

"图为[亚伦·伯登](https://unsplash.com/@aaronburden?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/blog?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) "

🔔这篇文章最初发布在我的网站上，[MihaiBojin.com](https://mihaibojin.com/personal-site/blog-on-gatsbyjs?utm_source=Medium&utm_medium=organic&utm_campaign=rss)。🔔

本帖简单介绍了盖茨比最常见的特征(大概是):**用** [**备注 JS**](https://remark.js.org/) 写博客。

希望在本文结束时，您将对一些用于配置博客功能的常用插件有一个基本的了解，以及它们能带来什么好处！

实现我们目标所需的核心插件是官方的[Gatsby-transformer-remark](https://www.gatsbyjs.com/plugins/gatsby-transformer-remark/)插件。

这篇文章的目的是解释一些常用的插件，你可以用它们来用 GatsbyJS 建立你自己的博客。因为配置是通用的，所以您可以面向任何受众，但是如果您不打算包含代码，您可以安全地跳过 PrismJS 插件。

我们开始吧！

## 安装盖茨比-变压器-备注

假设您已经有了一个 GatsbyJS 站点，从安装插件开始:

```
npm install gatsby-transformer-remark
```

## 安装附加插件

`gatsby-transformer-remark`插件支持多个输入源(例如，各种内容管理系统)，但是最简单的选择是在本地托管 markdown 文件。为此，您需要以下插件:

```
npm install gatsby-source-filesystem
```

## 图像优化

你很可能想在你的减价内容中包含图片。`gatsby-remark-images`插件有几个漂亮的特性，比如:

*   将图像调整到目标容器的大小
*   生成多种图像大小的 srcset(支持多种设备宽度)
*   加载图像时出现“模糊”

```
npm install gatsby-remark-images gatsby-plugin-sharp gatsby-transformer-sharp
```

## 内容标题链接

`gatsby-remark-autolink-headers`插件自动为每个标题生成 HTML IDs，允许你的读者链接到你文章中的特定部分。

```
npm install gatsby-remark-autolink-headers
```

## 优化的内部链接

由于 markdown 内容中的链接没有使用 React 的 [reach router](https://www.gatsbyjs.com/docs/reach-router-and-gatsby/) ，所以您还需要转换它们以实现更快的导航:

```
npm install gatsby-plugin-catch-links
```

## 静态内容

如果您链接了 markdown 中的任何文件(如 pdf)，您会希望它们与任何其他静态资源一起被复制。

```
npm install gatsby-remark-copy-linked-files
```

## 用 PrismJS 突出显示语法

假设您将在内容中包含一些代码，您将希望与 [PrismJS](https://prismjs.com/) 集成:

```
npm install gatsby-transformer-remark gatsby-remark-prismjs prismjs
```

## 配置插件

在继续之前，您需要决定在哪里存放您的文件。一个常见的选择是`/content`。您可以直接在那个目录中创建`.md`文件，或者您可以创建任意数量的子目录。您使用的路径将决定您的内容将被托管的 slugs。比如在`/content/my/article.md`下创建一个页面会渲染到下面的页面:`[https://SITE_URL/my/article](https://SITE_URL/my/article.)` [。](https://SITE_URL/my/article.)

> *slug* 是一个常用术语，用来表示 URL 中标识页面的部分。

## 盖茨比-配置. js

首先，将以下配置添加到您的`gatsby-config.js`中。

```
module.exports = {
  plugins: [
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        path: `${__dirname}/content`,
        name: `blog`,
      },
    },
    {
      resolve: `gatsby-transformer-remark`,
      options: {
        plugins: [
          {
            resolve: `gatsby-remark-images`,
            options: {
              maxWidth: 730,
            },
          },
          `gatsby-remark-autolink-headers`,
          `gatsby-plugin-catch-links`,
          `gatsby-remark-prismjs`,
          `gatsby-remark-copy-linked-files`,
        ],
      },
    },
    `gatsby-transformer-sharp`,
    {
      resolve: `gatsby-plugin-sharp`,
      options: {
        defaults: {
          formats: [`auto`, `webp`],
          placeholder: `blurred`,
          quality: 95,
          breakpoints: [750, 1080, 1366, 1920],
          backgroundColor: `transparent`,
          tracedSVGOptions: {},
          blurredOptions: {},
          jpgOptions: {},
          pngOptions: {},
          webpOptions: {},
          avifOptions: {},
        },
      },
    },
  ],
};
```

## 创建博客页面

完成所有这些之后，最后一步是从 [GraphQL](https://www.gatsbyjs.com/docs/graphql/) 中读取所有的 markdown 并生成页面。

编辑您的`gatsby-node.js`文件并添加以下内容(如果不存在，创建一个):

```
const path = require(`path`);
const { createFilePath } = require(`gatsby-source-filesystem`);

exports.onCreateNode = ({ node, actions, getNode }) => {
  const { createNodeField } = actions;

  if (node.internal.type === `MarkdownRemark`) {
    // generate post slug
    const value = createFilePath({ node, getNode });

    createNodeField({
      name: `slug`,
      node,
      value,
    });
  }
};

exports.createPages = async ({ graphql, actions, reporter }) => {
  const { createPage } = actions;

  // Get all markdown blog posts sorted by date
  const result = await graphql(
    `
      {
        allMarkdownRemark(sort: { fields: [frontmatter___date], order: ASC }) {
          nodes {
            id
            fields {
              slug
            }
          }
        }
      }
    `
  );

  if (result.errors) {
    reporter.panicOnBuild(
      `There was an error loading your blog posts`,
      result.errors
    );
    return;
  }

  // Create blog posts pages only if there's at least one markdown file
  const posts = result.data.allMarkdownRemark.nodes;
  if (posts.length == 0) {
    reporter.warn(`No blog posts found`);
    return;
  }

  // Define a template for blog post
  const component = path.resolve(`./src/components/blog-post.js`);

  posts.forEach((post, index) => {
    const previousPostId = index === 0 ? null : posts[index - 1].id;
    const nextPostId = index === posts.length - 1 ? null : posts[index + 1].id;

    console.log(`Creating blog page: ${post.fields.slug}`);
    createPage({
      path: post.fields.slug,
      component,
      context: {
        id: post.id,
        previousPostId,
        nextPostId,
      },
    });
  });
};
```

每个页面都基于一个模板`./src/templates/blog-post.js`。继续创建这个文件，并用 React 组件填充它，该组件将显示呈现的 markdown 和其他元素。这里有一个[的例子](https://github.com/gatsbyjs/gatsby-starter-blog/blob/master/src/templates/blog-post.js)。

## 结论

恭喜你！在这一点上，你应该有一个工作的博客功能，使你能够写简单的 markdown 并将其转换成页面，如你现在正在阅读的页面。

如果你喜欢这篇文章，并想阅读更多类似的文章，[请订阅我的简讯](https://motivated-founder-807.ck.page/db1cf284bf)；我每隔几周就发一封！