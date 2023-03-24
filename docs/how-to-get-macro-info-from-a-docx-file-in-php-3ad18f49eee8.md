# 如何在 PHP 中从 DOCX 文件中获取宏信息

> 原文：<https://medium.com/nerd-for-tech/how-to-get-macro-info-from-a-docx-file-in-php-3ad18f49eee8?source=collection_archive---------15----------------------->

宏本质上是可以构建到应用程序或文档中的代码，可以自动执行重复的任务，有助于提高生产率和简化流程。宏的唯一缺点之一是，如果不是您自己构建的，它们会带来安全风险。利用 PHP 中的以下 API，您可以检索关于 DOCX 文件中定义的宏的信息，使您能够评估文件的安全性。

![](img/f1eef823b68d09a3b0a841d529dca120.png)

要使用 API，首先运行以下命令来安装 SDK:

```
composer require cloudmersive/cloudmersive_document_convert_api_client
```

安装完成后，我们就可以调用 get 宏函数了:

```
<?php
require_once(__DIR__ . '/vendor/autoload.php');// Configure API key authorization: Apikey
$config = Swagger\Client\Configuration::getDefaultConfiguration()->setApiKey('Apikey', 'YOUR_API_KEY');$apiInstance = new Swagger\Client\Api\EditDocumentApi(

    new GuzzleHttp\Client(),
    $config
);
$input_file = "/path/to/inputfile"; // \SplFileObject | Input file to perform the operation on.try {
    $result = $apiInstance->editDocumentDocxGetMacroInformation($input_file);
    print_r($result);
} catch (Exception $e) {
    echo 'Exception when calling EditDocumentApi->editDocumentDocxGetMacroInformation: ', $e->getMessage(), PHP_EOL;
}
?>
```

如果操作成功运行，您将获得 Word 文档中任何已识别宏的信息。