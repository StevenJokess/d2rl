

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-17 18:02:50
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-14 23:28:15
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了.![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 多智能体强化学习

## 物联网支持

物联网：为AI提供发现、连接、通信、协同的支持，将AI从单智能体装置拓展到多智能体系统[8]

## MAL

多智能体学习（Multi­agent Learning，MAL）将机器学习技术引入多智能体系统领域，研究如何设计算法去创建动态环境下的自适应智能体。

在多个自适应智能体相互作用的情况下，一个智能体的收益通常依赖于其他个体的行动。学习环境已经不再是静态的了，而是非稳态（Non-Stationary）[5]的环境。所谓非稳态是指，环境迁移的分布会发生改变。

## MA+RL

MAL 领域被广泛研究的技术是强化学习（Reinforcement Learning）。单智能体的强化学习通常在马尔科夫决策过程（Markov Decision Processes，MDP）的框架内就能被较好地描述。一些独立的强化学习算法（如Q-­learning）在智能体所处环境满足马氏性且智能体能够尝试足够多行动的前提下会收敛至最优的策略。

尽管MDP为单智能体学习提供了可靠的数学框架，对多智能体学习却并非如此。当这个环境当中，除了有一个智能体在学习以外，还有别的智能体在相应的去发生交互和学习的这个过程当中。这个整个任务就变成了一个叫做多智能体强化学习，也叫Multi-agent reinforcement learning 。由于这时候环境变成了是多个智能体之间的交互，以至于它就变成了是一个非稳态的环境。

> 假如把其他智能体考虑成环境的一部分从而能够使用单智能体的Q学习算法，是否可行？
>
>- 这种做法破坏了理论上的收敛保证，使学习不稳定？？？
>- 即，每个智能体策略（学习目标）都依赖其他智能体的策略，并要不断变化以随机应变。

因此，有必要对原有的MDP 框架作相应的扩展，这其中有马尔科夫博弈和联合行动学习机等。


## 多智能体强化学习的环境

首先， MARL的环境是以马尔可夫决策过程为基础的随机博弈框架， 它是这样一个元组 $\left\langle S， A_1， \cdots， A_n， R_1， \cdots， R_n\right。$， $P\rangle$ 。其中， $n$ 指多智能体的数量; $A$ 是所有智能体的联合动作 空间集， $A=A_1 \times \cdots \times A_n ; R_i$ 是每个智能体的奖励函数， $R_i$ : $S \times A \times S \rightarrow R ; P$ 是状态转移函数， $P: S \times A \times S \rightarrow[0，1]$ 。我 们假设奖励函数是有界的。

![马尔可夫博弈](。。/。。/img/Markov_game。jpg)

在多智能体情况下， 状态转换是所有智能体共同行动的 结果， 因此智能体的奖励也取决于联合策略。定义策略 $H$ 是智能体的联合策略 $H_i: S \times A \rightarrow H$， 相应地， 每个智能体的奖励为：

$$
R_i^H=E\left[R_{t+1} \mid S_t=s， A_{t， i}=a， H\right]
$$

其贝尔曼方程为：

$$
\begin{aligned}
& v_i^H(s)=E_i^H\left[R_{t+1}+\gamma V_i^H\left(S_{t+1}\right) \mid S_t=s\right] \\
& Q_i^H(s， a)=E_i^H\left[R_{t+1}+\gamma Q_i^H\left(S_{t+1}， A_{t+1}\right) \mid S_t=s， A_t=a\right]
\end{aligned}
$$

## 算法分类

在多智能体强化学习中，我们一般将强化学习算法分为三类[1]：TODO

- 完全合作
- 完全竞争
- 混合合作与竞争[3]

### 完全合作的任务

在完全合作的环境中，agents与环境进行交互与学习，在交互与学习的过程中，agents获得相同的reward信号，即如果它们合作做的很好，那么就告诉给它们正的reward，如果它们并没有合作，或者没有做的比较好的时候，就不给予reward，甚至给予负的reward（cost），更一般地说法：agent具有相同的奖励函数（如果有不同的reward，那么也可以将这些reward相加，那么就可以转化成这样同一个reward的性质了，目的就是最大化全局的reward）。

所以，在这类的环境中，学习目标当然是：最大化折扣累计回报了。即所有agent一起努力，将大家的reward最大化。因为agent之间的目标并不冲突，所以可以直接地将single agent的算法直接运用过来（注意一下，因为reward函数相同，所以这个思路很直接），那么用过来就会有一个建模的问题：我是应该single agent还是multiagent呢？

single agent就是将所有agent的action看成一个action的向量，所以这个single agent的学习目标是学习出一个policy能够在每个state下做出相应的action向量，让折扣累积收益最大。

multiagent的角度呢，就是将每个agent单独拿出来学习，每个agent决定一个action，从集中式的action向量拆分成一个个独立的action。这样的方法就是independent q-learning。

但是直接independent q-learning会带来一些效率与其他的问题，比如agent直接的action可能会相互影响，导致最终收敛到的policy并不是全局最优的policy。比如：最优的策略是所有agent采用相同的action，一旦有一个action不同就给予很大的惩罚，那么在学习探索的过程中，因为惩罚的存在，agent可能很难学到这个最优的policy。

之后这部分可能涉及的算法有：JAL，FMQ，Team-Q，Distributed-Q，OAL

### 完全竞争的任务

在完全竞争的环境中，可以很直观地理解为：每个agent都是只关心自己的reward，想要最大化自己的reward，并不考虑自己想要最大化自己的reward的action对于他人的影响。（有点像：我死后哪怕洪水滔天那种感觉）

一个比较常见的环境就是：两个agent在环境中交互，他们的reward互相为相反数，即$r_1 = - r2$。在这样的环境中，最大化自己的reward就是最效果对手的reward，所以agent间不能存在合作与竞争的可能，这样的环境很常见，很多双人的棋类的游戏的reward经常就这样进行设计，比如围棋，alphago的目的就是最大化自己的胜率，最小化对手的胜率。

在这种环境中做planing的话，一种很经典的做法就是冯诺伊曼的minimax的算法，这里就不展开来讲，但是我们可以把这个思想扩展到MARL中agent的学习过程，比如minimax Q-learning：

$$
Q_1 = max_{a_1}min_{a_2}Q(s， a_1， a_2)
$$

这个任务中，我们只会介绍minimax Q-learning，因为很多在混合着竞争与合作的任务中算法的思想是可以迁移到完全竞争的环境中的

### 混合着竞争与合作的任务

这是我觉得最有趣的实验环境，在这个环境中，agent依然是独立获得自己的reward，但是reward的设计非常有意思，当每个agent只考虑最大化自己的reward时，反而可能会使得自己的reward与另外agent的reward陷入更差的情况。以囚徒困境来讲：

|  | 合作 | 背叛 |
| :--- | :---: | :---: |
| 合作 | 3，3 | 0，5 |
| 背叛 | 5，0 | 1，1 |

当agent想要最大化自己的reward时，每个agent都会选择背叛-背叛，因为在这种情况下：当另外的agent选择合作时，我能获得5的reward，比3高；当另外的agent选择竞争时，我能获得1的reward，比0高，这也就是所谓的nash均衡。所以当两个agent都这么考虑的话，最终就是竞争-竞争，agent的reward都是1，但是其实我们观察局面，会发现可能存在一种更好的结果，那就是：合作-合作，这样的话，双方的reward都是3，3比1好，同时两个agent的total reward为6，比任意的情况都好。

但是很多情况下我们并不知道另外一个agent会是什么类型，环境的reward是什么类型，所以其实学习出Nash均衡的策略在这样的环境中是一种保守的选择，虽然没有合作-合作好，但是确保了自己的reward，从这个角度出发，有：Nash-Q。Nash-Q或许好，但是我们却希望能够在学习中尝试与对手合作，达成合作-合作的局面，如果不可以的话，最终再收敛到竞争-竞争的局面。

所以在这种情况下会有很多有趣的问题，我们也会介绍更多的算法：WoLF-IGA，WoLF-PHC，GIGA，GIGA-WoLF，CE-Q等等算法。


## 博弈论的引入

智能体间复杂的关系和智能体之间的影响让 MARL 变得极其复杂。这个时候，博弈论的引入就会让建模变得轻松很多，在这种情况下，我们可以把智能车看作不同的玩家，而此时 NE 则代表不同智能车之间的平衡点。具体来说，MARL 可以分成三种「游戏」：

1。 静态游戏 (static games)：静态游戏中，智能体无法知道其他智能体所做的决策，故而我们可以认为所有智能体的决策是同步，相互之间不受影响。
2。 动态游戏 (stage games)：动态游戏中有很多不同的阶段，每个阶段都是一个游戏（stage game），上面提到的囚徒困境就可以看作其中一个阶段的游戏。
3。 重复游戏 (repeated games)：如果一个 MARL 系统中各个阶段的游戏都很相似，那么就可以被称为重复游戏。

## “集中训练，分布式执行”的训练范式

智能体强化学习中，每个智能体都采用强化学习对自己的策略进行训练，其中智能体的策略利用深度网络来表示。为解决在同时训练的过程中每个智能体的外部环境都不是静态的问题，以及多智能体之间的收益分配问题，“集中训练，分布式执行”的训练范式[25， 50] 被提出，并成为后续工作中的一个基本训练范式。该训练范式的核心思路是在执行过程中每个智能体只能根据自己的观察做出决策，但在训练过程中对于每个决策的评价可以通过一个使用全局信息的模块来进行，这种全局模块可以是一个Critic 网络[50]，一个反事实遗憾值[25]，或者一个专门用来做收益分配的Mix 网络[67， 80， 90]。这样的设计在基于粒子的环境[50] 和基于StarCraft II 的一组合作任务[72] 上都取得了很好的效果。[4]

## 多智能体博弈难求解

(１)均衡特性缺失[6]

纳什均衡作为非合作博弈中应用最广泛的解概念，在两人零和场景中具有成熟的理论支撑，但扩展到多智能体博弈时具有较大局限性。两人零和博弈具有纳什均衡存在性和可交换性等一系列优良特性[３９]。然而，多人博弈的纳什均衡解存在性缺乏理论保证，且计算复杂，两人一般和博弈的纳什均衡是PPAD难问题[４０]，多 人 一 般 和 的 计 算 复 杂 度 高 于PPAD。即使可以在多人博弈中有效地计算纳什均衡，但采取这样的纳什均衡策略并不一定是“明智”的。如果博弈中的每个玩家都独立地计算和采取纳什均衡策略，那么他们的策略组合可能并不是纳什均衡，并且玩家可能具有偏离到不同策略的动机[４１Ｇ４２]。

(２)多维学习目标

对于单智能体强化学习而言，学习目标是最大化期望奖
励，但是在多智能体强化学习中，所有智能体的目标不一定是一致的，学习目标呈现出了多维度[１３]。学习目标可以分为两类[４３]:理性和收敛性。当对手使用固定策略时，理性确保了智能体尽可能采取最佳响应，收敛性保证了学习过程动态收敛到一个针对特定对手的稳定策略，当理性和收敛性同时满足时，会达到新的纳什均衡。

(３)环境非平稳

当多个智能体同时根据自己的奖励来改进自身策略时，
从每个智能体角度来看，环境变得非平稳，学习过程难以解释[４４]。智能体本身无法判断状态转移或奖励变化是自身行为产生的结果，还是对手探索产生的。完全忽略其他智能体独立学习，这种方法有时能产生很好的性能，但是本质上违背了单智能体强化学习理论收敛性的平稳性假设[４５]。这种做法会失去环境的马尔可夫性，并且静态策略下的性能测度也随之改变。例如，多智能体中单智能体强化学习的策略梯度法的收敛结果在 简单线性二次型博弈[４６](Linear-Quadratic Games)中是不收敛的。


## 其他算法

Independent Learning(IL) : SARL算法在多智能体情况下的自然扩展。 IL将来自团队一侧的每个智能体视为不与其他人通信的独立实体。他们每个人都完全根据当地的观察做出决定。IL在完全可观察时易于实现且计算效率高，但在部分可观察下缺乏协作。示例算法包括：

- IQL : Independent Deep Q-Learning，DQN 的简单多智能体扩展。
- IA2C : Independent A2C，A2C 的简单多智能体扩展
- IPG : Independent Policy Gradient，PG 的简单多智能体扩展。
- IPPO : Independent Policy Gradient，PG 的简单多智能体扩展。

Centralised Critic (CC) : CC 专注于学习从集中状态到价值估计的批评网络映射。 具体来说，CC 通常包括一个集中式协调器，该协调器收集来自个人的局部观察并评估全局多智能体状态，每个智能体都在该状态下优化其执行。 CC 属于 Centralised-Training-Decentralised-Executing (CDTE)，一个必不可少的 MARL 框架。 示例算法包括：

- MATRPO : TRPO 与集中评论设置。
- MAPPO : PPO 与集中评论设置。
- MAA2C : A2C 与集中评论设置。
- COMA : 具有反事实多智能体策略梯度的 MAA2C。
- Value Decomposition (VD) : VD 是 CDTE 框架下的一种替代方法，它侧重于对个体的全局价值进行因式分解（或从个体价值函数构成全局价值）。 信用分配是其核心。 示例算法包括：
- VDN : IQL 的值分解变体。 VDN 将团队价值函数分解为智能体价值函数。
- QMIX : QMIX 采用了一个网络，该网络将联合动作值估计为仅以局部观察为条件的每个智能体值的复杂非线性组合。
- VDA2C : A2C 的价值分解变体。
- VDPPO : PPO 的价值分解变体。

## 应用

在当下的研究趋势中，研究者正在把MARL 应用到更为复杂和更大规模的多智能体学习任务上，诸如地面和空中交通管控、分布式监测、电子市场、机器人营救和机器人足球赛、智能电网等一系列实际应用场合。

### 例1：对战游戏

6Lianmin Zheng, Weinan Zhang et al. Magent: a many-agent reinforcement learning platform for artificial collective intelligence. NIPS17 & AAAI18.

### 例2：队伍排列

7Lianmin Zheng, Weinan Zhang et al. Magent: a many-agent reinforcement learning platform for artificial collective intelligence. NIPS17 & AAAI18.

https://github.com/boyu-ai/Hands-on-RL/issues/56

### 例3：去中心化的游戏人工智能（AI）

为复杂的集体游戏智能设计多智能体通信和协同学习的算法

8Peng, Peng, et al. "Multiagent bidirectionally-coordinated nets for learning to play starcraft combat games." NIPS workshop 2017

### 例4：城市大脑模拟器

 设计
• 车辆路由策略
• 交通灯控制
• 车队管理及出租车派车

Huichu Zhang, Weinan Zhang et al. CityFlow: A Multi-Agent Reinforcement Learning Environment for Large Scale
City Traffic Scenario. WWW 2019

### 例5：分拣机器人

10Haifeng Zhang, Weinan Zhang et al. Layout design for intelligent warehouse by evolution with fitness
approximation. IEEE Access 201

### 存在问题以及解决方案

在这些扩展中，学习发生在不同智能体的状态集和行动集的积空间上。因而当智能体、状态或行动的数量太大时，这些扩展面临积空间过大的问题。此外，共享的联合行动空间也未必可用。比如在信息不完全的情况下，智能体未必能观察到其他智能体的行动。如何处理复杂的现实问题，如何高效地处理大量的状态、大量的智能体以及连续的策略空间已经成为目前MAL研究的首要问题。

随着深度强化学习的兴起，上述问题的解决迎来了新的转机，多智能体强化学习（Multi­Agent Reinforcement Learning， MARL）乘势兴起。多智能体强化学习中，每个智能体都采用强化学习对自己的策略进行训练，其中智能体的策略利用深度网络来表示。[4]


## 分层强化学习 VS 多智能体强化学习

### 注重不同

- 分层强化学习：分层强化学习着重于解决复杂任务时候的分层决策，将复杂任务分解为多个或者多层次的简单任务去分布执行，主要是为了提高任务完成的效率。
- 多智能体强化学习，更加注重于多个智能体之间的协作或者说是竞争的问题，每个智能体能够单独感知环境并且作出决策。多智能体更注重于任务的完成或者说对抗过程中的胜率。

### 相结合

也有部分将多智能体强化学习和分层强化学习结合起来的场景。

例如:有10个异构智能体，需要完成不同的两组任务在不同的地方，需要有不同数量的异构智能体进行组合，组成小队去完成任务，目的是更快，更好的完成任务。

这个场景中，总的来说是多智能体强化学习，但是在实现过程中，使用分层强化学习进行设计，高层次决策用来指导10个异构智能体进行组队和选择具体任务，低层次决策只需要具体的智能体完成相应任务，即执行动作即可。

通过分层强化学习，找到分组策略以及任务选择策略，从而更高效的完成任务，缩短任务完成时间。

[1]: http://www.jidiai.cn/algorithm#marl_title
[2]: https://developer.aliyun.com/article/818419?spm=a2c6h.12873639.article-detail.55.7fa137a8RUrUg3
[3]: https://raw.githubusercontent.com/wwxFromTju/MARL-101/master/base/1-introduction-game-algorithm.md
[4]: https://personal.ntu.edu.sg/boan/Chinese/%E5%88%86%E5%B8%83%E5%BC%8F%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD%E7%AE%80%E4%BB%8B.pdf
[5]: https://zhuanlan.zhihu.com/p/545265220
[6]: https://www.jsjkx.com/CN/article/openArticlePDF.jsp?id=20967
[7]: https://www.zhihu.com/question/604708789/answer/3131170100
[8]: https://zhuanlan.zhihu.com/p/406690054
TODO:

https://opendilab.github.io/DI-engine/02_algo/multi_agent_cooperation_rl_zh.html
