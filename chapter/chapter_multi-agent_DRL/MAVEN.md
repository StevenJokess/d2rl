

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-10-10 03:47:29
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-10 03:48:53
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->

# MAVEN算法

## 一、研究背景和动机

深度强化学习对数据的依赖性非常大，其数据靠智能体和环境交互而来。因此，如何通过探索机制从环境中获取有效的数据帮助训练，就显得至关重要。而在多智能体强化学习（MARL）中，联合动作空间随智能体个数呈指数增加。因此，如何在这样大的动作空间中有效探索，就成了MARL中最根本的问题之一。我们在前面曾提到过，QMIX算法是一种满足单调性约束的Q值拟合器。这种约束能够解决大部分MARL问题，但是对于一些非单调性的任务，QMIX的函数拟合能力就会受到比较大的限制。这种限制也体现在智能体在联合动作空间中的探索上，由于智能体的策略总是满足单调性约束，因此探索也就被限制在一个特定的流形上，而不是整个联合动作空间。最后，算法就只能找到局部最优解。针对这样的问题，作者结合policy-based和value-based方法，基于QMIX框架提出了一个新的分层学习算法——MAVEN (Multi-agent variational exploration)。

## 二、QMIX的不足

在介绍QTRAN、WQMIX的时候，我们曾详细探讨过QMIX算法存在的固有问题——单调性约束。本文中，作者对该问题进一步进行了讨论。首先在数学上对非单调性进行了定义：定义1 (非单调性). 对任意状态 s∈Ss \in Ss \in S ，智能体 i∈Ai \in \mathcal{A}i \in \mathcal{A} ，其它智能体的联合动作 u−i∈Un−1u^{-i} \in U^{n-1}u^{-i} \in U^{n-1} ，Q值 Q(s,(ui,u−i))Q(s, (u^i, u^{-i}))Q(s, (u^i, u^{-i})) 构成了在智能体 iii 动作空间上的一个排序。定义 C(i,u−i):={(u1i,…,u|U|i)|Q(s,(uji,u−i))≥Q(s,(uj+1i,u−i)),j∈{1,…,|U|}}C(i, u^{-i}) := \{ (u^i_1, \dots, u^i_{|U|}) | Q(s, (u^i_{j}, u^{-i})) \ge Q(s, (u^i_{j+1}, u^{-i})), j \in \{1, \dots, |U|\} \}C(i, u^{-i}) := \{ (u^i_1, \dots, u^i_{|U|}) | Q(s, (u^i_{j}, u^{-i})) \ge Q(s, (u^i_{j+1}, u^{-i})), j \in \{1, \dots, |U|\} \} 是所有这种排序的集合。那么称联合动作值函数是非单调的，当其满足： \existi∈A,u1−i≠u2−i,s.t.C(i,u1−i)⋂C(i,u2−i)=∅\exist i \in \mathcal{A}, u^{-i}_1 \neq u^{-i}_2, s.t. C(i, u^{-i}_1) \bigcap C(i, u^{-i}_2) = \emptyset\exist i \in \mathcal{A}, u^{-i}_1 \neq u^{-i}_2, s.t. C(i, u^{-i}_1) \bigcap C(i, u^{-i}_2) = \emptyset .也就是说，当其它智能体的动作组合不一样时，通过 maxaiQi\max_{a^i} Q^i\max_{a^i} Q^i 选择的动作是不一样的，也就不一定是最优的。图1是作者举的一个“2智能体+3动作”博弈问题例子：图1. (a) 非单调收益矩阵；(b) QMIX拟合结果。为了进一步说明QMIX策略的局部最优性，作者还提出了两个定理。即分别在均匀探索和 ϵ\epsilon\epsilon -greedy探索两种情况下，QMIX在某些条件下只能收敛到局部最优。（关于定理的详细内容和证明，建议阅读原文）

## 三、算法思路

