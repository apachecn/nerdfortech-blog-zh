# Spring boot 系列—以 JSON 形式发送股票市场数据

> 原文：<https://medium.com/nerd-for-tech/spring-boot-series-sending-stock-market-data-in-json-form-cce978a9a90d?source=collection_archive---------8----------------------->

嗨伙计们。因此，我们创建了股票数据服务来获取单只股票的数据，在本教程中，我将讨论如何改进我们的代码，以 JSON 格式发送一组数据。教程到目前为止，

[](https://billa-code.medium.com/spring-boot-series-unit-testing-basics-3ce566250465) [## 弹簧靴系列—单元测试基础

### 嗨伙计们。所以到目前为止，我们已经开发了一个基本的 spring boot 应用程序，它从 Yahoo finance API 获取数据并显示…

billa-code.medium.com](https://billa-code.medium.com/spring-boot-series-unit-testing-basics-3ce566250465)  [## 春靴系列—股市数据终点

billa-code.medium.com](https://billa-code.medium.com/spring-boot-series-stock-market-data-end-point-356592487254) [](https://billa-code.medium.com/create-the-first-spring-boot-app-4e930d812a22) [## 创建第一个春季启动应用程序

### 我不打算深入了解许多功能和描述，而只是深入了解 Spring boot 的世界…

billa-code.medium.com](https://billa-code.medium.com/create-the-first-spring-boot-app-4e930d812a22) 

因此，我们将为此编写一个新的端点“/getStock”。所以代码如下。

```
@GetMapping(value="/getStock")
List<StockModel> getStocks() {
    StockService stockService = new StockService();
    List<StockModel> stocks = new ArrayList<>();
    String[] symbolArr = {"A", "AA", "AAC", "GOOG", "AMZN", "AAT", "AAN", "T", "TD", "TARO", "TM"};

    for (String s : symbolArr) {
        Stock stock = stockService.findStock(s).getStock();
        StockModel stockModel = new StockModel(s, stock.getName(), stock.getQuote().getAsk().toString(), stock.getQuote().getChangeInPercent().toString());
        stocks.add(stockModel);
    }

    return stocks;
}
```

因此，如果您像我们对另一个端点那样运行，您将毫无问题地获得数组中的数据。包含此内容后运行程序，并在浏览器中访问[http://localhost:8080/getStock](http://localhost:8080/getStock)，您将看到数据。因此，我们在这里所做的是，我们创建了一个模型来封装我们需要返回的股票数据。这个模型在 StockModel.java 的档案里。

```
public class StockModel {
    private final String symbol;
    private final String name;
    private final String price;
    private final String chg;

    public StockModel(String symbol, String name, String price, String chg) {
        this.symbol = symbol;
        this.name = name;
        this.price = price;
        this.chg = chg;
    }
}
```

我们循环一组硬编码的符号，获取股票的名称、价格和变化百分比值，并使用这些值创建 StockModel 对象。所以现在我们的要求是确保我们发送的数据是 JSON 格式的。因此，我们将添加“produces”参数，并在 GetMapping 注释中将它设置为 JSON 类型。所以现在代码看起来像这样。

```
@GetMapping(value="/getStock", produces= MediaType.*APPLICATION_JSON_VALUE*)
List<StockModel> getStocks() {
    StockService stockService = new StockService();
    List<StockModel> stocks = new ArrayList<>();
    String[] symbolArr = {"A", "AA", "AAC", "GOOG", "AMZN", "AAT", "AAN", "T", "TD", "TARO", "TM"};

    for (String s : symbolArr) {
        Stock stock = stockService.findStock(s).getStock();
        StockModel stockModel = new StockModel(s, stock.getName(), stock.getQuote().getAsk().toString(), stock.getQuote().getChangeInPercent().toString());
        stocks.add(stockModel);
    }

    return stocks;
}
```

现在，我们在运行应用程序时遇到了一个错误。当 spring boot 找不到 out StockModel 类的 JSON 映射时，运行时会触发异常。所以我们必须让 spring boot 检测它。为此，我们必须像这样更改 StockModel 类。

```
import com.fasterxml.jackson.annotation.JsonAutoDetect;

@JsonAutoDetect(fieldVisibility = JsonAutoDetect.Visibility.*ANY*)
public class StockModel {
    private final String symbol;
    private final String name;
    private final String price;
    private final String chg;

    public StockModel(String symbol, String name, String price, String chg) {
        this.symbol = symbol;
        this.name = name;
        this.price = price;
        this.chg = chg;
    }
}
```

现在代码可以了，我们可以从浏览器中检查数据，或者使用 Postman 检查数据。但是为了支持另一个前端应用程序，我们必须放宽跨源策略。为此，我们可以使用@CrossOrigin 注释。现在控制器看起来像这样。

```
@CrossOrigin(origins = "*")
@GetMapping(value="/getStock", produces= MediaType.*APPLICATION_JSON_VALUE*)
List<StockModel> getStocks() {
    StockService stockService = new StockService();
    List<StockModel> stocks = new ArrayList<>();
    String[] symbolArr = {"A", "AA", "AAC", "GOOG", "AMZN", "AAT", "AAN", "T", "TD", "TARO", "TM"};

    for (String s : symbolArr) {
        Stock stock = stockService.findStock(s).getStock();
        StockModel stockModel = new StockModel(s, stock.getName(), stock.getQuote().getAsk().toString(), stock.getQuote().getChangeInPercent().toString());
        stocks.add(stockModel);
    }

    return stocks;
}
```

这里的*标记表示我们已启用任何 URL 与此通信。因此，现在我们正在发送一个 JSON 格式的数据列表，但是当与前端应用程序一起使用时，我们在解码 JSON 时会遇到一个问题，因为列表会产生一些错误。为了避免这一点，我使用了另一个叫做 StockResponseModel.java 的模型，代码如下。

```
import com.fasterxml.jackson.annotation.JsonAutoDetect;

import java.util.List;

@JsonAutoDetect
public class StockResponseModel {
    private String stockExg;
    private List<StockModel> stock;

    public String getStockExg() {
        return stockExg;
    }

    public void setStockExg(String stockExg) {
        this.stockExg = stockExg;
    }

    public List<StockModel> getStock() {
        return stock;
    }

    public void setStock(List<StockModel> stock) {
        this.stock = stock;
    }
}
```

现在我将更改我的控制器，在一个参数中存储股票列表，在另一个参数中存储交易所名称。所以我最后的 StockController.java 看起来像这样。

```
import models.StockModel;
import models.StockResponseModel;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import service.StockService;
import yahoofinance.Stock;
import java.util.ArrayList;
import java.util.List;

@RestController
@EnableAutoConfiguration
public class StockController {

    @RequestMapping(value="/", produces= MediaType.*APPLICATION_JSON_VALUE*)
    StockModel home() {
        StockService stockService = new StockService();
        Stock stock = stockService.findStock("GOOG").getStock();
        StockModel stockModel = new StockModel("GOOG", stock.getName(), stock.getQuote().getAsk().toString(), stock.getQuote().getChangeInPercent().toString());

        return stockModel;
    }

    @CrossOrigin(origins = "*")
    @GetMapping(value="/getStock", produces= MediaType.*APPLICATION_JSON_VALUE*)
    StockResponseModel getStocks() {
        StockService stockService = new StockService();
        List<StockModel> stocks = new ArrayList<>();
        String[] symbolArr = {"A", "AA", "AAC", "GOOG", "AMZN", "AAT", "AAN", "T", "TD", "TARO", "TM"};

        for (String s : symbolArr) {
            Stock stock = stockService.findStock(s).getStock();
            StockModel stockModel = new StockModel(s, stock.getName(), stock.getQuote().getAsk().toString(), stock.getQuote().getChangeInPercent().toString());
            stocks.add(stockModel);
        }

        StockResponseModel res = new StockResponseModel();
        res.setStock(stocks);
        res.setStockExg("US");

        return res;
    }

    public static void main(String[] args) {
        SpringApplication.*run*(StockController.class, args);
    }
}
```

因此，我们已经成功地创建了一个端点，用于前端应用程序来获取股票市场数据。在接下来的教程中，我将使用这个端点来获取数据到一个 Flutter 前端应用程序。因此，请查看即将发布的帖子和快乐编码。