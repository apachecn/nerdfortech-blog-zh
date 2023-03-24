# 如何基于 Android 架构组件和协程编写 app 的单元测试

> 原文：<https://medium.com/nerd-for-tech/how-to-write-unit-test-of-app-based-on-android-architecture-components-and-coroutine-8e5473f6674c?source=collection_archive---------6----------------------->

> 单元测试是应用测试策略中的基础测试。通过针对您的代码创建和运行单元测试，您可以轻松地验证各个单元的逻辑是否正确。在每次构建后运行单元测试有助于您快速捕捉和修复由应用程序代码更改引入的软件退化。
> 
> 单元测试通常以可重复的方式测试最小可能代码单元(可以是方法、类或组件)的功能。当您需要验证应用程序中特定代码部分的逻辑时，您应该构建单元测试。例如，如果您正在对一个类进行单元测试，您的测试可能会检查该类是否处于正确的状态。典型地，代码单元被孤立地测试；您的测试只影响和监控那个单元的变化。你可以使用依赖提供者，比如 Robolectric 或者一个 [mocking framework](https://en.wikipedia.org/wiki/Mock_object) 来隔离你的单元和它的依赖。
> 
> [https://developer.android.com/training/testing/unit-testing](https://developer.android.com/training/testing/unit-testing)

正如谷歌的文件中所说，单元测试是软件开发中保证交付质量的重要环节。所以我将介绍如何给 Android App 编写单元测试。在本教程中，我们将使用`JUnit`、`[Spek](https://www.spekframework.org/setup-android/)`和`[Mockk](https://github.com/mockk/mockk)`。

# 先决条件

在本教程中，我们将解决根据[https://medium . com/nerd-for-tech/Android-architecture-components-tutorial-7e 822 CDF 2f 00](/nerd-for-tech/android-architecture-components-tutorial-7e822cdf2f00)构建的 app 的单元测试。所以让我们根据上面的教程来搭建这个 app。

app 构建成功后，让我们检查`com.ovlesser.pexels(test)`包下的单元测试，你会发现有一个名为`ExampleUnitTest`的文件，它是由模板生成的。我们会离开它，创建我们自己的单元测试。

## 安装 Spek 插件

我们需要做的第一件事是在 Android Studio 中安装`Spek` / `Spek Framework`插件。

启动 Android Studio，从菜单中打开`Android Studio` / `Preferences` / `Plugins`。然后搜索`Spek`，安装`Spek`和`Spek Framework`。安装完成后，您可能需要重启 Android Studio。

## 为单元测试添加依赖项

打开`build.gradle(Module: Pexels.app)`并将以下依赖关系添加到`dependencies`块和`Sync Now`中。

```
*// Unit testing* testImplementation 'org.jetbrains.kotlin:kotlin-reflect:1.4.10'
testImplementation "org.spekframework.spek2:spek-dsl-jvm:2.0.9"
testImplementation "org.spekframework.spek2:spek-runner-junit5:2.0.9"
testImplementation 'org.jetbrains.kotlinx:kotlinx-coroutines-test:1.4.3'
testImplementation "org.assertj:assertj-core:3.15.0"
implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk8:$kotlin_version"*// Mocking for Kotlin* testImplementation "io.mockk:mockk:1.10.0"
testImplementation "io.mockk:mockk-agent-jvm:1.11.0"
```

## 创建单元测试类

添加完依赖项后，让我们在`com.ovlesser.pexels(test)`包下创建几个新包`com.ovlesser.pexels.ui.home`和`com.ovlesser.pexels.ui.detail`。我们还需要在`com.ovlesser.pexels.ui.home`包下为`HomeViewModel`创建几个单元测试类`HomeViewModelTest`，在`com.ovlesser.pexels.ui.detail`包下为`DetailViewModel`创建`DetailViewModeTest`。

为什么我们只创建两个测试类？那是因为根据`MVVM`模式，app 包含了`Model (M)`、`View (V)` 和`ViewModel (VM)`三个部分。但是`Model`只是数据，不需要测试任何方法，而且`View`是 UI 相关的，需要用单元测试来测试。所以当编写单元测试时，我们将只关注`ViewModel`部分。这就是为什么我们只需要两个测试类来覆盖`HomeViewModel`类和`DetailViewModel`类。

好了，让我们先为`DetailViewModel`写单元测试，因为这个类比`HomeViewModel`简单。我们将只为`DetailViewModel`类的构造添加单元测试。

## DetailViewModelTest

让我们打开`DetailViewModelTest.kt`，并将`DetailViewModelTest`类更改如下:

```
class DetailViewModelTest: Spek(**{

}**)
```

这是一个带有 Spek 的测试类的框架。

我们需要在这个类中添加下面的`Application`类的模拟对象，因为`DetailViewModel`类有一个`application`对象的参数

```
val mockApplication = *mockk*<Application>() **{** *every* **{** *applicationContext* **}** returns *mockk*()
**}**
```

并实例化一个`Photo`对象，作为`DetailViewModel`类的另一个参数如下:

```
val photo = Data.Photo(id = 123, width = 200, height = 120)
```

并声明一个变量`detailViewModel`作为测试对象，如下所示:

```
lateinit var detailViewModel: DetailViewModel
```

注意，我们只是声明了它而没有实例化它，因为在`constructor` / `init`块中有一些`LiveData`更新，由于此类`LiveData`更新不能在`Main`线程中执行，这将触发以下错误:

```
Method getMainLooper in android.os.Looper not mocked. See [http://g.co/androidstudio/not-mocked](http://g.co/androidstudio/not-mocked) for details.
```

我们将通过以下方法修复它。

```
beforeGroup **{** ArchTaskExecutor.getInstance().setDelegate(object : TaskExecutor() {
        override fun executeOnDiskIO(runnable: Runnable) {
            runnable.run()
        }

        override fun isMainThread(): Boolean {
            return true
        }

        override fun postToMainThread(runnable: Runnable) {
            runnable.run()
        }
    })
    detailViewModel = *spyk*(DetailViewModel(photo, mockApplication))
**}**
```

如图所示，我们覆盖了`beforeGroup`块中的`ArchTaskExecutor`，它将在真正的测试代码运行之前运行。这段代码确保我们可以无错地运行`LiveData`更新。

我们还在`beforeGroup`块中实例化了`detailViewModel`对象。它将被成功创建，因为我们已经覆盖了`ArchTaskExecutor`。

而`spyk`是`Mockk`的一个关键字，通过它`detailViewModel`对象可以包含真实属性和模拟属性。

然后让我们编写如下所示的测试用例:

```
*describe*("init") **{** it("should init successfully") **{** assertThat(detailViewModel.photo.*value*).isEqualTo(photo)
    **}** it("should return correct size") **{** detailViewModel.size.observeForever **{** assertThat(detailViewModel.size.*value*).isEqualTo("${photo.width} * ${photo.height}")
        **}** detailViewModel.size.removeObserver **{}
    }
}**
```

如图所示，我们有两个测试用例来测试`DetailViewModel`类的初始化:验证`LiveData`属性`detailViewModel.photo`设置成功，验证另一个`LiveData`属性`detailViewModel.size`将返回正确的值。

小心，我们需要观察带有`LiveData.observeForever`的`LiveData`属性`detailViewModel.size`，一旦`LiveData`的值改变，就会触发 lambda 函数。

现在让我们通过点击`DetailViewModelTest`类前的绿色小三角形来运行单元测试，我们会发现两个测试通过了。

这是一个非常简单的用`Spek` / `Mockk`进行单元测试的例子。我们将为`HomeViewModel`类添加更复杂的单元测试。

## HomeViewModelTest

让我们打开`HomeViewModelTest.kt`，它应该在`com.ovlesser.pexels(test).ui.home`包里，把骨架写成如下:

```
class HomeViewModelTest: Spek(**{** val mockApplication = *mockk*<Application>() **{** *every* **{** *applicationContext* **}** returns *mockk*()
    **}** lateinit var homeViewModel: HomeViewModel

    beforeGroup **{** ArchTaskExecutor.getInstance().setDelegate(object : TaskExecutor() {
            override fun executeOnDiskIO(runnable: Runnable) {
                runnable.run()
            }

            override fun isMainThread(): Boolean {
                return true
            }

            override fun postToMainThread(runnable: Runnable) {
                runnable.run()
            }
        })
        homeViewModel = *spyk*(HomeViewModel(mockApplication))
    **}
}**)
```

如图所示，它与`DetailViewModelTest`类非常相似。

让我们添加第一个测试用例如下:

```
*describe*("init") **{** it("should init successfully") **{** homeViewModel.data.observeForever **{** Assertions.assertThat(homeViewModel.data.*value*).isEqualTo()
        **}
    }
}**
```

我们会发现第一个问题，`homeViewModel.data.value`的预期结果是什么？我们不知道，因为这个值来自`pexelsPhotoRepository`和`database.pexelsPhotoDao.getPhotos()`。它来自数据库，取决于数据库中保存的数据。更糟糕的是，数据库中的数据可能会发生变化，因为我们正在发送 HTTP 请求从后端服务获取新数据，一旦请求成功返回，新数据可能会被插入到数据库中。

所以我们会发现数据库和网络服务是`HomeViewModel`类的依赖项，而`HomeViewModel`类中的数据依赖于它的依赖项。这违反了单元测试的隔离原则。因此，我们需要模拟所有这些依赖关系，并确保模拟的依赖关系会给出我们期望的值。

让我们检查一下`HomeViewModel`类的实现，我们会发现网络服务的实例`PexelApi.retrofitService`在`PexelsPhotoRepository`类中，`PexelsPhotoRepository`实例在`HomeViewModel`类中，数据库对象也在`HomeViewModel`类中被实例化，并被传递给`PexelsPhotoRepository`实例。它们是完全耦合的，很难隔离。所以我们需要做的第一件事就是根据`Dependency Injection (DI)`的原则修改这些类。

我们将改变`HomeViewModel`类的签名，使其依赖关系可以被注入，因为当真正的应用程序运行和单元测试运行时，我们需要不同的依赖关系。因此，我们需要将`HomeViewModel`类的签名更改如下:

```
class HomeViewModel(apiService: PexelsApiService? = null, dao: PexelsPhotoDao? = null, application: Application) : AndroidViewModel(application)
```

如你所见，我们在`HomeViewModel`的构造函数中为依赖注入引入了两个额外的可选参数`apiService`和`dao`。但是我们将它们设置为可选的，因为我们不想在真正的应用程序运行时注入它们。我以后会解释的。

接下来，我们需要将 pexelsPhotoRepository 的实例化改为

```
private val pexelsPhotoRepository = PexelsPhotoRepository(apiService = apiService, dao = dao ?: *getDatabase*(application).pexelsPhotoDao)
```

将`apiService`和`dao`传入储存库。请注意，我们使用的是真实数据库`HomeViewModel`、`dao`的参数是`null`，这是真实应用运行的情况。

我们还需要将内部类`HomeViewModel.Fatory`的`create`方法从

```
return HomeViewModel(app) as T
```

到

```
return HomeViewModel(application = app) as T
```

因为我们改变了`HomeViewModel`类的构造函数的签名。

然后我们会注意到在创建`pexelsPhotoRepository`对象时的错误，因为在`PexelsPhotoRepository`类的构造函数中没有新的参数。让我们通过更改该类的构造函数来添加它们，如下所示:

```
class PexelsPhotoRepository(
    private val apiService: PexelsApiService? = PexelApi.retrofitService,
    private val dao: PexelsPhotoDao) {
```

同上，我们将使用真实的`PexelApi`。retrofitService 如果参数`apiService`为空，表示真正的 app 正在运行。

我们还需要将这个存储库类中的所有`database.pexelsPhotoDao`更改为`dao`，以确保我们使用的是传入的`dao`。

好了，我们已经根据依赖注入的原则做了修改。让我们先运行应用程序，以确保我们没有搞砸任何事情。

现在我们已经能够在运行单元测试时注入模拟的`ApiService`和模拟的`dao`。因此，让我们实现被嘲笑的依赖。

## 测试锁

首先，让我们在`com.ovlesser.pexels(test).ui.home`包中创建一个新文件 TestingMocks，并创建一个 MockApiService 类，如下所示:

```
class MockApiService(var result: Data): PexelsApiService {
    override fun getData(keyword: String, pageIndex: Int, perPage: Int) = MockCall<Data>(result)

    override suspend fun getDataCoroutine(keyword: String, pageIndex: Int, perPage: Int) = result
}
```

及其所属的`MockCall`类如下所示:

```
class MockCall<T>(result: Data): Call<T> {
    override fun clone(): Call<T> {
        *TODO*("Not yet implemented")
    }

    override fun execute(): Response<T> {
        *TODO*("Not yet implemented")
    }

    override fun enqueue(callback: Callback<T>) {
        *TODO*("Not yet implemented")
    }

    override fun isExecuted(): Boolean {
        *TODO*("Not yet implemented")
    }

    override fun cancel() {
        *TODO*("Not yet implemented")
    }

    override fun isCanceled(): Boolean {
        *TODO*("Not yet implemented")
    }

    override fun request(): Request {
        *TODO*("Not yet implemented")
    }

    override fun timeout(): Timeout {
        *TODO*("Not yet implemented")
    }

}
```

正如我们看到的，我们模仿了方法`getData`，它将返回由`MockCall`包装的输入值，以及`getDataCoroutine`，它将直接返回输入值。

我们还需要创建另一个模拟类`MockDao`，来模拟`PexelsPhotoDao`类，如下所示:

```
class MockDao( val data: Data): PexelsPhotoDao {
    private val _photos = MutableLiveData<List<DatabasePexelsPhoto>>()

    override fun getPhotos(): LiveData<List<DatabasePexelsPhoto>> {
        _photos.*value* = data.photos.*asDatabaseModel*()
        return _photos
    }

    override fun insertAll(photos: List<DatabasePexelsPhoto>) {
        _photos.*value* = photos
    }

    override fun clearAll() {
        _photos.*value* = *emptyList*()
    }
}
```

我们也覆盖了`PexelsPhotoDao`类的方法。

现在我们已经有了`ApiService`和`dao`的模拟类，让我们回到`HomeViewModelTest`类并添加下面的代码

```
val data = Data( photos = *listOf*(Data.Photo(id = 111), Data.Photo(id = 222), Data.Photo(id = 333)), totalResults = 3)
val mockApiService = MockApiService(data)
val mockDao = MockDao(data)
```

在这段代码中，我们用一些数据创建了两个模拟对象`mockApiService`和`mockDao`，所以这些模拟对象会用这些数据模拟回报。然后我们能够验证它们。

然后，我们需要将这些被模仿的对象注入到`homeViewModel`对象中，如下所示:

```
homeViewModel = *spyk*(HomeViewModel(apiService = mockApiService, dao = mockDao, application = mockApplication))
```

现在，`homeViewModel`并不依赖真正的网络服务或真正的数据库。然后我们可以将测试用例`init`的断言改为

```
Assertions.assertThat(homeViewModel.data.*value*).isEqualTo(data)
```

让我们运行这个测试用例，您会发现它通过了。

现在让我们添加更多的测试用例来测试`HomeViewModel`类的其他公共方法，如下所示:

```
*describe*("#${HomeViewModel::refreshRepository.name}") **{** it("should return") **{** homeViewModel.refreshRepository("")
        Assertions.assertThat(homeViewModel.status.*value*).isEqualTo(PexelsApiStatus.*DONE*)
    **}
}** *describe*("#${HomeViewModel::displayPhotoDetails.name}") **{** it("should return") **{** val photo = Data.Photo(id = 123)
        homeViewModel.displayPhotoDetails(photo)
        Assertions.assertThat(homeViewModel.selectedPhoto.*value*).isEqualTo(photo)
    **}
}** *describe*("#${HomeViewModel::displayPhotoDetailComplete.name}") **{** it("should return null") **{** homeViewModel.displayPhotoDetailComplete()
        Assertions.assertThat(homeViewModel.selectedPhoto.*value*).isNull()
    **}
}**
```

让我们点击`HomeViewModelTest`类前面的绿色三角形，我们将看到 4 个测试通过。

到目前为止，我们已经为`DetailViewModel`和`HomeViewModel`增加了单元测试。

另外，如果我们想用协程测试这样的方法，还有一个问题需要解决，如下所示:

我们需要在`HomeViewModelTest`类声明上方添加以下注释:

```
@ObsoleteCoroutinesApi
@ExperimentalCoroutinesApi
```

并创建一个新的`newSingleThreadContext` 实例如下:

```
val mainThreadSurrogate = *newSingleThreadContext*("UI thread")
```

并在`beforeGroup`块中分派该线程上下文，如下所示:

```
Dispatchers.*setMain*(mainThreadSurrogate)
```

并在`afterGroup`中添加重置所有上下文，如下所示:

```
afterGroup **{** ArchTaskExecutor.getInstance().setDelegate(null)
    Dispatchers.*resetMain*()
    mainThreadSurrogate.close()
**}**
```

本教程没有涵盖单元测试的所有代码，而是给出了一些关于如何编写单元测试以及如何模拟与`Spek`和`Mockk`的依赖关系的例子。

感谢浏览我的教程，欢迎任何反馈。