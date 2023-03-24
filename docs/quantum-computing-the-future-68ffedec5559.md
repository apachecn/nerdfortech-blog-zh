# 量子计算:未来

> 原文：<https://medium.com/nerd-for-tech/quantum-computing-the-future-68ffedec5559?source=collection_archive---------13----------------------->

撰稿人:Gaurav Lohkna

正如我们所观察到的，计算机技术正在促进我们社会从工业经济到信息经济的彻底转变。代码驱动的系统已经在环境信息和连接方面普及到世界上一半以上的居民，为他们提供了以前无法想象的机会。

让我们从一开始就用电子计算机来理解计算 **ENIAC，1946 年，**是第一台电子和可编程计算机，每秒可执行 10 千条指令(KIPS)，内存空间为 2 千字节(KB)，占用大小为 50x30 英尺的地下室。它有 17，000 多个真空管、70，000 个电阻、10，000 个电容、6，000 个开关和 1，500 个继电器，是迄今为止最复杂的电子系统。这是一个庞然大物，产生 174 千瓦的热量，每秒钟执行多达 5000 次加法，比它的机电前辈快几个数量级。

![](img/7a3eb5d0ae899298781214d0a281b816.png)

[1946 年埃尼阿克](https://www.google.com/url?sa=i&url=https%3A%2F%2Fsmartermsp.com%2Fmeet-eniac-first-computer%2F&psig=AOvVaw05SD-eIrmfMYUduLX77nVj&ust=1620548788510000&source=images&cd=vfe&ved=0CAIQjRxqFwoTCNDpserUufACFQAAAAAdAAAAABAD)

今天，我们有了更强大的计算机，它们拥有多核 GHz 处理器，每秒可以处理超过 1000 亿条指令，我们可以将它们放在口袋里，同时在日常生活中使用它们。简而言之， **iPhone 6** 的时钟比最好的**阿波罗时代的计算机(1960 年)**快 32600 倍，执行指令的速度也快 1.2 亿倍。敬畏？

现在，你可能想知道我们是如何实现所有这些计算能力的？所以答案是，我们制造的处理器越小，计算机的处理速度就越快。根据**摩尔定律** *，* *“密集集成电路中的晶体管数量大约每两年翻一番，因此处理能力也随之增加”*。然而，摩尔定律正在放缓，这意味着数十年来计算能力的无尽增强即将结束，因为为计算机芯片制造更小的硅基晶体管变得越来越昂贵。尽管如此，世界各地的研究人员已经找到了推动晶体管极限的方法，以至于今天我们在研究人员的实验室中已经有了 5 纳米、3 纳米、1 纳米、0.5 纳米甚至单原子晶体管。

但是最近，我们挑战物理定律，创造了 0 纳米的晶体管，是的，你没看错——0 纳米。科学家创造了一种不存在维度的单光子晶体管。

![](img/ed7e66dcfec91d4e2a4120350abf790d.png)

[单光子晶体管](https://i0.wp.com/www.fanaticalfuturist.com/wp-content/uploads/2020/07/photontransistor.jpg?resize=768%2C425&ssl=1)

有了这种小得多的晶体管，我们或许可以在不久的将来在智能手机中实现超级计算机的处理能力，但问题是，超级计算机真的像它们看起来那么快吗？

对于某些问题，超级计算机并没有那么超级。到目前为止，我们一直依靠超级计算机来解决大多数问题。这些是非常大的经典计算机，通常有数千个经典 CPU 和 GPU 核心。然而，超级计算机并不擅长解决某些类型的问题，这些问题乍一看似乎很简单。

这就是为什么我们需要一个全新的计算概念:量子计算。**量子计算机**利用量子力学现象实现计算的巨大飞跃，以解决当今最强大的超级计算机无法解决、也永远不会解决的某些复杂问题。

现在让我们用一个简单而经典的计算问题来理解超级计算机和量子计算机的计算能力。想象一下，你想在一个晚宴上安排几个挑剔的人入座，在所有不同的可能组合中，只有一个最佳的座位方案。你需要探索多少种不同的组合才能找到最佳组合？

![](img/05bffd938bea93bfe98cf08546fdceda.png)

[组合的可视化](https://d1whtlypfis84e.cloudfront.net/guides/wp-content/uploads/2018/09/11154224/permutation3.png)

2 个人有 2 种组合，3 个人有 6 种组合。你能猜出 10 个挑剔的人有多少种组合吗？将会有超过 300 万种组合，确切地说是 365 万种组合。惊讶？

而如果你要给 **100 个这样的人**安排座位呢？你将需要一台超级计算机来确定座位安排计划，它将大约是**9.34 x 10^157(9.34 之后的 157 个零)** **组合**，这可能需要**几天来计算。讽刺地说，如果客人们要等座位安排计划，他们会饿死的。量子计算机来了，它可以在几秒钟内计算出来，把客人从饥饿中拯救出来。很神奇，对吧？**

让我试着用外行人的理解方式告诉你两者速度的比较。*假设你想从 1 万亿人的队伍中找到一个人(****10 亿人*** *)，每个人花 1 微秒来检查*(这真的非常快)，那么一台**经典计算机将需要大约 1 周**来找到那个人，而一台**量子计算机将只需要大约 1 秒**。

![](img/a180f9514780fac1200ef7c99817bf80.png)

[IBM **Q |** 系统一](https://www.ibm.com/quantum-computing/_nuxt/img/8e98570.png)

这个由 IBM 设计的野生动物 **IBM Q** 目前大约有一个家用冰箱大小，带有一个衣柜大小的电子设备盒**可以创造出巨大的多维空间来表现这些非常大的问题。经典的超级计算机无法做到这一点**。然后，利用量子波干涉的算法被用来在这个空间中寻找解决方案，并将它们转换回我们可以使用和理解的形式。

量子计算机使用**量子位或量子位**，而不是使用二进制位(0 或 1)。量子位本身并不十分有用。然而，通过创造许多量子位并以一种叫做**叠加**的状态连接它们，我们可以创造巨大的计算空间。然后，我们用可编程门来表示这个空间中的复杂问题。这些量子比特表现随机，为了使用它们的计算能力，我们需要**纠缠，**量子纠缠使量子比特能够完美地相互关联。使用利用量子纠缠的量子算法，可以比在经典计算机上更有效地解决特定的复杂问题。

![](img/71d082bccda3faa605b213f28a3e1140.png)

[纠缠](https://www.ibm.com/quantum-computing/_nuxt/img/3cca902.png)

量子计算是一种异常强大的计算工具，有了这种强大的能力，我们可以提升人类文明。我们可以利用量子计算机的可能性非常广泛。我将讨论量子计算机的一些主要应用。

# 分子建模

最让我激动的话题之一是用于治疗不同类型蛋白质细胞癌症的一些最著名药物的分子水平生物学。我们可以为每个蛋白质细胞设计一个分子模型，以及它如何受到药物相互作用的影响，因为化学反应在本质上是量子的，因为它们形成高度纠缠的量子叠加态。

![](img/09d5f33f97478644bcd5d3ceba35f20f.png)

[蛋白质折叠](https://upload.wikimedia.org/wikipedia/commons/thumb/0/05/Protein_structure.png/1200px-Protein_structure.png)

# 天气预报

全球 30%的 GDP 都直接或间接地受到天气及其预报的影响。目前，我们使用传统的计算机来预测天气，但它不是那么有效，它可能需要比实际天气更长的时间来发展。在天气预报中，有数十亿数十亿的变量相互依赖，经典计算机预测起来非常复杂，但量子计算机的基本能力是并行执行指数级大型计算，减少时间和劳动力。有了这个，我们也许能够生产更好的庄稼，把人们从自然灾害中拯救出来，甚至可以预测人类对环境的长期影响。

![](img/9943e52ce06902c821befeb3d5513cbb.png)

[预测模型](http://wxguys.ssec.wisc.edu/wp-content/uploads/2019/03/WeatherModel.png)

# 密码系统

几乎所有在线交易的安全性都取决于将大数分解成质数的难度。经典计算机可以完成这项工作，但同样需要指数数量的时间进行因式分解，但量子计算机可以在很短的时间内完成这项工作，从而使素数因式分解密码学过时。因此，我们需要新的复杂的密码来保护互联网。

![](img/9cf0cd5fff8be4d487ce2ce102cded8d.png)

[图](https://images.theconversation.com/files/251023/original/file-20181217-185246-1vohit9.jpg?ixlib=rb-1.1.0&rect=0%2C820%2C7472%2C3730&q=45&auto=format&w=1356&h=668&fit=crop)

量子计算不亚于给人类最好的自我改造的礼物。对我来说，这感觉就像我们回到了 20 世纪 40 年代，我们试图创造我们的第一台可编程计算机，后来它将人类改造到一个没有计算机我们就无法生存的水平。量子计算机很可能在未来几年内商业化，这本身将是一个计算的新时代。

# 资源和推荐

1.  [参考文献[1]](https://pdf.sciencedirectassets.com/271027/1-s2.0-S0735109787X80730/1-s2.0-S073510978780102X/main.pdf?X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLWVhc3QtMSJGMEQCIEMeEguBvveJ%2FEbqqgXz030vNDEUXZfDOjPfW5QkurSlAiBhqh%2BMIgyaV0O0gGHVMitjl0CFwVIioNvC6rHZnB9ytiq0AwhHEAMaDDA1OTAwMzU0Njg2NSIM9YDDY%2FtHZJBLDWt%2BKpED4zfadF5nR4QtB1EyLS1eQ5k6pI4Qjz%2BL6Rf5p1JUXdQnbl8T7eA2%2FDm1CWZle%2BrOD9yU0FWG1R3rmctpc0fQCjLx9TY61inUbIdVRV%2FOgrI4OcQRtB6gCfXlIueZfWu9ttb9jqoy0xGJf0axPOJJDCDDIE8XZsTPC05SChA6JXyJ3pGv%2BQkXzz7yE3IyAbV56dgBTcdKJqPD1P2w95xaXhPTXAPOlDU%2BUJcYa2p3M3HgfDlALqs7b%2FFHdd%2FFNxCOnT3%2F%2FVWnAsjc9MUhigoSTc%2F9sNiLAgEAl%2FTgWj7a2OdfhSY9S8lr9h72199vSDuJNn4E53363Bz9f81kQ7C6xlGcpg84xmZLVPPkFehe75pD13lXhv%2Btb2Ui6MHo%2FlX4EDAgFgKRsUmsJCIJidS7yUheDLLfvEz8u%2BH9Ytx2vQLjddH9dLtg9hJ%2B4B4t0TzBCjYf5eyc3bOiGzoGPjgUNx5v8rQv24i1tzuaOnOM0jFxCe8w5v26zT%2BfqcRHyLKXd4%2BxCbtZVaAq9Rzp7KShQ0gwq5jVhAY67AFPd%2F90OiQlnZuoTxev8WsOPM0Gm%2BtfBn9hPxH3cYqG1XIwSswMc51mTGHHPUQP8pLlZ7%2B2geeDEVuTUhIXBEMhA%2BP8Yh4VvXYaJC1klpdf1KSFVC0VziNt4U9FjB68v0eVBQy8J8hveNbqLlkOGAb%2F2wWiAjRPiQwkpUkRGXLJHdEpCEwFKsz%2FRyflNkFhlpTovE2CHB%2FI0E6dQFN9kcW%2FyEVMky8p3ofKMfGDDwadQbHmfchBvWHQourCUX9cYWsp99EY%2B8neXZcTtHh1in3a5TptfLNcnKJmv5tUh9mxGIonL3j8Y3F5oyJ3ig%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20210507T153551Z&X-Amz-SignedHeaders=host&X-Amz-Expires=300&X-Amz-Credential=ASIAQ3PHCVTYXZUQ7RV5%2F20210507%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=1c5e2403d62b2c21848161862230447f81019dc7303cbc056ccac901935072ef&hash=68ffd1996ea8bb8c4b1e38cf46f83f0dbb7396320c0f9d09642293af1f146fe5&host=68042c943591013ac2b2430a89b270f6af2c76d8dfd086a07176afe7c76c2c61&pii=S073510978780102X&tid=spdf-3dba1df6-a44d-44ce-ad64-e40891b35f21&sid=6d3d04fd6c517445208b4155810fa9a6ed44gxrqb&type=client)
2.  [参考文献[2]](https://www.ibm.com/quantum-computing/what-is-quantum-computing/)
3.  [参考文献[3]](https://singularityhub.com/2017/06/25/6-things-quantum-computers-will-be-incredibly-useful-for/)
4.  [参考文献【4】](https://www.britannica.com/technology/ENIAC)
5.  [参考文献【5】](https://www.colocationamerica.com/blog/what-makes-computers-fast)
6.  [参考文献【6】](https://www.fanaticalfuturist.com/2020/07/at-just-a-single-photon-the-worlds-smallest-transistor-has-literally-zero-size/)