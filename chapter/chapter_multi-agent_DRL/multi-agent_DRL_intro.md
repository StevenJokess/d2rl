

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-21 19:18:57
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-24 01:26:00
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 多智能体深度强化学习（Multi-agent DRL）

深度强化学习 $\mathrm{Q}$ 可以分为两类: 单智能体算法和多智能体算法。

单智能体算法从 $D Q N$ 开始有policy gradient、actor critic、dpg、ppo、ddpg、sac等等，它们解决的是环境中存在一个智能体的情况 (或者是多个智能体可以转化为一个智能体决策的情况)，但是在某些环境（environment）下，似乎单智能体算法就有些心有余而力不足，例如足球比赛亦或是追逐游戏。如果依旧对每个agent采用单智能体算法会出现如下情况：在第 $\mathrm{i}$ 个agent做出动作 $\mathrm{a}_{\mathrm{i}}$ 的情况下由于其余agent的动作 $a_{\mathrm{j}}, \mathrm{j} \neq \mathrm{i}$ 未知，会导致第 $\mathrm{i}$ 个agent收到的奖励reward不稳定，也就是对于单个agent来说，环境是**不稳定(unstable)**的。

从另一个方面来考虑，大多数DRL算法都沿用了其开山鼻祖DQN的replay buffer机制，即在一个合适的时机通过sample buffer得到无序的训练数据用以训练网络，但在一个不稳定的环境下可能出现下面情况：buffer中存在相同状态、相同动作**下奖励reward却不同**的数据，这会直接导致训练的震荡，甚至是崩溃。

在面对一些真实场景下的复杂决策问题时，单agent 系统的决策能力是远远不够的。[2]

## 合作或竞争

例如在拥有多玩家的Atari  2600 游戏中，要求多个决策者之间存在相互合作或竞争的关系。

因此在特定的情形下，需要将DRL 模型扩展为多个agent 之间相互合作、通信及竞争的多agent 系统。

1. 突发行为分析（Analysis of emergent behaviors），主要侧重点是分析和评估单智能体中的DRL算法；
2. 智能体通信（Learning communication），智能体用通信协议共享信息，比如一些直观消息或一个共享内存；
3. 智能体合作（Learning cooperation），主要应用在合作场景或混合（既有合作也有对抗）场景；
4. 智能体建模（Agents modeling agents），不仅有助于智能体之间的合作，还有助于建模对手智能体的推断目标以及考虑其他智能体的学习行为。

## 游戏环境

游戏是探索人工智能（AI）的理想环境，在游戏中可以开发和评估解决问题的技术，并将其应用于更复杂的现实世界问题。过去十年间，人工智能被广泛应用于多种不同的游戏，取得令人瞩目的成就，如Atari，DOTA2，poker等。随着能力的不断提高，研究者一直在寻求复杂度更高的游戏，以捕捉解决科学和现实世界问题所需的不同智能元素。

###

### 星际争霸

《星际争霸》被认为是最具挑战性的实时战略(RTS)游戏之一，是当前我们的主要研究方向。游戏要求玩家与对方进行即时对抗，选择合适的策略，通过资源采集、基地建造、科技发展等形式，击败对方。为了赢得比赛，AI需要学会实现多种不同的策略，以适应具体的游戏环境，是我们研究的一大难点。

1. 博弈论：游戏不存在固定不变的策略，AI需要根据实际场景，选择合适的策略。
2. 部分观测信息：与象棋围棋等游戏不同，在星际争霸中，AI只能观测到部分环境信息，需要手动控制单位通过“侦察”来获得关键信息。
3. 实时性：AI需要在游戏中根据实际环境，做出实时的操作。
4. 更大的行动空间：智能体数目增多，带来了更大的动作空间，AI要在更大的行动空间中找到合适的策略。

[1]: https://gr.xjtu.edu.cn/web/zeuslan/deep-reinforcement-learning
[2]: https://blog.csdn.net/weixin_43145941/article/details/112726116?spm=1035.2023.3001.6557&utm_medium=distribute.pc_relevant_bbs_down_v2.none-task-blog-2~default~ESQUERY~Rate-5-112726116-bbs-614475561.264^v3^pc_relevant_bbs_down_v2_default&depth_1-utm_source=distribute.pc_relevant_bbs_down_v2.none-task-blog-2~default~ESQUERY~Rate-5-112726116-bbs-614475561.264^v3^pc_relevant_bbs_down_v2_default
