# 使用 Gatsby & Netlify CMS 重新创建灵活的内容

> 原文：<https://medium.com/nerd-for-tech/reacreate-flexible-content-with-gatsby-netlify-cms-8c34ee308919?source=collection_archive---------19----------------------->

![](img/a438c4e524cae9942c61196acfe71f04.png)

费伦茨·阿尔马西在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 如果你是一个 WordPress 开发者，那么你一定听说过一个叫做高级定制域的插件和一个灵活的内容域，它允许编辑轻松地生成新页面。

当我开始更多地进入 JAMStack 时，我想在 Gatsby 中重建 ACF 的灵活内容领域。可以把 WordPress 作为一个无头的 CMS 来使用，一些无头的 CMS 已经实现了某种替代方案。Prismic 有切片(不幸的是，您不能在字段中创建多个可重复的字段)。

**对于较小的项目，WordPress 或 Prismic 可能太复杂。在这种情况下，我通常选择我最喜欢的平面文件 CMS——Netlify CMS。**

Netlify CMS 提供你需要的一切，它是开源的，可以免费使用。唯一缺少的是什么？灵活的内容字段。幸运的是，有了 beta 特性——手动初始化和列表字段的变量类型,我们可以很容易地创建一个复制 ACF 灵活内容的解决方案。

# 为什么使用灵活的内容是一个好主意？

高级定制字段的灵活内容允许编辑人员**在没有开发人员参与的情况下快速做出重大更改。创建新页面是一件轻而易举的事，为转换而优化也更容易。**

使用单一模板可能不是组织内容的最佳方式，尤其是当您想要快速测试新的更改时。这就是为什么**基于组件的模块化设计为您提供了更大的灵活性。**

**它降低了开发和维护成本。**网站是产生商业价值的工具。你构建的系统越好，它在没有任何代码变化的情况下持续的时间就越长。

# Netlify CMS 的灵活内容—配置

