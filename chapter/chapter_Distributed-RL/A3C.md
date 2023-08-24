

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-29 01:01:56
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-07-04 21:46:59
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->

# A3C 算法

## A3C的引入

Actor-Critic算法的代码，其实很难收敛，无论怎么调参，最后的CartPole都很难稳定在200分，这是Actor-Critic算法的问题。但是我们还是有办法去有优化这个难以收敛的问题的。

回忆下之前的DQN算法，为了方便收敛使用了经验回放的技巧。那么我们的Actor-Critic是不是也可以使用经验回放的技巧呢？当然可以！不过A3C更进一步，还克服了一些经验回放的问题。经验回放有什么问题呢？ 回放池经验数据相关性太强，用于训练的时候效果很可能不佳。举个例子，我们学习下棋，总是和同一个人下，期望能提高棋艺。这当然没有问题，但是到一定程度就再难提高了，此时最好的方法是**另寻高手切磋**。

A3C的思路也是如此，它利用多线程的方法，同时在多个线程里面分别和环境进行交互学习，每个线程都把学习的成果汇总起来，整理保存在一个公共的地方。并且，**定期从公共的地方把大家的齐心学习的成果拿回来，指导自己和环境后面的学习交互。**通过这种方法，A3C避免了经验回放相关性过强的问题，同时做到了异步并发的学习模型。

## A3C介绍

异步优势行动-批判者（Asynchronous Advantage Actor-Critic，A3C）是由DeepMind研究人员于2016年提出的可以在多个计算设备上并行更新网络的学习算法。相比于之前的单节点强化学习系统，A3C通过创建一组工作者（Worker），并将每个工作者分配到不同的计算设备上且为他们各自创建可以交互的环境来实现并行采样和模型更新，同时用一个主（Master）节点维护这些行动者（Actor）和批判者（Critic）网络的更新。行动者是策略网络，批判者是价值网络，分别对应强化学习中的策略和价值函数。通过这样的设计，整个算法的各个工作者可以实时将所采集到样本计算出的梯度回传到主节点，来更新主节点的模型参数，并在主节点模型更新后即时下发到各个工作者进行模型更新。每个工作者可以单独在一个 GPU 上进行运算，从而整个算法可以在一个 GPU 集群上并行更新模型，算法结构如所示。研究表明，分布式强化学习训练除加速模型学习之外，由于其更新梯度是由多个计算节点各自对环境采样计算得到的，还有利于稳定学习表现。


评论家学习值函数，同时有多个演员并行训练并且不时与全局参数同步。A3C旨在用于并行训练，是 on-policy 的方法。

在A3C算法中，有多个并行的环境，每个环境中都有一个智能体执行各自的动作和并计算累计的参数梯度。在一定步数后进行累计，利用累计的参数梯度去更新所有智能体共享的全局参数。

> 使用了多线程的方式，一个主线程负责更新Actor和Critic的参数，多个辅线程负责分别和环境交互，得到梯度更新值，汇总更新主线程的参数。而所有的辅线程会定期从主线程更新网络参数。这些辅线程起到了类似DQN中经验回放的作用，但是效果更好。[5]

不同环境中的智能体可以使用不同的探索策略，会导致经验样本之间的相关性较小，可以提高学习效率。

A3C可根据critic所采用的算法进行同步/异步训练, 能适用于同步策略、异步策略。

使用在线Critic整合策略梯度, 降低训练样本的相关性, 在保证稳定性和无偏估计的前提下, 提升了采样效率和训练速度.[3]

A3C (Asynchronous advantage actor-critic) 算法[38]. 该算法包括一个全局执行器–评价器网络和多个对应于每个线程的执行器–评价器网络. 两种网络结构相同, 均为双输出的神经网络结构, 网络的一个输出表示状态值函数. 全局策略和值函数分别表示 为 $\pi(s \mid \theta)$ 和 $V(s \mid \phi)$, 每个线程的策略和值函数分别 表示为 $\pi\left(s \mid \theta^{\prime}\right)$ 和 $V\left(s \mid \phi^{\prime}\right)$, 其中 $\theta, \theta^{\prime}, \phi$ 和 $\phi^{\prime}$ 为网络的参数. 每执行 步或者达到某个终止状态时进行一次网络更新, 首先计算每个线程的值函数梯度和策略梯度, 然后将它们分别相加, 对全局的网络参数
进行更新, 随后再复制给每个线程的网络.

## 算法大纲：

- 定义全局参数 $\theta$ 和 $w$ 以及特定线程参数 $\theta^{\prime}$ 和 $w^{\prime}$ 。
- 初始化时间步 $t=1$ 。
- 当 $T<=T_{\max}$ :
- 重置梯度: $d \theta=0$ 并且 $d w=0$ 。
- 将特定于线程的参数与全局参数同步: $\theta^{\prime}=\theta$ 以及 $w^{\prime}=w$ 。
- 令 $t_{s t a r t}=t$ 并且随机采样一个初始状态 $s_t$ 。
- 当 $\left(s_{t} !=\right.$ 终止状态 $)$ 并 $t-t_{\text {start }}<=t_{\max }$ :
- 根据当前线程的策略选择当前执行的动作 $a_t \sim \pi_{\theta^{\prime}}\left(a_t \mid s_t\right)$ ，执行动作后接 收回报 $r_t$ 然后转移到下一个状态 $s_{t+1}$ 。
- 更新 $t$ 以及 $T: t=t+1$ 并且 $T=T+1$ 。
- 初始化保存累积回报估计值的变量
- 对于 $i=t_1, \ldots, t_{\text {start }}$ :
$-r \leftarrow \gamma r+r_i ;$ 这里 $r$ 是 $G_i$ 的蒙特卡洛估计。
- 累积关于参数 $\theta^{\prime}$ 的梯度: $d \theta \leftarrow d \theta+\nabla \theta^{\prime} \log \pi \theta^{\prime}\left(a_i \mid s_i\right)\left(r-V w^{\prime}\left(s_i\right)\right)$;
- 累积关于参数 $w^{\prime}$ 的梯度:
$d w \leftarrow d w+2\left(r-V w^{\prime}\left(s_i\right)\right) \nabla w^{\prime}\left(r-V w^{\prime}\left(s_i\right)\right)$.
- 分别使用 $d \theta$ 以及 $d w$ 异步更新 日以及 $w$ 。

## 优势函数?

$A(s, a)=Q(s, a)-V(s)$ 是为了解决基于价值方法具有高变异性。 它代表着与该状态下采取的平均行动相比所取得的进步。

- 如果 $A(s, a)>0$ : 梯度被推向了该方向
- 如果 $A(s, a)<0$ : (我们的action比该状态下的平均值还差) 梯度被推向了反方

但是这样就需要两套价值函数，所以可以使用**时序差分方法**做估计：

- TD(0)：$A(s, a)=r+\gamma V\left(s^{\prime}\right)-V(s)$ 。
- TD(n)：$\sum_{i=0}^{k-1} \gamma^i r_{t+i}+\gamma^k V\left(s_{t+k} ; \theta_v\right)-V\left(s_t ; \theta_v\right)$

[1]: https://raw.githubusercontent.com/openmlsys/openmlsys-zh/main/chapter_reinforcement_learning/distributed_node_rl.md
