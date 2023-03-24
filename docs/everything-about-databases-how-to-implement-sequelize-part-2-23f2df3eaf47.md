# 关于数据库的一切——如何实现 Sequelize？[第二部分]

> 原文：<https://medium.com/nerd-for-tech/everything-about-databases-how-to-implement-sequelize-part-2-23f2df3eaf47?source=collection_archive---------10----------------------->

![](img/b064bde1df8f87636838e3047d10c325.png)

# 前一篇文章的总结

感谢 [**Part-1**](https://faraz-faraji.medium.com/everything-about-databases-how-to-choose-part-1-35a7d1a0c9d1) 我们可以决定使用哪个数据库更好，之后我们已经讨论了在我们的操作系统中通过 docker 运行 Postgres。

# **什么是 ORM**

正如我在[中提到的](https://www.instagram.com/p/CQGpSEeD_j6/?utm_source=ig_web_button_share_sheet)我的 Instagram 帖子 ORM 是一个工具，你可以轻松地连接到你的数据库，并获取你想要的数据。获得更容易的数据不仅仅是使用 ORM 的目的，还有，增加安全性(SQL 注入)，清洁代码，避免硬代码，以及许多其他有价值的事情。由于这些原因，我们将继续通过 ORM 使用数据库。

我们有很多针对 node js 的 ORM 工具，比如 [**typeORM**](https://typeorm.io) 和 [**sequelize**](http://sequelize.org) 。

# 熟悉一些术语

## 1-迁移:(模式迁移)

正如[维基百科](https://en.wikipedia.org/wiki/Schema_migration)所解释的:

> 在软件工程中，模式迁移是指对关系数据库模式的增量、可逆更改和版本控制的管理。每当需要将数据库的模式更新或恢复到某个较新或较旧的版本时，都会在数据库上执行模式迁移。

这意味着迁移类似于表的版本控制器(例如 Git)。如果您想要添加一个新列，您可以很容易地将其添加到您的迁移文件中，并且它将自动影响您的表。

没有必要使用迁移，但最好在您的项目中使用。

## 双种子:

假设您创建了一个表来插入您的用户数据，但是您需要其上的数据来编写测试函数。一种方法是手动向表中添加数据。但是另一种方法是使用种子数据来填充您的表。

## 3 种型号:

模型是一种抽象，代表数据库中的一个表。

# 让我们试试顺序

在我们的项目中，我们有许多步骤来用 sequelize 完成数据库。
在本例中，我们将创建迁移、种子和模型文件来实现用户的表。

1-安装 sequelize 和 sequelize-cli 以及您的数据库的依赖项(这里是 Postgres)。在你的项目中初始化 Postgres 结构。3-生成迁移、模型和种子数据 4-在你的项目中使用它

# 1:安装 Sequelize 和 sequelize-cli。

根据 [**之前的帖子**](https://faraz-faraji.medium.com/everything-about-databases-how-to-choose-part-1-35a7d1a0c9d1) ，我们已经通过 docker 在我们的机器上运行了 Postgres，这里安装了我们需要的所有东西，运行下面的代码:

```
mkdir sequelize-test
cd sequelize-test
npm init
npm i sequelize sequelize-cli pg pg-hstore faker
npx sequelize-cli init
```

要了解更多信息并熟悉 CLI 命令，请看一下它们的 [**文档**](https://www.npmjs.com/package/sequelize-cli) 。

# 2:使用。Sequelize 项目中的 env 文件(可选)

在正常模式下，sequelize 不能接受。env 文件自动生成，所以我们必须编辑我们的项目中的一些文件。

首先，转到 Sequelize 生成的**数据库**文件夹，打开**配置**文件夹。然后将文件名改为 config.js，并将这些内容放在上面:

```
require('dotenv').config(); *// this is important!* module.exports = {
    "development": {
        "username": process.env.DB_USERNAME_DEV,
        "password": process.env.DB_PASSWORD_DEV,
        "database": process.env.DB_DATABASE_DEV,
        "host": process.env.DB_HOST_DEV,
        "dialect": "postgres"
    },
    "test": {
        "username": process.env.DB_USERNAME_TEST,
        "password": process.env.DB_PASSWORD_TEST,
        "database": process.env.DB_DATABASE_TEST,
        "host": process.env.DB_HOST_TEST,
        "dialect": "postgres"
    },
    "production": {
        "username": process.env.DB_USERNAME,
        "password": process.env.DB_PASSWORD,
        "database": process.env.DB_DATABASE,
        "host": process.env.DB_HOST,
        "dialect": "postgres"
    }
};
```

然后创建一个文件，命名为**。将这些内容放在上面:**

```
const path = require('path');

module.exports = {
  'config': 'database/config/config.js', //config file path
  'models-path': 'database/models', //model path
  'migrations-path': 'database/migrations', //migration path
  'seeders-path': 'database/seeders' //seeders path
};
```

现在，您的序列化 CLI 将使用您的。env 文件来应用您的更改。

# 3:创建迁移文件

通过以下命令生成迁移文件:

```
npx sequelize migration:create --name name_of_your_migration
```

正如您在 migrations 文件夹中看到的，有一个以时间戳开始的文件。意思是什么时候。您想要运行迁移，CLI 将从较旧的文件运行到较新的文件。

将以下代码放入生成的文件中:

```
'use strict';

module.exports = {
    up: *async* (queryInterface, Sequelize) => {
        *return* queryInterface.createTable('users',
            {
                user_id: {
                    type: Sequelize.INTEGER,
                    autoIncrement: *true*,
                    primaryKey: *true*,
                },
                username: {
                    type: Sequelize.STRING
                },
                email: {
                    type: Sequelize.STRING
                },
                password: {
                    type: Sequelize.STRING
                },                
                createdAt: {
                    type: Sequelize.DATE
                },
                updatedAt: {
                    type: Sequelize.DATE
                },
            }
        );
    },

    down: *async* (queryInterface, Sequelize) => {
        *await* queryInterface.dropTable('users');
    }
};
```

现在您有了一个包含迁移脚本的文件。这意味着无论何时运行迁移命令，您的表都会自动创建。让我们通过这个命令来完成它:

```
npx sequelize db:migrate
```

## 有关更多信息:(更新您的表模式)

假设过一段时间，当您有很多用户时，您决定添加每个客户的昵称。

让我们通过这个命令**创建**另一个**迁移**文件:

```
npx sequelize migration:create --name name_of_your_migration
```

然后将内容更改为:

```
module.exports = {
  up: function(queryInterface, Sequelize) {
    return queryInterface.addColumn(
      'users',
      'nickname',
     Sequelize.STRING
    );

  },

  down: function(queryInterface, Sequelize) {
    // logic for reverting the changes
    return queryInterface.removeColumn(
      'users',
      'nickname'
    );
  }
}
```

并再次运行生成命令:

```
npx sequelize-cli db:migrate
```

您将会看到，昵称列是**添加到您的用户表中的**。你可以随时通过这个命令**撤销**你一个版本接一个版本的修改:

```
npx sequelize db:migrate:undo
```

# 4:创建种子文件

让我们通过创建一个种子文件来开始这个过程:

```
npx sequelize seed:create --name name_of_your_seeds
```

然后将以下代码放入文件中

```
'use strict';

var faker = require("faker");

module.exports = {

    up: (queryInterface, Sequelize) => {

        var newData = [];

        for (let i = 0; i < 10; i++) {
            const seedData = {
                nickname: faker.name.findName(),
                username: faker.internet.username(),
                email: faker.internet.email(),
                password: faker.internet.password(),
                createdAt: new Date(),
                updatedAt: new Date()
            };
            newData.push(seedData);
        }

        return queryInterface.bulkInsert('users', newData);
    },

    down: (queryInterface, Sequelize) => {
        return queryInterface.bulkDelete('users', null, {});
    }
};
```

现在你已经有了关于数据库和序列的所有基本知识

只需运行此命令，通过 seeder 填充您的数据:

```
npx sequelize-cli db:seed:all
```

在下一篇文章中，我将深入解释 Sequelize 以及如何在表中获取和插入数据。

别忘了关注我的 Instagram 页面，获取更新和通知。

[**https://www.instagram.com/p/CQGpSEeD_j6/?UTM _ source = ig _ web _ button _ share _ sheet**](https://www.instagram.com/p/CQGpSEeD_j6/?utm_source=ig_web_copy_link)