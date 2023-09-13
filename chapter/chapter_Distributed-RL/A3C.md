

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-29 01:01:56
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-12 20:54:53
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->

# A3C 算法

Policy Gradient(PG)算法的最大问题在于On-Policy所带来的样本效率(sample efficiency)问题。具体而言，

$$
\begin{aligned}
\nabla_\theta \eta\left(\pi_\theta\right) & =\nabla_\theta E_{\tau \sim p\left(\pi_\theta\right)}[R(\tau)] \\
& =\nabla_\theta \sum_{\tau \sim p\left(\pi_\theta\right)} \log p_\theta(\tau) R(\tau)
\end{aligned}
$$

由于样本是从 $p\left(\pi_\theta\right)$ 中采样的，因此在PG进行更新后 $\left(\theta \rightarrow \theta^{\prime}\right)$ ，正确的采样分布已经变成 了 $p\left(\pi_{\theta^{\prime}}\right)$ ，故原来的样本都必须全部被丟弃，然后重新采样样本并更新。可以发现，**每个样本只能用于一次策略更新**，这显然是效率低下的。

A3C算法就是为了解决上述问题。但事实上，A3C算法本质上是逃避了这个问题，即“大力出奇迹”。既然PG的样本效率低，那么**同时并行用很多个Agent去采样**不就可以了（蚌）。但为了保证On-Policy，并行采样的Agent必须保持同步，而这显然限制了采样效率。A3C算法直接忽略了这一点，即**采样异步采样算法**，并定期同步所有Agent的权重，其潜在的假设就是只要**同步频率得当**，那么不同Agent间的步调不会相差太远。[4]

但A3C实际上没有解决On-Policy样本效率低的问题。

## A3C的引入

Actor-Critic算法的代码，其实很难收敛，无论怎么调参，最后的CartPole都很难稳定在200分，这是Actor-Critic算法的问题。但是我们还是有办法去有优化这个难以收敛的问题的。

回忆下之前的DQN算法，为了方便收敛使用了经验回放的技巧。那么我们的Actor-Critic是不是也可以使用经验回放的技巧呢？当然可以！不过A3C更进一步，还克服了一些经验回放的问题。经验回放有什么问题呢？ 回放池经验数据相关性太强，用于训练的时候效果很可能不佳。举个例子，我们学习下棋，总是和同一个人下，期望能提高棋艺。这当然没有问题，但是到一定程度就再难提高了，此时最好的方法是**另寻高手切磋**。

A3C的思路也是如此，它利用多线程的方法，同时在多个线程里面分别和环境进行交互学习，每个线程都把学习的成果汇总起来，整理保存在一个公共的地方。并且，**定期从公共的地方把大家的齐心学习的成果拿回来，指导自己和环境后面的学习交互**。通过这种方法，A3C避免了经验回放相关性过强的问题，同时做到了异步并发的学习模型。

A3C是 Asynchronous Advantage Actor Critic的简称

首先是异步，A3C在采样过程和训练过程都是异步的，首先是采样，由于A3C需要从采样的数据来不断进行策略更新，计算梯度需要依赖当前的策略模型，得到序列，因此这就是一个on-policy的算法，为了加快采样速度，A3C使用了异步采样的方法。

A3C异步更新是每个work单独计算其损失梯度后上传到全局网络，然后全局网络再根据这个梯度更新，然后再用新的参数更新这个work。

后来A3C被认为异步其实完全没有必要，尽管加快了速度，但这么做确实会损失性能，与其这样不如取消异步，于是就有了非异步的A2C。[2]

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

## 与DDPG的异同

### 相同点

- Advantage Actor-Critic: 和DDPG架构类似，actor网络的梯度[5]

$$
\nabla_{\theta^{\prime}} \log \bar{\pi}\left(a_t \mid \dot{s} ; \theta^{\prime}\right) A\left(s_t, \dot{a_t} ; \theta, \theta_v\right)
$$

### 不同点

