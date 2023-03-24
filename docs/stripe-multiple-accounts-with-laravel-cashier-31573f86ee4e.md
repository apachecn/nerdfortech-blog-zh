# 用 Laravel 收银机将多个账户分条

> 原文：<https://medium.com/nerd-for-tech/stripe-multiple-accounts-with-laravel-cashier-31573f86ee4e?source=collection_archive---------8----------------------->

![](img/efb144f88589b5da5a870482b7342e40.png)

[斯科特·格雷厄姆](https://unsplash.com/@homajob?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

我试图解决的问题如下。假设您有两家公司销售几乎相同类型的套餐，但每家公司都位于不同的国家，适用不同的法律，因此您开设了两个 stripe 帐户，并希望将它们集成到您当前运行的支持一个 Stripe 帐户的应用程序中。

开箱即用的 Laravel 收银机支持一个条纹账户，你把你的关键和秘密，你都设置好了。

**作为一个想法，这个解决方案只对登录用户有效。**

所以我是这样解决我的问题的。我有来自两个不同国家的客户。在此基础上，我根据这些国家划分了我的计划，因此我的计划、国家和用户表如下所示。

查看 vendor/laravel/cashier/src/billable . PHP 中的收银员代码，我们注意到许多调用如下所示:

```
StripeCustomer::retrieve($this->stripe_id, $this->stripeOptions());

StripePaymentIntent::create($options, $this->stripeOptions());

StripePaymentIntent::retrieve($paymentIntent, $this->stripeOptions());
```

**$ this->stripe options()**实现为:

```
// vendor/laravel/cashier/src/Concerns/ManagesCustomer.php

*/**
 * Get the default Stripe API options for the current Billable model.
 *
 ** ***@param*** *array  $options
 ** ***@return*** *array
 */* **public function** stripeOptions(**array** $options = [])
{
    **return** Cashier::*stripeOptions*($options);
}
```

这又来自于:

```
// vendor/laravel/cashier/src/Cashier.php*/**
 * Get the default Stripe API options.
 *
 ** ***@param*** *array  $options
 ** ***@return*** *array
 */* **public static function** stripeOptions(**array** $options = [])
{
    **return** *array_merge*([
        **'api_key'** => config(**'cashier.secret'**),
        **'stripe_version'** => **static**::***STRIPE_VERSION***,
    ], $options);
}
```

解决方案是用代码在 **$options** 数组中设置适当的秘密来覆盖**可收费的**特征。为了实现我们的目标，我们创建了一个表，其中存储了每个国家的条带帐户，如下所示:

到目前为止，我们知道我们有来自不同国家的用户，每个国家都有一个条纹帐户。我们在用户模型中覆盖了 **stripeOptions** ，它看起来像下面这样:

**提示:**作为一种安全措施，不要存储来自任何平台的普通密钥和秘密。一个解决方案是加密敏感字段。其思想是在插入时加密数据，并在需要时解密。有很多库可以帮助你做到这一点。

另一件事，我做了，以确保我的解决方案的工作，我必须从收银台覆盖支付控制器。为此，我们首先采用我们的控制器，而不是来自供应商的控制器:

```
$this->app->bind(\Laravel\Cashier\Http\Controllers\PaymentController::class, \App\Http\Controllers\PaymentController::class);
```

这一行你需要放入 **AppServiceProvider。**最终的解决方案看起来有点像下图。