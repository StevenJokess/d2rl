

<!--
 * @version:
 * @Author:  StevenJokes https://github.com/StevenJokes
 * @Date: 2023-02-21 21:18:59
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-13 00:56:42
 * @Description:
 * @TODO::
 * @Reference:
-->
# 多臂老虎机（multi-armed bandit，MAB）环境

下面考虑这样一个**非关联性**的简化环境，智能体不涉及学习如何在多种情况下行动，而只涉及一步，从而避免了完整强化学习问题的大部分复杂性，多臂老虎机问题可被视为序列决策问题的一种特殊情况，只是为了帮助我们更好地理解强化学习的基本概念和方法。有一个拥有 $K$ 根拉杆的老虎机，拉动每一根拉杆都对应一个关于奖励 $R$ 的概率分布。我们每次拉动其中一根拉杆，就可以从该拉杆对应的奖励概率分布中获得一个奖励 $r$。我们在各根拉杆的奖励概率分布未知的情况下，从头开始尝试，目标是在操作 $T$ 次拉杆后获得尽可能高的累积奖励。由于奖励的概率分布是未知的，因此我们需要在“探索拉杆的获奖概率”和“根据经验选择获奖最多的拉杆”中进行权衡。

## 形式化描述

将其问题表示成一个元组 $\langle\mathcal{A}, \mathcal{R}\rangle$ ，其中:

- $\mathcal{A}$ 为动作集合，其中一个动作表示拉动一个拉杆。若多臂 老虎机一共有 $K$ 根拉杆，那动作空间就是集合 $\{a_1, \ldots, a_K\}$ ，我们用 $a_t \in \mathcal{A}$ 表示任意一个动作；
- $\mathcal{R}$ 为奖励概率分布，拉动每一根拉杆的动作 $a$ 都对应一个 奖励概率分布 $\mathcal{R}(r \mid a)$ ，不同拉杆的奖励分布通常是不同的。

## 优化目标

假设每个时间步只能拉动一个拉杆，多臂老虎机的目标为最大化一段时间步 $T$ 内（累积的，因为无延迟回报，所以即时回报就是所有回报）回报: $\max \sum_{t=1}^T r_t, r_t \sim \mathcal{R}\left(\cdot \mid a_t\right)$ 。 其中 $a_t$ 表示在第 $t$ 时间步拉动某一拉杆的动作， $r_t$ 表示动作 $a_t$ 获得的奖励。

### 累积懊悔（累计误差）

对于每一个动作 $a$ ，我们定义其期望回报为 $Q(a)=\mathbb{E}_{r \sim \mathcal{R}(\cdot \mid a)}[r]$ 。

由于，至少存在一根拉杆，它的期望回报不小于拉动其他任意一根拉杆，我们将该最优期望回报表示为 $Q^*=\max _{a \in \mathcal{A}} Q(a)$ 。

为了更加直观、方便地观察拉动一根拉杆的期望回报离最优拉杆期望回报的差距，我们引入懊悔 (regret) 概念。懊悔定义为拉动当前拉杆的动作 $a$ 与最优拉杆 的期望回报差距，即 $R(a)=Q^*-Q(a)$ （其中，$R(a) \geq 0$）。

累积懊悔(cumulative regret) 即操作 $T$ 次拉杆后累积的懊悔总量，对于一次完整的 $T$ 步决策 $\left\{a_1, a_2, \ldots, a_T\right\}$ ，累积懊悔为 $\sigma_R=\sum_{t=1}^T R\left(a_t\right)$ 。

那么MAB 问题的目标为最大化累积回报，等价于最小化累积懊悔。

### 价值（估值用期望回报算的——也叫期望回报估值）

为了知道拉动哪一根拉杆能获得更高的回报，我们需要估计拉动这根拉杆的期望回报值。由于只拉动一次拉杆获得的回报存在随机性，所以需要多次拉动一根拉杆，然后计算得到的多次回报的期望，其算法流程如下所示：

