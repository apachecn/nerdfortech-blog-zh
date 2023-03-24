# 在 Android 对话框中集成谷歌地图

> 原文：<https://medium.com/nerd-for-tech/integrate-google-maps-in-android-dialogfragment-a8536c581e95?source=collection_archive---------6----------------------->

使用**谷歌地图** & **公开天气 API** 获取固定位置天气。

![](img/170c793fc7d2c4376cb320f68a7f61c3.png)

照片由 [GeoJango Maps](https://unsplash.com/@geojango_maps?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/map-pin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

🚀嗨，在这篇文章中，我们将把谷歌地图整合到安卓对话框中。我们将构建一个应用程序，用户将从对话框屏幕锁定一个位置，并使用**开放天气 API** 显示该地区的当前天气。

匆匆忙忙！这是给你的项目链接。

[](https://github.com/iamnaran/bloomweather) [## 伊姆纳兰/布卢姆韦瑟

### 天气应用程序。在 GitHub 上创建一个帐户，为 iamnaran/bloomweather 的发展做出贡献。

github.com](https://github.com/iamnaran/bloomweather) 

让我们开始吧。

首先让我们为谷歌地图和开放天气 API 获取 **API 键**。

1.  [获取 API 密钥](https://developers.google.com/maps/documentation/android-sdk/get-api-key) —谷歌地图
2.  [获取 API 密钥](https://openweathermap.org/api) —打开天气 API

为了简单起见，本教程是用 Java-Android &使用改型& RxJava 编写的

> 项目结构

![](img/2a2426f2ff2a5ef5b90fef9467f59d49.png)

项目结构

我假设我们都有地图和天气 API 键。复制您的 API 密钥，并将**BASE _ URL**&**WEATHER _ API _ KEY**添加到(**build . gradle)build config**

```
BASE_URL == https://api.openweathermap.org/
```

![](img/1d9a7dbb43bdb1752e60ecaa151242a5.png)

添加基本 URL 和天气 API 密钥

然后，添加您的地图 API 密钥

在你的 **local.properties** 中添加 MAPS_API_KEY

```
MAPS_API_KEY=AIzaSyBoOA6wz0XayigGxsR7sA0TdpVeo1hpiOo
```

![](img/b8e5126bebf99f8c025b35f84360a583.png)

地图 API 键

要将这些 MAPS_API_KEY 导出到我们的应用程序中，请将它们添加到您的 **build.gradle** (应用程序模块)中

```
plugins **{** id 'com.android.application'
    id 'com.google.secrets_gradle_plugin' version '0.5'**}**implementation 'com.google.android.gms:play-services-maps:17.0.1'
implementation 'com.google.android.gms:play-services-location:18.0.0'
```

并将它们添加到应用程序标签内的 **AndroidManifest.xml** 中。

```
<meta-data
    android:name="com.google.android.gms.version"
    android:value="@integer/google_play_services_version" />

<meta-data
    android:name="com.google.android.geo.API_KEY"
    android:value="${MAPS_API_KEY}" />
```

现在，所有凭证设置都已完成。

![](img/5bc31d708ac4da9dcffa9cc017ba8602.png)

资料来源:Giphy

让我们在对话框中集成谷歌地图。在此之前，让我们为我们的开放天气 API 创建 API 服务。

> 天气预报服务

```
public interface WeatherApiService {

    @Headers("x-api-key: " + BuildConfig.*WEATHER_API_KEY*)
    @GET("data/2.5/forecast")
    Observable<WeatherData> requestWeather(
            @Query("lat") String lat, @Query("lon") String lon, @Query("units") String units, @Query("cnt") String count
    );

}
```

> 其中 WeatherData 是从开放气象 API 响应中为我们的应用程序自动生成的模型。[参见型号](https://gist.github.com/iamnaran/d4316619c0d2f27ce509faf443f38efd)

现在，让我们创建一个控制器来调用我们的开放天气 API。

> WeatherController.java

现在，是时候将谷歌地图集成到我们的对话片段中了。

![](img/179ec7482c41cdc7188c5d3bf367f568.png)

首先，我们需要这些权限来使我们的地图视图工作。

```
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

为了获得固定位置纬度和经度，我们创建了一个简单的接口。

```
public interface OnLocationSelectedListener {
    void onLocationSelected(Double lat, Double lng);
}
```

并且我们可以从**FusedLocationProviderClient**获取设备位置。

```
private FusedLocationProviderClient fusedLocationProviderClient;

fusedLocationProviderClient = LocationServices.*getFusedLocationProviderClient*(getActivity());
```

我是这样做的。

仅此而已。从下面抓取源代码。

[](https://github.com/iamnaran/bloomweather) [## 伊姆纳兰/布卢姆韦瑟

### 天气应用程序。在 GitHub 上创建一个帐户，为 iamnaran/bloomweather 的发展做出贡献。

github.com](https://github.com/iamnaran/bloomweather) 

谢谢你。

![](img/bbb5e68aaf0d5fe447a0d68e0f6f48de.png)