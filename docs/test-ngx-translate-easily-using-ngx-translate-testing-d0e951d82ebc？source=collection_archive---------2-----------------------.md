# 使用 ngx-translate-testing 轻松测试 ngx-translate

> 原文：<https://medium.com/nerd-for-tech/test-ngx-translate-easily-using-ngx-translate-testing-d0e951d82ebc?source=collection_archive---------2----------------------->

您可以使用 **ngx-translate-testing** 库在您的应用程序中测试 ngx-translate **国际化**支持。让我们开始吧。

***安装:***

```
npm install @ngx-translate-testing --save-dev
```

***用法:***

将 *TranslateTestingModule* 添加到您想要测试的规范文件中，如下所示:

```
**import** { TranslateTestingModule } **from** 'ngx-translate-testing';describe('HomeComponent', () => {
  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [ TranslateTestingModule.withTranslations({}) ]
    });
  }});
```

如果您在 html 文件中只使用了 TranslatePipe，并且不需要在组件中测试任何翻译，那么 TranslateTestingModule 的上述配置就足够了。

如果您在 component.ts 文件中使用了 TranslateService，并且想要测试翻译，请按如下方式修改配置:

```
const translations = {
    'login': {
        'errorMessage': { 
          'Something went wrong': 'Something went wrong.'
        } 
     }
};describe('TranslationService', () => {
  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [ 
        TranslateTestingModule.withTranslations('en', translations)
      ]
    });
  }});
```

将您想要测试的所有翻译添加到 translations 数组中，并将其作为参数提供给 withTranslations 和 locale。

您可以通过如下链接方式添加多个翻译:

```
TranslateTestingModule
  .withTranslations('en', ENGLISH_TRANSLATION),
  .withTranslations('he', HEBREW_TRANSLATION),
  .withTranslations('de', '../../assets/i18n/de.json')
```

您也可以指定默认语言，如下所示:

```
TranslateTestingModule.withDefaultLanguage('en')
```

开心测试:)鼓掌看更多这样的帖子:)