- 对于 $\forall a \in \mathcal{A}$ ，初始化计数器 $N(a)=0$ 和期望回报估值 $\hat{Q}(a)=0$
- for $t=1 \rightarrow T$ do
  - 选取某根拉杆，该动作记为 $a_t$
  - 得到回报 $r_t$
  - 更新该杆的计数器: $N\left(a_t\right)=N\left(a_t\right)+1$
  - 更新期望回报估值:
    $$\hat{Q}\left(a_t\right)=\hat{Q}\left(a_t\right)+\frac{1}{N\left(a_t\right)}\left[r_t-\hat{Q}\left(a_t\right)\right]$$
- end for

以上 for 循环中的第四步如此更新估值，是因为这样可以进行增量式的期望更新，公式如下：

$$
\begin{aligned}
Q_k & =\frac{1}{k} \sum_{i=1}^k r_i \\
& =\frac{1}{k}\left(r_k+\sum_{i=1}^{k-1} r_i\right) \\
& =\frac{1}{k}\left(r_k+(k-1) Q_{k-1}\right) \\
& =\frac{1}{k}\left(r_k+k Q_{k-1}-Q_{k-1}\right) \\
& =Q_{k-1}+\frac{1}{k}\left[r_k-Q_{k-1}\right]
\end{aligned}
$$

如果将所有$r_i$求和再除以次数，其缺点是每次更新的时间复杂度和空间复杂度均为 $O(n)$ 。而采用增量式更新，时间复杂度和空间复杂度均为 $O(1)$ 。

下面我们编写代码来实现一个拉杆数为 10 的多臂老虎机。其中拉动每根拉杆的回报服从伯努利分布（Bernoulli distribution），即每次拉下拉杆有 $p$ 的概率获得的回报为 1，有 $1-p$ 的概率获得的回报为 0。奖励为 1 代表获奖，奖励为 0 代表没有获奖。

接下来我们用一个 `Solver` 基础类来实现上述的多臂老虎机的求解方案。根据前文的算法流程，我们需要实现下列函数功能：根据策略选择动作、根据动作获取奖励、更新期望奖励估值、更新累积懊悔和计数。在下面的 MAB 算法基本框架中，我们将**根据策略选择动作、根据动作获取奖励**和**更新期望回报估值**放在 `run_one_step()` 函数中，由每个继承 `Solver` 类的策略具体实现。而**更新累积懊悔和计数**则直接放在主循环 `run()` 中。

### 决策（Q -> A）

决策是根据各个动作价值函数值确定选择哪个动作。

#### 探索 VS 利用

- 探索（exploration）是指尝试拉动更多可能的拉杆，这根拉杆不一定会获得最大的奖励，但这种方案能够摸清楚所有拉杆的获奖情况。例如，对于一个 10 臂老虎机，我们要把所有的拉杆都拉动一下才知道哪根拉杆可能获得最大的回报。
- 利用（exploitation）是指拉动已知期望回报最大的那根拉杆，由于已知的信息仅仅来自有限次的交互观测，所以当前的最优拉杆不一定是全局最优的。例如，对于一个 10 臂老虎机，我们只拉动过其中 3 根拉杆，接下来就一直拉动这 3 根拉杆中期望奖励最大的那根拉杆，但很有可能期望奖励最大的拉杆在剩下的 7 根当中，即使我们对 10 根拉杆各自都尝试了 20 次，发现 5 号拉杆的经验期望回报是最高的，但仍然存在着微小的概率—另一根 6 号拉杆的真实期望回报是比 5 号拉杆更高的。

> 真实期望回报是指在实际情况下，一个动作所能获得的平均回报。
> 历史期望回报是基于历史数据进行计算，一个动作所能获得的平均回报。

于是在多臂老虎机问题中，设计策略时就需要平衡探索和利用的次数，使得累积奖励最大化。一个比较常用的思路是在开始时做比较多的探索，在对每根拉杆都有比较准确的估计后，再进行利用。

目前已有一些比较经典的算法来解决这个问题，例如$\epsilon$-贪婪算法、上置信界算法和汤普森采样算法等，我们接下来将分别介绍这几种算法。


#### $\epsilon$ -贪婪算法（$\epsilon-greedy$）