代码示例假设使用手动初始化，[查看这篇关于 DRY Netlify CMS 配置的文章](https://mrkaluzny.com/blog/dry-netlify-cms-config-with-manual-initialization/)以了解更多信息。

我强烈建议使用这种配置来代替标准配置，以获得最佳的开发体验。手动初始化利用了 javascript 文件而不是 YAML，这使得长期维护更加容易。

您可以查看我的[Gatsby started called Henlo](https://github.com/clean-commit/gatsby-starter-henlo)来查看该配置的示例，并将其作为起点。

对于每个灵活的内容项(在本文中我称之为部分),我们需要 2 个文件。一个 JSX 文件渲染部分(我倾向于放置它们)。/src/Sections '文件夹)和 CMS 的配置文件('。/src/cms/sections/'文件夹)。

# 准备 Netlify CMS 配置

首先，我们必须为集合设置一个配置，我们将使用它来创建包含节的页面。

```
*import* seo *from* '@/cms/partials/seo'
*import* hero *from* '@/cms/sections/hero'
...*const* services = {
  name: 'pages',
  label: 'Pages',
  editor: {
    preview: false,
  },
  folder: 'content/pages',
  slug: '{{slug}}',
  create: true,
  fields: [
	  {
		  label: 'Layout'
		  name: 'layout'
		  widget: 'hidden',
		  *default*: 'page',
	  }
    {
      label: 'Title',
      name: 'title',
      widget: 'string',
      required: true,
    },
    {
      label: 'Slug',
      name: 'slug',
      widget: 'string',
      required: true,
    },
    {
      label: 'Sections',
      name: 'sections',
      widget: 'list',
      types: [
        hero,
        ...
      ],
    },
    seo,
  ],
}*export* *default* services
```

在这个例子中，我使用一个 javascript 文件为 Netlify CMS 生成一个集合。查看这篇[关于 Netlify CMS 配置](https://mrkaluzny.com/blog/dry-netlify-cms-config-with-manual-initialization/)的文章，了解为什么它比 YAML 文件更好。

要使用的最重要的字段是`layout`。我使用它们来传递用于呈现该内容类型的模板文件的名称。

正如你所看到的，我添加了 2 个部分——SEO 部分，处理 SEO 内容和英雄部分。将这些字段分离到不同的文件中，可以更容易地维护组件并在整个项目中重用它们。

这里是英雄部分的配置示例。

```
*const* hero = {
  label: 'Hero',
  name: 'hero',
  widget: 'object',
  collapsed: false,
  fields: [
    {
      label: 'Title',
      name: 'title',
      widget: 'string',
      required: false,
    },
    {
      label: 'Subtitle',
      name: 'subtitle',
      widget: 'string',
      required: false,
    },
    {
      label: 'Content',
      name: 'content',
      widget: 'markdown',
      required: false,
    },
  ],
}*export* *default* hero
```

现在我们已经有了 Netlify CMS 的初始配置，我们可以开始从我们的集合生成页面了。

# 通过 Gatsby & Netlify 使用灵活的内容生成页面

另一个要遵循的良好实践是利用父组件来管理我们的部分。这样，您可以向其他模板或页面添加部分。

# 创建截面生成器组件

组件的想法非常简单。我最近与 Prismic 合作的一个项目给了我灵感，[这个组件模仿 SliceZone 组件](https://prismic.io/docs/technologies/tutorial-3-create-homepage-slices-gatsby#4.-update-the-slicezone-component)。

添加新的部分就像导入组件并将其与部分类型(Netlify CMS 配置中的对象名称)匹配一样简单。

```
*import* React *from* 'react'
*import* { graphql } *from* 'gatsby'*import* Hero *from* '@/Sections/Hero'*export* *default* *function* SectionsGenerator({ sections }) {
	*const* sectionsComponents = {
		hero: Hero
	}

	*const* sectionsContent = sections.map((section, key) => {
		*const* Section = sectionsComponents[section.type]
		*if* (Section) {
			*return* <Section key={key} data={section} />
		}
		*return* <div>{section.type}</div>
	})

	*return* (
		<>
			{sectionsContent}
		</>
	)
}*export* *const* query = graphql`
  fragment Sections on MarkdownRemarkFrontmatter {
    sections {
      id
      type
      title
      subtitle
      content
  }
}
`
```

此外，我建议在同一个文件中创建一个 graphql 片段。通过一次导入，我们可以将数据传递给查询，并将部分呈现给项目中的任何模板页面(请参见下一个代码示例)

# 准备呈现页面的模板

我们的模板必须做一件事——获取部分数据并将它们作为道具传递给`SectionsGenerator`组件。

使用这种方法，还可以对每个页面使用单一布局。使用`useStaticQuery`钩子可以给每个部分添加额外的数据。

```
*import* React *from* 'react'
*import* { graphql } *from* 'gatsby'*import* Layout *from* '@/components/Layout'
*import* SectionsGenerator *from* '@/components/SectionsGenerator'*import* SEO *from* '@/components/SEO/Seo'*const* SectionPageTemplate = ({ data }) => {
  *const* sections = data.frontmatter.sections
  *return* (
    <>
      <SectionsGenerator sections={sections} />
    </>
  )
}*const* LandingPage = ({ data }) => {
  *return* (
    <Layout hideNav={true}>
      <SEO data={data.page.frontmatter.seo} />
      <SectionPageTemplate data={data.page} />
    </Layout>
  )
}*export* *default* SectionPage*export* *const* sectionsPageQuery = graphql`
  query SectionPage($id: String!) {
    page: markdownRemark(id: { eq: $id }) {
      id
      fields {
        slug
      }
      html
      frontmatter {
        title
        ...Sections
        ...SEO
      }
    }
  }
`
```

通过编写一个片段，无论项目支持多少个部分，我们的查询都会非常短。

# 配置 Gatsby-node 以使用具有灵活内容的 Netlify CMS

所有组件准备就绪后，我们可以继续进行`gatsby-node`配置。

```
exports.createPages = ({ actions, graphql }) => {
  *const* { createPage } = actions *return* graphql(`
    {
      allMarkdownRemark(limit: 1000) {
        edges {
          node {
            id
            fields {
              slug
            }
            frontmatter {
              layout
              title
              slug
            }
          }
        }
      }
    }
  `).then((result) => {
    *if* (result.errors) {
      result.errors.forEach((e) => console.error(e.toString()))
      // return Promise.reject(result.errors);
    } // Filter out the footer, navbar, and meetups so we don't create pages for those
    *const* postOrPage = result.data.allMarkdownRemark.edges.filter((edge) => {
	    *let* layout = edge.node.frontmatter.layout
	    *return* layout == *null* || layout == 'hidden'
    }) postOrPage.forEach((edge) => {
      *const* id = edge.node.id
      *let* component = path.resolve(
        `src/templates/${String(edge.node.frontmatter.layout)}.js`,
      )
      *if* (fs.existsSync(component)) {
        *switch* (edge.node.frontmatter.layout) {
          *case* 'page':
            createPage({
              path: `/${Helper.slugify(edge.node.frontmatter.slug)}/`,
              component,
              context: {
                id,
              },
            })
            *break*
            ...
        }
      }
    })
  })
}
```

为了生成正确的 slug，我们必须利用添加到集合中每个页面的 slug 字段。通过这种方式，编辑可以更新 URL 来创建许多页面，甚至是有层次结构的页面(尽管这不会反映在 Netlify CMS 的 UI 中)。

在我的项目中，我倾向于在 URL 中使用斜杠，这有助于避免 Gatsby 和 Netlify 的一些 SEO 优化问题。

我用一个助手来清理网页的网址，并确保它不会引起任何问题。

# 请注意这些问题

有一个问题，如果我们试图创建一个页面并调用一个不存在的元素，页面生成将会失败。为什么？

**Gatsby 根据我们提供的内容推断字段的类型。**如果该字段的内容不存在，则构建过程失败。为了避免这个问题，我们必须让盖茨比知道会发生什么。

我们这样做，但是在`gatsby-node.js`文件中定义类型。这是我在为 Clean Commit 网站设计新的登陆页面时写的一个例子。

```
exports.createSchemaCustomization = ({ actions }) => {
  actions.createTypes(`
    type Button {
      text: String
      link: String
    } type List {
      title: String
      content: String
    } type Form {
      provider: String
      title: String
      formid: Int
      redirect: String
      button: String
    } type FAQ {
      question: String
      answer: String
    } type MarkdownRemarkFrontmatterSections @infer {
      id: String
      type: String
      subheader: String
      title: String
      subtitle: String
      background: String
      content: String
      variant: String
      video: String
      bulletpoints: [String]
      secondarycontent: String
      button: Button
      list: [List]
      form: Form
      faqs: [FAQ]
    }
  `)
}
```

我们准备了 17 个不同的部分，我们的团队可以用它们来创建新的登录页面和服务。有了定义的类型，我们可以安全地部署网站，在构建过程中不会出现任何问题。

# 部分字段命名

使用 Netlify CMS 创建灵活的内容体验不同于其他任何无头 CMS。此时，没有办法只查询一个部分的内容。这就是为什么字段的命名约定必须一致(否则您将花费大量时间编写自定义类型定义)。

这就是为什么在多个部分中重用相同的名称并尽可能保持一致非常重要。对于 Clean Commit 的登录页面，几乎每个部分都使用`title`、`content`、`subheader`和`button`字段。所以在你做项目的时候要记住这一点！

如果你想了解这个解决方案是如何工作的，你可以创建什么，看看干净提交服务页面，如[网站开发](https://cleancommit.io/services/website-development/)、[应用程序开发](https://cleancommit.io/services/app-development/)或[前端开发](https://cleancommit.io/services/frontend-development/)。

与我的团队一起，我们为 Netlify CMS 创建(并积极维护)了我们自己的 [Gatsby Starter，名为 Henlo](https://github.com/clean-commit/gatsby-starter-henlo)——来看看吧，给我们一些爱！

# 如何在 Netlify CMS 中创建灵活的内容字段

*   使用手动初始化使配置文件管理更容易。
*   利用列表小部件和使用类型选项
*   创建一个组件来呈现每个部分组件
*   将该组件添加到您想要使用它模板中
*   定义节中使用的字段类型，以避免 Gatsby 中类型推断的构建错误