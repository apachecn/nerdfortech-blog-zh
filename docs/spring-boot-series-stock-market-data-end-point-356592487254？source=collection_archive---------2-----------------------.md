# 春靴系列—股市数据终点

> 原文：<https://medium.com/nerd-for-tech/spring-boot-series-stock-market-data-end-point-356592487254?source=collection_archive---------2----------------------->

因此，在我的上一篇教程中，我向你们承诺，我将对 spring boot 应用程序进行生产部署。在此之前，让我们对我们的代码库做一些改进。上一个教程链接，

[](https://billa-code.medium.com/create-the-first-spring-boot-app-4e930d812a22) [## 创建第一个春季启动应用程序

### 我不打算深入了解许多功能和描述，而只是深入了解 Spring boot 的世界…

billa-code.medium.com](https://billa-code.medium.com/create-the-first-spring-boot-app-4e930d812a22) 

因此，在本教程中，我将修改上一教程中的代码，做一些有意义的事情。因此，我将创建一个端点，从 Yahoo financial API 获取数据，并将其发送到客户端。为此，我将把 yahoo finance api 包添加到 gradle 文件中。所以我们新的 gradle 文件应该是这样的。

```
plugins **{** id 'org.springframework.boot' version '2.4.3'
    id 'io.spring.dependency-management' version '1.0.11.RELEASE'
    id 'java'
**}** group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'

repositories **{** mavenCentral()
**}** dependencies **{** implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'com.yahoofinance-api:YahooFinanceAPI:3.15.0'
    testImplementation('org.springframework.boot:spring-boot-starter-test')
**}** test **{** useJUnitPlatform()
**}**
```

添加后，在 intellij 中同步项目，然后这个包将被下载。所以现在让我们创建一个从 yahoo finance API 获取数据的服务。创建一个名为 StockService.java 文件。这里，当我们将符号传递给 find stock 方法时，我们将获得该特定符号的股票详细信息。我们必须创建另一个名为 StockWrapper.java 的类来存储股票数据。StockService.java 的代码如下。

```
import org.springframework.stereotype.Service;
import wrapper.StockWrapper;
import yahoofinance.YahooFinance;

@Service
public class StockService {
    public StockWrapper findStock(String symbol) {
        try {
            return new StockWrapper(YahooFinance.*get*(symbol));
        } catch (Exception e) {
            System.*out*.println(e.getMessage());
        }

        return null;
    }
}
```

StockWrapper.java 的代码如下。

```
import yahoofinance.Stock;
import java.time.LocalDateTime;

public class StockWrapper {
    private final Stock stock;
    private final LocalDateTime lastAccess;

    public StockWrapper(Stock stock) {
        this.stock = stock;
        this.lastAccess = LocalDateTime.*now*();
    }

    public LocalDateTime getLastAccess() {
        return lastAccess;
    }

    public Stock getStock() {
        return stock;
    }
}
```

在这个包装器中，我们修改了构造函数来创建包含访问时间的股票。因此，现在我们要修改我们的 StockController.java 文件，我们有我们的终点写。因此，在我们的控制器中，我们从一个方法中处理“/”请求的 get 和 post 请求。因此，我们将使用此服务向 from end 发送 Google 的股票价格。StockController.java 的代码如下。

```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import service.StockService;
import java.math.BigDecimal;

@RestController
@EnableAutoConfiguration
public class StockController {

    @RequestMapping("/")
    BigDecimal home() {
        StockService stockService = new StockService();

        return stockService.findStock("GOOG").getStock().getQuote().getPrice();
    }

    public static void main(String[] args) {
        SpringApplication.*run*(StockController.class, args);
    }
}
```

运行这个并访问 [http://localhost:8080/](http://localhost:8080/) 你会在浏览器中找到一只谷歌股票的价格。我们可以修改这些方法，做出不可思议的事情。但在这里，我只向你展示了我们能做的一个基本活动。

因此，正如我上次所承诺的，我将向您展示如何创建我们到目前为止编写的这个 spring boot 应用程序的生产版本。要构建生产版本，只需像以前一样键入 clean build 命令。

对于 windows，

```
gradlew clean build
```

对于 Linux

```
./gradlew clean build
```

现在您的项目中将会有一个名为 build 的文件夹。转到那里，在那个文件夹中有另一个名为 libs 的文件夹，在里面你会找到一个. jar 文件。因此，复制 jar 文件名，并在终端中键入。

```
java -jar build/libs/JARFILE.jar
```

现在，您正在运行 spring boot 的生产版本。所以下次让我们通过使用一些其他的酷技术来改进我们的股票市场应用程序。快乐的编码伙计们。