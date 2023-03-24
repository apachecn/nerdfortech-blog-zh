# Spring Boot 注解

> 原文：<https://medium.com/nerd-for-tech/spring-boot-annotations-cb2487304062?source=collection_archive---------3----------------------->

![](img/d4479c91eb7d114b37c3fc94aacb551c.png)

注释使得在 spring 中配置依赖注入和定义/管理 beans 变得更加容易。

org . Spring framework . context . application context 接口代表 Spring IoC 容器，负责实例化、配置和组装 beans

Spring Boots**Spring application**类用于从 Java main 方法引导和启动 Spring 应用程序。该类从类路径自动创建 ApplicationContext，扫描配置类并启动应用程序。

**@ spring foot application**

从 Spring Boot 1.2.0 开始，我们可以使用这个注释，它和声明@Configuration、@EnableAutoConfiguration、@ComponentScan 是一样的。

```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;@SpringBootApplication // same as @Configuration, //@EnableAutoConfiguration, and @ComponentScan
public class Application { public static void main(String[] args) {        SpringApplication.run(Application.class, args);

    }}
```

**@配置**

从 spring 2 开始，我们一直将 bean 配置写入 xml 文件。但是 Spring 3+提供了将 bean 定义移出 xml 文件的自由。

在任何类的顶部使用@Configuration 来声明该类提供了一个或多个@bean 方法，并将由 Spring 容器处理，以便在运行时为这些 Bean 生成 Bean 定义和服务请求。

**@启用自动配置**

@EnableAutoConfiguration 注释使 Spring Boot 能够自动配置应用程序上下文。因此，它会根据类路径中包含的 jar 文件和我们定义的 bean 自动创建和注册 bean。

当我们在 classpath/pom 中定义 spring-boot-starter-web 依赖时，Spring boot 会自动配置 Tomcat 和 Spring MVC。但是，在我们定义自己的配置时，这种自动配置的优先级较低。

**@ComponentScan**

这用于自动挑选所有 Spring 组件(@Component、@Controller、@Service、@Repository)，包括@Configuration 类作为 beans。

**@自动连线**

用于将 spring bean 管理器中的对象依赖注入到使用该注释的类中。

```
@Controller // Defines that this class is a spring bean
@RequestMapping(“/admin”)public class ExampleController { //Tells the application context to inject an instance of     
    AdminService here @Autowired
    private AdminService adminService; @RequestMapping(“/login”)
    public void login(@RequestParam(“name”) String name,
          @RequestParam(“password”) String password) { //The AdminService is already injected and you can use it adminService.login(name, password);   }}
```

@Autowired 用于注入 AdminService bean 依赖项。如果这不是 Spring，而是一个普通的 Java 应用程序，那么我们必须使用 new 运算符创建 AdminService 对象:

AdminService AdminService = new AdminService()；

这种用法使对象的管理变得重复和复杂，因为 AdminService 本身可以处理其他对象，这对于主要关注业务逻辑的应用程序来说变得难以管理。

**@组件**

用@Component 标记类使得它有资格在 SpringApplicationContext 中被扫描和配置为 Spring Bean。

下面是组件接口的源代码:

```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Component {
}
```

**@Bean**

这也用于将类配置为 Bean。然而，它不同于@Component。

1.  组件是类级别的注释，而 Bean 是方法级别的注释。
2.  @Bean 必须在一个类中使用，并且该类应该用@Configuration 标记。
3.  如果类在 Spring 容器之外，我们不能从@Component 创建 bean，但是我们可以从任何地方从@bean 创建 Bean。

***我们在哪里用@Bean 代替@Component？***

当您想要将一个类配置为来自第三方库的 bean，而该库的代码不在您身边时。在这种情况下，您没有在类中使用@Component 的自由，因为您没有代码。

```
@Configuration
public class App { @Bean
  public TestService testService() { return new TestService(); }}
```

这将执行该方法，并将返回值注册为 SpringContext 中的 bean，方法名为 Bean 名，即 testService

**@控制器**

通过使用这个注释，我们做了两件事，首先，我们声明这个类是一个 Spring bean，应该由 SpringApplicationContext 创建和维护，其次，它表明它是 MVC 设置中的一个控制器。

> @Controller 是一个@组件(就像@Service、@Repository、@Endpoint 等。)

以下是控制器界面的源代码:

```
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Controller {}
```

当发出请求时，该注释告诉 dispatcher servlet 在标有@Controller 的组件中寻找所需的端点请求路径。

**@服务**

在服务层注释类。服务层保存从控制器层调用的业务逻辑。

**@仓库**

在持久层注释类，持久层将充当数据库存储库。它使 beans 有资格进行持久性异常转换——这意味着将低级别的 ORM 异常转换为通用异常，由 Spring 管理和处理，而不是像 Hibernate、JDBC 这样的 O/R 映射工具。

@Controller @Serivce 和@Repository 不能被@Component 替代，因为它有自己的特定角色，为开发人员提供了每个类角色的清晰思路，并且 SpringBoot 团队很容易对其进行更改以获得更多功能。

如有任何疑问或自由职业工作，请联系我。[领英](https://www.linkedin.com/in/vivek-singh-a109b511a/)。或者发邮件给我，地址是[vivek.sinless@gmail.com](mailto:vivek.sinless@gmail.com)

快乐学习！干杯。