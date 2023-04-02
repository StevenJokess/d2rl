

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


[1]: https://aistudio.baidu.com/aistudio/projectdetail/4565322?channelType=0&channel=0
