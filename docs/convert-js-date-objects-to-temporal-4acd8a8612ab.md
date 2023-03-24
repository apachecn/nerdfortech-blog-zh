# 将 JS 日期对象转换为时态

> 原文：<https://medium.com/nerd-for-tech/convert-js-date-objects-to-temporal-4acd8a8612ab?source=collection_archive---------12----------------------->

Temporal 将为 ECMAScript 语言带来一个现代的日期/时间 API，取代普通的 JavaScript 日期对象

![](img/db5866682e6b0d38e5f053d89525bbd2.png)

# 2022 年 2 月更新

*   根据[建议-临时](https://github.com/tc39/proposal-temporal)中的建议，将库更改为[js-temporal/temporal-poly fill](https://github.com/js-temporal/temporal-polyfill)。

# 从 JS 日期对象迁移的原因

当我看代码时，它是如此的令人困惑，以至于我需要不断地将月份值递增 1，然后才能将其转换成实际的月份。这个概念并不自然，至少对我来说是这样。看下面例子，[Jatupat Boonpatraksa aka '*Pai Dao Din*'](https://en.wikipedia.org/wiki/Jatupat_Boonpattararaksa)于 2021 年 3 月 8 日入狱，2021 年 4 月 23 日被保释。

*   `new Date(2021, 2, 8)` = >月份值为 2 = >递增 1 =>2021 年 3 月 8 日
*   `new Date(2021, 3, 23)` = >月份值为 3= >递增 4 =>2021 年 4 月 23 日

但是！！我可以用`dateString`数据类型创建`Date`对象，而不是将日期组件值传递给`new Date()` 构造函数。

*   `new Date(2021, 2, 8)` = `new Date('8 Mar 2021')` = `new Date('2021-03-08')`和
*   `new Date(2021, 3, 23)` = `new Date('23 Apr 2021')` = `new Date('2021-04-23')`

这个比较好理解。但是再一次！！从 [MDN Web 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/Date)来看，由于浏览器的差异和不一致，使用`Date`构造函数解析日期字符串(如`'23 Apr 2021'`或`'2021-04-23'`)是 ***强烈反对的。***

另一个痛点是缺乏支持一些常见操作的方法。参见下面的示例，下面的函数需要手动计算日期差异，方法是手动减去两个日期(纪元值)并除以一天中的毫秒数，这也是手动计算的。

```
const DAYS_IN_MS = 24 * 60 * 60 * 1000;
function getNumberOfDaysUnderDetained(
  detainedDate,
  releasedDate = todayDate
) {
  return Math.floor((releasedDate - detainedDate) / DAYS_IN_MS);
}
```

这只是其中的两个难点。还有什么？你可以从[这里](https://maggiepint.com/2017/04/09/fixing-javascript-date-getting-started/)看到，远离`Date`的动力。

# 可供选择的事物

## 使用库

[**卢克森**](https://moment.github.io/luxon/) ， [**Day.js**](https://github.com/iamkun/dayjs) ， [**date-fns**](https://date-fns.org/) ，**[**js-Joda**](https://js-joda.github.io/js-joda/)注意，我还没有把[**moment . js**](https://momentjs.com/)**放在这里是有原因的——“项目组不鼓励在新项目中使用 [**Moment.js**](https://momentjs.com/) 。****

## ****暂时的****

******未来就是现在！**(大概**还没有****但是快到了！！)******

******[**时态**](https://github.com/tc39/proposal-temporal) 是在 JavaScript 语言中实现的新的日期和时间 API。它将不再需要使用第三方库，并将取代 [**Date**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) 作为在 JavaScript 中处理日期和时间的 API。目前，[](https://github.com/tc39/proposal-temporal)**处于 TC39 流程的[阶段 3](https://tc39.es/process-document/)********

********备注**:由于 [**时态**](https://github.com/tc39/proposal-temporal) 处于第三阶段，其 API 可能会发生变化。**不建议在生产中使用。********

# ****履行****

****为了开始实现 [**时态**](https://github.com/tc39/proposal-temporal) ，我需要依赖 poly fill——因为浏览器还没有实现这个 API。****

****要了解更多如何使用 [**时态**](https://github.com/tc39/proposal-temporal) 的信息，请查看[时态食谱](https://tc39.es/proposal-temporal/docs/cookbook.html#overview)。****

1.  ****首先，安装`[proposal-temporal](https://github.com/tc39/proposal-temporal/tree/main/polyfill)`依赖项****

```
**$ npm install --save @js-temporal/polyfill**
```

****2.将`[proposal-temporal polyfill](https://github.com/tc39/proposal-temporal/tree/main/polyfill)`作为 ES6 模块导入文件 App.svelte****

```
**import { Temporal } from '@js-temporal/polyfill';**
```

****3.获取一个代表系统时区和 ISO-8601 日历中当前日期的`Temporal.PlainDate`对象，并将其存储在`todayDate`变量中。****

```
**const todayDate = Temporal.Now.plainDateISO();**
```

****4.用静态方法`Temporal.PlainDate.from`创建一个`Temporal.PlainDate`对象，接受代表期望日期的值。该方法返回一个新的`Temporal.PlainDate`对象，表示与特定时间或时区无关的日历日期，例如本例中的 2021 年 3 月 8 日。****

```
**Temporal.PlainDate.from({ year: 2021, month: 3, day: 8 }),**
```

****5.用`*date*.until(*other*, *options*)`计算两个日期(下例中的`detainedDate`和`releasedDate`)之间的差值，并将其作为`Temporal.Duration`对象返回。`options`对象提供了`largestUnit`属性，用于控制结果持续时间的表达方式；例如，当`largestUnit`为`"months"`时，两年的差异将变成 24 个月。然而，即使`largestUnit`是`"years"`，两个月的差异仍然是两个月****

```
**detainedDate.until(releasedDate, { largestUnit: 'days' }).days;**
```

****6.将所有内容放在一起，最终的文件将如下所示:****

```
**<script> **import { Temporal } from '@js-temporal/polyfill';****//**get the current date in the system time zone and ISO-8601 calendar
**const todayDate = Temporal.Now.plainDateISO();**const people = [
{
  name: `Jatupat 'Pai Dao Din' Boonpatraksa`,
  nickname: 'pai', //A Temporal.PlainDate object represents March 8th, 2021 date
  detainedDate: **Temporal.PlainDate.from({ year: 2021, month: 3, day: 8 })**, //A Temporal.PlainDate object represents April 23rd, 2021 date 
  releasedDate: **Temporal.PlainDate.from({ year: 2021, month: 4, day: 23 })**,
},
{
  name: `Parit 'Penguin' Chiwarak`,
  nickname: 'penguin',
  detainedDate: **Temporal.PlainDate.from({ year: 2021, month: 2, day: 9 })**,
},
{
  name: `Panupong 'Mike' Jadnok`,
  nickname: 'mike',
  detainedDate: **Temporal.PlainDate.from({ year: 2021, month: 3, day: 8 })**,
},
{
  name: `Somyot Pruksakasemsuk`,
  nickname: 'somyot',
  detainedDate: **Temporal.PlainDate.from({ year: 2021, month: 2, day: 9 })**,
  releasedDate: **Temporal.PlainDate.from({ year: 2021, month: 4, day: 23 })**,
},
{
  name: `Arnon Nampa`,
  nickname: 'arnon',
  detainedDate: **Temporal.PlainDate.from({ year: 2021, month: 2, day: 9 })**,
},
{
  name: `Patiwat Saraiyaem, 'Mor Lam Bank'`,
  nickname: 'bank',
  detainedDate: **Temporal.PlainDate.from({ year: 2021, month: 2, day: 9 })**,
  releasedDate: **Temporal.PlainDate.from({ year: 2021, month: 4, day: 9 })**,
},
{
  name: `Panusaya Sithijirawattanakul`,
  nickname: 'rung',
  detainedDate: **Temporal.PlainDate.from({ year: 2021, month: 3, day: 8 })**,
},
{
  name: `Chaiamorn 'Ammy' Kaewwiboonpan`,
  nickname: 'ammy',
  detainedDate: **Temporal.PlainDate.from({ year: 2021, month: 3, day: 4 })**,
},
{
  name: `Parinya Cheewin Kulpathom aka 'Port Faiyen'`,
  nickname: 'port',
  detainedDate: **Temporal.PlainDate.from({ year: 2021, month: 3, day: 6 })**,
},
{
  name: `Piyarat 'Toto' Jongthep`,
  nickname: 'toto',
  detainedDate: **Temporal.PlainDate.from({ year: 2021, month: 3, day: 6 })**,
  releasedDate: T**emporal.PlainDate.from({ year: 2021, month: 5, day: 5 })**,
},
{
  name: `Promsorn 'Fah' Veerathamjaree`,
  nickname: 'fah',
  detainedDate: **Temporal.PlainDate.from({ year: 2021, month: 3, day: 17 })**,
},
{
  name: `Chookiat 'Justin' Saengwong`,
  nickname: 'justin',
  detainedDate: **Temporal.PlainDate.from({ year: 2021, month: 3, day: 23 })**,
}]/**until* method computes the difference between the two dates
the *largestUnit* option controls how the resulting duration is expressed. The returned Temporal.Duration object will not have any nonzero fields that are larger than the unit in largestUnit. A difference of two years will become 24 months when largestUnit is "months", for example. However, a difference of two months will still be two months even if largestUnit is "years"*/function getNumberOfDaysUnderDetained(detainedDate, releasedDate = todayDate) {
  return **detainedDate.until(releasedDate, { largestUnit: 'days' }).days;**
}
</script><main>...<style>...**
```

****回到页面，最终结果将是相同的—持续时间仍然是正确计算的:****

****![](img/3a74490524de824c5189d6206d29a0e2.png)****

# ****参考****

*   ****[tc39](https://github.com/tc39) / [提议-时间](https://github.com/tc39/proposal-temporal)****
*   ****[世俗食谱](https://tc39.es/proposal-temporal/docs/cookbook.html)****
*   ****[Moment.js 项目状态](https://momentjs.com/docs/#/-project-status/recommendations/)****