作者提出使用committed exploration[2]思想来解决探索效率低的问题。所谓committed exploration，就是同时学习多个Q，并用这些Q值的 ϵ\epsilon\epsilon -greedy策略在环境中探索（该思想出自bootstrapped DQN算法）。bootstrapped DQN算法是将 <st,at,rt+1,st+1,mt><s_t, a_t, r_{t+1}, s_{t+1}, m_t><s_t, a_t, r_{t+1}, s_{t+1}, m_t> 存入replay buffer，其中 mt∼Mm_t \sim Mm_t \sim M ， MMM 是一个隐藏的概率分布（或随机过程），因此 mtm_tm_t 可以理解为“隐变量”。通过考虑这样的隐变量，设计多个具有随机性的Q函数，能够有效提升算法的探索能力。基于这样的思想，本文作者通过设计分层的结构来控制隐变量的输出。每个智能体的Q值共享同一个隐变量。然后根据智能体轨迹信息和隐变量信息得到它们之间的互信息，通过最大化该互信息来增大关于智能体策略的散度，使得动作更加多样化。下面我们来看看算法的具体设计。

## 四、算法设计

MAVEN算法是在QMIX的结构基础上做的进一步改进。它并没有改Mixing网络的结构，只是在智能体网络的最后全连接层中，权重矩阵由另外一个网络 gϕg_{\phi}g_{\phi} 生成。这个网络的输入是隐变量和动作。这样，智能体网络的输出就和隐变量 zzz 关联上了。算法整体结构如图2所示：图2. MAVEN算法框架图2中，左边是隐变量 zzz 的生成（分层策略）以及超网络 gϕg_{\phi}g_{\phi} ，右边是互信息损失函数设计，中间的部分和QMIX算法相同。以上的结构中，需要学习的参数如下：θ\theta\theta : 隐变量生成网络参数；ϕ\phi\phi : 超网络 gϕg_{\phi}g_{\phi} 参数；η\eta\eta : 智能体网络参数；ψ\psi\psi : Mixing网络参数；vvv : 隐变量后验概率分布参数。和策略梯度算法类似，关于分层策略的目标函数，定义如下：JRL(θ)=∫R(τA|z)pθ(z|s0)ρ(s0)dzds0\mathcal{J}_{RL}(\theta) = \int \mathcal{R}(\tau_{\mathcal{A}}|z) p_{\theta}(z|s_0) \rho(s_0) dz ds_0\mathcal{J}_{RL}(\theta) = \int \mathcal{R}(\tau_{\mathcal{A}}|z) p_{\theta}(z|s_0) \rho(s_0) dz ds_0 .    （1）为了使不同变量 zzz 对应的策略更加分散，还增加了互信息 (Mutual Information) 损失：JMI=H(σ(τ))−H(σ(τ)|z)=H(z)−H(z|σ(τ))\mathcal{J}_{MI} = \mathcal{H}(\sigma(\boldsymbol{\tau})) - \mathcal{H}(\sigma(\boldsymbol{\tau}) | z) = \mathcal{H}(z) - \mathcal{H}(z | \sigma(\boldsymbol{\tau}))\mathcal{J}_{MI} = \mathcal{H}(\sigma(\boldsymbol{\tau})) - \mathcal{H}(\sigma(\boldsymbol{\tau}) | z) = \mathcal{H}(z) - \mathcal{H}(z | \sigma(\boldsymbol{\tau})) .    (2)其中， H\mathcal{H}\mathcal{H} 表示信息熵， σ\sigma\sigma 表示一个算子，作用是保证 JMI\mathcal{J}_{MI}\mathcal{J}_{MI} 可微。 但是上式中 H(σ(τ))\mathcal{H}(\sigma(\boldsymbol{\tau}))\mathcal{H}(\sigma(\boldsymbol{\tau})) 和 H(z|σ(τ))\mathcal{H}(z | \sigma(\boldsymbol{\tau}))\mathcal{H}(z | \sigma(\boldsymbol{\tau})) 这两个信息熵都是无法直接计算的。因此作者使用如下近似：JMI≥H(z)+Eσ(τ),z[log⁡(qv(z|σ(τ)))]\mathcal{J}_{MI} \ge \mathcal{H}(z) + \mathbb{E}_{\sigma(\boldsymbol{\tau}), z} [ \log(q_v(z|\sigma(\boldsymbol{\tau}))) ]\mathcal{J}_{MI} \ge \mathcal{H}(z) + \mathbb{E}_{\sigma(\boldsymbol{\tau}), z} [ \log(q_v(z|\sigma(\boldsymbol{\tau}))) ] .    (3)其中， qv(z|σ(τ))q_v(z|\sigma(\boldsymbol{\tau}))q_v(z|\sigma(\boldsymbol{\tau})) 是一个随着 τ\boldsymbol{\tau}\boldsymbol{\tau} 动态变化的任意概率分布，由参数 vvv 表示。作者在文章附件中，证明了(3)式对任意 qvq_vq_v 均成立。因此，为了逼近 JMI\mathcal{J}_{MI}\mathcal{J}_{MI} ，就要最大化(3)式右边的部分。即为了学习参数 vvv ，定义以下目标函数：JV=H(z)+Eσ(τ),z[log⁡(qv(z|σ(τ)))]\mathcal{J}_{V} = \mathcal{H}(z) + \mathbb{E}_{\sigma(\boldsymbol{\tau}), z} [ \log(q_v(z|\sigma(\boldsymbol{\tau}))) ]\mathcal{J}_{V} = \mathcal{H}(z) + \mathbb{E}_{\sigma(\boldsymbol{\tau}), z} [ \log(q_v(z|\sigma(\boldsymbol{\tau}))) ] .    (4)

