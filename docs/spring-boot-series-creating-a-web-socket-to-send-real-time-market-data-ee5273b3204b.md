# Spring boot 系列—创建一个 Web 套接字来发送实时市场数据

> 原文：<https://medium.com/nerd-for-tech/spring-boot-series-creating-a-web-socket-to-send-real-time-market-data-ee5273b3204b?source=collection_archive---------6----------------------->

![](img/3e86835f3ba3cc3ee5f89e0de927dac6.png)

嗨，伙计们，我们已经完成了创建一个很酷的股票市场应用程序，它用一组符号显示真实的股票市场信息。在本教程中，我们将看到如何使用 web 套接字将实时股票市场数据发送到前端。

为了创建 web 套接字，首先我们必须向 build.gradle 文件添加一些依赖项。所以在添加了 web socket 依赖项之后，现在我们的 build.gradle 文件的依赖项应该是这样的。

```
dependencies **{** implementation 'org.springframework.boot:spring-boot-starter'
   implementation 'org.springframework.boot:spring-boot-starter-websocket'
   implementation 'com.yahoofinance-api:YahooFinanceAPI:3.15.0'
   implementation 'org.projectlombok:lombok'
   testImplementation 'org.apache.httpcomponents:httpclient'
   testImplementation 'org.springframework.boot:spring-boot-starter-test'
**}**
```

到目前为止，关于该项目的代码和细节可以找到，如果你访问本教程。

