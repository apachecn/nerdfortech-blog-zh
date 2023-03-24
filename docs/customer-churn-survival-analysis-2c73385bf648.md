# 生存分析导论

> 原文：<https://medium.com/nerd-for-tech/customer-churn-survival-analysis-2c73385bf648?source=collection_archive---------0----------------------->

![](img/0448e572c2504e204ab7bacb9677e28f.png)

生存分析是一种统计方法，用于预测特定事件发生前的预期持续时间。这些事件可以是死亡、移除、变动、损坏、失败、赎回、到期等。所有这些事件中最常见的参数是时间，这是任何生存分析的主要部分。生存分析总是试图测量概率，检测部分人口或评估特定环境对某个**时间**内事件的**生存**的影响。这篇文章试图涵盖最重要的细节，数学直觉和理论，以及**生存曲线**和**生存回归**的整个过程。

## 编程偏好

[生命线:Python 中的生存分析](https://lifelines.readthedocs.io/en/latest/index.html)

在这篇文章中使用了 Python 中的生命线库。Lifelines 是一个完整的生存分析库，用纯 Python 编写，具有以下优点:

1.  易于安装
2.  内部绘图方法
3.  简单直观的 API
4.  处理右，左和区间删失数据
5.  包含最流行的参数、半参数和非参数模型

## Ch 0。介绍

## 0.0 应用程序

当研究受试者群体中某一事件的发生时，如果事件发生的时间不重要，可以使用逻辑回归模型将该事件作为二元结果进行分析。对于其他一些事件，事件发生前的时间很重要。生存分析用于分析与事件发生前的时间相关的数据。响应变量是事件发生前的时间，通常称为故障时间、存活时间或事件时间。客户流失是指客户选择退出订阅的事件，而时间很重要。企业希望了解他们的客户如何以及为什么流失以提高利润。在这里，在调查了**生存分析**之后，我将通过客户流失和更新来探索它。

## 0.1 审查

当您想要对持续时间做出推断时，可能不是所有的事件都已经发生了。例如，医学专业人员不会等到研究中的每个人去世 50 年后才进行调查——他或她有兴趣在几年后或可能几个月后做出决定。没有经历死亡事件的人群中的个体被标记为右删截，即由于某些外部环境，我们没有(或不能)查看他们的剩余生活史。我们拥有的关于这些人的所有信息都是他们当前的生命周期(这自然比他们的实际寿命要短)。数据分析师常犯的一个错误是选择忽略那些被正确审查的个体。接下来我们会看到为什么这是一个错误。

考虑这样一种情况，人口实际上由两个子人口组成，𝐴和𝐵.𝐴人的寿命很短，平均只有 2 个月，而𝐵人的寿命要长得多，平均只有 12 个月。我们事先并不知道这种区别。在𝑡=10，我们希望调查整个人口的平均寿命。

在下图中，红线表示已观察到死亡事件的个体的寿命，蓝线表示右删截个体的寿命(未观察到死亡)。如果我们被要求估计我们人口的平均寿命，并且我们天真地决定不包括右删失的个体，很明显我们会严重低估真实的平均寿命。

![](img/1aa1a7b515c5b0b3d31dbf87196ccefa.png)

```
Observed lifetimes at time 10:
 [ 1.1947688  10\.          2.44345568  5.36683806  3.70008185  0.48173596
  0.13144326  0.45511328 10\.          0.69205379 10\.         10.
  0.59919553  1.97439122  3.43325156  1.09599444  4.38248954  4.47287191
  8.59646432 10\.          2.66794281  2.5696101   0.27496312 10.
  2.006956  ]
```

![](img/b215a56f5b38684f79db779517548e44.png)![](img/92062d30836f166967ded906e7fedce4.png)![](img/8a4c4584eb27fc42fe4ca4975652cc20.png)

最初，生存分析被发展来解决删失问题，即估计我们的数据何时被正确删失。然而，即使在所有事件都被观察到的情况下，即没有审查，存活分析仍然是理解持续时间和比率的非常有用的工具。

观察也不需要总是从零开始。这样做只是为了便于理解上面的例子。考虑顾客进入商店是一个出生的例子:顾客可以在任何时候进入，而不一定是在零点。在生存分析中，持续时间是相对的:个体可能在不同的时间开始。(我们实际上只需要观测的持续时间，而不一定需要起止时间。)

## 0.2 数学直觉

*   终极生存输入/输出是什么？
*   **卡普兰-迈耶曲线** ( **生存曲线**):
*   输入:每个客户的持续时间(开始日期、结束日期，如果有的话)以及他们是否失败/破产/死亡。
*   产出:总生存曲线
*   **生存回归:Cox 比例风险模型**:
*   输入:协变量(特征)+卡普兰-迈耶曲线输入数据
*   输出:单个客户的生存概率
*   [生存分析的用例](https://towardsdatascience.com/survival-analysis-intuition-implementation-in-python-504fde4fcf8e)
*   客户分析(客户保留):在生存分析的帮助下，我们可以专注于低生存时间的高价值客户的流失预防工作。这种分析还有助于我们计算客户生命周期价值。在这个用例中，事件被定义为客户搅动/取消订阅的时间。
*   营销分析(群组分析):生存分析评估每个营销渠道的留存率。在这种情况下，事件被定义为客户退订营销渠道的时间。
*   精算师:给定人群的风险，生存分析评估人群在特定时间范围内死亡的概率。这种分析有助于保险公司评估保险费。

让我们假设一个**非负连续随机变量** $T$，代表直到某个感兴趣的事件发生的时间。例如:

*   从客户订阅到客户流失的时间，从机器启动到机器故障的时间，从疾病诊断到死亡的时间。(T 是从客户(随机选择的客户)订阅到客户流失的时间；t 是从随机选择的机器启动到故障的时间；t 是从疾病诊断到随机选择的患者死亡的时间。)
*   T 是连续的非负随机变量(T ≥ 0)。
*   对于这类随机变量，通常用概率密度函数(pdf)和累积分布函数(cdf)来表征其分布。因此，我们将假设这个随机变量具有概率密度函数 f(t)和累积分布函数 F(t):

![](img/9a8056b9aa64135dcbd942a7c00b5c97.png)

<>生存函数 S(t)

*   给出了事件在时间 t 之前未发生的概率。简单地说，S(t)给出了事件发生时间值大于 t 的人口比例。

![](img/cbf9ca8dda1151a0d3792783dd87d714.png)

q 分位数是一部分受试者失败的时间。是值 t 使得 S(t)=1-q 或 T_q=S^{-1}(1-q).

*   T_{0.5}:中位寿命是半数受试者失败的时间，S(t)=0.5 或 T_{0.5}=S^{-1}(0.5).
*   设 T_i 表示第 I 个受试者的反应。这种反应是直到感兴趣的事件发生的时间，如果受试者没有被随访足够长的时间以观察到该事件，这种反应可能会被审查。
*   设 C_i 表示第 I 个受试者的删失时间，事件指标定义为
*   e_i=1 如果事件被观察到(T_i ≤C_i)，
*   如果响应被删截(T_i>C_i ),则 e_i=0
*   观察到的响应为 Y_i=\min(T_i，C_i)，即最先发生的时间:失效时间或截尾时间。
*   值对(Y_i，e_i)包含大多数情况下的所有响应信息(即，如果事件发生在 C_i 之前，则通常不关心潜在的审查时间 C_i)。

## <>危险函数 h(t)

*   它是事件发生的速率，在任何给定时间 t 的存活人口中。在医学术语中，我们可以将其定义为“**在时间 t 存活的人口中，这些人的死亡率是多少**”:

![](img/dda833985e135a43ea5a6b08e765950e.png)

*   假定事件在时间 t 之前未发生，时间$t$处的风险与事件在 t 附近的小间隔内发生的概率相关。风险函数不是密度或概率。然而，我们可以认为它是在 t 和 t+dt 之间的无限小的时间段内失败的概率，假定受试者已经存活到时间 t。在这个意义上，危险是风险的度量:时间 t1 和 T2 之间的危险越大，在这个时间间隔内失败的风险越大。

## <>累积危险函数

*   *H(t)* 和 s(t):h(t)= \int_0^t h(t’)dt’，因此:

![](img/19fff71b90318a1f236475a8a7c90856.png)

在 Cox 模型中

![](img/1795a5d2811a84a88ded58645865f85a.png)

*   H(t)的期望值是 1。

![](img/20092df95df7a6fff5791cd64fe1e2bf.png)

## Ch 1。估计单变量模型(生存曲线)

估计单变量模型中生存函数 S(t)的最常见方法是非参数 Kaplan-Meier 估计。让我们从卡普兰-迈耶估计量开始吧。

## 1.0 非参数 Kaplan-Meier 估计量

由于我们没有真实的总体生存曲线，因此我们将使用非参数 Kaplan-Meier 估计量从数据中估计生存曲线

![](img/3b8980e475fb393901cb43fee85b7091.png)

让我们举个例子:

我们将调查世界各地政治领袖的一生。在这种情况下，政治领袖是由控制统治政权的个人在任时间来定义的。这种政治领袖可以是民选总统、非民选独裁者、君主等。**出生事件是个人任期的开始，死亡事件是个人退休**。**如果他们(a)在数据汇编时(2008 年)仍在办公室，或(b)在执政期间死亡(包括暗杀)，审查就会发生。**

例如，布什政权始于 2000 年，正式结束于 2008 年他退休之时，因此该政权的寿命是 8 年，并且观察到了“死亡”事件。另一方面，JFK 政权持续了两年，从 1961 年到 1963 年，该政权的官方死亡事件没有被观察到——JFK 在他正式退休之前就去世了。

(这个例子很高兴地重新定义了出生和死亡事件，事实上通过使用死亡作为审查事件完全颠倒了这个想法。这也是当前时间不是审查的唯一原因的例子；其他事件(例如，在办公室死亡)也可能是审查的原因。

![](img/98d8933203a09e99f16456367bb28992.png)![](img/84d1a28c868533f4521f2d6289824e0a.png)

在生命线库中，我们需要 KaplanMeierFitter 来完成这个练习:

![](img/e0b81b9f7ccc3c7352faf316cfeb0ac6.png)

```
<lifelines.KaplanMeierFitter:"KM_estimate", fitted with 1808 total observations, 340 right-censored observations>from matplotlib import pyplot as plt

kmf.survival_function_.plot()
plt.title('Survival function of political regimes');
```

![](img/8fb600e2de75bda0a69381a3e040b70f.png)

**解读**:**y 轴代表领导者在 t 年后仍然存在的概率，其中 t 年在 x 轴上。**

我们看到很少有领导人在位超过 20 年。当然，我们需要报告我们对这些点估计的不确定程度，也就是说，我们需要置信区间。它们是在对 fit()的调用中计算的，位于 confidence_interval_ property 下。(该方法使用指数格林伍德置信区间。数学在这些笔记中被发现。)我们可以调用 KaplanMeierFitter 本身的 plot()来绘制 KM 估计及其置信区间:

```
kmf.plot_survival_function()<matplotlib.axes._subplots.AxesSubplot at 0x13a17cac0>
```

![](img/14195af658913b0b91293aaee443550c.png)

任职时间中位数是一个属性，它定义了平均 50%的人口已经到期的时间点:

```
kmf.median_survival_time_4.0
```

有趣的是只有四年。这意味着，在世界范围内，当选的领导人有 50%的机会在四年或更短的时间内戒烟！要获得中位数的置信区间，您可以使用:

![](img/3470fc80ac0d945bfbb759db694010cf.png)

让我们把民主政权和非民主政权分开。对评估本身或 fitter 对象调用 plot 将返回一个轴对象，该轴对象可用于绘制进一步的评估:

![](img/73327e6a1c39da3e2605317b8a301cda.png)![](img/ad5e285818ea0ed234d9730fab1b9225.png)

我们可能对估计某些点之间的概率感兴趣。我们可以用时间线来证明。我们指定感兴趣的时间，并返回一个数据帧，显示在这些时间点的生存概率:

![](img/093b9657ebcf7801153504a4a280c809.png)![](img/ceb7a326e5c28201eea29756eee25a17.png)

令人难以置信的是，这些非民主政权还能存在多久。然而，一个民主政权确实对死亡有一种天然的偏好:通过选举和自然限制(美国规定了严格的八年限制)。非民主政体的中值只有民主政体的两倍左右，但差异在尾部是显而易见的:如果你是一位非民主政体的领导人，并且你已经度过了 10 年，你可能会有很长的人生。与此同时，民主党领导人很少能撑过 10 年，之后的寿命也很短。

在这里，生存函数之间的差异是非常明显的，进行统计测试似乎是迂腐的。如果曲线更相似，或者我们拥有的数据更少，我们可能有兴趣进行统计检验。在这种情况下，lifelines 包含 lifelines.statistics 中的例程来比较两个生存函数。下面我们演示这个例程。函数 life lines . statistics . log rank _ test()是生存分析中比较两个事件序列的生成器的常用统计测试。如果返回值超过某个预先指定的值，那么我们判定系列有不同的生成器。

![](img/2463e721e9df4c1b2aecd81d31755cb0.png)![](img/084a758590d553d1c2ee84f776e3146e.png)

生存函数有替代的(有时更好的)检验，我们在这里解释更多:统计比较两个群体

让我们比较数据集中存在的不同类型的制度:

![](img/6360f922c6396158968ed6b0c0ae0532.png)![](img/693c9330160ef88c88c1d92973f030db.png)

## <>展示卡普兰迈耶图的最佳实践

最近对统计学家、医疗专业人员和其他利益相关者的调查表明，增加两条信息(汇总表和置信区间)大大提高了 Kaplan Meier 图的有效性，参见“Morris TP，Jarvis CI，Cragg W，et al . Proposals on Kaplan-Meier plots in medical research and a survey of stakeholder views:KMunicate。BMJ 公开赛 2019；9:e030215。doi:10.1136/bmjopen-2019–030215”。

在生命线中，置信区间是自动添加的，但也有 at_risk_counts kwarg 来添加汇总表:

![](img/25c3b17e0b38e86fdc2520540ef871f9.png)![](img/982bf7519c5498ed75dae6260c6f8ba3.png)

## <>将数据转换成正确的格式

生命线数据格式在所有估计器类和函数中是一致的:个体持续时间的数组和个体事件观察(如果有的话)。这些通常分别表示为 T 和 E。例如:

```
T = [0, 3, 3, 2, 1, 2]
E = [1, 1, 0, 0, 1, 1]
kmf.fit(T, event_observed=E)<lifelines.KaplanMeierFitter:"all_regimes", fitted with 6 total observations, 2 right-censored observations>
```

原始数据并不总是以这种格式提供——生命线包括一些帮助函数，用于将数据格式转换为生命线格式。它们位于 lifelines.utils 子库中。例如，函数 datetimes_to_durations()接受开始时间/日期的数组或 Pandas 对象，以及结束时间/日期的数组或 Pandas 对象(如果未观察到，则不接受):

![](img/47a848fb10ef0136d9cdfea2095df562.png)

```
T (durations):  [3\. 1\. 5.]
E (event_observed):  [ True  True False]
```

函数 datetimes _ to _ durations()非常灵活，有许多可以修改的关键字。

## 1.1 使用 Nelson-Aalen 估算风险率

生存函数是总结和可视化生存数据集的一种很好的方法，但它不是唯一的方法。如果我们对总体的风险函数 h(t)感到好奇，不幸的是，我们无法转换卡普兰·迈耶估计值——统计学不太管用。幸运的是，累积风险函数有一个合适的非参数估计量:

![](img/b1b4659a4a26a79ba15d5c4102db1f6f.png)

这个量的估计量称为尼尔森艾伦估计量(Altschuler–Nelson estimator):

![](img/60b1b31ba13c91b72628a2a3102d0b64.png)

其中 d_{i}是时间 t_{i}时的死亡人数，n_{i}是易感个体的人数。

![](img/23279f0c70de3b6d57ae88bc628059b7.png)

该估计量优于 Kaplan-Meier 估计量。

*   估计器给出正确的预期事件数。
*   有大量基于 Altschuler-Nelson 估计量的渐近理论。

![](img/f89e5bf0cc1056cbe7660187ac396765.png)

```
<lifelines.NelsonAalenFitter:"NA_estimate", fitted with 1808 total observations, 340 right-censored observations>print(naf.cumulative_hazard_.head())
naf.plot_cumulative_hazard()NA_estimate
timeline             
0.0          0.000000
1.0          0.325912
2.0          0.507356
3.0          0.671251
4.0          0.869867

<matplotlib.axes._subplots.AxesSubplot at 0x139cd3a60>
```

![](img/0df5a9fa4e3e394c44324575fb1e7112.png)

累积风险的理解不如生存函数明显，但风险函数是生存分析中更高级技术的基础。回想一下，我们正在估计累积风险函数 H(t)。(为什么？估计的和比逐点估计稳定得多。)因此，我们知道这条曲线的变化率是对风险函数的估计。

看上面的图，看起来危险从高开始变得越来越小(从变化率的降低可以看出)。让我们把前 20 年的政权分为民主和非民主:

![](img/11f5736217f2d83a96bda5e63f0dd9a8.png)![](img/69cc1e9e08dccd6b8e250d93d92ad8dc.png)

看看变化的速度，我会说这两种政治哲学都有持续的危险，尽管民主政权有更高的持续危险。

## <>平滑危险函数

对累积风险函数的解释可能很困难——这不是我们通常解释函数的方式。另一方面，大多数生存分析是使用累积风险函数完成的，因此建议理解它。

或者，我们可以推导出更易解释的风险函数，但有一个问题。推导涉及一个核平滑器(平滑累积风险函数的差异)，这需要我们指定一个控制平滑量的带宽参数。此功能位于 smoothed_hazard_()和 smoothed _ hazard _ confidence _ intervals _()方法中。为什么是方法？它们需要一个表示带宽的参数。

还有一个 plot_hazard()函数(也需要一个 bandwidth 关键字)将绘制估计值和置信区间，类似于传统的 plot()函数。

![](img/91fde37702be8b5482c86d3a63bb4a95.png)![](img/0a29d5af1af9d6eaddc1a37739e69396.png)

在这里，哪一个群体的风险更高就更清楚了，而非民主政权似乎总是有风险。

没有明显的方法来选择一个带宽，不同的带宽产生不同的推论，所以这里最好非常小心。我的建议是:坚持累积风险函数。

![](img/aa79d572c708c1f73c059acfe26a790c.png)![](img/462b721a9379a28d2e8691e3875bbcfc.png)

## 1.2 使用参数模型估算累积危害

参数方法基于某些分布，如指数分布、威布尔分布、对数正态分布、对数逻辑分布等。然后我们估计参数，最后形成生存函数的估计量。

## <>符合威布尔模型

另一个非常流行的生存数据模型是威布尔模型。与 Nelson-Aalen 估计量相比，这个模型是一个参数模型，这意味着它有一个函数形式，带有我们要拟合数据的参数。(Nelson-Aalen 估计量没有适合的参数)。生存函数看起来像这样:

![](img/484de379ea8f766a7bbc21fae35e5b07.png)![](img/ce151247c717ca3a586df925b16c85cd.png)![](img/6cd887c3810cb730c8e544b959ad8206.png)

```
Text(0.5, 1.0, 'Cumulative hazard of Weibull model; estimated parameters')
```

![](img/4ddf4a61c6a9bf1e764ee81ff2e103a8.png)

## 其他参数模型:指数模型、对数逻辑模型、对数正态模型和样条模型

同样，生命线中还有其他参数模型。一般来说，选择哪个参数模型是由持续时间分布的知识或某种模型拟合优度决定的。以下是相同数据的内置参数模型和 Nelson-Aalen 非参数模型。

**指数模型**

*   简单的假设风险函数是常数:$h(t) = h$
*   $h(t|x)=h$
*   $H(t|X)= h t$
*   $S(t|X)=\exp(-h t)$
*   $T_{0.5}=\ln(2)/\lambda$
*   它具有这样的性质:一个受试者的未来寿命是相同的，不管它有多“老”，或者 Prob{T>t0 + t|T >t0} = Prob{T>t}。这种“永恒”的特性也使得指数分布成为模拟人类生存的一个很差的选择，除非是在很短的时间内。

![](img/efafda51857f2f573bf7a9192993b8d4.png)

```
<matplotlib.axes._subplots.AxesSubplot at 0x13a494f70>
```

![](img/b214938aabb92ca5431dde92eafab92a.png)

参数模型也可以用来创建和绘制生存函数。下面我们比较了参数模型和非参数 Kaplan-Meier 估计:

![](img/fe45d00ecc71155c28695be34d8abd2b.png)

```
<matplotlib.axes._subplots.AxesSubplot at 0x139aad460>
```

![](img/ecac42284cc9260abaf9bc5eb931c1f5.png)

有了参数模型，我们就有了一个函数形式，允许我们将生存函数(或风险或累积风险)扩展到超过最大观察持续时间。这叫做外推法。我们可以用几种方法做到这一点。

![](img/994d00c47511760e44c72f9376986ff6.png)

```
Text(0.5, 1.0, 'Survival function of Weibull model; estimated parameters')
```

![](img/52752965d3bd37c0d8837ae359c38766.png)

## 甲烷。[生存回归](https://lifelines.readthedocs.io/en/latest/Survival%20Regression.html#parametric-survival-models)

除了我们想要使用的持续时间之外，我们通常还有额外的数据。这种技术被称为生存回归——这个名字意味着我们回归协变量(如年龄、国家等)。)相对于另一个变量—在这种情况下是**持续时间**。生存回归中有几个流行的模型，如 **Cox 比例风险模型**、**加速失效模型**和 **Aalen 的加法模型**。所有模型都试图将风险率$h(t|x)$表示为$t$和某些协变量$x$的函数。在这里，我们探讨 Cox 比例风险回归模型和生存回归模型，简称 **Cox 比例风险回归模型**。

生存分析持续用于各种行业，例如:

*   保险——保单失效的时间到了。
*   抵押贷款——抵押贷款赎回时间
*   邮购目录—下次购买的时间
*   零售—食品顾客开始购买非食品的时间
*   制造—机器部件的寿命
*   公共部门—关键事件的时间间隔

**注意事项**:

1.  因为**删截**，我们不能使用线性回归等传统方法。
2.  为什么我们在这里说“比例危险”？因为这是这个模型中最重要的假设，后面会讲到。

## 要使用的 2.0 数据集

为了回顾 Cox 比例风险回归模型，使用了 Rossi 累犯数据集(在 lifelines 中以 load_rossi()提供)。该数据集最初来自 Rossi 等人(1980 年)，并在 Allison (1995 年)中用作示例。这些数据与 20 世纪 70 年代从马里兰州立监狱释放的 432 名罪犯有关，他们在释放后被随访了一年。一半被释放的囚犯被随机分配到一个实验性的治疗中，在这个治疗中他们得到了经济援助；一半人没有得到援助。周列是持续时间，停止列表示事件(再次停止)是否发生，其他列表示我们希望回归的变量。

以下是对列的描述:

1.  周:以周为单位的持续时间。
2.  逮捕:事件；1 表示被捕，0 表示未被捕。
3.  财政资助:不，是。
4.  年龄:发布时的年龄。
5.  种族:人种；黑色或其他。
6.  wexp:入狱前的全职工作经历:否或是。
7.  mar:释放时的婚姻状况；已婚或未婚。
8.  帕罗:假释出狱？不是或者是。
9.  prio:当前监禁前的定罪数量。

![](img/e928d74955f7cc4645078e87b02422ec.png)![](img/86eb739cb55cb5d6ab471ff4f872d24b.png)

## 2.1 Cox 比例风险模型

Cox 比例风险模型背后的思想是，个体的对数风险是其协变量的线性函数，是随时间变化的人群水平的基线风险。数学上:

![](img/e705a82f3abce55c46f9fab878b544fe.png)

**备注:**

1.  b_{0}(t)是唯一的时间成分。在上述等式中，部分危险是一个不随时间变化的标量因子，仅增加或减少基线危险。因此，协变量的变化只会放大或缩小基线风险。
2.  与时间无关的变量被定义为对于给定个体其值不随时间变化的任何变量。例如性别和吸烟状况(SMK)。然而，请注意，一个人的吸烟状况实际上可能会随着时间的推移而改变，但为了分析的目的，SMK 变量被假设为一旦被测量就不会改变，因此每个人只使用一个值。
3.  还要注意的是，虽然年龄和体重等变量会随时间变化，但如果这些变量的值不会随时间变化太大，或者如果这些变量对生存风险的影响主要取决于仅一次测量的值，则在分析中将这些变量视为与时间无关可能是合适的。
4.  协变量的影响在对数率尺度上是可加的和线性的[如果你从两边取一个对数，你会看到]。

## <>危险比率

*   一般来说，风险比(HR)定义为一个人的风险除以另一个人的风险。被比较的两个个体可以通过它们的协变量集，即 x 的值来区分

![](img/9b9ca89d897742239ddda3d3c37e7865.png)

*   大于 1 的危险比意味着事件更有可能发生，小于 1 的危险比意味着事件不太可能发生。危险比 1 表示预测因子对事件的危险没有影响。
*   基线风险:在估计基线风险函数时，Cox 模型使用所谓的 Aalen-Breslow [估计量](https://data.princeton.edu/pop509/NonParametricSurvival.pdf)，它是累积风险[函数](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4111957/pdf/nihms595225.pdf)的非参数 Nelson-Aalen 估计量的推广。

## <>考克斯偏似然函数

对于 i = 1，…，n，Cox 的部分似然函数为:

![](img/b1305db48946499afd46ccb582b1046a.png)

R(t_{i})是在时间 t_{i}的风险集，即刚好在时间 t_{i}之前处于风险中的个体集。

*   在出现故障时间的关系时，应使用 Breslow 或 Efron 公式来近似对数似然性。
*   一旦导出了部分对数似然函数，就可以像使用普通对数似然函数一样来估计β，估计β的标准误差，获得置信限，并进行统计检验。

## <>拟合回归

Cox 模型在生命线中的实现在 **CoxPHFitter** 下进行。我们使用 fit()将模型拟合到数据集。它有一个 print_summary()函数，可以打印系数和相关统计数据的表格视图。

![](img/1cca9fa1076dbaf0fef4cbb6b5d54c5e.png)

```
<lifelines.CoxPHFitter: fitted with 432 total observations, 318 right-censored observations>
```

我们也可以用公式来处理线性模型的右边。例如带有相互作用项的线性模型:

𝛽1fin+𝛽2wexp+𝛽3age+𝛽4prio+𝛽5age⋅prio

cph.fit(rossi，duration_col='week '，event_col='arrest '，formula="fin + wexp + age * prio ")

![](img/3e52333e27bf6096c461cd345d55d154.png)![](img/47708b852318ab73945da94f8a6aea5d.png)

## 如何解释这些结果

```
cph.print_summary()  # access the individual results using cph.summary
```

![](img/47708b852318ab73945da94f8a6aea5d.png)

要直接访问系数和基线危险，您可以分别使用*参数 _* 和*基线 _ 危险 _* 。看一下这些系数，prio(以前被捕的次数)的系数约为 0.09。因此，prio 增加一个单位意味着基线风险将增加 exp(0.09)=1.10 倍，大约增加 10%。回想一下，在 Cox 比例风险模型中，更高的风险意味着事件发生的风险更大。值 exp(0.09)是**危险比**，这个名字用另一个例子就清楚了。

*   示例:

考虑 mar 的系数(受试者是否已婚)。列中的值是二进制的:0 或 1，表示未婚或已婚。与 mar 相关的系数值 exp(0 . 43)是与结婚相关的风险比率值，即:

![](img/9b4fa30ed9f0b1af13e9cbf09b960c74.png)

注意，左手边是一个常数(具体来说，它与时间无关，𝑡)，但右手边有两个可能随时间变化的因子。比例风险假设是关系成立。也就是说，危险会随着时间的推移而变化，但它们在不同级别之间的比率保持不变。稍后我们将处理检查这个假设。然而，在现实中，风险比在研究期间发生变化是很常见的。然后，风险比可以解释为特定时期风险比的某种加权平均值。因此，风险比可能主要取决于随访的持续时间。

```
cph.params_covariate
fin        -0.330167
wexp       -0.238619
age        -0.028550
prio        0.307433
age:prio   -0.009743
Name: coef, dtype: float64#cph.baseline_hazard_
```

## <>收敛

将 Cox 模型拟合到数据涉及到使用迭代方法。生命线通过提供详细的警告，在收敛方面得到了很好的发展。修复任何警告通常会有助于收敛并减少所需的迭代步骤。如果您希望在拟合过程中查看更多信息，fit()函数中有一个 *show_progress* 参数。

拟合后，**最大对数似然**的值可用 log_likelihood_ 获得。系数的方差矩阵可在 variance_matrix_ 下获得。

对数似然值不能单独用作拟合指数，因为它们是样本大小的函数，但可用于比较不同系数的拟合。因为您想要最大化对数似然，所以值越高越好。自然对数函数对于小于 1 的值为负，对于大于 1 的值为正。所以有可能对数似然的结果是负值(对于离散变量总是如此)。

```
cph.log_likelihood_-659.3864712540503cph.variance_matrix_
```

![](img/5ea5ef7790ec9cca18e54717872bee73.png)

## <>拟合优度

拟合后，您可能想知道模型与数据的拟合程度。接下来，介绍了评估模型拟合度的 4 种主要方法:

1.  检查生存概率校准图

*   函数根据观察到的事件频率来测量拟合的生存模型。我们遵循 P. Austin and co .的“生存模型的图形校准曲线和综合校准指数(ICI)”中的建议，并使用灵活的样条回归模型创建平滑的校准曲线(这避免了宁滨连续值概率的传统问题，并处理删失数据)。
*   ICI:ICI 是校准曲线和完美校准的对角线之间的绝对差的加权平均值，其中绝对差由权重的密度函数加权。这相当于在预测概率的分布上对 f(x)进行积分。

1.  看看 concordance-index，它可以作为 concordance_index_ 或在 print_summary()中作为预测准确性的一种度量。

*   和谐指数(CI)是一个删截敏感的措施，也被称为 c 指数。此度量评估预测时间排名的准确性。事实上，它是 AUC(另一种常见损失函数)的推广，其解释类似:

1.  0.5 是随机预测的预期结果，
2.  1.0 是完美的一致，
3.  0.0 是完美的反一致性(预测乘以-1 得到 1.0)

*   拟合的生存模型通常具有 0.55 到 0.75 之间的一致指数(这可能看起来很糟糕，但即使是完美的模型也有很多噪声，不可能获得高分)。在生命线中，拟合模型的一致指数出现在 score()的输出中，但也可以在 concordance_index_ property 下获得。通常，该度量在 lifelines . utils . concordance _ index()下的 lifelines 中实现，并接受实际时间(以及任何审查的主题)和预测时间。

1.  查看 print_summary()或 log_likelihood_ratio_test()中的对数似然测试结果
2.  使用 check_assumptions()方法检查比例危险假设。更多详细信息，请参见本页后面的部分。

```
from lifelines.calibration import survival_probability_calibration

survival_probability_calibration(cph, rossi, t0=25)ICI =  0.010308090904927618
E50 =  0.00935319647035604

(<matplotlib.axes._subplots.AxesSubplot at 0x13a2974c0>,
 0.010308090904927618,
 0.00935319647035604)
```

![](img/cd52e5a67d173fbd18454fdc6f3c009c.png)

## <>预测

拟合后，可以使用预测方法套件:predict_partial_hazard()、predict_survival_function()等。

关于受试者的预测，请访问:[受试者预测](https://lifelines.readthedocs.io/en/latest/Survival%20Regression.html#prediction-on-censored-subjects)

```
X = rossi

#cph.predict_survival_function(X)
#cph.predict_median(X)
cph.predict_partial_hazard(X)0      1.157686
1      3.768718
2      4.919516
3      0.699116
4      1.447786
         ...   
427    0.520233
428    1.385036
429    0.784224
430    0.812407
431    0.672850
Length: 432, dtype: float64
```

## <>惩罚和稀疏回归

也可以在考克斯回归中加入一个惩罚项。人们可以使用这些来:

1.  稳定系数
2.  将估计缩小到 0
3.  鼓励贝叶斯观点
4.  创建稀疏系数。

所有的回归模型，包括考克斯模型，都包含了 L1 和 L2 惩罚:

$ $ \ frac { 1 } { 2 } \ textrm { penalizer }((1−l_{1_{ratio}})⋅||𝛽||*{2}^{2}+l*{1_{ratio}}⋅||𝛽||_{1})$ $

要在生命线中使用它，可以在类创建中指定 penalizer 和 l1_ratio:

```
from lifelines import CoxPHFitter

cph = CoxPHFitter(penalizer=0.1, l1_ratio=0.001) # sparse solutions,
cph.fit(rossi, 'week', 'arrest')
cph.print_summary()
```

![](img/2c662f277048229e52bc97a5df49e3f4.png)

代替浮点数，可以提供一个数组，其大小与被罚参数的数量相同。数组中的值是每个协变量的特定惩罚系数。这对于更复杂的协变量结构很有用。一些例子:

你有很多混杂因素想要惩罚，但不是主要的治疗方法。

## <>绘制系数

对于拟合模型，查看系数及其范围的另一种方法是使用绘图方法。

```
from lifelines.datasets import load_rossi
from lifelines import CoxPHFitter

cph = CoxPHFitter()
cph.fit(rossi, duration_col='week', event_col='arrest')

cph.plot()<matplotlib.axes._subplots.AxesSubplot at 0x13c75a9d0>
```

![](img/09bc98ce459a4776316be64ad276129f.png)

## <>绘制改变协变量的效果

拟合后，我们可以画出生存曲线，当我们改变一个协变量，而保持其他一切不变。在给定模型的情况下，这有助于理解协变量的影响。为此，我们使用 plot _ partial _ effects _ on _ outcome()方法，并为其提供感兴趣的协变量以及要显示的值。

```
from lifelines.datasets import load_rossi
from lifelines import CoxPHFitter

cph = CoxPHFitter()
cph.fit(rossi, duration_col='week', event_col='arrest')

cph.plot_partial_effects_on_outcome(covariates='prio', values=[0, 2, 4, 6, 8, 10], cmap='coolwarm')<matplotlib.axes._subplots.AxesSubplot at 0x13c836070>
```

![](img/0bbfea0747896405ce6a82d6b8ccf073.png)

## <>检查比例风险假设

为了做出正确的推论，我们应该问问我们的 Cox 模型是否适合我们的数据集。回想一下，在使用 Cox 模型时，我们隐含地应用了比例风险假设。我们应该问，我们的数据集服从这个假设吗？

1.  非比例风险:不同地层的生存曲线必须具有与时间成比例的风险函数。这是上文针对单个解释变量和多变量模型讨论的比例风险假设。
2.  对数风险和每个协变量之间应该有线性关系。最好使用残差图进行检查。
3.  对个体进行审查的机制必须与事件发生的概率无关。这种假设被称为无信息审查，适用于几乎所有形式的生存分析。

CoxPHFitter 有一个 check_assumptions()方法，它将输出违反比例风险假设的情况。有关如何修复违规的教程，请参见测试比例风险假设。建议是寻找将列分层的方法(参见下面的文档)，或者使用时变模型。

**备注:**

只有当你的目标是推断或关联时，才需要检查这样的假设。也就是说，你希望了解一个协变量对生存期和预后的影响。如果您的目标是预测，检查模型假设就不那么重要了，因为您的目标是最大化准确性度量，而不是了解模型是如何做出预测的。

比例风险假设是所有人都有相同的风险函数，但有唯一的比例因子。所以风险函数的形状对所有个体都是一样的，每个个体只有一个标量倍数的变化。

![](img/18cc56fc8af8826e8fff686460649166.png)![](img/5a7019cd43e8d3a05e9830435b9875bf.png)![](img/9b14eabba9a007d2faed99daa266076d.png)

## <>用 check_assumptions 检查假设

生命线 0.16.0 的新特性是**Cox phfitter . check _ assumptions**方法。该方法将计算检查比例风险假设的统计数据，生成检查假设的图，等等。还包括一个向控制台显示建议的选项。

*   首先呈现的是测试任何时变系数的统计测试的结果。时变系数意味着协变量相对于基线随时间变化的影响。这意味着违反了比例风险假设。对于每个变量，我们将时间转换四次(这些是要执行的常见时间转换)。如果生命线拒绝空值(也就是说，生命线拒绝系数不是时变的)，我们向用户报告这一点。
*   根据变量的一些汇总统计数据，提出了一些关于如何纠正比例危险违规的建议。
*   作为对上述统计测试的补充，对于违反 PH 假设的每个变量，针对四个时间变换呈现了缩放的舍恩菲尔德残差的可视图。还提供了拟合的 lowess，以及 10 条自举 lowess 线(作为原始 lowess 线的置信区间的近似值)。理想情况下，这条 lowess 线是恒定的(平坦的)。偏离恒定线违反了 PH 假设。

```
cph.check_assumptions(rossi, p_value_threshold=0.05, show_plots=True)The ``p_value_threshold`` is set at 0.05\. Even under the null hypothesis of no violations, some
covariates will be below the threshold by chance. This is compounded when there are many covariates.
Similarly, when there are lots of observations, even minor deviances from the proportional hazard
assumption will be flagged.

With that in mind, it's best to use a combination of statistical tests and visual tests to determine
the most serious violations. Produce visual plots using ``check_assumptions(..., show_plots=True)``
and looking for non-constant lines. See link [A] below for a full example.
```

![](img/d25f2fc29371b60da95ea08e2d33ebc1.png)

```
1\. Variable 'age' failed the non-proportional test: p-value is 0.0007.

   Advice 1: the functional form of the variable 'age' might be incorrect. That is, there may be
non-linear terms missing. The proportional hazard test used is very sensitive to incorrect
functional forms. See documentation in link [D] below on how to specify a functional form.

   Advice 2: try binning the variable 'age' using pd.cut, and then specify it in `strata=['age',
...]` in the call in `.fit`. See documentation in link [B] below.

   Advice 3: try adding an interaction term with your time variable. See documentation in link [C]
below.

   Bootstrapping lowess lines. May take a moment...

2\. Variable 'wexp' failed the non-proportional test: p-value is 0.0063.

   Advice: with so few unique values (only 2), you can include `strata=['wexp', ...]` in the call in
`.fit`. See documentation in link [E] below.

   Bootstrapping lowess lines. May take a moment...

---
[A]  https://lifelines.readthedocs.io/en/latest/jupyter_notebooks/Proportional%20hazard%20assumption.html
[B]  https://lifelines.readthedocs.io/en/latest/jupyter_notebooks/Proportional%20hazard%20assumption.html#Bin-variable-and-stratify-on-it
[C]  https://lifelines.readthedocs.io/en/latest/jupyter_notebooks/Proportional%20hazard%20assumption.html#Introduce-time-varying-covariates
[D]  https://lifelines.readthedocs.io/en/latest/jupyter_notebooks/Proportional%20hazard%20assumption.html#Modify-the-functional-form
[E]  https://lifelines.readthedocs.io/en/latest/jupyter_notebooks/Proportional%20hazard%20assumption.html#Stratification

[[<matplotlib.axes._subplots.AxesSubplot at 0x13c869ee0>,
  <matplotlib.axes._subplots.AxesSubplot at 0x13c8eefa0>],
 [<matplotlib.axes._subplots.AxesSubplot at 0x13c991ca0>,
  <matplotlib.axes._subplots.AxesSubplot at 0x13c9d9dc0>]]
```

![](img/60583b33c6067a811a3215557c73413f.png)![](img/567f95471d4646d5afcf67cc99826efd.png)

```
from lifelines.statistics import proportional_hazard_test

results = proportional_hazard_test(cph, rossi, time_transform='rank')
results.print_summary(decimals=3, model="untransformed variables")
```

![](img/cb59c45bc66e15835cfa4149f2a6fada.png)

## <>分层

在上面的建议中，我们可以看到 **wexp** 的基数很小，所以我们可以通过在层中指定它来轻松解决这个问题。地层是做什么的？让我们回到比例风险假设。

在介绍中，我们说过比例风险假设是

$$h_{i}(t) = a_{i}h(t)$$

在一个简单的例子中，可能有两个分组具有非常不同的基线危害。也就是说，我们可以根据某个变量(我们称之为分层变量)将数据集分成子样本，对所有子样本运行 Cox 模型，并比较它们的基线风险。如果这些基线危害非常不同，那么显然上面的公式是错误的——$ h(t)$是亚组基线危害的加权平均值。这种不合适的平均基线会导致$a_{i}$具有依赖于时间的影响。更好的模型可能是:

$ $ h _ { I | I \ in G(t)} = a _ { I } h _ { G }(t)$ $

现在我们每个亚组都有一个独特的基线危险。由于 Cox 模型的设计方式，系数的推断是相同的(预期现在有更多的基线危险，并且在亚组$G$内没有分层变量的变化)。

```
cph.fit(rossi, 'week', 'arrest', strata=['wexp'])
cph.print_summary(model="wexp in strata")
```

![](img/5e42fbfa7916392564ec2f985c0f0e5a.png)

```
cph.check_assumptions(rossi, show_plots=True)The ``p_value_threshold`` is set at 0.01\. Even under the null hypothesis of no violations, some
covariates will be below the threshold by chance. This is compounded when there are many covariates.
Similarly, when there are lots of observations, even minor deviances from the proportional hazard
assumption will be flagged.

With that in mind, it's best to use a combination of statistical tests and visual tests to determine
the most serious violations. Produce visual plots using ``check_assumptions(..., show_plots=True)``
and looking for non-constant lines. See link [A] below for a full example.
```

![](img/cba7ae11067ae660a720f33e20ebf9a3.png)

```
1\. Variable 'age' failed the non-proportional test: p-value is 0.0008.

   Advice 1: the functional form of the variable 'age' might be incorrect. That is, there may be
non-linear terms missing. The proportional hazard test used is very sensitive to incorrect
functional forms. See documentation in link [D] below on how to specify a functional form.

   Advice 2: try binning the variable 'age' using pd.cut, and then specify it in `strata=['age',
...]` in the call in `.fit`. See documentation in link [B] below.

   Advice 3: try adding an interaction term with your time variable. See documentation in link [C]
below.

   Bootstrapping lowess lines. May take a moment...

---
[A]  https://lifelines.readthedocs.io/en/latest/jupyter_notebooks/Proportional%20hazard%20assumption.html
[B]  https://lifelines.readthedocs.io/en/latest/jupyter_notebooks/Proportional%20hazard%20assumption.html#Bin-variable-and-stratify-on-it
[C]  https://lifelines.readthedocs.io/en/latest/jupyter_notebooks/Proportional%20hazard%20assumption.html#Introduce-time-varying-covariates
[D]  https://lifelines.readthedocs.io/en/latest/jupyter_notebooks/Proportional%20hazard%20assumption.html#Modify-the-functional-form
[E]  https://lifelines.readthedocs.io/en/latest/jupyter_notebooks/Proportional%20hazard%20assumption.html#Stratification

[[<matplotlib.axes._subplots.AxesSubplot at 0x13c71aac0>,
  <matplotlib.axes._subplots.AxesSubplot at 0x13caef580>]]
```

![](img/afbb5e03e5b22dc541aec48baa23a8cc.png)

由于年龄仍然违反比例风险假设，我们需要更好地建模。从上面的残差图中，我们可以看到年龄的影响随着时间的推移开始变得消极。这个以后会有关系。下面，我们提出三种处理年龄的选择。

## <>修改功能表单

当变量的函数形式不正确时，比例风险测试非常敏感(即大量假阳性)。例如，如果协变量和对数风险之间的关联是非线性的，但模型只包含线性项，那么比例风险测试可能会产生假阳性。

建模者可以选择添加二次或三次项，即:

```
rossi['age**2'] = (rossi['age'] - rossi['age'].mean())**2
rossi['age**3'] = (rossi['age'] - rossi['age'].mean())**3
```

但是我认为包含非线性项的更正确的方法是使用基本样条:

```
cph.fit(rossi, 'week', 'arrest', strata=['wexp'], formula="bs(age, df=4, lower_bound=10, upper_bound=50) + fin +race + mar + paro + prio")
cph.print_summary(model="spline_model"); print()
cph.check_assumptions(rossi, show_plots=True, p_value_threshold=0.05)
```

![](img/e066f2035359480ca057507d1c65cda6.png)

```
Proportional hazard assumption looks okay.

[]
```

我们看到可能仍然有一些潜在的违规，但这是一个很大的减少。此外，有趣的是，当我们包括这些非线性的年龄项时，wexp 的比例违反消失了。改变一个变量的函数形式会影响另一个变量的比例检验，通常是积极的，这种情况并不少见。因此，如果我们愿意，我们可以删除 strata=['wexp']。

## <>绑定变量并在其上分层

提出的第二个选择是将变量分成大小相等的容器，并像我们在 wexp 中所做的那样分层。这里有一个估计和信息损失之间的权衡。如果我们有大的箱，我们将丢失信息(因为不同的值现在被放在一起)，但是我们需要估计较少的新基线危险。另一方面，对于小容器，我们允许年龄数据有最大的“回旋空间”，但必须计算许多基线危险，每个危险都有较小的样本量。像大多数东西一样，最优值介于两者之间。

```
rossi_strata_age = rossi.copy()
rossi_strata_age['age_strata'] = pd.cut(rossi_strata_age['age'], np.arange(0, 80, 3))

rossi_strata_age[['age', 'age_strata']].head()
```

![](img/a25a089add60564f58fefef156a408d2.png)

```
# drop the orignal, redundant, age column
rossi_strata_age = rossi_strata_age.drop('age', axis=1)
cph.fit(rossi_strata_age, 'week', 'arrest', strata=['age_strata', 'wexp'])<lifelines.CoxPHFitter: fitted with 432 total observations, 318 right-censored observations>cph.print_summary(3, model="stratified age and wexp")
cph.plot()
```

![](img/6365c676767b05d0e919e94461c073c3.png)

```
<matplotlib.axes._subplots.AxesSubplot at 0x13c7027c0>
```

![](img/4a617ffb7f0e39273faa50cfc256214a.png)

```
cph.check_assumptions(rossi_strata_age)Proportional hazard assumption looks okay.

[]
```

## <>引入时变协变量

我们纠正违反比例风险假设的变量的第二个选择是直接建模时变成分。这分两步完成。第一个是将数据集转换成情节格式。这意味着我们将单个行中的主题拆分成𝑛新行，每个新行代表该主题的某个时间段。在这个新的时间段内，变量是静态的，这没关系，我们稍后将引入一些时变协变量。

关于如何在生命线中做到这一点，请参见下文:

```
from lifelines.utils import to_episodic_format

# the time_gaps parameter specifies how large or small you want the periods to be.
rossi_long = to_episodic_format(rossi, duration_col='week', event_col='arrest', time_gaps=1.)
rossi_long.head(25)
```

![](img/00dc92405a2a4fa1337762b3f922f0f1.png)

每个主题都有一个新的 id(如果已经在数据帧中提供，也可以指定)。该 id 用于跟踪一段时间内的主题。请注意，在(可能的)事件发生之前的所有期间，停止 col 也是 0。

上面我提到有两个步骤来修正年龄。第一个是转换成一个插曲式的格式。第二是在年龄和停止之间创建一个交互项。这是一个时变变量。

我们必须使用 CoxTimeVaryingFitter，而不是 CoxPHFitter，因为我们正在处理一个情节数据集。

```
rossi_long['time*age'] = rossi_long['age'] * rossi_long['stop']from lifelines import CoxTimeVaryingFitter
ctv = CoxTimeVaryingFitter()

ctv.fit(rossi_long,
        id_col='id',
        event_col='arrest',
        start_col='start',
        stop_col='stop',
        strata=['wexp'])<lifelines.CoxTimeVaryingFitter: fitted with 19809 periods, 432 subjects, 114 events>ctv.print_summary(3, model="age * time interaction")
```

![](img/820c5d3c92ebca926c6f7cd0ff108157.png)

```
ctv.plot()<matplotlib.axes._subplots.AxesSubplot at 0x13cc249a0>
```

![](img/14dcda272bab6e1a27bca6547eeabe32.png)

在上面按比例缩放的舍恩菲尔德年龄残差图中，我们可以看到时间值越高，影响越小。这在 CoxTimeVaryingFitter 的输出中得到证实:我们看到时间*年龄的系数是-0.005。

## <>我需要关心比例风险假设吗？

你可能会惊讶，通常你不需要关心比例风险假设。有很多原因不能:

如果你的目标是生存预测，那么你就不需要关心比例风险。你的目标是最大化一些分数，与预测是如何产生的无关。给定足够大的样本量，即使非常小的违反比例危险的情况也会出现。有合理的理由假设所有数据集都将违反比例风险假设。这在 Stensrud & Hernán 的“为什么要测试比例风险？”。“即使危险不是成比例的，改变模型以适应一组假设也从根本上改变了科学问题。正如图基所说，“最好是对确切问题的近似回答，而不是对近似问题的确切回答。“如果在存在非比例风险的情况下，你要符合考克斯模型，净效应是什么？功率略低。事实上，您可以通过稳健的标准误差(指定 robust=True)来恢复大部分能力。在这种情况下,(指数化)模型系数的解释是风险比的时间加权平均值——我每次都这样做。”来自 AdamO，稍加修改以适应生命线。鉴于上述考虑，现状仍然是检查成比例的危险。因此，如果你在避免比例风险测试，一定要理解并能够回答你为什么要避免测试。

## 参考

以下资源对发表这篇文章非常有帮助。如果你想了解更多关于生存分析的知识，请查阅它们。

1.  弗兰克·小哈勒尔..回归建模策略:应用于线性模型、逻辑和有序回归以及生存分析
2.  2012 年，生存分析，自学书籍
3.  [评估](http://www.math.ucsd.edu/~rxu/math284/slect9.pdf)考克斯模型的拟合度
4.  [假设检验](http://www.drizopoulos.com/courses/emc/ep03_%20survival%20analysis%20in%20r%20companion)生存包在 R 段
5.  Cox 比例风险回归模型([幻灯片](http://publicifsv.sund.ku.dk/~pka/SACT18-part1/cox.pdf))
6.  [Barry Leventhal 的商业见解演讲](http://www.barryanalytics.com/Downloads/Presentations/Survival%20Analysis.pdf)
7.  [R 中的生存包](https://cran.r-project.org/web/packages/survival/survival.pdf)
8.  [Python 中的生命线包](https://lifelines.readthedocs.io/en/latest/)