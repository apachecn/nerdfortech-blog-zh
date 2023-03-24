# R 中的系综——装袋、助推和堆叠的概念和工作原理

> 原文：<https://medium.com/nerd-for-tech/ensembles-in-r-the-concept-and-working-of-bagging-boosting-and-stacking-c79f320f6e73?source=collection_archive---------14----------------------->

《牛津词典》将“合奏”定义为一起表演的一群音乐家、演员或舞蹈演员。最终交付的性能是无与伦比的。同样，R 中的集成是组合几个模型以创建健壮模型的过程。

有三种方式可以创建合奏，即装袋、助推和堆叠。

**Bagging:** Bagging 是‘Boot Strap Aggregation’的缩写。顾名思义，涉及的两个主要步骤是…