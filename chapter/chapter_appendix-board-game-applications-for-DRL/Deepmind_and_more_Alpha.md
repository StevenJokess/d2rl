

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-04 20:38:00
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-05-12 01:30:16
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# Deepmind 和 Alpha智能体家族

## Deepmind

twitter[2]


## 进化之路

1. 2016年3月, DeepMind开发的AlphaGo程序以4： 1击败韩国围棋冠军李世石 (Lee Se-dol)，成为近年来人工智能领域里程碑事件。
1. 2017年, DeepMind发布了AlphaGo Zero, 在自我训练3天后以100-0击败了AlphaGo。
1. 2018年，DeepMind发表了AlphaZero，能下围棋、将棋和国际象棋。
1. 2019年, AlphaStar学会了玩《星际争霸II》, 在所有三场比赛中都被评 为大师级, 在官方的人类玩家排名中排名前 $99.8 \%$ 以上。
1. 2020年12月23日, DeepMind发表了MuZero, 不仅能下围棋、将棋和国 际象棋, 还在30多款雅达利游戏中展示出了超人类表现。
1. 2022年2月16日, DeepMind发布了可以对托卡马克装置中的等离子体构 型进行磁控制, 帮助达到可控核聚变的人工智能。
1. 2022 年10月5号, 使用Al(RL)自动设计算法(矩阵乘), 开创Al4science 新赛道
1. 以及2020年用于量子优化、2018年用于化学分子计算等[1]

## DeepMind使用的TPU是什么？TPUv1和v2有什么区别？

## StarCraft

星际争霸（StarCraft）就是一款这样的游戏，于1998年由暴雪娱乐公司发行（见图14.19）。它的资料片母巢之战（Brood  War）提供了专给AI程序使用的API，激发起很多AI研究者的研究热情。

在平台方面，DeepMind在成功使用深度学习攻克Atari游戏后，宣布和暴雪公司合作，将StarCraft  II作为新一代AI测试环境，发布SC2LE平台，开放给AI研究者测试他们的算法。SC2LE平台包括暴雪公司开发的Machine  Learning  API、匿名化后的比赛录像数据集、DeepMind开发的PySC2工具箱和一系列简单的RL迷你游戏。Facebook也早在2016年就宣布开源TorchCraft，目的是让每个人都能编写星际争霸AI程序。TorchCraft是一个能让深度学习在即时战略类游戏上开展研究的库，使用的计算框架是Torch。

在算法方面，Facebook在2016年提出微操作任务，来定义战斗中军事单位的短时、低等级控制问题，称这些场景为微操作场景。为了解决微操作场景下的控制问题，他们运用深度神经网络的控制器和启发式强化学习算法，在策略空间结合使用直接探索和梯度反向传播两种方法来寻找最佳策略。阿里巴巴的一批人也在2017年参与到这场AI挑战赛中，提出一个多智能体协同学习的框架，通过学习一个多智能体双向协同网络，来维护一个高效的通信协议，实验显示AI可以学习并掌握星际争霸中的各类战斗任务。

一般说来，玩星际争霸有三个不同层面的决策：最高层面是战略水平的决策，要求的信息观察强度不高；最低层面是微操作水平的决策，玩家需要考虑每个操控单位的类型、位置及其他动态属性，大量的信息都要通过观察获取；中间层面是战术水平的决策，如兵团的位置及推进方向，如图14.20所示。可见，即时战略类游戏对AI来讲有着巨大的挑战，代表着智能水平测试的最高点。[3]



[1]: https://www.bilibili.com/video/BV1PD4y147gK/?spm_id_from=333.337.search-card.all.click&vd_source=bca0a3605754a98491958094024e5fe3
[2]: https://twitter.com/DeepMind/status/1600929160335351809
[3]: http://www.nvidia.com/object/accelerate-inference.html
