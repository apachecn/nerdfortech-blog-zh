# 用 Kotlin 构建 Vertx 应用程序

> 原文：<https://medium.com/nerd-for-tech/building-avertx-application-with-kotlin-1d55cb389f67?source=collection_archive---------8----------------------->

正如前面的帖子中提到的，Eclipse Vertx 通过官方绑定将其 API 扩展到不同的语言，如 Kotlin、Groovy，甚至通过社区支持扩展到 Node/Typescript 和 PHP。

![](img/fe3ecba124cb84b0272f8faaa03f86ab.png)

照片由[本高](https://unsplash.com/@bengao?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/china-landscape?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在这篇文章中，我们将用 Kotlin 语言重新实现以前的 RESTful APIs。

打开浏览器，导航到 [Eclipse Vertx Starter](https://start.vertx.io) ，生成项目框架。不要忘记在*语言*字段中选择 **Kotlin** 。

对于现有项目，将以下依赖项添加到 *pom.xml* 文件中。

```
<dependency>
    <groupId>io.vertx</groupId>
    <artifactId>vertx-lang-kotlin</artifactId>
</dependency>
```

配置`kotlin-compiler-plugin`来编译 Kotlin 源代码。

```
<plugin>
    <groupId>org.jetbrains.kotlin</groupId>
    <artifactId>kotlin-maven-plugin</artifactId>
    <version>${kotlin.version}</version>
    <configuration>
        <jvmTarget>16</jvmTarget>
    </configuration>
    <executions>
        <execution>
            <id>compile</id>
            <goals>
                <goal>compile</goal>
            </goals>
        </execution>
        <execution>
            <id>test-compile</id>
            <goals>
                <goal>test-compile</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

我们将使用相同的文件结构，并将[原始项目(用 Java 编写)](https://github.com/hantsy/vertx-sandbox/tree/master/post-service)迁移到 Kotlin。

首先让我们来看看入门级产品— `MainVerticle`。

```
class MainVerticle : AbstractVerticle() {
    companion object {
        private val LOGGER = Logger.getLogger(MainVerticle::class.java.name) /**
         * Configure logging from logging.properties file.
         * When using custom JUL logging properties, named it to vertx-default-jul-logging.properties
         * or set java.util.logging.config.file system property to locate the properties file.
         */
        @Throws(IOException::class)
        private fun setupLogging() {
            MainVerticle::class.java.getResourceAsStream("/logging.properties")
                .use { f -> LogManager.getLogManager().readConfiguration(f) }
        } init {
            LOGGER.info("Customizing the built-in jackson ObjectMapper...")
            val objectMapper = DatabindCodec.mapper()
            objectMapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS)
            objectMapper.disable(SerializationFeature.WRITE_DATE_TIMESTAMPS_AS_NANOSECONDS)
            objectMapper.disable(DeserializationFeature.READ_DATE_TIMESTAMPS_AS_NANOSECONDS)
            val module = JavaTimeModule()
            objectMapper.registerModule(module)
        }
    } @Throws(Exception::class)
    override fun start(startPromise: Promise<Void>) {
        LOGGER.log(Level.INFO, "Starting HTTP server...")
        //setupLogging(); //Create a PgPool instance
        val pgPool = pgPool() //Creating PostRepository
        val postRepository = PostRepository(pgPool) //Creating PostHandler
        val postHandlers = PostsHandler(postRepository) // Initializing the sample data
        val initializer = DataInitializer(pgPool)
        initializer.run() // Configure routes
        val router = routes(postHandlers) // Create the HTTP server
        vertx.createHttpServer() // Handle every request using the router
            .requestHandler(router) // Start listening
            .listen(8888) // Print the port
            .onSuccess {
                startPromise.complete()
                println("HTTP server started on port " + it.actualPort())
            }
            .onFailure {
                startPromise.fail(it)
                println("Failed to start HTTP server:" + it.message)
            }
    } //create routes
    private fun routes(handlers: PostsHandler): Router { // Create a Router
        val router = Router.router(vertx)
        // register BodyHandler globally.
        //router.route().handler(BodyHandler.create());
        router.get("/posts")
            .produces("application/json")
            .handler { handlers.all(it) } router.post("/posts")
            .consumes("application/json")
            .handler(BodyHandler.create())
            .handler { handlers.save(it) } router.get("/posts/:id")
            .produces("application/json")
            .handler { handlers.getById(it) }
            .failureHandler {
                val error = it.failure()
                if (error is PostNotFoundException) {
                    it.response().setStatusCode(404).end(error.message)
                }
            } router.put("/posts/:id")
            .consumes("application/json")
            .handler(BodyHandler.create())
            .handler { handlers.update(it) } router.delete("/posts/:id")
            .handler { handlers.delete(it) } router.get("/hello").handler { it.response().end("Hello from my route") } return router
    } private fun pgPool(): PgPool {
        val connectOptions = PgConnectOptions()
            .setPort(5432)
            .setHost("localhost")
            .setDatabase("blogdb")
            .setUser("user")
            .setPassword("password") // Pool Options
        val poolOptions = PoolOptions().setMaxSize(5) // Create the pool from the data object
        return PgPool.pool(vertx, connectOptions, poolOptions)
    }
}
```

在这个类中，我们将原来的*静态*块移动到一个*伴随对象*。

在路由器功能中，它在路由中组装请求处理程序。让我们看看`PostsHandlers`班。

```
class PostsHandler(val posts: PostRepository) {
    fun all(rc: RoutingContext) {
//        var params = rc.queryParams();
//        var q = params.get("q");
//        var limit = params.get("limit") == null ? 10 : Integer.parseInt(params.get("q"));
//        var offset = params.get("offset") == null ? 0 : Integer.parseInt(params.get("offset"));
//        LOGGER.log(Level.INFO, " find by keyword: q={0}, limit={1}, offset={2}", new Object[]{q, limit, offset});
        posts.findAll()
            .onSuccess {
                rc.response().end(Json.encode(it))
            } } fun getById(rc: RoutingContext) {
        val params = rc.pathParams()
        val id = params["id"]
        posts.findById(UUID.fromString(id))
            .onSuccess { rc.response().end(Json.encode(it)) }
            .onFailure { rc.fail(404, it) }
    } fun save(rc: RoutingContext) {
        //rc.getBodyAsJson().mapTo(PostForm.class)
        val body = rc.bodyAsJson
        LOGGER.log(Level.INFO, "request body: {0}", body)
        val (title, content) = body.mapTo(CreatePostCommand::class.java)
        posts.save(Post(title = title, content = content))
            .onSuccess { savedId: UUID ->
                rc.response()
                    .putHeader("Location", "/posts/$savedId")
                    .setStatusCode(201)
                    .end()
            }
    } fun update(rc: RoutingContext) {
        val params = rc.pathParams()
        val id = params["id"]
        val body = rc.bodyAsJson
        LOGGER.log(Level.INFO, "\npath param id: {0}\nrequest body: {1}", arrayOf(id, body))
        var (title, content) = body.mapTo(CreatePostCommand::class.java)
        posts.findById(UUID.fromString(id))
            .flatMap { post: Post ->
                post.apply {
                    title = title
                    content = content
                }
                posts.update(post)
            }
            .onSuccess { rc.response().setStatusCode(204).end() }
            .onFailure { rc.fail(it) }
    } fun delete(rc: RoutingContext) {
        val params = rc.pathParams()
        val id = params["id"]
        val uuid = UUID.fromString(id)
        posts.findById(uuid)
            .flatMap { posts.deleteById(uuid) }
            .onSuccess { rc.response().setStatusCode(204).end() }
            .onFailure { rc.fail(404, it) }
    } companion object {
        private val LOGGER = Logger.getLogger(PostsHandler::class.java.simpleName)
    }
}
```

我们转到`PostRepository`类。

```
class PostRepository(private val client: PgPool) { fun findAll() = client.query("SELECT * FROM posts ORDER BY id ASC")
        .execute()
        .map { rs: RowSet<Row?> ->
            StreamSupport.stream(rs.spliterator(), false)
                .map { mapFun(it!!) }
                .toList()
        } fun findById(id: UUID) = client.preparedQuery("SELECT * FROM posts WHERE id=$1")
        .execute(Tuple.of(id))
        .map { it.iterator() }
        .map {
            if (it.hasNext()) mapFun(it.next());
            throw PostNotFoundException(id)
        } fun save(data: Post) = client.preparedQuery("INSERT INTO posts(title, content) VALUES ($1, $2) RETURNING (id)")
        .execute(Tuple.of(data.title, data.content))
        .map { it.iterator().next().getUUID("id") } fun saveAll(data: List<Post>): Future<Int> {
        val tuples = data.map { Tuple.of(it.title, it.content) } return client.preparedQuery("INSERT INTO posts (title, content) VALUES ($1, $2)")
            .executeBatch(tuples)
            .map { it.rowCount() }
    } fun update(data: Post) = client.preparedQuery("UPDATE posts SET title=$1, content=$2 WHERE id=$3")
        .execute(Tuple.of(data.title, data.content, data.id))
        .map { it.rowCount() } fun deleteAll() = client.query("DELETE FROM posts").execute()
        .map { it.rowCount() } fun deleteById(id: UUID) = client.preparedQuery("DELETE FROM posts WHERE id=$1").execute(Tuple.of(id))
        .map { it.rowCount() } companion object {
        private val LOGGER = Logger.getLogger(PostRepository::class.java.name)
        val mapFun: (Row) -> Post = { row: Row ->
            Post(
                row.getUUID("id"),
                row.getString("title"),
                row.getString("content"),
                row.getLocalDateTime("created_at")
            )
        } }
}
```

POJO 类被转换成 Kotlin 数据类。

```
//Models.kt
data class Post(
    var id: UUID? = null,
    var title: String,
    var content: String,
    var createdAt: LocalDateTime? = LocalDateTime.now()
)data class CreatePostCommand(
    val title: String,
    val content: String
)
```

`DataIntializer`仍然用于在应用程序启动时插入一些样本数据。

```
class DataInitializer(private val client: PgPool) { fun run() {
        LOGGER.info("Data initialization is starting...")
        val first = Tuple.of("Hello Quarkus", "My first post of Quarkus")
        val second = Tuple.of("Hello Again, Quarkus", "My second post of Quarkus")
        client
            .withTransaction { conn: SqlConnection ->
                conn.query("DELETE FROM posts").execute()
                    .flatMap {
                        conn.preparedQuery("INSERT INTO posts (title, content) VALUES ($1, $2)")
                            .executeBatch(listOf(first, second))
                    }
                    .flatMap {
                        conn.query("SELECT * FROM posts").execute()
                    }
            }
            .onSuccess { data: RowSet<Row?> ->
                StreamSupport.stream(data.spliterator(), true)
                    .forEach {
                        LOGGER.log(Level.INFO, "saved data:{0}", it!!.toJson())
                    }
            }
            .onComplete {
                //client.close(); will block the application.
                LOGGER.info("Data initialization is done...")
            }
            .onFailure { LOGGER.warning("Data initialization is failed:" + it.message) }
    } companion object {
        private val LOGGER = Logger.getLogger(DataInitializer::class.java.name)
    }
}
```

通过 maven 命令运行应用程序。

```
mvn clean compile exec:java
```

此外，Vertx Kotlin bindings 提供了一个`Json` DSL 扩展来简化 JSON 编码。

```
it.response()
     .setStatusCode(404)
     .end(
         json {// an example using JSON DSL
             obj(
                 "message" to "${it.failure().message}",
                 "code" to "not_found"
             )
         }.toString()
     )
```

从我的 Github 获取源代码。