# 我在工作中使用的 CSS 伪类列表。

> 原文：<https://medium.com/nerd-for-tech/list-of-css-pseudo-classes-that-i-use-in-my-work-f1193b528e12?source=collection_archive---------2----------------------->

![](img/9be5f4f49ad13b56276ed9227effe863.png)

CSS 有很多伪类。在这篇文章中，我将展示我可以在前端的伪类。伪类列表小+>[]~:root:focus:hover:active:n-child:is:not:has::after::before。

:root —将样式应用于文档的根元素。这意味着我们可以设置整个页面的样式。比如我们可以全局设置字体。

+ > [] ~

你可能不会经常用到这些类，但是你可能会看到它们。因此，我需要解释他们在做什么。

[]-按属性或属性值应用样式。

~ —仅当元素具有相同的父元素时应用样式。

+ —如果元素位于他的正后方，则对元素应用样式。

>—将样式应用于子元素。

![](img/554159c3cf86581ab50b83f83271b3f7.png)![](img/ee7cd7283e38377ed29aa592ff7726a0.png)

:focus —如果元素聚焦，则对元素应用样式。

![](img/f4a7318f44ccf63cbef03e740fa76748.png)![](img/ecff65ec19f349c311fff0fb36e4f9ed.png)

:悬停—将样式应用于鼠标悬停时的元素。

![](img/d33e76521b8f80803062ddcaf41ebea3.png)![](img/b2c3de8a1501353404190bd737b0ffe5.png)

:活动—当用户单击元素时将样式应用于元素。

![](img/9c36215bc7ac2d9338c61104eb6ceea4.png)![](img/e553551634399f065d5e02fc00e5ed13.png)

:n-child(number)—按编号将样式应用于元素。我们可以将样式设置为奇数或偶数元素，或者每 3 个元素，或者只有 2 个元素。

![](img/41f7629b38671b49a0a42130e479b0a7.png)![](img/bf71db7b962b881a09ff2ce2e1b2dc65.png)

:is( css 选择器)—仅当元素等于 css 选择器时，才对元素应用样式。

![](img/2e5bab1c979dd5166e20a5a171f2e9f3.png)![](img/ef6cd663e4081493c6683f247f7c04c1.png)

:has( css 选择器)-仅当元素有 css 选择器时，才对元素应用样式。

![](img/658808e168760e02804cf51e8c78ac87.png)![](img/49c431780bfa919215ab5de1994dfc13.png)

:not (css 选择器)—将样式应用于不是 css 选择器元素的每个元素。

![](img/ae2af79c13a500dbf16f296f3a22e96a.png)![](img/efc0ea38edf0f0172e9df4d8589d8fb3.png)

* after-在元素后插入内容。

* before—在元素前插入内容。

![](img/eaf4c4133162586d5c1e1cfa2489d28d.png)![](img/8ea3615ac45965085f02ad96094d2f11.png)

如果你需要仔细看看这个项目[，这里是链接](https://github.com/8Tesla8/pseudo-selectors-css)。

*原载于 2022 年 12 月 1 日*[](https://tomorrowmeannever.wordpress.com/2022/12/01/list-of-css-pseudo-classes-that-i-use-in-my-work/)**。**