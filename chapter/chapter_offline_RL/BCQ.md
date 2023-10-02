# 批量限制 Q-learning （BCQ）算法

图 18-1 中的离线强化学习和离线策略强化学习很像，都要从经验回放池中采样进行训练，并且离线策略算法的策略评估方式也多种多样。因此，研究者们最开始尝试将离线策略算法直接照搬到离线的环境下，仅仅是去掉算法中和环境交互的部分。然而，这种做法完全失败了。研究者进行了 3 个简单的实验。第一个实验，作者使用 DDPG 算法训练了一个智能体，并将智能体与环境交互的所有数据都记录下来，再用这些数据训练离线 DDPG 智能体。第二个实验，在线 DDPG 算法在训练时每次从经验回放池中采样，并用相同的数据同步训练离线 DDPG 智能体，这样两个智能体甚至连训练时用到的数据顺序都完全相同。第三个实验，在线 DDPG 算法在训练完毕后作为专家，在环境中采集大量数据，供离线 DDPG 智能体学习。这 3 个实验，即完全回放、同步训练、模仿训练的结果依次如图 18-2 所示。



让人惊讶的是，3 个实验中，离线 DDPG 智能体的表现都远远差于在线 DDPG 智能体，即便是第二个实验的同步训练都无法提高离线智能体的表现。在第三个模仿训练实验中，离线智能体面对非常优秀的数据样本却什么都没学到！针对这种情况，研究者指出，**外推误差**（extrapolation error）是离线策略算法不能直接迁移到离线环境中的原因。

外推误差，是指由于当前策略可能访问到的状态动作对与从数据集中采样得到的状态动作对的分布不匹配而产生的误差。为什么在线强化学习算法没有受到外推误差的影响呢？因为对于在线强化学习，即使训练是离线策略的，智能体依然有机会通过与环境交互及时采样到新的数据，从而修正这些误差。但是在离线强化学习中，智能体无法和环境交互。因此，一般来说，离线强化学习算法要想办法**尽可能地限制外推误差的大小**，从而得到较好的策略。

为了减少外推误差，当前的策略需要做到只访问与数据集中相似的数据。满足这一要求的策略称为**批量限制策略**（batch-constrained policy）。具体来说，这样的策略在选择动作时有 3 个目标：

- 最小化选择的动作与数据集中数据的距离；
- 采取动作后能到达与离线数据集中状态相似的状态；
- 最大化函数 $Q$。

对于标准的表格（tabular）型环境，状态和动作空间都是离散且有限的。标准的 Q-learning 更新公式可以写为：

$$
Q(s, a) \leftarrow(1-\alpha) Q(s, a)+\alpha\left(r+\gamma Q\left(s^{\prime}, \operatorname{argmax}_{a^{\prime}} Q\left(s^{\prime}, a^{\prime}\right)\right)\right)
$$

这时，只需要把策略 $\pi$ 能选择的动作限制在数据集 $\mathcal{D}$ 内，就能满足上述 3 个目标 的平衡，这样就得到了表格设定下的批量限制 Q-learning (batch-constrained Q-learning， BCQ) 算法:

$$
Q(s, a) \leftarrow(1-\alpha) Q(s, a)+\alpha\left(r+\gamma Q\left(s^{\prime}, \operatorname{argmax}_{a^{\prime} \text { s.t. }\left(s^{\prime}, a^{\prime}\right) \in \mathcal{D}} Q\left(s^{\prime}, a^{\prime}\right)\right)\right)
$$

可以证明，如果数据中包含了所有可能的 $(s, a)$ 对，按上式进行迭代可以收敛到最优的价值函数 $Q^*$ 。

连续状态和动作的情况要复杂一些，因为批量限制策略的目标需要被更详细地定义。例如，该如何定义两个状态动作对的距离呢? BCQ 采用了一种巧妙的方法: 训练一个生成模型 $G_\omega(s)$ 。对于数据集 $\mathcal{D}$ 和其中的状态 $s$ ，生成模型 $G_\omega(s)$ 能给出与 $\mathcal{D}$ 中数据接近的一系列动作 $a_1, \ldots, a_n$ 用于 $Q$ 网络的训练。更进一步， 为了增加生成动作的多样性，减少生成次数， $B C Q$ 还引入了扰动模型 $\xi_\phi(s, a, \Phi)$ 。输入 $(s, a)$ 时，模型给出一个绝对值最大为 $\Phi$ 的**微扰**并附加在动作上。这两个模型综合起来相当于给出了一个批量限制策略 $\pi$ :

