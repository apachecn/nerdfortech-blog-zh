# 将服务器端应用部署到 Azure

> 原文：<https://medium.com/nerd-for-tech/deploying-a-server-side-app-to-azure-dba0ae99532b?source=collection_archive---------26----------------------->

![](img/77f240a51c99412d00408b0bf82d6a5c.png)

照片由[弗洛里安·克拉姆](https://unsplash.com/@floriankrumm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

对于我上学期间的最后一个项目，我的任务是将其部署到云，特别是 Azure。现在，虽然微软有使用他们服务的 OK 文档，但他们很难适应我选择的项目，这是一个利用 Sequelize ORM 的 ExpressJS SSR 应用程序。

所以我在这里写了一个关于如何部署这样一个应用程序的简短教程。

当你初始化一个 sequelize 应用程序时，你会得到一些文件夹、配置、模型和种子。我们要做的第一件事是修改我们的配置。默认情况下，我们的配置如下所示:

```
{
  "development": {
    "username": "root",
    "password": null,
    "database": "database_development",
    "host": "127.0.0.1",
    "dialect": "mysql"
  },
  "test": {
    "username": "root",
    "password": null,
    "database": "database_test",
    "host": "127.0.0.1",
    "dialect": "mysql"
  },
  "production": {
    "username": "root",
    "password": null,
    "database": "database_production",
    "host": "127.0.0.1",
    "dialect": "mysql"
  }
}
```

这被导入到在模型文件夹中创建的`index.js`文件中。要将 Sequelize 与 Azure 一起使用，这将不起作用。为什么？因为不像在你的开发环境中，你用 dotenv(或者应该用 dotenv)获取你的信息，你将调用 Azure KeyVault 来获取秘密形式的密码。

当您执行一个 Sequelize 操作时，所有的魔法实际上都发生在我前面提到的`index.js`文件中。第一个序列调用我们的`config.js`文件，以及它需要导入的其他东西。当 Sequelize 调用`config.js`文件时，应用程序需要从保险库中调出秘密，否则需要秘密的字段将被未定义地返回，Sequelize 将无法工作。为此，需要一个异步 await 函数。也许有一个我没有想到的替代方案，但是我已经尝试了一些更简单的方法，它们都不起作用。

让我们重新配置我们的配置文件。首先，如果你还没有安装`@azure/identity and @azure/keyvault-secrets`，你需要安装它。最后，我是这样写我的 config.js 的:

```
const { DefaultAzureCredential } = require('@azure/identity');
const { SecretClient } = require('@azure/keyvault-secrets');

const credential = new DefaultAzureCredential();
const vault = 'vault';

const url = `https://${vault}.vault.azure.net`;

const client = new SecretClient(url, credential);

const vaultUtility = async () => {
     try {
          const database = await client.getSecret('db_title');
          const username = await client.getSecret('db_user');
          const password = await client.getSecret('db_pass');

          return {
               development: {
                    username: username.value,
                    password: password.value,
                    database: database.value,
                    host: '{servername}.database.windows.net', // replace {servernmame} with name of db servername
                    dialect: 'mssql',
                    encrypt: 'true'
               },
               test: {
                    username: username.value,
                    password: password.value,
                    database: database.value,
                    host: '{servername}.database.windows.net',
                    dialect: 'mssql',
                    encrypt: 'true'
               },
               production: {
                    username: username.value,
                    password: password.value,
                    database: database.value,
                    host: '{servername}.database.windows.net',
                    dialect: 'mssql',
                    encrypt: 'true'
               },
          };
     } catch (err) {
          ***console***.log(err);
     }
};

module.exports = vaultUtility;
```

最终结果是一样的，它按照 Sequelize 的预期返回 config 对象，但这一次只是在它检索到机密之后。然而，这并没有完全解决我们的问题！现在，我想我们可以自动调用我们的函数，并将数据存储在一个变量中，然后我们可以导入该变量，但这有一个小小的问题…它将您的秘密暴露在代码中的时间太长了，使整个密钥库变得毫无意义。因此，我们应该只在需要的时候调用和存储秘密，在不需要的时候丢弃它。

经过几个小时的争论和研究，我想到了在一个匿名异步函数中存储除 db 变量之外的所有内容。

```
'use strict';const ***db*** = {};

(async () => {
    const ***fs*** = require('fs');
    const ***path*** = require('path');
    const Sequelize = require('sequelize');
    const basename = ***path***.basename(__filename);
    const env = ***process***.env.NODE_ENV || 'production';
    const vaultUtility = require('../config/config');
    let config = await vaultUtility();
    config = config[env];

    let sequelize;
    if (config.use_env_variable) {
        sequelize = new Sequelize(***process***.env[config.use_env_variable], config);
    } else {
        sequelize = new Sequelize(
            config.database,
            config.username,
            config.password,
            config
        );
    }

    ***fs***.readdirSync(__dirname)
        .filter(file => {
            return (
                file.indexOf('.') !== 0 &&
                file !== basename &&
                file.slice(-3) === '.js'
            );
        })
        .forEach(file => {
            const model = require(***path***.join(__dirname, file))(
                sequelize,
                Sequelize.DataTypes
            );
            ***db***[model.name] = model;
        });

    ***Object***.keys(***db***).forEach(modelName => {
        if (***db***[modelName].*associate*) {
            ***db***[modelName].*associate*(***db***);
        }
    });

    ***db***.sequelize = sequelize;
    ***db***.Sequelize = Sequelize;
})()

module.exports = ***db***;
```

现在你的应用程序应该可以和 Azure 一起工作了。如果您使用 Azure 和 sequelize，请告诉我是否有更好/更简单的方法来做到这一点，而不暴露任何不应该暴露的内容。我要指出，这是利用为审判提供的资源完成的。