**完全贪婪算法**即在每一时刻采取期望回报估值最大的动作（拉动拉杆），这就是**纯粹的利用**，而没有探索，所以我们通常需要对完全贪婪算法进行一些修改，其中比较经典的一种方法为 $\epsilon$-贪婪（ $\epsilon$-Greedy）算法。 $\epsilon$-贪婪算法在完全贪婪算法的基础上添加了噪声，每次以概率 $1 - \epsilon$  选择以往经验中期望奖励估值最大的那根拉杆（利用），以概率 $\epsilon$ 随机选择一根拉杆（探索），公式如下：

$$
a_{t}= \begin{cases}{\operatorname{argmax}_{a \in \mathcal{A}}} \hat{Q}_t(a), & \text { 采样概率: } 1-\epsilon \\
 \text { 从 } \mathcal{A} \text { 中随机选择, } & \text { 采样概率: } \epsilon\end{cases} [2]
$$

随着探索次数的不断增加，我们对各个动作的价值估计得越来越准，此时我们就没必要继续花大力气进行探索。所以在 -贪婪算法的具体实现中，我们可以令 $\epsilon$ 随时间衰减，即探索的概率将会不断降低。但是请注意，$\epsilon$ 不会在有限的步数内衰减至 0，因为基于有限步数观测的完全贪婪算法仍然是一个局部信息的贪婪算法，永远距离最优解有一个固定的差距。

我们接下来编写代码来实现一个 $\epsilon$-贪婪算法，并用它去解决 上一节 生成的 10 臂老虎机的问题。设置 $\epsilon = 1$ ，以及$T =500$。


通过上面的实验可以发现，在经历了开始的一小段时间后，$\epsilon$-贪婪算法的累积懊悔几乎是线性增长的。这是  $\epsilon = 0.01$ 时的结果，因为一旦做出了随机拉杆的探索，那么产生的懊悔值是固定的。其他不同的 $\epsilon$ 取值又会带来怎样的变化呢？我们继续使用该 10 臂老虎机，我们尝试不同的$\left\{10^{-4}, 0.01, 0.1, 0.25, 0.5\right\}$参数，查看相应的实验结果（另见彩插图 1）。

通过实验结果可以发现，基本上无论 $\epsilon$ 取值多少，累积懊悔都是线性增长的。在这个例子中，随着 $\epsilon$ 的增大，累积懊悔增长的速率也会增大。 接下来我们尝试 $\epsilon$ 值随时间衰减的 $\epsilon$ -贪婪算法，采取的具体衰减形式为反比例衰减，公式为$\epsilon = \frac{1}{t}$ 。

从实验结果图中可以发现，随时间做反比例衰减的 $\epsilon$ -贪婪算法能够使累积懊悔与时间步的关系变成**次线性**（sublinear）的，这明显优于固定 $\epsilon$ 值的  $\epsilon$ -贪婪算法。

#### 上置信界算法

设想这样一种情况：对于一台双臂老虎机，其中第一根拉杆只被拉动过一次，得到的奖励为 $0$；第二根拉杆被拉动过很多次，我们对它的奖励分布已经有了大致的把握。这时你会怎么做？或许你会进一步尝试拉动第一根拉杆，从而更加确定其奖励分布。这种思路主要是基于不确定性，因为此时第一根拉杆只被拉动过一次，它的**不确定性很高**。一根拉杆的不确定性越大，它就越具有探索的价值，因为探索之后我们可能发现它的期望奖励很大。我们在此引入不确定性度量 ，它会随着一个动作被尝试次数的增加而减小。我们可以使用一种基于不确定性的策略来综合考虑现有的期望奖励估值和不确定性，其核心问题是如何估计不确定性。

##### 上置信界（UCB）

