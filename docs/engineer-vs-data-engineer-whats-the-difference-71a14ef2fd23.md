# 工程师和数据工程师有什么区别

> 原文：<https://medium.com/nerd-for-tech/engineer-vs-data-engineer-whats-the-difference-71a14ef2fd23?source=collection_archive---------10----------------------->

作为一名数据工程师，与作为一名 web 开发人员、后端开发人员相比，主要的区别是什么？我在这两个职位上工作了几年，这是我列出的 6 项完全不同的事情:)

![](img/2817b4c4ad5c6e0a4715babc5ec83ee6.png)

**大小很重要**
你正在处理的数据的大小真的很重要。在设计管道时，你需要考虑到这一点，你需要为你正在处理的数据选择合适的算法，以便它稳定、足够快，并且不会花费太多(太多的意思当然会因具体公司而异)

至于后端开发者比较，数据的大小是你看的东西，但它通常只对那些真正为他们的应用/网站/系统带来巨大流量的公司重要。因此，在实现新功能和维护服务时，您很可能不需要为此进行太多的优化。

**算法很重要**
如前所述，你使用的算法也很重要。通常，会有许多处理数据的选择，其中一些会比其他的好得多。它们可能需要额外的工作和逻辑，但是例如将使流水线更快，或者成本更低/需要更少的资源。你将要做的一些更大的任务是把一种算法换成另一种算法，在某个时刻，这种算法会表现得更好。

做全栈工程师的时候，通常不会太关注算法，通常是因为不需要。如前所述，大部分重点是编写易于更改/维护和添加功能的代码。

你将有很多时间
这实际上可能是作为一名数据工程师(或专业人士)工作时的一个缺点，如果你习惯于即时反馈某个给定的东西是否工作，你就不会那么容易得到它。许多变化需要对大量数据进行测试。因此，你经常会等待某项工作结束，以获得这种反馈。如果你能在这段时间里找到一些有成效的事情去做，这很好。通常，可以是写测试/学习新的东西，等等。最重要的是，你仍然应该努力把时间减到最少，调试需要几个小时的工作比调试需要几分钟的工作要困难得多。短一点的通常也便宜很多；)但那是带我们去一个不同的东西。

![](img/7796c673b2e1334b04662fbffabfb1e1.png)

你的工作成本
当然，对于后端/前端工程师来说，用他们的代码进行生产也是有成本的，但是在这里你将在另一个层面上填充它。通常你做的每一件事，比如测试，都要花钱(通常相当多)。如果你犯了错误，需要重新运行，这也是要花钱的。

总的来说，没什么好担心的(除了当你有老板的时候，经常问你这个问题)。也有可能你会看到并做改进，这可以减少你的月薪或更多的成本，然后你会很容易觉得你值得这笔钱；)

你不会确定代码是否有效
这是数据工程师工作中最令人沮丧的事情之一。对于网站开发者来说，如果你的网站看起来工作正常，那么它“通常”是正常的。对于数据工程师来说，当您的工作的唯一输出通常是转换后的数据时，您的管道很容易悄悄地产生完全不正确的输出。如果您在一天后发现它，这很好，但是您可能会在引入错误(不一定是您的团队)后的这个星期/月发现它。

为了帮助解决这个问题，我构建了一个开源工具来发现这些问题(你可以在这里找到它[](https://github.com/redata-team/redata)****:)而且很多时候你会构建定制的仪表板来帮助识别这些情况，并确保你所产生的是正确的。****

****更少的谷歌搜索和使用更少的库作为一名网络开发人员，你通常会搜索你正在做的事情的一半。特别是如果做一些新的事情，尝试使用一个新的库，增加一个特性，这当然是由一些 Django/Flask 库完成的。对于数据工程师来说，情况往往并非如此。你需要学习一些东西(例如 Spark ),并且你会谷歌很多运行 pipeline 的错误(当使用 Spark 时),但是新的特性通常不会通过添加一个新的库和谷歌如何使用它来实现:)****

****因此，无论你是否正在考虑改变你的职业道路，成为一名数据工程师，或者已经是了，请告诉我你对这一点的看法。你同意并在你的工作中看到它(无论是作为一名数据工程师还是网站开发人员)还是你有不同的看法？****