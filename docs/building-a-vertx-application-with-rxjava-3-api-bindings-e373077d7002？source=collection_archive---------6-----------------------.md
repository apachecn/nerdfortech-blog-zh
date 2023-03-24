# 使用 RxJava 3 API 绑定构建 Vertx 应用程序

> 原文：<https://medium.com/nerd-for-tech/building-a-vertx-application-with-rxjava-3-api-bindings-e373077d7002?source=collection_archive---------6----------------------->

Eclipse Vertx 有一个很棒的 codegen 机制，可以将其基于*事件循环*的异步编程模型带到不同的平台，包括其他异步库，如 [RxJava2/3](https://github.com/ReactiveX/RxJava) 和 [SmallRye 哗变](https://smallrye.io/smallrye-mutiny/)以及不同的语言，如 Kotlin、Kotlin 协同例程，甚至 Node/Typescript、PHP 等。

![](img/886aac470d16b610f3337655049a2ecb.png)

[Mèng Jiǎ](https://unsplash.com/@justin73?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/china-landscape?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

在这篇文章中，我们将重构我们在[上一篇文章](https://itnext.io/building-restful-apis-with-eclipse-vertx-4ce89d8eeb74)中所做的 RESTful APIs，并用 RxJava 3 重新实现它。RxJava 3 完全实现了[反应流规范](http://www.reactive-streams.org/)。

打开浏览器，进入 [Vertx Starter](https://start.vertx.io) ，创建一个 Vertx 项目框架。不要忘记将 *RxJava 3* 添加到依赖项中。

如果您正在处理一个现有的项目，直接添加 *rxjava3* 依赖项。

```
<dependency>
    <groupId>io.vertx</groupId>
    <artifactId>vertx-rx-java3</artifactId>
</dependency>
```

将项目源代码导入 IDE，例如 Intellij IDEA。

首先让我们来看看`MainVerticle`。

```
import io.vertx.rxjava3.core.AbstractVerticle;
import io.vertx.rxjava3.ext.web.Router;
import io.vertx.rxjava3.ext.web.RoutingContext;
import io.vertx.rxjava3.ext.web.handler.BodyHandler;
import io.vertx.rxjava3.ext.web.validation.ValidationHandler;
import io.vertx.rxjava3.json.schema.SchemaParser;
import io.vertx.rxjava3.json.schema.SchemaRouter;
import io.vertx.rxjava3.pgclient.PgPool;
// other imports@Slf4j
public class MainVerticle extends AbstractVerticle { @Override
    public Completable rxStart() {}

     //create routes
    private Router routes(PostsHandler handlers) {}

    private PgPool pgPool() {}
}
```

如你所见，我们从`io.vertx.rxjava3`导入了所有的基本类，并使用 RxJava 3 API，而不是 Vertx 内置的`Future`和`Promise`。在这个实现中，我们覆盖了返回 RxJava `Completable`的`rxStart`。

好了，我们转到`PostRepository`类。

```
import io.reactivex.rxjava3.core.Flowable;
import io.reactivex.rxjava3.core.Single;
import io.vertx.rxjava3.pgclient.PgPool;
import io.vertx.rxjava3.sqlclient.Row;
import io.vertx.rxjava3.sqlclient.RowSet;
import io.vertx.rxjava3.sqlclient.SqlResult;
import io.vertx.rxjava3.sqlclient.Tuple;
// other imports...@Slf4j
public class PostRepository { private static Function<Row, Post> MAPPER = (row) ->
        Post.of(
            row.getUUID("id"),
            row.getString("title"),
            row.getString("content"),
            row.getLocalDateTime("created_at")
        ); private final PgPool client; private PostRepository(PgPool _client) {
        this.client = _client;
    } //factory method
    public static PostRepository create(PgPool client) {
        return new PostRepository(client);
    } public Flowable<Post> findAll() {
        return this.client
            .query("SELECT * FROM posts")
            .rxExecute()
            .flattenAsFlowable(
                rows -> StreamSupport.stream(rows.spliterator(), false)
                    .map(MAPPER)
                    .collect(Collectors.toList())
            );
    } public Single<Post> findById(UUID id) {
        Objects.requireNonNull(id, "id can not be null");
        return client.preparedQuery("SELECT * FROM posts WHERE id=$1").rxExecute(Tuple.of(id))
            .map(RowSet::iterator)
            .flatMap(iterator -> iterator.hasNext() ? Single.just(MAPPER.apply(iterator.next())) : Single.error(new PostNotFoundException(id)));
    } public Single<UUID> save(Post data) {
        return client.preparedQuery("INSERT INTO posts(title, content) VALUES ($1, $2) RETURNING (id)")
            .rxExecute(Tuple.of(data.getTitle(), data.getContent()))
            .map(rs -> rs.iterator().next().getUUID("id"));
    } public Single<Integer> saveAll(List<Post> data) {
        var tuples = data.stream()
            .map(d -> Tuple.of(d.getTitle(), d.getContent()))
            .collect(Collectors.toList()); return client.preparedQuery("INSERT INTO posts (title, content) VALUES ($1, $2)")
            .rxExecuteBatch(tuples)
            .map(SqlResult::rowCount);
    } public Single<Integer> update(Post data) {
        return client.preparedQuery("UPDATE posts SET title=$1, content=$2 WHERE id=$3")
            .rxExecute(Tuple.of(data.getTitle(), data.getContent(), data.getId()))
            .map(SqlResult::rowCount);
    } public Single<Integer> deleteAll() {
        return client.query("DELETE FROM posts").rxExecute()
            .map(SqlResult::rowCount);
    } public Single<Integer> deleteById(UUID id) {
        Objects.requireNonNull(id, "id can not be null");
        return client.preparedQuery("DELETE FROM posts WHERE id=$1").rxExecute(Tuple.of(id))
            .map(SqlResult::rowCount);
    }}
```

在这个类中，我们使用基于 RxJava 3 API 的`PgPool`来代替，它包装了原始的 Vertx `PgPool`并添加了额外的 RxJava 3 API 支持。所有方法都类似于以前的 Vertx 版本，这里我们使用一个`rxExecute`方法来执行 SQL 查询，返回的结果被切换到 RxJava 3 世界。

让我们来看看`PostsHandler`。

```
import io.vertx.rxjava3.ext.web.RoutingContext;
//other imports@Slf4j
class PostsHandler { PostRepository posts; private PostsHandler(PostRepository _posts) {
        this.posts = _posts;
    } //factory method
    public static PostsHandler create(PostRepository posts) {
        return new PostsHandler(posts);
    } public void all(RoutingContext rc) {
//        var params = rc.queryParams();
//        var q = params.get("q");
//        var limit = params.get("limit") == null ? 10 : Integer.parseInt(params.get("q"));
//        var offset = params.get("offset") == null ? 0 : Integer.parseInt(params.get("offset"));
//        LOGGER.log(Level.INFO, " find by keyword: q={0}, limit={1}, offset={2}", new Object[]{q, limit, offset});
        this.posts.findAll().takeLast(10).toList()
            .subscribe(
                data -> rc.response().end(Json.encode(data))
            );
    } public void get(RoutingContext rc) throws PostNotFoundException {
        var params = rc.pathParams();
        var id = params.get("id");
        var uuid = UUID.fromString(id);
        this.posts.findById(uuid)
            .subscribe(
                post -> rc.response().end(Json.encode(post)),
                throwable -> rc.fail(404, throwable)
            ); } public void save(RoutingContext rc) {
        //rc.getBodyAsJson().mapTo(PostForm.class)
        var body = rc.getBodyAsJson();
        log.info("request body: {0}", body);
        var form = body.mapTo(CreatePostCommand.class);
        this.posts
            .save(Post.builder()
                .title(form.getTitle())
                .content(form.getContent())
                .build()
            )
            .subscribe(
                savedId -> rc.response()
                    .putHeader("Location", "/posts/" + savedId)
                    .setStatusCode(201)
                    .end() );
    } public void update(RoutingContext rc) {
        var params = rc.pathParams();
        var id = params.get("id");
        var body = rc.getBodyAsJson();
        log.info("\npath param id: {}\nrequest body: {}", id, body);
        var form = body.mapTo(CreatePostCommand.class);
        this.posts.findById(UUID.fromString(id))
            .flatMap(
                post -> {
                    post.setTitle(form.getTitle());
                    post.setContent(form.getContent()); return this.posts.update(post);
                }
            )
            .subscribe(
                data -> rc.response().setStatusCode(204).end(),
                throwable -> rc.fail(404, throwable)
            ); } public void delete(RoutingContext rc) {
        var params = rc.pathParams();
        var id = params.get("id"); var uuid = UUID.fromString(id);
        this.posts.findById(uuid)
            .flatMap(
                post -> this.posts.deleteById(uuid)
            )
            .subscribe(
                data -> rc.response().setStatusCode(204).end(),
                throwable -> rc.fail(404, throwable)
            ); }}
```

在`subscribe`方法中，使用 RxJava 3 特有的`RoutingContext`来发送响应。

重构`DataInitializer`以使用 RxJava 3 API 绑定。

```
@Slf4j
public class DataInitializer { private PgPool client; public DataInitializer(PgPool client) {
        this.client = client;
    } public static DataInitializer create(PgPool client) {
        return new DataInitializer(client);
    } public void run() {
        log.info("Data initialization is starting..."); Tuple first = Tuple.of("Hello Quarkus", "My first post of Quarkus");
        Tuple second = Tuple.of("Hello Again, Quarkus", "My second post of Quarkus"); client
            .rxWithTransaction(
                (SqlConnection tx) -> tx.query("DELETE FROM posts").rxExecute()
                    .flatMap(result -> tx.preparedQuery("INSERT INTO posts (title, content) VALUES ($1, $2)").rxExecuteBatch(List.of(first, second)))
                    .toMaybe()
            )
            .flatMapSingle(d -> client.query("SELECT * FROM posts").rxExecute())
            .subscribe(
                (data) -> {
                    data.forEach(row -> {
                        log.info("saved row: {}", row.toJson());
                    });
                },
                err -> log.warn("failed to initializing: {}", err.getMessage())
            );
    }
}
```

`MainVerticle`类中`rxStart`方法的完整代码。

```
@Override
public Completable rxStart() {
    log.info("Starting HTTP server...");
    InternalLoggerFactory.setDefaultFactory(Slf4JLoggerFactory.INSTANCE); //Create a PgPool instance
    var pgPool = pgPool(); //Creating PostRepository
    var postRepository = PostRepository.create(pgPool); //Creating PostHandler
    var postHandlers = PostsHandler.create(postRepository); // Initializing the sample data
    var initializer = DataInitializer.create(pgPool);
    initializer.run(); // Configure routes
    var router = routes(postHandlers); // Create the HTTP server
    return vertx.createHttpServer()
        // Handle every request using the router
        .requestHandler(router)
        // Start listening
        .rxListen(8888)
        // to Completable
        .ignoreElement()
        ;
}
```

现在您可以运行应用程序了。

```
mvn clean compile exec:java
```

从我的 github 获取[源代码，如果你还在使用 RxJava 2，这里还包括一个](https://github.com/hantsy/vertx-sandbox/tree/master/rxjava3) [RxJava 2 版本](https://github.com/hantsy/vertx-sandbox/tree/master/rxjava2)。