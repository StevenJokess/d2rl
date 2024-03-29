

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-12 20:38:30
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-12 20:40:24
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 多智能体深度强化学习

随着深度学习的发展，利用神经网络的强大表达能力来搭建逼近模型（value approximation）和策略模型（常见于 policy-based 的 DRL 方法）。深度强化学习的方法可以分为基于值函数（value-based）和基于策略（policy-based）两种，在考虑多智能体问题时，主要的方式是在值函数的定义或者是策略的定义中引入多智能体的相关因素，并设计相应的网络结构作为值函数模型和策略模型，最终训练得到的模型能够适应（直接或者是潜在地学习到智能体相互之间的复杂关系），在具体任务上获得不错的效果。

2.1 policy-based 的方法

在完全合作的 setting 下，多智能体整体通常需要最大化全局的期望回报。前面提到一种完全集中式的方式：通过一个中心模块来完成全局信息的获取和决策计算，能够直接地将适用于单智能体的 RL 方法拓展到多智能体系统中。但通常在现实情况中，中心化的控制器（centralized controller）并不一定可行，或者说不一定是比较理想的决策方式。而如果采用完全分布式的方式，每个智能体独自学习自己的值函数网络以及策略网络、不考虑其他智能体对自己的影响，无法很好处理环境的不稳定问题。利用强化学习中 actor-critic 框架的特点，能够在这两种极端方式中找到协调的办法。

## DRL在多智能体协作中的应用

近年来，DRL在多智能体协作方面也得到了广泛的应用。采用 DRL 解决多智能体协作问题涉及智能体之间的交互，需要考虑状态-动作维数过大、环境非静态、部分可观测等问题[50]，因此相对于单智能体来说，对多智能体的研究更具有挑战性。多智能体协作是指多个智能体通过相互合作达到共同的目标，从而得到联合的奖励值。DRL在多智能体协作方面的研究主要包括独立学习者协作、集中式评价器协作以及通信协作等[51]。

独立学习者是指智能体在更新自身的策略时，把其他智能体作为环境的一部分，每个智能体采用独立更新的方式，不考虑其他智能体的状态和动作。若采用这种更新方式，每个智能体在训练时比较简单，方便智能体进行数量上的扩展。但其他智能体的策略在不断更新，因此智能体所处的环境是不断变化的，这导致智能体不满足 MDP 条件，即MADRL面临环境非静态问题。Omidshafiei S等人[52]提出使用滞回强化学习[53]的方法来解决环境非静态问题，通过对不同的TD误差采用不同大小的学习率，减弱环境变化对Q值的影响，并采用一种并行经验回放的方式，保证多个智能体在使用经验回放时能够得到最优的联合动作。对于多智能体来说，当智能体的策略不断变化时，经验回放技术也不再适用，这给MADRL带来了很大的挑战。Palmer G等人[55]提出了宽松DQN（lenient DQN）算法来解决环境非静态问题，以宽松条件来决定经验池中的采样数据，不满足条件的经验数据将被忽略。Jin Y等人[57]提出了对其他智能体的动作进行估计的方法来解决环境非稳态问题，在评估Q函数时加入对其他智能体动作的估计，减弱环境非静态带来的影响。Liu X等人[58]对邻近智能体的关系进行建模，提出了注意力关系型编码器来聚合任意数量邻近智能体的特征，并采用参数共享[59]的方式来减少参数的更新量，使算法可扩展到大规模智能体的训练。

Lowe R 等人[60]采用集中式训练、分布式执行的训练机制，提出了多智能体深度确定性策略梯度（multi-agent deep deterministic policy gradient， MADDPG）算法，结构如图7所示。该机制采用集中式评价器，假设智能体的评价器在训练时能够得到其他所有智能体的状态和动作信息，这样即使其他智能体的策略发生变化，环境也是稳定的。而执行器只能得到环境的局部信息来执行动作，训练结束后，算法只采用独立的执行器进行分布式执行。这种机制能够很好地解决环境非静态问题，利用了AC 结构的优势，方便智能体的训练和执行。Foerster J等人[61]提出了COMA策略梯度算法，采用集中式训练、分散式执行的机制，使得每个智能体在协作过程中都能收到对应于自身行动的奖励值，同时提高所有智能体共同的奖励值。Sunehag P等人[62]将所有智能体的联合Q网络分解为每个智能体单独的Q网络，提出VDN算法。Mao H等人[63]提出了基于注意力机制的MADDPG算法，对其他智能体的策略进行自适应建模，以促进多智能体之间的协作，同时引入注意力机制来提升智能体建模的效率。Iqbal S等人[64]在集中式评判器中采用自我注意力机制，使每个智能体都对其他智能体的观测和动作信息进行不同程度的关注，有效提升了算法的效率，并且可以扩展到大规模智能体的情况，同时引入了 SAC 算法来避免收敛到次优的策略，采用 COMA 算法思想解决多智能体信度分配的问题。

多智能体通信一方面可以促进智能体之间的协作，另一方面，训练时智能体能够得到其他智能体的信息，从而缓解环境非静态问题。Foerster J N等人[65]使用通信来促进智能体之间的协作，并提出了RIAL和DIAL两种通信方法。RIAL的Q网络中不仅要输出环境动作，还要输出通信动作到其他智能体的Q网络中。DIAL利用集中式学习的优势，直接在两个智能体的 Q 网络之间建立一个可微信道，促进智能体之间的双向交流。Sukhbaatar S 等人[66]提出了一种通信神经网络模型 CommNet，使得多智能体在协作的过程中能够连续通信。Jiang J等人[67]提出了注意力通信模型ATOC，通过注意力单元来选取智能体的通信对象，采用双向长短期记忆（long short term memory，LSTM）单元来收集通信智能体的信息，进而选取动作。Kim D 等人[68]考虑更实际的多智能体通信，即带宽有限以及智能体共享通信介质的情况，提出了SchedNet结构，采用基于权重调度的方法和调度向量来确定需要通信的智能体，解决通信资源有限以及智能体竞争通信的问题。Das A等人[69]提出了一种目标通信结构TarMAC，采用自注意力机制来计算智能体与其他智能体的通信权重，并根据权重来整合其他智能体的通信信息。 [1]

[1]: http://www.infocomm-journal.com/znkx/article/2020/2096-6652/2096-6652-2-4-00314.shtml
