# 按需合并 LiveData

> 原文：<https://medium.com/nerd-for-tech/merging-livedata-like-you-need-it-3abcf6b756ca?source=collection_archive---------4----------------------->

## 如何将任意类型的多个 LiveData 合并成一个 LiveData。

![](img/fb45cdd96485e7d27f0a244062ef7d64.png)

信用:[信用](http://s3.amazonaws.com/clarocity-wordpress/clarocity/wp-content/uploads/2017/03/20230657/road_merging-righttrim.jpg)

在与`LiveData`合作的几个项目中，我经常面临与`MediatorLiveData`相关的挑战:

*   它允许一个接一个地添加多个源，但是没有一个源知道其他源的值。如果一个源发生了变化，并不是所有的源值都可以通过相互作用来确定新值。
*   它不能用初始源和映射进行实例化。

在将代码块从一个项目复制到另一个项目之后——将类改为方法，然后再转回来——我最终得到了这里给出的解决方案。我们的目标是有一个通用的解决方案，它可以很容易地集成并适合每一个用例——*编码一次，在任何地方使用*。

`MergerLiveData`，对`MediatorLiveData`的扩展，解决了将一个或多个源类型转换成一个目标类型的挑战:

当第一个观察者被连接时，`MergerLiveData`订阅一个或多个源的改变，当最后一个观察者分离时，移除所有的源。通过提供一个*合并函数，*你可以很容易地将任何源类型转换成任何目标类型。
只有非空值被发送到*合并函数*中，以尽可能安全地执行。通过使用`postValue(T)`将合并结果异步发送到主线程上，也允许在后台服务中使用`MergerLiveData`。`DistinctUntilChanged`规定是过账所有值，还是仅过账与当前值不同的值。

通过使用`sealed class`，所有的实现都被分组到一个根下，IDE 自动完成功能可以得到最好的利用——只需输入`MergerLiveData.`,所有类型都会显示出来。
尽管在`MergerLiveData.One`的情况下，这是一对一的映射，因此该类应该被称为`MappingLiveData`。这个特例保留在`MergerLiveData`组中，以保持尽可能高的一致性和易用性。它的变换函数叫做*映射。*这可能看起来不一致，但是有了*结尾的 lambdas* ,这个函数基本上不会作为(命名的)参数传递，所以函数名反映了执行的动作。

`MergerLiveData`的一个用例是观察联系人表单的多个`LiveData`输入，如果满足某些条件，就启用提交按钮。

在不使用`MergerLiveData`的情况下，验证在布局内执行，因此业务逻辑在*视图模型*和*布局*之间分离。此外，组合布尔表达式在 XML 文件中可读性不强。

通过引入一个`MergerLiveData.Two`，布尔表达式可以转移到*视图模型*中。因此，所有的业务逻辑都集中在一个类中，而不是分散在两个文件中。作为一名开发人员，您只需查看视图模型*的内部，这将成为唯一的事实。此外，通过将复杂的布尔语句分成不同的方法或变量，可以更好地描述它们。
最后，`MergerLiveData`通过允许立即设置初始源和映射简化了`MediatorLiveData`的创建——无需使用利用`apply()`的通用工作区:*

```
val mediatorLiveData = MediatorLiveData<T>().apply { 
       addSource(source) { value ->          
           // do stuff          
       }     
}
```

`MergerLiveData`的其他用例可能是:

*   将多个本地(例如使用[房间](https://developer.android.com/jetpack/androidx/releases/room))或远程数据源转换为一个 UI 模型
*   将多个源合并成一个代表当前 UI 状态的`[Composable](https://developer.android.com/jetpack/compose)`

`MergerLiveData`是 [*LiveData-Kit*](https://github.com/DanielKnauf/livedata-kit) 的一部分，通过 [JitPack](https://jitpack.io) 可以很容易的将其包含到一个项目中。你可以在 GitHub 上找到所有必要的代码行。

感谢阅读！

[](https://github.com/DanielKnauf/livedata-kit) [## 丹尼尔可耐福/livedata-kit

### 在 GitHub 上创建一个帐户，为 DanielKnauf/livedata-kit 的开发做出贡献。

github.com](https://github.com/DanielKnauf/livedata-kit)