# Nodejs GraphQl 认证与 JWT，阿波罗服务器，MySql 和 Sequelize ORM。

> 原文：<https://medium.com/nerd-for-tech/nodejs-graphql-authentication-with-jwt-apollo-server-mysql-and-sequelize-orm-21cf1503f7f?source=collection_archive---------4----------------------->

![](img/f057f3f8e1857c0bd123465dd1aa805d.png)

来自 [Pexels](https://www.pexels.com/photo/woman-writing-on-whiteboard-3861943/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [ThisIsEngineering](https://www.pexels.com/@thisisengineering?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 摄影

在过去的几年里，我们见证了微服务架构在一个完全不同的层面上的发展。它专注于开发软件系统，这些软件系统试图专注于构建具有良好定义的接口和操作的单一功能模块。与此同时，我们也看到了敏捷、Devops 和 API 的大规模增长。直到几年前，back REST APIs 还是行业标准和热门话题，但在 2015 年，脸书推出了 GraphQL，并在 2018 年发布了它的第一个稳定版本。我不会深入探讨它，但这里有两篇关于为什么使用 Graphql 的文章。[【1】](https://www.prisma.io/blog/top-5-reasons-to-use-graphql-b60cfa683511)[【2】](/@ajaysaini.official/why-graphql-886ba866ae75#:~:text=Here%2C%20Facebook%20team%20started%20to,to%20ask%20what%20they%20need.)

GitHub 库:-[https://github.com/foxy17/GraphQl-Authentication](https://github.com/foxy17/GraphQl-Authentication)

你可以复制这个并按照我的解释去做。

在本文中，我们将关注使用 JWT 令牌的本地认证。对于数据库，您可以使用任何 MySql 数据库。Apollo-server 是一个开源的 GraphQL 服务器，兼容任何种类的 GraphQL 客户端。我将使用 apollo 来公开 API，而不是 express。

我们将进行一个简单的身份验证，其中用户将有一个名字，姓氏，电子邮件，密码，公司和唯一的雇员 Id。Company 将存储在另一个表中，这样我们就可以探索与 GraphQL 的提取关联。让我们首先安装必要的软件包:

> NPM I Apollo-server bcrpytjs dotenv JSON web token sequelize MySQL 2 graph QL
> 
> npm i -D 序列-cli 节点

```
const getUser = token => {
    try {
        if (token) {
            return jwt.verify(token, JWT_SECRET)
        }
        return null
    } catch (error) {
        return null
    }
}
```

这是导入后的第一行，这是我们如何定义 JWT 中间件的，它将验证我们的 JWT 令牌是否有效。

```
const server = new ApolloServer({
    typeDefs,
    resolvers,
    context: ({ req }) => {
        const token = req.get('Authorization') || ''
        return { user: getUser(token.replace('Bearer', ''))}
    },
    introspection: true,
    playground: true
})
server.listen({ port: PORT || 8080 }).then(({ url }) => {
    console.log(`🚀 Server ready at ${url}`);
  });
```

在此之后，我们定义我们的 Apollo 服务器，我们必须传递一个包含以下内容的对象:

typeDefs:这是 graphQL API 的模式，它定义了我们可以在 API 上调用的查询和变化。

*解析器:这些是负责为各个 API 调用返回结果的函数。*

*上下文:是一个特定执行的所有解析器共享的对象。在这里，我们从标头中检索 JWT 令牌，并运行我们之前定义的 getUser 函数来检查它是否有效，并将结果存储在任何解析器都可以访问的用户变量中。*

*自省:它定义了我们是否可以查询模式，以获得关于它支持哪些查询及其结构的信息。(通常在生产中是错误的)*

*playground:是一个图形化的、交互式的浏览器内****graph QL****IDE，我们可以用它来运行查询。*

让我们检查我们的类型定义或模式。

```
const typeDefs = gql`
    input Pagination {
        page: Int!
        items: Int!
    }
    input UserFilter {
        employeeId: Int
        firstName: String
        lastName: String
    }
    type User {
        employeeId: Int!
        firstName: String!
        lastName: String!
        password: String!
        email: String!
        company: String!
    }
    type AuthPayload {
        token: String!
        user: User!
    }
    type Query {
        getUserList(search:UserFilter, pagination:Pagination, sort:String): [User]
    }
    type Mutation {
        registerUser(firstName: String!, lastName: String!, employeeId: Int!, email: String!, password: String!, company: String!): AuthPayload!
        login (email: String!, password: String!): AuthPayload!
    }
` 
```

`gql`模板文字标签可以用来简洁地编写一个 GraphQL 查询，该查询被解析成一个标准的 GraphQL [AST](https://stackoverflow.com/questions/46163036/what-is-ast-in-graphql/46164403) 。`type`用其参数定义一个对象。`!`标记表示参数是强制的，不能是未定义的或空的。有两种截然不同的类型，**查询**和**突变**。简单来说**查询**是选择语句**变异**是插入操作。

![](img/6252dbea40e99a3bca1920509a35459b.png)

分解变异

除了标量类型`String`、`Int`、`Float`、`Boolean`和`ID`之外，我们可以将它们作为一种类型直接分配给自变量或参数，也可以将我们自己定义的复杂类型作为输入。为此，我们使用输入标签。`UserFilter`输入是一个定制输入，它被传递以获取用户列表查询。`[User]`意味着将返回一个用户类型的数组。

所有这些都是 GraphQL 的主要外壳，现在剩下的是数据库模型，它将根据您的数据库选择和解析器函数而变化，就像您为特定路径上的 REST API 定义的函数一样。让我们看看顺序模型。

```
//User.js
module.exports = (sequelize, DataTypes) => {
    const User = sequelize.define('User', {
        firstName: { type: DataTypes.STRING, allowNull: true },
        lastName: { type: DataTypes.STRING, allowNull: true },
        email: { type: DataTypes.STRING, allowNull: false, unique: true },
        password: {type: DataTypes.STRING,allowNull: false},
        employeeId:{ type: DataTypes.INTEGER, allowNull: false, primaryKey: true, unique: true },
    }, {timestamps: false,
        hooks: {
            beforeCreate: async (user) => {
             if (user.password) {
              const salt = await bcrypt.genSaltSync(10, 'a');
              user.password = bcrypt.hashSync(user.password, salt);
             }
            },
            beforeUpdate:async (user) => {
             if (user.password) {
              const salt = await bcrypt.genSaltSync(10, 'a');
              user.password = bcrypt.hashSync(user.password, salt);
             }
            }
           }
    });
    User.associate = function (models) {
        User.hasOne(models.Company, { foreignKey: "employeeId" });
      };
    User.validPassword = async (password, hash) => {
        return await bcrypt.compareSync(password, hash);
       }
    return User;
  };
//Company.js
module.exports = (sequelize, DataTypes) => {
    const Company = sequelize.define('Company', {
        company: {type: DataTypes.STRING,allowNull: false},
        employeeId:{ type: DataTypes.INTEGER, allowNull: false, primaryKey: true, unique: true },
    }, {
      timestamps: false,
      freezeTableName: true,
    });
    Company.associate = function (models) {
        Company.belongsTo(models.User, { foreignKey: "employeeId" });
      };
    return Company;
  };
```

`beforeCreate`是在调用创建查询时调用的钩子。钩子包含用盐散列密码的逻辑，这样我们就不会在数据库中存储未加密的密码。`beforeUpdate`当在用户表上调用更新查询时，调用这个钩子。就像之前一样，它散列更新后的密码。`User.validPassword` 是一个用户加密的类方法，用来比较存储在数据库中的散列和一个字符串，以检查两者是否相同。`User.associate`是与带有 employeeId 外键的公司表的一对一关联。`Timestamp:false`默认情况下，sequelize 在 SQL 表中包含一个 createdAt 和 updateAt 记录，但这会将其设置为 false。默认情况下，sequelize 使表名成为复数，这会导致错误，除非我们默认这样设置它们。因为我没有这样做，`freezeTableName`帮助我保持表名与我定义的完全一致，并且没有将**用户更改为用户**或者将**公司更改为公司**。Index.js 只是连接数据库的默认序列文件。它还获取 models 文件夹中定义的所有模型，并将它们应用于“db”对象。

```
const resolvers = {
    Query: {
        async getUserList(root, args, { user }) {
            try {
                if(!user) throw new Error('You are not authenticated!')
                const {search,pagination,sort} =args;
                var query={
                    offset:0,
                    limit:5,
                    raw: true,
                    //this is done to flaten out the join command
                    attributes: ['firstName','lastName','email','employeeId','Company.company',],
                    include: [{ model: models.Company,attributes:[]}]
                    }
                    //by defaults query is paginated to limit 5 items
                if(pagination){
                    query.limit=pagination.items;
                    query.offset=pagination.items*(pagination.page-1)
                }
                if(search){
                    query.where={
                        [Op.or]: [
                            search.firstName?{ firstName: search.firstName }:null,
                            search.lastName?{ lastName: search.lastName}:null,
                            search.employeeId?{ employeeId: search.employeeId}:null
                        ] 
                    }
                }
                if(sort){
                    query.order= [
                        [sort, 'ASC'],
                    ];
                }
                return await models.User.findAll(query);
            } catch (error) {
                throw new Error(error.message)
            }
        }
    },

    Mutation: {
        async registerUser(root, { firstName, lastName, email, password, employeeId,company }) {
            try {
                const userCheck = await models.User.findOne({ 
                    where: { 
                        [Op.or]: [
                            { email: email },
                            { employeeId: employeeId }
                    ] 
                }})
                if (userCheck) {
                    throw new Error('Email or Employee id already exists')
                }
                const user = await models.User.create({
                    firstName,
                    lastName,
                    employeeId,
                    email,
                    password
                })
                const companyModel = await models.Company.create({
                    employeeId,
                    company
                })
                const token = jsonwebtoken.sign(
                    { employeeId: user.employeeId, email: user.email},
                    process.env.JWT_SECRET,
                    { expiresIn: '1y' }
                )
                let createdUser={
                    company:companyModel.company,
                    employeeId: user.employeeId,
                    firstName: user.firstName, 
                    lastName: user.lastName, 
                    email: user.email
                }

                return {
                    token, user:createdUser, message: "Registration succesfull"
                }
            } catch (error) {
                throw new Error(error.message)
            }
        },

        async login(_, { email, password }) {
            try {
                const user = await models.User.findOne({ where: { email }})

                if (!user) {
                    throw new Error('No user with that email')
                }
                const isValid = await models.User.validPassword(password, user.password)
                if (!isValid) {
                    throw new Error('Incorrect password')
                }

                // return jwt
                const token = jsonwebtoken.sign(
                    { employeeId: user.employeeId, email: user.email},
                    process.env.JWT_SECRET,
                    { expiresIn: '1d'}
                )

                return {
                   token, user
                }
            } catch (error) {
                throw new Error(error.message)
            }
        }

    },
}
```

解析器包含用于相应查询和变异的函数。他们接受 4 个论点

1.  `root`包含从父字段的解析器返回的结果。
2.  `args`参数传递到查询中的字段。
3.  `context`特定查询中所有解析器共享的对象。
4.  `info`包含关于查询的执行状态的信息。

`getUserList`中的`query`对象是一个动态对象，它根据传递给查询的参数改变值。所有参数都是可选的。所有查询都需要带有有效 jwt 令牌的授权头。这正由 `if(!user) throw new Error(‘You are not authenticated!’)`验证

这是从我们之前在 server.js 中传递的上下文中获取的用户变量。如果我们不希望路由被验证，我们只需删除这一行。让我们解释一下基本的查询。`offset` 和`limit`是分页参数。`raw`用于返回一个 JSON 对象，而不是一个 sequelize 对象，这样更容易被 parase。属性让我们定义希望从 SQL 返回哪些列。包括我们如何在公司和用户表之间应用连接，这样我们就不能获取特定用户的公司名称。您会注意到，我们已经将 include 的属性设置为空。这意味着尽管它们将在查询中返回，但不会显示。如果返回`{Company.company:"name",Company.employeeId:2}`，它们看起来会像这样，并且当我们试图使用 graphQL 模式对其进行并行化时，会抛出一个错误，因为我们已经将用户定义为拥有 company key，而不是 Company.company 作为键。因此，为了解决这个问题，我们选择`’Company.company’`作为映射到公司的用户属性。

记住，如果你使用 query，你需要像这样在头中传递 JWT 令牌。该令牌在登录或注册时返回。

`{
“Authorization”:”eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbXBsb3llZUlkIjoyLCJlbWFpbCI6ImJAZ21haWwuY29tIiwiaWF0IjoxNjIyNTMwNTAyLCJleHAiOjE2MjI2MTY5MDJ9.3qQOHPzhKOjM6r5JNRWoMsvyt2IzwX8aa7Bj7s1zEZw”
}`

谢谢你坚持和我在一起，如果你喜欢这个并且想看更多的东西，你可以 **star** 或者**fork**GitHub repo。

![](img/0027d2d61204fa3e1060b194093e2221.png)![](img/cb9a430be380e286e47c5d31f3ecf142.png)![](img/c13a42bdf58f9fe3744e116caa610c4a.png)