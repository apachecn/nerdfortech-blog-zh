# GraphQL 和带有 Laravel 8 的搜索引擎

> 原文：<https://medium.com/nerd-for-tech/graphql-and-search-engine-with-laravel-8-eb47d6cf16ca?source=collection_archive---------6----------------------->

我使用 *GraphQL* 作为 REST APIs 的替代品，允许我有一个单一的入口点，并在我需要的时候获取我需要的东西，并有可能浏览我雄辩的关系。这也使我能够大幅减少大量型号的控制器数量。

起点将取决于您的实际环境，就我个人而言，我使用的是 *Laradock* ，这是一个 Docker Compose 栈，提供了许多现成可用的服务。以下内容针对我的工作方式，仅供参考。

```
docker-compose up -d workspace nginx mysql phpmyadmindocker-compose exec workspace bashcomposer create-project laravel/laravel .php artisan migrate // after editing properly my .env to use same mysql credential as Laradock
```

为了制作我们的 *GraphQL* API，我们将使用 nuwave/lighthouse 包，它包含必要的功能和模式实现。

[](https://github.com/nuwave/lighthouse) [## 巨浪/灯塔

### Lighthouse 是一个与您的 Laravel 应用程序集成的 GraphQL 框架。它吸收了两者的最佳思想…

github.com](https://github.com/nuwave/lighthouse) 

我还想添加一个搜索引擎。这是可选的，但我发现通过关键字找到我们的资源真的很有用，我们将使用 *Laravel Scout* 和 *tntsearch* 驱动程序将我们的索引存储到我们应用程序内的 *sqlite* 数据库中，这是一个简单的驱动程序，可能会带来一些麻烦，主要是电子邮件索引，你可以使用其他驱动程序，如 *Algolia，Elastic，TypeSens，MeiliSearch* ...如果你不想要，就忽略 step around *Scout* 和 *tntsearch* 。我真的更喜欢使用索引系统，而不是在代码中的某个地方制造一个难看的 whereLike()链！

 [## 拉勒维尔童子军

### Laravel Scout 提供了一个简单的、基于驱动程序的解决方案，为您的雄辩模型添加全文搜索。使用模型…

laravel.com](https://laravel.com/docs/8.x/scout) [](https://github.com/teamtnt/tntsearch) [## team TNT/TNT 搜索

### TNTSearch 是一个完全用 PHP 编写的全功能全文搜索(FTS)引擎。简单的配置允许您…

github.com](https://github.com/teamtnt/tntsearch) 

```
composer require nuwave/lighthouse laravel/scout teamtnt/laravel-scout-tntsearch-driver
```

一旦我们的包被安装，我们将配置我们的应用程序来使用它们，首先我们想通过编辑 config/app.php 文件将 *Laravel Scout* 和 *tntseach* 添加到我们的服务提供者中:

```
'providers' => [ // default Providers ... Laravel\Scout\ScoutServiceProvider::class,
  TeamTNT\Scout\TNTSearchScoutServiceProvider::class,
]
```

然后我们会发布一些文件:

```
php artisan vendor:publish --provider=”Laravel\Scout\ScoutServiceProvider”php artisan vendor:publish --tag=lighthouse-schema
```

我们将编辑新发布的 config/scout.php，添加 tntsearch 的默认配置:

```
'tntsearch' => [
  'storage'  => storage_path(),
  'fuzziness' => env('TNTSEARCH_FUZZINESS', false),
  'fuzzy' => [
    'prefix_length' => 2,
    'max_expansions' => 50,
    'distance' => 2
  ],
  'asYouType' => false,
  'searchBoolean' => env('TNTSEARCH_BOOLEAN', false),
  'maxDocs' => env('TNTSEARCH_MAX_DOCS', 500),
],
```

而且精确到我们的。env 我们想用它:

```
SCOUT_DRIVER=tntsearch
```

对于其余的，我想偷懒，用我的用户模型工作，我必须像这样编辑它，让 tell Scout 索引一些字段:

```
use Laravel\Scout\Searchable;class User extends Authenticatable
{
  use HasFactory, Notifiable, Searchable; /**
  * Get the indexable data array for the model.
  *
  * @return array
  */
  public function toSearchableArray() : array
  {
     return [
       'id' => $this->id,
       'name' => $this->name,
       'email' => $this->email
     ];
  } // rest of User Model
```

Laravel 已经为这个模型提供了一个工厂(database/factories/UserFactory)，所以我可以直接用 Laravel Tinker 生成一百个假用户:

```
php artisan tinker>> User::factory()->count(100)->create() // This will store 100 User in my databasephp artisan tntsearch:import App\\Models\\User
```

所以，现在我们已经实现了 *GraphQL* 并处理了一些数据。我们可以开始尝试这个了！对于它，我用的是 altar[https://addons . Mozilla . org/fr/Firefox/addon/Altair-graph QL-client/](https://addons.mozilla.org/fr/firefox/addon/altair-graphql-client/)，Mozilla Firefox 的一个扩展，随便用你喜欢的客户端。注意，Laravel 的游乐场是存在的【https://github.com/graphql/graphql-playground 但是我不喜欢在我实现 GraphQL 的每个项目上都安装它，我更喜欢使用一个独特的外部工具。

```
POST [http://localhost/graphql](http://localhost/graphql){
   users {
     data {
       name
       email 
     }
     paginatorInfo {
       currentPage 
       hasMorePages
       lastPage
     }
   }
}
```

它将返回如下内容:

```
{
  "data": {
    "users": {
      "data": [
        {
          "name": "Dr. Alan Mitchell",
          "email": "[pietro46@example.org](mailto:pietro46@example.org)"
        },
        ...
      ],
      "paginatorInfo": {
        "currentPage": 1,
        "hasMorePages": true,
        "lastPage": 10
      }
    }  
  }
}
```

为了添加我们的搜索引擎，我们必须编辑我们的 graphql 模式(默认情况下在。/graphl/schema.graphql)将“搜索”指令添加到用户查询中:

```
type Query {
  users(search: String @search): [User!]! @paginate(defaultCount: 10)
}
```

并尝试它(双引号是强制性的，没有单引号！):

```
POST [http://localhost/graphql](http://localhost/graphql){
   users(search: "Alan") {
     data {
       name
       email 
     }
     paginatorInfo {
       currentPage 
       hasMorePages
       lastPage
     }
   }
}
```

这将返回这个名称或电子邮件中包含 Alan 的所有用户，这取决于在 User::toSearchableArray()方法中确定的索引！

就是这样，这是构建我们的 GraphQL API 的一个非常有效的起点！如果你真的不了解 GraphQL，除了想了解更多，看看 php lighthouse 文档:[https://light house-PHP . com/master/API-reference/directives . html](https://lighthouse-php.com/master/api-reference/directives.html)。