[](https://billa-code.medium.com/flutter-series-connecting-ui-to-spring-boot-backend-f9874dc3dcd5) [## Flutter 系列—将 UI 连接到 spring boot 后端

### 嗨，伙计们，这是股市应用程序创建教程的最后一个教程。将来如果可能的话，我会努力…

billa-code.medium.com](https://billa-code.medium.com/flutter-series-connecting-ui-to-spring-boot-backend-f9874dc3dcd5) [](https://billa-code.medium.com/flutter-series-implementing-stock-market-watch-list-ui-dccd37a9ef34) [## Flutter 系列—实现股票市场观察列表 UI

### 嗨伙计们。在上一个教程中，我告诉你们，我们将使用 Flutter 来创建一个前端应用程序，以便…

billa-code.medium.com](https://billa-code.medium.com/flutter-series-implementing-stock-market-watch-list-ui-dccd37a9ef34) [](https://billa-code.medium.com/flutter-series-creating-the-first-flutter-application-793e5816f816) [## 颤振系列——创造第一个颤振应用

### 嗨伙计们。所以在我们的系列教程中，现在你应该已经创建了一个 spring boot 应用程序，它有一个获取股票的端点…

billa-code.medium.com](https://billa-code.medium.com/flutter-series-creating-the-first-flutter-application-793e5816f816)  [## Spring boot 系列—以 JSON 形式发送股票市场数据

### 嗨伙计们。因此，我们创建了股票数据服务来获取单只股票的数据，在本教程中，我将…

billa-code.medium.com](https://billa-code.medium.com/spring-boot-series-sending-stock-market-data-in-json-form-cce978a9a90d) [](https://billa-code.medium.com/spring-boot-series-unit-testing-basics-3ce566250465) [## 弹簧靴系列—单元测试基础

### 嗨伙计们。所以到目前为止，我们已经开发了一个基本的 spring boot 应用程序，它从 Yahoo finance API 获取数据并显示…

billa-code.medium.com](https://billa-code.medium.com/spring-boot-series-unit-testing-basics-3ce566250465)  [## 春靴系列—股市数据终点

billa-code.medium.com](https://billa-code.medium.com/spring-boot-series-stock-market-data-end-point-356592487254) [](https://billa-code.medium.com/create-the-first-spring-boot-app-4e930d812a22) [## 创建第一个春季启动应用程序

### 我不打算深入了解许多功能和描述，而只是深入了解 Spring boot 的世界…

billa-code.medium.com](https://billa-code.medium.com/create-the-first-spring-boot-app-4e930d812a22) 

现在我们必须创建一个 web 套接字消息配置器来配置我们的 Web 套接字端点。因此，我将创建 web 套接字作为“/ws-message”，然后对于应用程序前缀，我将使用值“/app”。此外，我还将创建一个名为“/topic”的目的地前缀。现在，文件 WebSocketMessageConfig.java 的代码应该是这样的。

```
import org.springframework.context.annotation.Configuration;
import org.springframework.messaging.simp.config.MessageBrokerRegistry;
import org.springframework.web.socket.config.annotation.EnableWebSocketMessageBroker;
import org.springframework.web.socket.config.annotation.StompEndpointRegistry;
import org.springframework.web.socket.config.annotation.WebSocketMessageBrokerConfigurer;

@Configuration
@EnableWebSocketMessageBroker
public class WebSocketMessageConfig implements WebSocketMessageBrokerConfigurer {
    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        registry.addEndpoint("/ws-message").setAllowedOriginPatterns("*").withSockJS();
    }

    @Override
    public void configureMessageBroker(MessageBrokerRegistry registry) {
        registry.setApplicationDestinationPrefixes("/app");
        registry.enableSimpleBroker("/topic");
    }
}
```

在这里，我们使用了 withStockJs()方法，所以将来当我们使用我们的 Flutter 应用程序访问它时，它将很容易处理。现在我将创建另一个文件作为套接字连接的控制器，并将其命名为 SocketController.java。在这里，我将创建一个调度程序，定期发送我们的市场数据。所以代码如下。

```
import com.billa.code.stockMarket.service.StockService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.messaging.handler.annotation.MessageMapping;
import org.springframework.messaging.simp.SimpMessagingTemplate;
import org.springframework.stereotype.Controller;
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;

import static java.util.concurrent.TimeUnit.*SECONDS*;

@Controller
public class SocketController {
    @Autowired
    SimpMessagingTemplate template;

    @Autowired
    StockService stockService;

    private static final ScheduledExecutorService *scheduler* = Executors.*newScheduledThreadPool*(10);

    @MessageMapping("/hello")
    public void greeting() {
        *scheduler*.scheduleAtFixedRate(() -> {
            template.convertAndSend("/topic/message", stockService.getStockResponse());
        }, 0, 2, *SECONDS*);
    }
}
```

这里，我在我们的股票服务类中添加了一个名为 getStockResponse 的新方法，因此修改后的 StockService.java 的代码如下所示。

```
import com.billa.code.stockMarket.model.StockModel;
import com.billa.code.stockMarket.model.StockResponseModel;
import com.billa.code.stockMarket.wrapper.StockWrapper;
import org.springframework.stereotype.Service;
import yahoofinance.Stock;
import yahoofinance.YahooFinance;
import java.util.ArrayList;
import java.util.List;

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

    public StockResponseModel getStockResponse() {
        List<StockModel> stocks = new ArrayList<>();
        String[] symbolArr = {"A", "AA", "AAC", "GOOG", "AMZN", "AAT", "AAN", "T", "TD", "TARO", "TM"};

        for (String s : symbolArr) {
            Stock stock = findStock(s).getStock();
            StockModel stockModel = new StockModel(s, stock.getName(), stock.getQuote().getAsk().toString(), stock.getQuote().getChangeInPercent().toString());
            stocks.add(stockModel);
        }

        StockResponseModel res = new StockResponseModel();
        res.setStock(stocks);
        res.setStockExg("US");

        return res;
    }
}
```

所以逻辑是这样的。使用 StockJS 的前端应用程序可以在***ws://localhost:8080/ws-message 上创建到我们的 web 套接字的连接。*** 现在它将连接到我们的 web socket，在连接创建方法的回调函数的前端应用程序中，我们必须订阅我们的 web socket 的 **/topic/message** 目的地。然后，当我们从我们的前端应用程序向我们的 web 套接字的 **/app/hello** 端点发送消息时，后端应用程序将通过 web 套接字每 2 秒钟向前端发送一次市场数据。

这个项目的家伙代码基础是在以下链接。分叉的项目，请启动它，如果你觉得有用。在 Github 上关注我，你会收到我新的酷项目的通知。

[](https://github.com/debilla-academy/yahoo-stock-backend) [## debilla-学院/雅虎-股票-后端

### 在 GitHub 上创建一个帐户，为 debilla-academy/Yahoo-stock-back end 开发做贡献。

github.com](https://github.com/debilla-academy/yahoo-stock-backend) 

因此，在下一个教程中，我们将从 web socket 获取数据，并将其显示在我们的 Flutter 应用程序中。编码快乐！！！