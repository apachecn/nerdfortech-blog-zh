# 浅析自动驾驶的四大关键技术(一)

> 原文：<https://medium.com/nerd-for-tech/a-brief-analysis-of-the-four-key-technologies-of-autonomous-driving-part-1-4027510e1f88?source=collection_archive---------6----------------------->

![](img/75466c43eafe3d1e57635281431eda60.png)

世界卫生组织 2017 年 5 月提供的数据显示，全球每年因道路交通事故死亡的人数约为 125 万，相当于每天有 3500 人死于交通事故。媒体记者从全国安全生产工作会议上获悉，2016 年全国共发生交通事故 6 万起，死亡人数达 4.1 万人。

为了提高驾驶安全性，目前有两个重要的方向。一种是加强交通管制，用高压政策强制司机安全驾驶；另一种是将汽车从人类的操作中分离出来，这也是全球车企和科技公司正在做的事情。从技术上来说，从人类操作员那里移走汽车是自动的或无人驾驶的。自动驾驶汽车依靠人工智能、视觉计算、雷达、监控设备和 GPS(全球定位系统)的合作，让计算机自动安全地运行，而无需任何人类的主动操作。

自动驾驶的关键技术可以分为四个部分:环境感知、行为决策、路径规划和运动控制。

## **感知技术**

环境感知作为第一步，是对环境信息和车载信息的采集和处理，是智能车辆自主驾驶的基础和前提。周围环境信息的获取涉及道路边界检测、车辆检测、行人检测，这就是所谓的传感器技术。传感器通常包括摄像机、车载雷达、速度和加速度传感器等。感知也是智能汽车最昂贵的部分。

尽管如此，数百万个雷达，几个高清摄像头对于感知技术来说还是不够的。由于传感器的局限性，单个传感器无法满足各种工况下的精确感知。车辆的平稳运行需要多传感器融合技术的应用。

## **决策技术**

完成感知部分后，接下来要做的就是根据获得的信息做出判断，确定合适的工作模式，制定相应的控制策略。这部分的作用类似于给车辆分配相应的任务。例如，在车道保持、车道偏离警告、车辆距离保持和障碍物警告的情况下，需要预测一段时间内车辆和其他人、车道和行人的状态。高级决策理论包括模糊推理、强化学习、神经网络和贝叶斯网络技术。

## 自动驾驶的产业化焦点

自动驾驶的主流算法模型主要基于有监督的深度学习。它是一种算法模型，推导出已知变量和因变量之间的函数关系。需要大量的结构化标记数据来训练和调整模型。

在此基础上，要想让自动驾驶汽车变得更加“智能”，形成可在不同垂直落地场景下复制的自动驾驶应用商业模式闭环，模型需要有海量、高质量的真实道路数据支撑。

## 2D-三维融合数据

例如，为了开发自动驾驶汽车的多模型机器学习算法，一些制造商需要融合两个不同维度的不同数据集。这个操作很重要，但是手动执行很有挑战性。

AI 公司甚至希望数据公司能够更好的了解算法技术和需求场景，参与算法的研发，给出数据采集的优化建议。创造竞争优势也成为数据服务提供商关注的焦点。

## 常见的数据标注类型包括:

*   2D 包围盒
*   [车道标线](https://tinyurl.com/u7u4me)
*   [视频跟踪标注](http://tinyurl.com/wmu4yfhh)
*   点标注
*   [语义分割](https://tinyurl.com/48w576p7)
*   三维物体识别
*   3D 分割
*   传感器融合:传感器融合长方体/传感器融合分割/传感器融合长方体跟踪

# 结束

将你的数据标注任务外包给 [ByteBridge](https://tinyurl.com/2sfx9sxb) ，你可以更便宜更快的获得高质量的 ML 训练数据集！

*   无需信用卡的免费试用:您可以快速获得样品结果，检查输出，并直接向我们的项目经理反馈。
*   100%人工验证
*   透明和标准定价:[有明确的定价](https://www.bytebridge.io/#/?module=price)(包括人工成本)

为什么不试一试？

来源:https://new . QQ . com/omn/20220104/20220104 a 06 swm 00 . html