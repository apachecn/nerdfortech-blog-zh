# Sanity 将 Transifex 作为插件加入

> 原文：<https://medium.com/nerd-for-tech/sanity-joins-transifex-as-a-plugin-3dc1f948ac31?source=collection_archive---------20----------------------->

![](img/d54d02da81f3432d367fd258f28c1ebc.png)

[Sanity](https://www.sanity.io/) 作为世界上最好的内容平台之一而闻名，现在您可以通过 Transifex 使用它！

这里是你需要知道的关于 Sanity-Transifex 插件的所有内容，如何使用它，以及如何安装它！

# 什么是理智

Sanity 是结构化内容的平台，让您可以构建更好的数字体验。他们将内容视为数据，这意味着您可以使用他们的 API 来构建最佳工作流，并在系统之间共享内容以提高速度。

Sanity 附带了一个名为 [Sanity Studio](https://www.sanity.io/studio) 的开源编辑环境，以及一个名为 [Content Lake](https://www.sanity.io/docs/datastore) 的实时数据库。您可以使用他们的平台来支持电子商务网站、营销网站、产品、移动应用等。由于它们是基于结构化内容构建的，您可以轻松地跨应用程序、设备和渠道发布和重用您的内容。

如果你有兴趣了解更多，请随意查看 Sanity 的网站。

因此，Sanity 专注于内容管理，而 Transifex 专注于本地化。使用这两个工具可能是天作之合，这就是 Sanity 插件的用武之地！

# 健全插件

这个插件允许你将文档发送到 Transifex，然后在文档完成时取回它们，或者如果你想的话，甚至在文档没有完成时取回它们——只需按一个按钮。

由于这一点，你将不必花费额外的时间在两个平台之间下载和上传文件。

一切都发生在健全工作室内。为了使这一过程更容易，你可以为你的“待翻译”文档设置一个专门的标签。

此外，您还可以获得可定制的 HTML 和文档修补工具，以及用于 [Transifex API](https://docs.transifex.com/api-3-0/introduction-to-api-3-0) 的可定制适配器。

# 如何安装

要安装健全插件:

1.  转到您的 Sanity Studio 文件夹
2.  运行“sanity install sanity-plugin-transifex”
3.  创建一个文档，其中包含您的 Transifex 的组织名称、项目名称和一个具有适当访问级别的 API 令牌(这是为了对您的 Transifex 帐户进行合理的访问)
4.  回到工作室，创建“populateTransifexSecrets.js”

现在，您希望打开该文件并以这种方式插入您的值:一旦完成:

```
import sanityClient from 'part:@sanity/base/client'const client = sanityClient.withConfig({ apiVersion: '2021-03-25' })client.createOrReplace({
_id: 'transifex.secrets',
_type: 'transifexSettings',
organization: 'YOUR_ORG_HERE',
project: 'YOUR_PROJECT_HERE',
token: 'YOUR_TOKEN_HERE',
})
```

打开命令行并运行“sanity exec populatestransifexsecrets . js-with-user-token”。您可以通过使用 Vision 查询*[_id == 'transifex.secrets']来验证它是否有效。如果您有多个数据库，那么您必须对所有数据库都这样做。

一旦确认一切正常，就删除“populateTransifexSecrets.js”文件。文档的 id 不会暴露给外界。但是如果这让你担心，你可以继续使用 [Sanity 的基于角色的访问控制](https://www.sanity.io/docs/access-control)来控制对这条路径的访问。

最后，但同样重要的是，使用[桌面结构](https://www.sanity.io/docs/structure-builder-introduction)来获取您的文档类型的 Transifex 选项卡。一个典型的例子是这样的:

```
import S from '@sanity/desk-tool/structure-builder'
//...your other desk structure imports...
import { TranslationTab, defaultDocumentLevelConfig, defaultFieldLevelConfig } from 'sanity-plugin-transifex' export const getDefaultDocumentNode = (props) => {
  if (props.schemaType === 'myTranslatableDocumentType') {
    return S.document().views([
      S.view.form(),
      //...my other views -- for example, live preview, the i18n plugin, etc.,
      S.view.component(TranslationTab).title('Transifex').options(
        defaultDocumentLevelConfig  
      )
    ])
  }
  return S.document();
};
```

仅此而已。如果你有任何问题或者你想知道更多的细节，请随时参考[指南](https://www.sanity.io/plugins/sanity-plugin-transifex) [或者 GitHub 回购。](https://github.com/sanity-io/sanity-plugin-transifex)

[原帖发表在本页。](https://www.transifex.com/blog/2021/sanity-joins-transifex-as-an-optional-plugin/)