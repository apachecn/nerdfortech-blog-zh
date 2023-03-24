# 运行时更改翻新基础 URL。

> 原文：<https://medium.com/nerd-for-tech/change-retrofit-base-url-on-runtime-2036ef1dee44?source=collection_archive---------0----------------------->

之间切换多个基本网址喜欢产品|开发改造容易。

![](img/3f9b997489d744178294dc856ca89bce.png)

🗡Hi，很高兴在这里见到你。在这篇文章中，我们将实现一个简短的技术来改变改造基础网址。

# **简介**

**改型**是 Square 开发的一款用于 Android、Java 和 Kotlin 的类型安全 HTTP 客户端。该库提供了一个强大的框架，用于验证 API 并与之交互，以及使用 [OkHttp](http://square.github.io/okhttp/) 发送网络请求。

我们通过提供默认的基本 url 来实例化改造实例。

```
return new Retrofit.Builder()
            .client(okHttpClient)
            .baseUrl(BuildConfig.PRODUCTION_BASE_URL)
            .addConverterFactory(-)
            .addCallAdapterFactory(-)
            .build();
```

**问题报告**

运行时在不同的 url 之间切换很困难。为了解决这个困难，开发者被迫在应用程序中使用两个改型实例。为了避免在应用程序中出现这两种或两种以上的改型，我们开发人员使用了不同的技术，例如

1.  更新动态 URL——冗长乏味
2.  借助共享首选项✅更改主机 URL 的拦截器
3.  将 BASE_URL 直接存储在共享 Pref 中—
4.  reinit ok http/翻新—

更多..

*这里，我们将实施方法 2。*

让我们通过设置依赖项来深入了解一下。

```
ext {
      okHttpVersion = "4.8.1"
      retrofitVersion = "2.9.0"
      hiltVersion = "2.38.1"
}
// Implementation
// OkHttp
implementation "com.squareup.okhttp3:okhttp:$rootProject.okHttpVersion"
implementation "com.squareup.okhttp3:logging-interceptor:$rootProject.okHttpVersion"

// Retrofit2
implementation "com.squareup.retrofit2:retrofit:$rootProject.retrofitVersion"
implementation "com.squareup.retrofit2:converter-gson:$rootProject.retrofitVersion"
implementation "com.github.akarnokd:rxjava3-retrofit-adapter:3.0.0"
implementation "com.squareup.retrofit2:converter-protobuf:$rootProject.retrofitVersion"

// Hilt
implementation "com.google.dagger:hilt-android:$rootProject.hiltVersion"
annotationProcessor "com.google.dagger:hilt-android-compiler:$rootProject.hiltVersion"
annotationProcessor 'androidx.hilt:hilt-compiler:1.0.0'
```

> App.java
> 
> 带匕首 2 刀柄。

```
@HiltAndroidApp
public class App extends Application {

    @Override
    public void onCreate() {
        super.onCreate();
        Hawk.init(this).build();
        AppLog.init();
    }

    @Override
    protected void attachBaseContext(Context base) {
        super.attachBaseContext(base);
        MultiDex.install(this);
    }
}
```

现在，创建一个带有两个助手函数的 preference helper(SharedPrefManager)来读/写当前环境选择模式的状态。

```
 // Store & Get Prod or Dev Environment

 void setProdEnvironmentStatus(boolean status);
 boolean isProdEnvironment();
```

然后，创建**拦截器**来处理切换环境工作。😮

对于默认的基本 url，将使用 DEVELOPMENT_BASE_URL 注入更新，假定为应用程序的默认选择。

```
@Provides
@Singleton
public Retrofit provideRetrofitClient(ProtoConverterFactory protoConverterFactory, RxJava3CallAdapterFactory rxJava3CallAdapterFactory, OkHttpClient okHttpClient) {return new Retrofit.Builder()
            .client(okHttpClient)
            .baseUrl(BuildConfig.DEVELOPMENT_BASE_URL)
            .addConverterFactory(protoConverterFactory)
            .addCallAdapterFactory(rxJava3CallAdapterFactory)
            .build();
}
```

**现在**提供**HostSelectionInterceptor**依赖给 NetworkModule.java 的 hant as，

```
@Provides
@Singleton
public HostSelectionInterceptor provideHostSelectionInterceptor(PreferenceHelper preferenceHelper) {
    return new HostSelectionInterceptor(preferenceHelper);
}
```

然后，将这个拦截器提供给我们的 OkHttpClient 构建器，如下所示。

```
@Provides
@Singleton
public OkHttpClient provideOkHttpClient(@ApplicationContext Context context, HttpLoggingInterceptor httpLoggingInterceptor, HostSelectionInterceptor hostSelectionInterceptor) {

    long cacheSize = 5 * 1024 * 1024;
    Cache mCache = new Cache(context.getCacheDir(), cacheSize);
    if (BuildConfig.DEBUG) {
        return new OkHttpClient().newBuilder()
                .cache(mCache)
                .retryOnConnectionFailure(true)
                .connectTimeout(200, TimeUnit.SECONDS)
                .readTimeout(200, TimeUnit.SECONDS)
                .addInterceptor(httpLoggingInterceptor)
                .addInterceptor(hostSelectionInterceptor)
                .followRedirects(true)
                .followSslRedirects(true)
                .build();

    } else {
        return new OkHttpClient().newBuilder()
                .connectTimeout(200, TimeUnit.SECONDS)
                .readTimeout(200, TimeUnit.SECONDS)
                .retryOnConnectionFailure(true)
                .followRedirects(true)
                .followSslRedirects(true)
                .addInterceptor(hostSelectionInterceptor)
                .build();
    }

}
```

现在，我们只需提供@ **Inject** 注释，就可以**将****HostSelectionInterceptor**注入到我们的视图模型或活动中。

```
@Inject
    HostSelectionInterceptor hostSelectionInterceptor;
```

**最后**，我们可以在这些环境之间切换，只需更改共享偏好设置上的选择模式状态，并调用**HostSelectionInterceptor**中的 **setHostBaseUrl** 函数。

![](img/44e22776b2f3b4717529da6f0106c60b.png)

“开发”和“生产”环境的微调开关示例。

```
private void doSetupHostSelection() {

    ArrayAdapter<String> arrayAdapter = new CustomSpinnerAdapter(this).spinnerAdapter();
    //Setting the ArrayAdapter data on the Spinner
    arrayAdapter.add("DEV");
    arrayAdapter.add("PROD");
    getViewDataBinding().spinnerHostSelection.setAdapter(arrayAdapter);

// selector    
getViewDataBinding().spinnerHostSelection.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
        @Override
        public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {

            if (position == 0) {
                preferenceHelper.setProdEnvironmentStatus(false);
                hostSelectionInterceptor.setHostBaseUrl();

            } else {
                preferenceHelper.setProdEnvironmentStatus(true);
                hostSelectionInterceptor.setHostBaseUrl();
            }
        }

        @Override
        public void onNothingSelected(AdapterView<?> parent) {

        }
    });

}
```

这个函数 **setHostBaseUrl** 将完成剩下的工作。

```
public void setHostBaseUrl() {
    if (preferenceHelper.isProdEnvironment()) {
        this.host = HttpUrl.parse(BuildConfig.PRODUCTION_BASE_URL);
    } else {
        this.host = HttpUrl.parse(BuildConfig.DEVELOPMENT_BASE_URL);
    }
}
```

就这些…感谢您的阅读…

> Ps:上面的实现只是一个样本，我们如何使用不同的网址进行改造。这种技术的实现取决于您的需求。

抓住这个想法并以你最好的方式实现它！

如有任何困惑或建议，请填写评论部分🐧