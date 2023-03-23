

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-22 00:26:59
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-22 01:08:22
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 策略学习

本章的内容是策略学习 (Policy-Based Reinforcement Learning) 以及策略梯度 (Policy Gradient)。策略学习的意思是通过求解一个优化问题，学出最优策略函数或它的近似（比如策略网络）。

## 策略网络

本章考虑离散动作空间, 比如 $\mathcal{A}=\{$ 左, 右, 上 $\}$ 。策略函数 $\pi$ 是个条件概率质量函数：

$$
\pi(a \mid s) \triangleq \mathbb{P}(A=a \mid S=s) .
$$

策略函数 $\pi$ 的输人是状态 $s$ 和动作 $a$, 输出是一个 0 到 1 之间的概率值。举个例了, 把 超级玛丽游戏当前屏幕上的画面作为 $s$ ，策略函数会输出每个动作的概率值：

$$
\begin{aligned}
& \pi(\text { 左 } \mid s)=0.5, \\
& \pi(\text { 右 } \mid s)=0.2, \\
& \pi(\text { 上 } \mid s)=0.3 .
\end{aligned}
$$

如果我们有这样一个策略函数, 我们就可以拿它控制智能体。每当观测到一个状态 $s$, 就 用策略函数计算出每个动作的概率值, 然后做随机抽样，得到一个动作 $a$, 让智能体执行 $a$ 。

怎么样才能得到这样一个策略函数呢? 当前最有效的方法是用神经网络 $\pi(a \mid s ; \boldsymbol{\theta})$ 近 似策略函数 $\pi(a \mid s)$ 。神经网络 $\pi(a \mid s ; \boldsymbol{\theta})$ 被称为策略网络。 $\boldsymbol{\theta}$ 表示神经网络的参数；一开始随机初始化 $\theta$, 随后利用收集的状态、动作、奖励去更新 $\theta$ 。

策略网络的结构如图 7.1 所示。策略网络的输入是状态 $s$ 。在 Atari 游戏、围棋等应用中, 状态是张量 (比如图片), 那么应该如图 7.1 所示用卷积网络处理输入。在机器人控制等应用中, 状态 $s$ 是向量, 它的元素是多个传感器的数值, 那么应该把卷积网络换成全连接网络。策略网络输出层的激活函数是 Softmax, 因此输出的向量 (记作 $\boldsymbol{f}$ ) 所有元 素都是正数, 而且相加等于 1 。动作空间 $\mathcal{A}$ 的大小是多少, 向量 $\boldsymbol{f}$ 的维度就是多少。在 超级玛丽的例了中, $\mathcal{A}=\{$ 左, 右, 上 $\}$, 那么 $f$ 就是 3 维的向量, 比如 $f=[0.2,0.1,0.7]$ 。 $f$ 描述了动作空间 $\mathcal{A}$ 上的离散概率分布, $f$ 每个元素对应一个动作：

$$
\begin{aligned}
& f_1=\pi(\text { 左 } \mid s)=0.2, \\
& f_2=\pi(\text { 右 } \mid s)=0.1, \\
& f_3=\pi(\text { 上 } \mid s)=0.7 .
\end{aligned}
$$

## 策略学习的目标函数

为了推导策略学习的日标函数, 我们需要先复习回报和价值函数。回报 $U_t$ 是从 $t$ 从 时刻开始的所有坣励之和。 $U_t$ 依赖于 $t$ 时刻开始的所有状态和动作：

$$
S_t, A_t, S_{t+1}, A_{t+1}, S_{t+2}, A_{t+2}, \cdots
$$

在 $t$ 时刻, $U_t$ 是随机变量, 它的不确定恈来自于末来末知的状态和劫作。动作价值函数的定义是：

$$
Q_\pi\left(s_t, a_t\right)=\mathbb{E}\left[U_t \mid S_t=s_t, A_t=a_t\right]
$$

条件期望把 $t$ 时刻状态 $s_t$ 和动作 $a_t$ 看做已知观测值, 把 $t+1$ 时刻后的状态和动作看做 末知变量，并消除这些变量。状态价值函数的定义是

$$
V_\pi\left(s_t\right)=\mathbb{E}_{A_t \sim \pi\left(\cdot \mid s_t ; \theta\right)}\left[Q_\pi\left(s_t, A_t\right)\right] .
$$

状态价值既依赖于当前状态 $s_t$, 也依赖于策略网络 $\pi$ 的参数 $\boldsymbol{\theta}$ 。

- 当前状态 $s_t$ 越好, 则 $V_\pi\left(s_t\right)$ 越大, 也就是回报 $U_t$ 的期望越大。例如, 在超级玛丽 游戏中，如果玛丽奥已经接近终点（也就是说当前状态 $s_t$ 很好）, 那么回报的期望 就会很大。
- 策略 $\pi$ 越好 (即参数 $\boldsymbol{\theta}$ 越好), 那么 $V_\pi\left(s_t\right)$ 也会越大。例如, 从同一起点出发打游 戏, 高于 (好的策略) 的期望回报远高于初学者 (差的策略)。

如果一个策略很好, 那么对于所有的状态 $S$, 状态价值 $V_\pi(S)$ 的均值应当很大。因此我 们定义日标函数:

$$
J(\boldsymbol{\theta})=\mathbb{E}_S\left[V_\pi(S)\right]
$$
这个日标函数排除掉了状态 $S$ 的因素, 只依赖于策略网络 $\pi$ 的参数 $\theta$; 策略越好, 则 $J(\theta)$ 越大。所以策略学习可以描述为这样一个优化问题：

$$
\max _\theta J(\theta)
$$

我们希望通过对策略网络参数 $\theta$ 的更新, 使得日标函数 $J(\theta)$ 越来越大, 也就意味着策略 网络越来越强。想要求解最大化问题, 显然可以用梯度上升更新 $\theta$, 使得 $J(\theta)$ 增大。设 当前策略网络的参数为 $\theta_{\text {now }}$ 。做梯度上升更新参数, 得到新的参数 $\theta_{\text {new }}$ :

$$
\theta_{\text {new }} \leftarrow \boldsymbol{\theta}_{\text {now }}+\beta \cdot \nabla_{\boldsymbol{\theta}} J\left(\boldsymbol{\theta}_{\text {now }}\right)
$$

此处的 $\beta$ 是学习率, 需要于动调。上而的公式就是训练策略网络的基本想法, 其中的梯度

$$
\left.\nabla_{\boldsymbol{\theta}} J\left(\boldsymbol{\theta}_{\text {now }}\right) \triangleq \frac{\partial J(\theta)}{\partial \boldsymbol{\theta}}\right|_{\theta=\boldsymbol{\theta}_{\text {now }}}
$$

被称作策略梯度。策略梯度可以兴成下面定理中的期望形式。之后的算法推导都要基于这个定理，并对其中的期望做近似。




[1]: https://www.math.pku.edu.cn/teachers/zhzhang/drl_v1.pdf