与DDPG不同的是A3C利用的是max(Advantage)而非max $(Q)$ ，其中A(s_\{t $\}$, a_ $\{t\} ;$ itheta， Itheta \{ $\{v\})$ 是利用n-steps TD error进行更新的，即:
$$
\sum_{i=0}^{k-1} \gamma^i r_{t+i}+\gamma^k V\left(s_{t+k} ; \theta_v\right)-V\left(s_t ; \theta_v\right)
$$




Estimate state-value function
$$
V(s, \mathbf{v}) \approx \mathbb{E}\left[r_{t+1}+\gamma r_{t+2}+\ldots \mid s\right]
$$
- Q-value estimated by an $n$-step sample
$$
q_t=r_{t+1}+\gamma r_{t+2} \ldots+\gamma^{n-1} r_{t+n}+\gamma^n V\left(s_{t+n}, \mathbf{v}\right)
$$
- Actor is updated towards target
$$
\frac{\partial I_u}{\partial \mathbf{u}}=\frac{\partial \log \pi\left(a_t \mid s_t, \mathbf{u}\right)}{\partial \mathbf{u}}\left(q_t-V\left(s_t, \mathbf{v}\right)\right)
$$
- Critic is updated to minimise MSE w.r.t. target
$$
I_v=\left(q_t-V\left(s_t, \mathbf{v}\right)\right)^2
$$




## 算法大纲：

n-step Q-learning A3C算法训练过程：

![A3C](../../img/A3C.png)

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


## 改进：GA3C

为了更好地利用GPU的计算资源从而提高整体计算效率，A3C进一步优化提升为GA3C.与A3C不同，GA3C中的Actor并没有模型参数，整个架构中只有一个模型，保存在Learner中。当Actor需要采样时，将状态放入预测队列，Learner的采样线程将队列中的所有状态拿出来进行一次采样），将得到的结果返回给相应的Actors。Actor收到对应的动作之后，在环境中进行step，并得到对应的reward信号。Actor收集到一定的样本之后，会将这些样本放入训练队列，Learner的训练线程使用这些样本进行模型更新。下图展示了A3C和GA3C架构的差别：



采用这个架构之后，随着模型变得越来越复杂，GA3C带来的加速比也变得越来越大。[3]


---

• 论文题目：Asynchronous Methods for Deep Reinforcement Learning
所解决的问题？
  在强化学习算法中agent所观测到的data是非平稳和强相关（ non-stationary和strongly correlated）。通过设置memory的方式可以减少非平稳性和解耦轨迹之间的相关性，但是这样会限制这些方法只能去使用off-policy的RL算法，并且会增加额外的运算。
  作者主要是通过多个智能体并行地采样数据，以一种更加平稳的处理方式(more stationary process，传递梯度参数) 来解耦智能体数据采样数据之间的相关性，并且可以使用on-policy的策略。
背景
  在此之前也有一些研究，比如The General Reinforcement Learning Architecture (Gorila)中：actor与环境互动采样(多台电脑)，将数据放入replay memory中，learner从replay memory中获取数据，并计算DQN算法所定义的Loss梯度，但是这个梯度并不用于更新learner的参数，梯度信息被异步地分发到参数服务中心(central parameter server)，去更新一个中心模型的副本，更新完的policy参数在隔固定步数发送到actor中去。(learner的target拿central parameter server所更新的参数更新learner)。流程图如下所示：
  还有一些研究将Map Reduce framework引入用于加快矩阵运算，(并不是加快采样)。也还有一些工作是learner之间通过通讯共享一些参数信息。
所采用的方法？
  作者所使用的方法与Gorila框架的方法类似，但是并没有用多台机器和参数服务器(parameter server)，而是使用一个多线程的GPU在单台机器上运行，每一个线程上都有一个learner，它们采样的数据就更加丰富了，多个learner online更新最后汇总梯度，其实也是相当于切断了数据之间的关联性。因此作者没有使用replay memory而是对每个learner使用不同的exploration policy，因此这种方法也可以使用on-policy的强化学习算法，比如sarsa这种。将其用于Q-Learning算法的话，可以得到如下单线程learner伪代码：
  对于actor-critic框架，单线程learner伪代码如下所示：
取得的效果？
  所需的计算资源更小，使用一个multi-core CPU就可以进行训练。比较了在Nvidia K40 GPU上训练的DQN算法的学习速度和在五个Atari 2600游戏上使用16个CPU核心训练的异步方法：
  还有一些什么鲁棒性地分析可以参考原文，这里就不说了，在讨论部分作者强调了，并不是说experience replace不好，把其引入进来可能效果会改进采样效率，可能会使得效果更好。
论文小节
  整个网络中有多个local worker，一个global worker。多个local worker异步更新，更新完的参数传到global worker中去。local worker采样到新的样本之后，在更新之前需要把global worker中的参数拉取过来之后再进行更新，更新之后再传到global worker中去。
  这种方式只能是CPU层面的并行，之后的A2C，同步版本的，每一个worker仅采集数据，然后集中起来通过GPU进行更新，只传数据。
所出版信息？作者信息？
  这篇文章是ICML2016上面的一篇文章。第一作者Volodymyr Mnih是Toronto大学的机器学习博士，师从Geoffrey Hinton，同时也是谷歌DeepMind的研究员。硕士读的Alberta大学，师从Csaba Szepesvari。
参考链接
1. The General Reinforcement Learning Architecture (Gorila) of (Nairetal.,2015) performs asynchronous training of reinforcement learning agents in a distributed setting. The gradients are asynchronously sent to a central parameter server which updates a central copy of the model. The updated policy parameters are sent to the actor-learners at ﬁxed intervals.
• 参考文献：Nair, Arun, Srinivasan, Praveen, Blackwell, Sam, Alcicek, Cagdas, Fearon, Rory, Maria, Alessandro De, Panneershelvam, Vedavyas, Suleyman, Mustafa, Beattie, Charles, Petersen, Stig, Legg, Shane, Mnih, Volodymyr, Kavukcuoglu, Koray, and Silver, David. Massively parallel methods for deep reinforcement learning. In ICML Deep Learning Workshop. 2015.
2. We also note that a similar way of parallelizing DQN was proposed by (Chavez et al., 2015).
• 参考文献：Chavez, Kevin, Ong, Hao Yi, and Hong, Augustus. Distributed deep q-learning. Technical report, Stanford University, June 2015.
3. In earlier work, (Li & Schuurmans, 2011) applied the Map Reduce framework to parallelizing batch reinforcement learning methods with linear function approximation. Parallelism was used to speed up large matrix operations but not to parallelize the collection of experience or stabilize learning.
• 参考文献：Li, Yuxi and Schuurmans, Dale. Mapreduce for parallel reinforcement learning. In Recent Advances in Reinforcement Learning - 9th European Workshop, EWRL 2011, Athens, Greece, September 9-11, 2011, Revised Selected Papers, pp. 309–320, 2011.
4. (Grounds & Kudenko, 2008) proposed a parallel version of the Sarsa algorithm that uses multiple separate actor-learners to accelerate training. Each actor learner learns separately and periodically sends updates to weights that have changed signiﬁcantly to the other learners using peer-to-peer communication.
• 参考文献：Grounds, Matthew and Kudenko, Daniel. Parallel reinforcement learning with linear function approximation. In Proceedings of the 5th, 6th and 7th European Conference on Adaptive and Learning Agents and Multi-agent Systems: Adaptation and Multi-agent Learning, pp. 60– 74. Springer-Verlag, 2008.
扩展阅读
  基于value estimation的critic方法。广泛应用于各种领域，但有一些缺点使它的应用受到局限。如 ：
1. 难以应用到随机型策略（stochastic policy）和连续的动作空间。
2. value function的微小变化会引起策略变化巨大，从而使训练无法收敛。尤其是引入函数近似（function approximation，FA）后，虽然算法泛化能力提高了，但也引入了bias，从而使得训练的收敛性更加难以保证。
  而基于actor方法通过将策略参数化，从而直接学习策略。这样做的好处是与前者相比拥有更好的收敛性，以及适用于高维连续动作空间及stochastic policy。但缺点包括梯度估计variance比较高，且容易收敛到非最优解。另外因为每次梯度的估计不依赖以往的估计，意味着无法充分利用老的信息。
  但对于AC算法来说其架构可以追溯到三、四十年前。 最早由Witten在1977年提出了类似AC算法的方法，然后Barto, Sutton和Anderson等大牛在1983年左右引入了actor-critic架构。但由于AC算法的研究难度和一些历史偶然因素，之后学界开始将研究重点转向value-based方法。之后的一段时间里value-based方法和policy-based方法都有了蓬勃的发展。前者比较典型的有TD系的方法。经典的Sarsa, Q-learning等都属于此列；后者比如经典的REINFORCE算法。之后AC算法结合了两者的发展红利，其理论和实践再次有了长足的发展。直到深度学习（Deep learning, DL）时代，AC方法结合了DNN作为FA，产生了化学反应，出现了DDPG，A3C这样一批先进算法，以及其它基于它们的一些改进和变体。可以看到，这是一个先分后合的圆满故事。[6]


[1]: https://raw.githubusercontent.com/openmlsys/openmlsys-zh/main/chapter_reinforcement_learning/distributed_node_rl.md
[2]: https://zhuanlan.zhihu.com/p/478990678
[3]: https://blog.csdn.net/crazy_girl_me/article/details/123263603
[4]: https://www.zhihu.com/column/c_1664539238795296768
[5]: https://zhuanlan.zhihu.com/p/25239682
[6]: https://developer.aliyun.com/article/1294160?spm=a2c6h.14164896.0.0.743a47c54FghY1
