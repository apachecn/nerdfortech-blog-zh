# 如何用本地 Docker Postgres 容器设置 Prisma

> 原文：<https://medium.com/nerd-for-tech/how-to-set-up-prisma-with-a-local-docker-postgres-container-9e0958d08544?source=collection_archive---------1----------------------->

## 这是一个关于如何将 Docker Postgres 容器连接到 Prisma 模式设置的简单指南。这不是棱镜教程。

# 步骤 1:创建 Docker Postgres 实例

这里你可以创建一个`compose.yml`文件或者直接运行`docker run`命令。

## 码头运行

`docker run -d -e POSTGRES_DB=mydb -e POSTGRES_PASSWORD=testpass123 -e POSTGRES_USER=postgres -p “6500:5432” postgres`

这将启动一个分离的`postgres:latest`容器，其中包含:

*   `POSTGRES_DB=mydb`
*   `POSTGRES_PASSWORD=testpass123`
*   `POSTGRES_USER=postgres`
*   端口范围:`6500:5432`

请注意端口范围与通常的 5432:5432 有什么不同。这是因为本地主机上的端口有时已经被其他服务占用了。通过手动将其设置为一些较少使用的值，您**减少了冲突的风险**。

# 第二步:现在我们来看看棱镜`DATABASE_URL`

## 结构

`DATABASE_URL=”postgresql://{**POSTGRES_USER**}:{**POSTGRES_PASSWORD**}@localhost:{**PORT**}/{**POSTGRES_DB**}?schema=public”`

## 实际的

`DATABASE_URL=”postgresql://**postgres**:**testpass123**@localhost:**6500**/mydb?schema=public”`

## Docker 撰写示例

或者，下面是如何编写一个`compose.yml`文件。(`docker-compose.yml`是否计划弃用)

```
services:
  postgres:
    container_name: postgres
    image: postgres
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=mydb
    ports:
      - 6500:5432
    volumes:
      - postgres_data:/var/lib/postgresql/data/

volumes:
  postgres_data:
```

我添加了一个`postgres_data`卷来保存数据。最初我纯粹是出于开发和测试的目的使用它，所以我觉得没有必要。

如果您需要在使用带有`docker run`指令的独立容器时复制这种行为，只需使用以下语法添加`-v`标志:

```
-v /your/volume/path:/var/run/postgresql
```

当您声明本地卷的路径时，请确保它存在，否则将会引发错误。

# 步骤 3:检查 Prisma 是否成功连接到您的 Postgres 实例

## **运行**

`npx prisma db pull`

## 期望成果

![](img/2ea3cc493cb321914e57c3fddd52a085.png)