**上置信界** (upper confidence bound，UCB) 算法是一种经典的基于不确定性的策略算法，它的思想用到了一个非常著名的数学原理: **霍夫丁不等式** (Hoeffding's inequality)。

> 在霍夫丁不等式中，令 $X_1, \ldots, X_n$ 为 $n$ 个独立同分布的随机变量，取值范围为 $[0,1]$ ，其经验期望为 $\bar{x}_n=\frac{1}{n} \sum_{j=1}^n X_j$ ，则有
>
> $$
> \mathbb{P}\{\mathbb{E}[X] \geq \bar{x}_n+u\} \leq e^{-2 n u^2}
> $$

现在我们将霍夫丁不等式运用于多臂老虎机问题中。将 $\hat{Q}_t(a)$ 代入 $\bar{x}_t$ ，不等式中的参数 $u=\hat{U}_t(a)$ 代表**不确定性度量**，给定一个概率 $p=e^{-2 N_t(a) U_t(a)^2}$ ，根据上述不等式， $Q_t(a)<\hat{Q}_t(a)+\hat{U}_t(a)$ 至少以概率 $1-p$ 成立。其中，我们用$N$表示到目前为止按压所有臂的次数和，$N_t$代表为目前为止按压第t个臂的次数。

当 $p$ 很小时， $Q_t(a)<\hat{Q}_t(a)+\hat{U}_t(a)$ 就以很大概率成立， $\hat{Q}_t(a)+\hat{U}_t(a)$ 便是**期望回报上界**。此时，上置信界算法便选取期望回报上界最大的动作，即 $a=\underset{a \in \mathcal{A}}{\operatorname{argmax}}[\hat{Q}(a)+\hat{U}(a)]$ 。

那其中的 $\hat{U}_t(a)$ 具体是什么呢?

根据等式 $e^{-2 N_t(a) U_t(a)^2}$ ，解之 即得 $\hat{U}_t(a)=\sqrt{\frac{-\ln p}{2 N_t(a)}}$ 。

因此，设定一个概率 $p$ 后，就可以计算相应的**不确定性度量** $\hat{U}_t(a)$ 了。

更直观地说，UCB 算法在每次选择拉杆前，先估计每根拉杆的期望回报的上界，使得拉动每根拉杆的期望回报只有一个较小的概率 $p$ 超过这个上界，接着选出期望回报上界最大的拉杆，从而选择最有可能获得最大期望回报的拉杆。

我们编写代码来实现 UCB 算法，并且仍然使用 2.2.4 节定义的 10 臂老虎机来观察实验结果。在具体的实现过程中，设置 $p=\frac{1}{N}$ ，并且在分母中为拉动每根拉杆的次数加上常数 1 ，这确保每个动作至少被探索一次，同时也避免了出现分母为 0 的情形，即此时 $\hat{U}_t(a)=\sqrt{\frac{\ln N}{2\left(N_t(a)+1\right)}}$ 。

同时，我们设定一个系数 $c$ 来控制不确定性的比重，此时 $a=\arg \max _{a \in \mathcal{A}} \hat{Q}(a)+c \cdot \hat{U}(a) 。$

在A Finite-Time Analysis of Multi-armed Bandits Problems with Kullback-Leibler Divergences的中的UCB1中[6]这个$c = 2$。[7]

TODO:WHY？考虑了价值函数本身的大小和搜索次数, 能够自动实现探索和利用的自动平衡, 并能够有效减少探索次数.
[3]

#### 汤普森采样算法

MAB 中还有一种经典算法——**汤普森采样**（Thompson sampling）[8]，先假设拉动每根拉杆的奖励服从一个特定的概率分布，然后根据拉动每根拉杆的期望奖励来进行选择。但是由于计算所有拉杆的期望奖励的代价比较高，汤普森采样算法使用采样的方式，即根据当前每个动作 $a$ 的奖励概率分布进行一轮采样，得到一组各根拉杆的奖励样本，再选择样本中奖励最大的动作。可以看出，汤普森采样是一种计算所有拉杆的最高奖励概率的蒙特卡洛采样方法。

了解了汤普森采样算法的基本思路后，我们需要解决另一个问题：怎样得到当前每个动作 $a$ 的奖励概率分布并且在过程中进行更新？在实际情况中，我们通常用 Beta 分布对当前每个动作的奖励概率分布进行建模。具体来说，若某拉杆被选择了 $k$ 次，其中 $m_1$ 次奖励为 1，$m_2$ 次奖励为 0，则该拉杆的奖励服从参数为$(m_1+1,m_2+1)$ 的 Beta 分布。图 2-2 是汤普森采样的一个示例（另见彩插图 2）。

![汤普森采样示例](img\Thompson_sampling.png)

我们编写代码来实现汤普森采样算法，并且仍然使用 2.2.4 节定义的 10 臂老虎机来观察实验结果。

通过实验我们可以得到以下结论： $\epsilon$-贪婪算法的累积懊悔是随时间线性增长的，而另外 3 种算法（ $\epsilon$-衰减贪婪算法、上置信界算法、汤普森采样算法）的累积懊悔都是随时间次线性增长的（具体为对数形式增长）。


#### 总结

探索与利用是与环境做交互学习的重要问题，是强化学习试错法中的必备技术，而多臂老虎机问题是研究探索与利用技术理论的最佳环境。了解多臂老虎机的探索与利用问题，对接下来我们学习强化学习环境探索有很重要的帮助。对于多臂老虎机各种算法的累积懊悔理论分析，有兴趣的同学可以自行查阅相关资料。 $\epsilon$ -贪婪算法、上置信界算法和汤普森采样算法在多臂老虎机问题中十分常用，其中上置信界算法和汤普森采样方法均能保证对数的渐进最优累积懊悔。

多臂老虎机问题与强化学习的一大区别在于其与环境的交互并不会改变环境，即多臂老虎机的每次交互的结果和以往的动作无关，所以可看作无状态的强化学习（stateless reinforcement learning）。第 3 章将开始在有状态的环境下讨论强化学习，即马尔可夫决策过程。

#### 更多无向随机抖动策略

我们重新审视一下 \varepsilon -greedy 策略、玻尔兹曼策略和高斯策略，不难发现，这些策略的探索方法都是在贪婪策略或确定性策略上面加上一个**随机无向的噪声**。它们是通过噪声进行探索的。Iosband 称这种策略为抖动策略（dithering strategy）。[5]

利用抖动策略的好处是：

1. 抖动策略计算容易，不需要复杂的计算公式。
1. 能保证充分探索。

相应的坏处是：

1. 需要大量探索，数据利用率低。
2. 需要无限长时间。

#### 玻尔兹曼探索

在玻尔兹曼探索中，我们假设对于任意的 $s、a ， Q(s, a) \geqslant 0$ ，因此 $a$ 被选中的概率与 $e^{Q(s, a) / T}$ 呈正比，即

$$
\pi(a \mid s)=\frac{\mathrm{e}^{Q(s, a) / T}}{\sum_{a^{\prime} \in A} \mathrm{e}^{Q\left(s, a^{\prime}\right) / T}}
$$
其中， $T>0$ 称为温度系数。如果 $T$ 很大，所有动作几乎以 等概率选择 (探索) ; 如果 $T$ 很小，Q值大的动作更容易被选 中 (利用) ；如果 $T$ 趋于 0 ，我们就只选择最优动作。[4]

#### 高斯策略

在连续系统中，最常用的随机策略为高斯策略，即：

$\pi_{\theta}=\mu_{\theta}+\varepsilon ,\varepsilon \sim N\left(0,\sigma^2\right)$


[1]: https://hrl.boyuai.com/chapter/1/%E5%A4%9A%E8%87%82%E8%80%81%E8%99%8E%E6%9C%BA
[2]: https://www.jianshu.com/p/590d98967a93
[3]: http://www.c-s-a.org.cn/html/2020/12/7701.html#outline_anchor_19
[4]: https://datawhalechina.github.io/easy-rl/#/chapter6/chapter6?id=_61-%e7%8a%b6%e6%80%81%e4%bb%b7%e5%80%bc%e5%87%bd%e6%95%b0
[5]: https://zhuanlan.zhihu.com/p/32717586
[6]: https://arxiv.org/abs/1105.5820
[7]: https://stats.stackexchange.com/questions/498158/is-there-any-difference-between-ucbupper-bound-confidence-and-ucb1upper-bound
[8]: https://arxiv.org/pdf/1707.02038.pdf

更多资料：

[1]: http://banditalgs.com/
[2]: http://slivkins.com/work/MAB-book.pdf
[3]: https://arxiv.org/pdf/1204.5721.pdf
