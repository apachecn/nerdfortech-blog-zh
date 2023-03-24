# 更好的梅文中心

> 原文：<https://medium.com/nerd-for-tech/the-better-maven-central-90d7e529d606?source=collection_archive---------2----------------------->

![](img/b84076fc58994e2179e94ed3f27f9b1f.png)

Maven Central 是使用最多的 Java 工件存储库。几乎每个 Gradle 文件都以:

```
buildscript **{** *repositories* **{
       ** mavenCentral()
        // maybe other repos here 
```

然而，使用 Sontatype、OSSHR 和 Nexus 进行发布([这篇文章](https://proandroiddev.com/publishing-a-maven-artifact-1-3-glossary-bc0068a440e0)很好地解释了这些术语)是一个非常缓慢而痛苦的过程:

![](img/ca547bfb5c34340384b27aebc6623a6f.png)

来源:[https://jfrog . com/blog/bin tray-as-pain-free-gateway-to-maven-central/](https://jfrog.com/blog/bintray-as-pain-free-gateway-to-maven-central/)。

你可能会说 Jfrog 有偏见，但我完全同意上面所有的陈述。既然 Jfrog 替代方案 [JCenter 已经不复存在](https://jfrog.com/blog/into-the-sunset-bintray-jcenter-gocenter-and-chartcenter/)，那么对于开源项目来说，imo 只剩下一个解决方案，而且这个解决方案是惊人的: [jitpack.io](https://jitpack.io) 。

令人惊讶的是，一旦`maven`或`maven-publish`插件被配置好(这在任何情况下都需要完成)，就不需要发布工件了——>见我的另一篇文章[这里](/nerd-for-tech/oh-no-another-publishing-android-artifacts-to-maven-central-guide-9d7f300ebd74)。

如果你有一个 GitHub 帐户，几乎不需要做什么就可以让 Jitpack.io 运行起来。只需授予 Jitpack.io 访问您的公共回购的权限，就大功告成了。我说的完成是指字面上的完成。

Jitpack.io 是如此简单，以至于我甚至没有注意到我的工件，例如这个 repo:[https://github.com/1gravity/Android-RTEditor](https://github.com/1gravity/Android-RTEditor)是自动发布的(当我在 Maven Central publication 上工作时)。Jitpack 自动构建并发布了我的库:

*   因为它可以访问我的回购协议
*   因为我用版本号标记了我的提交，例如`v1.7.3`
*   因为已经配置了`maven-publish`任务
*   因为 jitpack.io 是个牛逼的工具；-)

要使用发布的工件，您需要添加 jitpack.io 作为 Maven repo，但这是与 Maven Central 中发布的工件相比唯一的小缺点:

```
allprojects {
    repositories {
	...
	maven { url 'https://jitpack.io' }
    }
}
```

我不需要经历 Sonatype 痛苦的注册过程，也不需要在我选择的开源库中建立构建管道。

我对 Android 项目(比如这里的和这里的)和后端 Java 项目(比如这里的)都是这么做的，同样简单。

![](img/aaf43f8fc1ee86b4f0bbb7cbd69b9884.png)