$$
\pi(s)=\operatorname{argmax}_{a_i+\xi_\phi\left(s, a_i, \Phi\right)} Q_\theta\left(s, a_i+\xi_\phi\left(s, a_i, \Phi\right)\right), \quad\left\{a_i \sim G_\omega(s)\right\}_{i=1}^n
$$

其中，生成模型 $G_\omega(s)$ 用**变分自动编码器** (variational auto-encoder, VAE) 实现；扰动模型直接通过确定性策略梯度算法训练，目标是使函数 $Q$ 最大化:

$$
\phi \leftarrow \operatorname{argmax}_\phi \sum_{(s, a) \in \mathcal{D}} Q_\theta\left(s, a+\xi_\phi(s, a, \Phi)\right)
$$

总结起来，BCQ 算法的流程如下：

- 随机初始化 $Q$ 网络 $Q_\theta$ 、扰动网络 $\xi_\phi$ 、生成网络 $G_\omega=\left\{E_{\omega_1}, D_{\omega_2}\right\}$
- 用 $\theta$ 初始化目标 $Q$ 网络 $Q_{\theta^{-}}$，用 $\phi$ 初始化目标扰动网络 $\xi_{\phi^{\prime}}$
- for 训练次数 $e=1 \rightarrow E$ do
  - 从数据集 $\mathcal{D}$ 中采样一定数量的 $\left(s, a, r, s^{\prime}\right)$
  - 编码器生成均值和标准差 $\mu, \sigma=E_{\omega_1}(s, a)$
  - 解码器生成动作 $\tilde{a}=D_{\omega_2}(s, z)$, 其中 $z \sim \mathcal{N}(\mu, \sigma)$
  - 更新生成模型:
  $\omega \leftarrow \operatorname{argmin}_\omega \sum(a-\tilde{a})^2+D_{K L}(\mathcal{N}(\mu, \sigma) \| \mathcal{N}(0,1))$
  - 从生成模型中采样 $n$ 个动作: $\left\{a_i \sim G_\omega\left(s^{\prime}\right)\right\}_{i=1}^n$
  - 对每个动作施加扰动: $\left\{a_i \leftarrow a_i+\xi_\phi\left(s^{\prime}, a_i, \phi\right)\right\}_{i=1}^n$
  - 计算 $Q$ 网络的目标值 $y=r+\gamma \max _{a_i} Q_{\theta^{-}}\left(s^{\prime}, a_i\right)$
  - 更新 $Q$ 网络: $\theta \leftarrow \operatorname{argmin}_\theta \sum\left(y-Q_\theta(s, a)\right)^2$
  - 更新扰动网络:
  $\phi \leftarrow \operatorname{argmax}_\phi \sum Q_\theta\left(s, a+\xi_\phi(s, a, \Phi)\right), a \sim G_\omega(s)$
  - 更新目标 $Q$ 网络: $\theta^{-} \leftarrow \tau \theta+(1-\tau) \theta^{-}$
  - 更新目标扰动网络: $\phi^{\prime} \leftarrow \tau \phi+(1-\tau) \phi^{\prime}$
- end for

除此之外，BCQ 还使用了一些实现上的小技巧。由于不是 BCQ 的重点，此处不再赘述。考虑到 VAE 不属于本书的讨论范围，并且 BCQ 的代码中有较多技巧，有兴趣的读者可以参阅 BCQ 原文[2]，*自行实现代码*。此处介绍 BCQ 算法，一是因为它对离线强化学习的误差分析和实验很有启发性，二是因为它是无模型离线强化学习中限制策略集合算法中的经典方法。下面我们介绍另一类直接限制函数 $Q$ 的算法的代表：保守 Q-learning。

更多见：https://zhuanlan.zhihu.com/p/493039753

[1]: https://hrl.boyuai.com/chapter/3/%E7%A6%BB%E7%BA%BF%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0/#182-%E6%89%B9%E9%87%8F%E9%99%90%E5%88%B6-q-learning-%E7%AE%97%E6%B3%95
[2]: https://arxiv.org/abs/1812.02900