综上，整体的优化目标就可以用下式表示：maxv,ϕ,η,ψ,θJRL(θ)+λMIJV(v,ϕ,η,ψ)−λQLLQL(ϕ,η,ψ)\max_{v, \phi, \eta, \psi, \theta} \mathcal{J}_{RL}(\theta) + \lambda_{MI} \mathcal{J}_V(v, \phi, \eta, \psi) - \lambda_{QL} \mathcal{L}_{QL}(\phi, \eta, \psi)\max_{v, \phi, \eta, \psi, \theta} \mathcal{J}_{RL}(\theta) + \lambda_{MI} \mathcal{J}_V(v, \phi, \eta, \psi) - \lambda_{QL} \mathcal{L}_{QL}(\phi, \eta, \psi) .    (5)

其中， λMI\lambda_{MI}\lambda_{MI} 和 λQL\lambda_{QL}\lambda_{QL} 均为正的拉格朗日系数。有了优化目标，作者将算法流程用如下伪代码表示：以上就是MAVEN算法的主要内容。

五、总结

总的来说，该算法借鉴了bootstrapped DQN的思想，考虑了一个隐策略来生成隐变量。这种思想经常在因果推断中用到，即变量X和变量Y之间，在不同场景下它们的联合概率分布P(X, Y)是不一样的。这样就导致我用现有的样本学不出来从X到Y之间的映射。出现这样的原因往往是没有从不同场景的样本中找到它们共同的隐变量Z。在bootstrapped DQN中，也考虑隐变量z，将不同的Q值和隐变量关联起来。这样我就能扩大值函数的探索方向和范围，也就能帮助算法收集更多不一样的数据。数据越具有多样性，就越有可能学习到全局最优解。因此，MAVEN继续发扬这种思想，将每个智能体的值函数或策略都赋予该隐变量。这种隐变量能够使智能体的策略和轨迹数据更加多样化，不会受到单调性的严格限制。因此，这样的算法就容易找到非单调的最优解。

[1]: https://zhuanlan.zhihu.com/p/366750122
