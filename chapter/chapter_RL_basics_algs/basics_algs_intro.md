

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-06-04 19:46:31
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-06-04 20:06:20
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 强化学习的基础算法的简介

强化学习的基础算法，据学习目标[1]/优化方法分类：

- 不基于模型，即免模型学习（model-free）方法：该方法放弃了对环境的建模，直接与真实环境进行交互，所以其通常需要较多的数据或者采样工作来优化策略，这也使其对于真实环境具有更好的泛化性能[2]；由于这种方式更加容易实现，也容易在真实场景下调整到很好的状态。所以免模型学习方法更受欢迎，得到更加广泛的开发和测试。
  1. **基于价值函数**（value-based）：该方法是智能体通过学习价值函数（value function）（如状态值函数或动作值函数）来隐式地构建最优策略，即选择具有最大值的动作。包括，采取回合更新的蒙特卡洛方法（MC）、采取单步或多步更新的时间差分方法（TD）{使用表格学习的 Q-learning、Sarsa算法以及一系列基于Q-learning的算法（具体见off-policy）}。此时我们在训练 $Q_\theta$ 以满足自洽方程，间接地优化智能体的表现，即训练的是一个主要完成任务的Actor。 优点：Value-based算法因为其更能有效地重用历史，所以样本利用率高、价值函数估值方差小, 不易陷入局部最优；缺点：此类算法只能解决离散动作空间问题, 容易出现过拟合, 且可处理问题的复杂度受限. 同时, 由于动作选取对价值函数的变化十分敏感, value-based算法收敛性质较差。[3]
     1. 在线控制 或 **同策学习**（on-policy）是指直接对策略进行建模和优化的方法，其目标是找到一个能够最大化期望累积奖励的最优策略。要优化的策略网络（更新参数时使用的策略）恰也是行动策略（生成样本的策略），即学习者与决策者统一。包括Sarsa，Sarsa（λ）[4]。on-policy方法更加稳定但收敛速度较慢。例如，SARAS是基于当前的策略直接执行一次动作选择，然后用动作和对应的状态更新当前的策略，因此生成样本的策略和学习时的策略相同，所以SARAS算法为on-policy算法。该算法会遭遇探索-利用窘境，仅利用目前已知的最优选择，可能学不到最优解，不能收敛到局部最优，而加入探索又降低了学习效率。ϵ - 贪心算法是这种矛盾下的折衷，其优点是直接了当、速度快，缺点是不一定能够找到最优策略。[5]
     2. 离线控制 或 **异策学习**（off-policy）则是指在训练过程中使用一个不同于当前策略的策略进行采样和更新，也就是说，智能体在执行动作时可以采用任意策略生成的动作进行训练。常见的off-policy方法包括Q-learning，Deep-Q-Network，Deep Deterministic Policy Gradient (DDPG)等方法。通过之前的历史（可是自己的也可以是别人的）进行学习，要优化的策略网络（更新参数时使用的策略）与行动策略（生成样本的策略）不同，即学习者和决策者不需要相同。[6]而off-policy方法则更容易出现不稳定性但收敛速度较快。包括Q-Learning ， Deep Q Network。例如，Q-learning在计算下一状态的预期奖励时使用了最大化操作，直接选择最优动作，而当前策略并不一定能选择到最优的动作，因此这里生成样本的策略和学习时的策略不同，所以Q-learning为off-policy算法。[6]
  2. **基于策略**（policy-based）：该方法是跨越价值函数, 直接搜索最佳策略。[22]包括无梯度方法(Gradient-Free)、策略梯度方法Policy Gradient及其衍生的 REINFORCE算法、带基准线的REINFORCE算法。此时我们直接再优化你想要的奖励[24]，即训练的是不完成任务的一个Critic。优点：相比value-based算法, policy-based算法能够处理离散/连续空间问题, 并且具有更好的收敛性；policy-based方法轨迹方差较大、样本利用率低, 容易陷入局部最优的困境。
     1. Gradient-Free：能够较好地处理低维度问题。[22]Cross-Entropy Method的DQN演化而成的QT-Opt、Evolution Strategy的SAMUEL.
     2. Gradient-Based：基于策略梯度算法仍然是目前应用最多的一类强化学习算法, 尤其是在处理复杂问题时效果更佳, 如AlphaGo 在围棋游戏中的惊人表现。算法在Hopper问题的效果对比，Policy Gradient、VPG（如REINFORCE）、TRPO/PPO、ACKTR。SAC=TD3＞DDPG=TRPO=DPG＞VRG。[22]
  3. **基于执行者/评论者**（actor-critic）：该方法是智能体结合了值函数和策略的思想。它包含一个执行者（actor）网络和一个评论者（critic）网络，执行者网络用于生成动作，而评论者网络用于估计值函数。![在Actor-Critic 基础上扩展的 DDPN (Deep Deterministic Policy Gradient)、A3C (Asynchronous Advantage Actor-Critic)、DPPO (Distributed Proximal Policy Optimization)。优点：actor-critic算法多是off-policy，能够通过经验重放(experience replay)解决采样效率的问题；缺点：策略更新与价值评估相互耦合, 导致算法的稳定性不足, 尤其对超参数极其敏感。Actor-critic算法的调参难度很大, 算法也难于复现, 当推广至应用领域时, 算法的鲁棒性也是最受关注的核心问题之一。[15] ![执行者/评论者的智能体](..\..\img\A+C.png)
- **基于模型** （model-based）或 基于效用（utility-based）[31] ：该“模型”特指环境，即环境的动力学模型。[21]该类智能体尝试学习环境的动态模型，即预测从给定状态和动作转移到下一个状态的概率。然后，智能体可以使用学习到的环境模型来提前（look-ahead）规划决策。（model + policy and/or + value function）但缺点是如果模型跟真实世界不一致，那么限制其泛化性能，即在实际使用场景下会表现的不好。预测从给定状态和动作转移到下一个状态的概率: $$\mathbf{P}_{s}^{a}=P\left[S_{t+1}=s^{\prime} \mid S_{t}=s, A_{t}=a\right], \mathbf{R}$$ 环境模型一般可以从数学上抽象为状态转移函数 P (transition function) 和奖励函数 R (reward function)。 在学习R和P之后，所有环境元素都已知，理想情况下，智能体可以不与真实环境进行交互，而只在模拟的环境中，通过RL算法（值迭代、策略迭代等规划方法）最大化累积折扣奖励，得到最优策略。[16]包括：动态规划算法（策略迭代算法、值迭代算法），已给定的模型（Given the Model） MCTS（AlphaGo/**AlphaZero**），学习这模型（Learn the Model） I2A 和 World Model。其优点是可以大幅度提升采样效率；有最大的缺点就是智能体往往不能获得环境的真实模型。如果智能体想在一个场景下使用模型，那它必须完全从经验中学习，这会带来很多挑战。最大的挑战就是，智能体探索出来的模型和真实模型之间存在误差，而这种误差会导致智能体在学习到的模型中表现很好，但在真实的环境中表现得不好（甚至很差）。基于模型的学习从根本上讲是非常困难的，即使你愿意花费大量的时间和计算力，最终的结果也可能达不到预期的效果。[11]
- 更多相关：模仿学习智能体（imitation learning agent）：该类智能体不是直接学习环境奖励，而是尝试模仿人类或其他专家的决策。模仿学习可以提供一种简单而有效的方式，使智能体学习到正确的行为。

> 在这个项目中选取了能够呈现强化学习近些年发展历程的核心算法。目前，在可靠性 (stability)和 采样效率 (sample efficiency)这两个因素上表现最优的策略学习算法是 PPO 和 SAC。从这些算法的设计和实际应用中，可以看出可靠性和采样效率两者的权衡。[11]


[1]: https://imzhanghao.com/2022/02/10/reinforcement-learning/#%E7%AE%97%E6%B3%95%E5%88%86%E7%B1%BB
[2]: https://www.cnblogs.com/kailugaji/p/16140474.html
[3]: http://www.c-s-a.org.cn/html/2020/12/7701.html#outline_anchor_19
[4]: https://baike.baidu.com/item/%E9%A9%AC%E5%B0%94%E5%8F%AF%E5%A4%AB%E9%93%BE/6171383?fromModule=search-result_lemma-recommend
[5]: https://www.cnblogs.com/kailugaji/p/16140474.html
[6]: https://anesck.github.io/M-D-R_learning_notes/RLTPI/notes_html/1.chapter_one.html
[7]: https://www.cnblogs.com/kailugaji/p/16140474.html
[22]: http://www.c-s-a.org.cn/html/2020/12/7701.html#outline_anchor_19
[15]: https://blog.csdn.net/Hansry/article/details/80808097
[16]: https://opendilab.github.io/DI-engine/02_algo/model_based_rl_zh.html
[11]: https://spinningup.qiwihui.com/zh_CN/latest/spinningup/rl_intro.html
