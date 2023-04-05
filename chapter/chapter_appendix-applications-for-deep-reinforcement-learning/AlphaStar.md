

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-01 02:35:04
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-01 02:49:53
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# AlphaStar及背景简介

相比于之前的深蓝和AlphaGo，对于《星际争霸Ⅱ》等策略对战型游戏，使用AI与人类对战的难度更大。比如在《星际争霸Ⅱ》中，要想在玩家对战玩家的模式中击败对方，就要学会各种战术，各种微操和掌握时机。在游戏中玩家还需要对对方阵容的更新实时地做出正确判断以及行动，甚至要欺骗对方以达到战术目的。总而言之，想要让AI上手这款游戏是非常困难的。但是DeepMind做到了。

AlphaStar是DeepMind与暴雪使用深度强化学习技术实现的计算机与《星际争霸Ⅱ》人类玩家对战的产品，其因为近些年在《星际争霸Ⅱ》比赛中打败了职业选手以及99.8%的欧服玩家而被人所熟知。北京时间2019年1月25日凌晨2点，暴雪公司与DeepMind合作研发的AlphaStar正式通过直播亮相。按照直播安排，AlphaStar与两位《星际争霸Ⅱ》人类职业选手进行了5场比赛对决演示。加上并未在直播中演示的对决，在人类对阵AlphaStar的共计11场比赛中，人类仅取得了1场胜利。DeepMind也将研究工作发表在了2019年10月的 Nature 杂志上。本章将对这篇论文进行深入的分析，有兴趣的读者可以阅读原文。

AlphaStar是DeepMind在解决了围棋问题之后，在RTS游戏领域的尝试。

里程碑事件：

2018.12 5:0击败TLO，5:0击败MaNa。

2019.1 表演赛不敌MaNa。

论文：

《Grandmaster level in StarCraft II using multi-agent reinforcement learning》

## 关于AlphaStar的总结

关于AlphaStar的总结如下。

（1）AlphaStar设计了一个高度可融合图像、文本、标量等信息的神经网络架构，并且对于网络设计使用了自回归（autoregressive）技巧，从而解耦了结构化的动作空间。

（2）其融合了模仿学习和监督学习的内容，例如人类统计量  Unexpected text node: 'Z'
 Z 的计算方法。

（3）其拥有复杂的深度强化学习方法以及超复杂的训练策略。

（4）其完整模型的端到端训练过程需要大量的计算资源。对于此，原文表述如下：每个智能体使用32个第三代张量处理单元（tensor processing unit，TPUs）进行了44天的训练；在训练期间，创建了近900个不同的游戏玩家。


## 其中与博弈论相关的内容

比如掌控了星际争霸的 AlphaStar 的开发过程中就充满了博弈论的影子。星际争霸作为一个游戏，就像剪刀石头布一样，很难得到一个单一的最佳方案，是永远在博弈中的，要对当下的已有的策略进行学习，并选择最好的那个。[2]

AlphaStar 所参考的算法就是 Double Oracle Algorithm（DO Algo），这个算法将目标定为寻找当下 stage game 的 NE。DO Algo 的原理如下图所示。它使用深度神经网络进行函数逼近，迭代计算子游戏的收益矩阵（Gt）。这个子游戏就是上文提到的 stage games。在每个时间 t 处（每个 stage game），都会计算出符合 NE 的回应（σ），并得到最优策略（π），然后添加新的策略来扩展 Gt 为 Gt + 1，继续重复上述过程。

以下图为例，开始两个玩家的收益矩阵 Gt 很像刚刚举的囚徒案例，π代表各个玩家可以采取的策略，初始的 Gt 中各个玩家只有两个策略（比如两个玩家各有两个进攻计划，佯攻和攻击分矿），然后通过求 NE 响应来获得两个玩家在这个场景下能得到的最佳相应 P1，P2。这时，两个玩家都发现，对现有的进攻计划都不是很有信心，希望拖入后期，于是玩家继续从自己的策略库中选择策略加入到 Gt 中（比如开分矿），从而得到 Gt+1，然后此时对应的 P1，P2 又被计算出来。总的来说就是在当下的 stage game 中计算 NE 响应，再根据情况扩展游戏，再计算，直到你对结果变得有信心为止。

强化学习的每一个 step 都可能需要很长时间才能收敛到好结果，而有些已经学过的东西可能会在后面的学习过程中再学一遍，为了解决 PSRO 中计算量过大的问题，这篇论文又提出了 DHC 算法以使得 PSRO 并行化。具体来说，就是不再挨个 epoch 进行训练，而是预先确定 K 个 level（代替 epoch），并开启 NK 个线程（N 为玩家数），每个线程训练一个玩家的一个 oracle，并将结果定期保存到中央存储器中。每个线程还会有当前 level 或者更低 level 的策略信息（类似后面的 epoch 知晓前面 epoch 的训练结果）。由此可见，DHC 牺牲了准确性（减少了 epoch 数）以换取其可扩展性（减少计算量）。
可以看出，这个算法是博弈论与 RL 算法密切结合的产物，它已经被应用在 AlphaStar 并获得了巨大的成功，由此可见，博弈论所代表的 RL 学习策略是很值得信赖的。

[1]: https://aistudio.baidu.com/aistudio/projectdetail/4565322?channelType=0&channel=0
[2]: https://developer.aliyun.com/article/818419?spm=a2c6h.12873639.article-detail.55.7fa137a8RUrUg3
