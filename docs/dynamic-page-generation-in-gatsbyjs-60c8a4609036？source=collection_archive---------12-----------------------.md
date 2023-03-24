# GatsbyJS 中的动态页面生成

> 原文：<https://medium.com/nerd-for-tech/dynamic-page-generation-in-gatsbyjs-60c8a4609036?source=collection_archive---------12----------------------->

## 避免复制粘贴 HTML，支持使用布局、组件和 GraphQL 变量！

![](img/8b2a087ae91a4cd8e8384a21c72b38c5.png)

"照片由[艾萨克·斯洛曼](https://unsplash.com/@isaac_slo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/dynamic?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄"

🔔这篇文章最初发布在我的网站上，[MihaiBojin.com](https://mihaibojin.com/personal-site/dynamic-page-generation-gatsbyjs?utm_source=Medium&utm_medium=organic&utm_campaign=rss)。🔔

最近，我一直在努力为我的网站生成内容。因此，很自然地，它分为多个类别。

我发现自己在复制粘贴页面，更改标题、描述和 GraphQL 查询，这对于我的喜好来说有点太多了。

我没有受到启发！

并决定做一些更好的事情…

下面是我使用一个通用模板动态生成分类页面的方法。

## 在 gatsby-config.js 中创建类别元数据

首先，我在`gatsby-config.js`的`siteMetadata`数组中定义了我的类别。

```
"categories": [
    {
      "title": "Blog",
      "href": "/blog",
      "description": "My blog. I write about my journey as a software engineering lead and content creator.",
      "pageHeading": "From the blog",
      "pageSubtitle": "Explore some of my blog posts as I embark on a journey to build my site from scratch using GatsbyJS and TailwindUI."
    },
    ...
  ]
```

## 在 gatsby-node.js 中动态创建页面

然后，我扩展了 Gatsby 的页面创建 API:

```
exports.createPages = async ({ graphql, actions, reporter }) => {
  await createCategoryPages({ graphql, actions, reporter }); // a new async function I created
};
```

我根据上下文来划分不同的功能。我定义了一个负责创建类别的函数。

```
async function createCategoryPages({ graphql, actions, reporter }) {
  const { createPage } = actions;

  // logic here, see below
}
```

我在函数体中加载了站点的元数据；我需要关于我的类别和作者的信息。

```
const result = await graphql(
    `
      {
        site {
          siteMetadata {
            categories {
              title
              href
              description
              pageHeading
              pageSubtitle
            }
            author {
              name
              href
            }
          }
        }
      }
    `,
  );
```

接下来是验证步骤。我跳过了验证代码，但基本上，这是对这些对象的空检查，然后调用`reporter.warn`或`reporter.panicOnBuild`。

```
const siteMetadata = result.data.site?.siteMetadata;
  const categories = siteMetadata?.categories;
  const author = siteMetadata?.author;
```

因为组件不会改变，所以我在迭代之前缓存了它。

```
const component = path.resolve(`./src/components/category.js`);
```

最后，我遍历了所有类别来创建每个页面。

```
categories.forEach((page) => {
    createPage({
      path: page.href,
      component,
      context: {
        ...page,
        author,
      },
    });
  });
```

注意:`component`将需要加载每个类别的文章列表。由于站点有多个类别的文章，我们需要传递一些东西来构造正确的查询。在 Gatsby 中，这是通过页面上下文传递数据来实现的。GraphQL 页面查询可以利用由上下文传递的任何[变量。](https://www.gatsbyjs.com/docs/how-to/querying-data/page-query/#how-to-add-query-variables-to-a-page-query)

## 使用变量按类别过滤文章

对于我的网站，我选择按每个类别的标题进行过滤。我可以很容易地通过类别的 href 进行过滤，但我也不打算做太多改变。时间会证明这是否是正确的选择…

我更新了旧的查询:

```
query {
    allMarkdownRemark(
        sort: { fields: [frontmatter___date], order: DESC }
        filter: {
          frontmatter: {
            category: {
              title: { eq: "A category title" }
            }
          }
        }
    ) {
        nodes {
        ...
        }
    }
}
```

通过给它一个名字并传递`$title`变量。

这个变量从页面的上下文中自动填充——🪄**魔法** 🪄！

```
query CategoryQuery($title: String!) {
    allMarkdownRemark(
        sort: { fields: [frontmatter___date], order: DESC }
        filter: {
          frontmatter: {
            category: {
              title: { eq: $title }
            }
          }
        }
    ) {
        nodes {
        ...
        }
    }
}
```

我剩下要做的就是加载页面数据并填充我的模板:

```
export default function Page({ data, pageContext }) {
  const {
    title,
    description,
    pageHeading,
    pageSubtitle,
    author,
  } = pageContext;
  const posts = data.allMarkdownRemark.nodes;
  return (
    <>
        ...
    </>
  );
}
```

瞧啊。动态生成的页面:

*   [博客](https://mihaibojin.com/blog?utm_source=Medium&utm_medium=organic&utm_campaign=rss)
*   [编码拼图](https://mihaibojin.com/coding-puzzles?utm_source=Medium&utm_medium=organic&utm_campaign=rss)
*   [个人贡献者](https://mihaibojin.com/ic?utm_source=Medium&utm_medium=organic&utm_campaign=rss)

如果你喜欢这篇文章，并想阅读更多类似的文章，请订阅我的简讯。我每隔几周就发一封！