

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-02-25 23:35:41
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-02-26 00:17:10
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 离线强化学习

## 简介

在前面的学习中，我们已经对强化学习有了不少了解。无论是在线策略（on-policy）算法还是离线策略（off-policy）算法，都有一个共同点：智能体在训练过程中可以不断和环境交互，得到新的反馈数据。二者的区别主要在于在线策略算法会直接使用这些反馈数据，而离线策略算法会先将数据存入经验回放池中，需要时再采样。然而，在现实生活中的许多场景下，让尚未学习好的智能体和环境交互可能会导致危险发生，或是造成巨大损失。例如，在训练自动驾驶的规控智能体时，如果让智能体从零开始和真实环境进行交互，那么在训练的最初阶段，它操控的汽车无疑会横冲直撞，造成各种事故。再例如，在推荐系统中，用户的反馈往往比较滞后，统计智能体策略的回报需要很长时间。而如果策略存在问题，早期的用户体验不佳，就会导致用户流失等后果。因此，**离线强化学习**（offline reinforcement learning）的目标是，在智能体不和环境交互的情况下，仅从已经收集好的确定的数据集中，通过强化学习算法得到比较好的策略。离线强化学习和在线策略算法、离线策略算法的区别如图 18-1 所示。

## 批量限制 Q-learning 算法

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

连续状态和动作的情况要复杂一些，因为批量限制策略的目标需要被更详细地定义。例如，该如何定义两个状态动作对的距离呢? BCQ 采用了一种巧妙的方法: 训练一个生成模型 $G_\omega(s)$ 。对于数据集 $\mathcal{D}$ 和其中的状态 $s$ ，生成模型 $G_\omega(s)$ 能给出与 $\mathcal{D}$ 中数据接近的一系列动作 $a_1, \ldots, a_n$ 用于 $Q$ 网络的训练。更进一步， 为了增加生成动作的多样性，减少生成次数， $B C Q$ 还引入了扰动模型 $\xi_\phi(s, a, \Phi)$ 。输入 $(s, a)$ 时，模型给出一个绝对值最大为 $\Phi$ 的微扰并附加在动作上。这两个模型综合起来相当于给出了一个批量限制策略 $\pi$ :

$$
\pi(s)=\operatorname{argmax}_{a_i+\xi_\phi\left(s, a_i, \Phi\right)} Q_\theta\left(s, a_i+\xi_\phi\left(s, a_i, \Phi\right)\right), \quad\left\{a_i \sim G_\omega(s)\right\}_{i=1}^n
$$

其中，生成模型 $G_\omega(s)$ 用变分自动编码器 (variational auto-encoder, VAE) 实现；扰动模型直接通过确定性策略梯度算法训练，目标是使函数 $Q$ 最大化:

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

除此之外，BCQ 还使用了一些实现上的小技巧。由于不是 BCQ 的重点，此处不再赘述。考虑到 VAE 不属于本书的讨论范围，并且 BCQ 的代码中有较多技巧，有兴趣的读者可以参阅 BCQ 原文，*自行实现代码*。此处介绍 BCQ 算法，一是因为它对离线强化学习的误差分析和实验很有启发性，二是因为它是无模型离线强化学习中限制策略集合算法中的经典方法。下面我们介绍另一类直接限制函数 $Q$ 的算法的代表：保守 Q-learning。

## 保守 Q-learning 算法

18.2 节已经讲到，离线强化学习面对的巨大挑战是如何减少外推误差。实验证明，外推误差主要会导致在远离数据集的点上函数的过高估计，甚至常常出现值向上发散的情况。因此，如果能用某种方法将算法中偏离数据集的点上的函数保持在很低的值，或许能消除部分外推误差的影响，这就是保守 Q-learning（conservative Q-learning，CQL）算法的基本思想。CQL 在普通的贝尔曼方程上引入一些额外的限制项，达到了这一目标。接下来一步步介绍 CQL 算法的思路。

在普通的 Q-learning中，$Q$ 的更新方程可以写为：

$$
\hat{Q}^{k+1} \leftarrow \operatorname{argmin}_Q \beta \mathbb{E}_{s \sim \mathcal{D}, a \sim \mu(a \mid s)}[Q(s, a)]+\frac{1}{2} \mathbb{E}_{(s, a) \sim \mathcal{D}}\left[\left(Q(s, a)-\hat{\mathcal{B}}^\pi \hat{Q}^k(s, a)\right)^2\right]
$$

其中， $\beta$ 是平衡因子。可以证明，上式迭代收敛给出的函数 $Q$ 在任何 $(s, a)$ 上的值都比真实值要小。不过，如果我们放宽条件，只追求 $Q$ 在 $\pi(a \mid s)$ 上的期望值 $V^\pi$ 比真实值小的话，就可以略微放松对上式的约束。一个自然的想法是，对于符合用于生成数据集的行为策略 $\pi_b$ 的数据点，我们可以认为 $Q$ 对这些点的估值较为准确，在这些点上不必限制让 $Q$ 值很小。作为对第一项的补偿，将上式改为:

$$
\hat{Q}^{k+1} \leftarrow \operatorname{argmin}_Q \beta \cdot\left(\mathbb{E}_{s \sim \mathcal{D}, a \sim \mu(a \mid s)}[Q(s, a)]-\mathbb{E}_{s \sim \mathcal{D}, a \sim \hat{\pi}_b(a \mid s)}[Q(s, a)]\right)+\frac{1}{2} \mathbb{E}_{(s, a) \sim \mathcal{D}}\left[\left(Q(s, a)-\hat{\mathcal{B}}^\pi \hat{Q}^k(s, a)\right)^2\right]
$$

将行为策略 $\pi_b$ 写为 $\hat{\pi}_b$ 是因为我们无法获知真实的行为策略，只能通过数据集中
已有的数据近似得到。可以证明，当 $\mu=\pi$ 时，上式迭代收敛得到的函数 $Q$ 虽然
不是在每一点上都小于真实值，但其期望是小于真实值的，即

$$
\mathbb{E}_{\pi(a \mid s)}\left[\hat{Q}^\pi(s, a)\right] \leq V^\pi(s) 。
$$

可以注意到，简化后式中已经不含有 $\mu$ ，为计算提供了很大方便。该化简需要进行一些数学推导，详细过程附在 18.6 节中，感兴趣的读者也可以先尝试自行推导。

至此，CQL 算法已经有了理论上的保证，但仍有一个缺陷: 计算的时间开销太大 了。当令 $\mu=\pi$ 时，在 $Q$ 迭代的每一步，算法都要对策略 $\hat{\pi}^k$ 做完整的离线策略评估来计算上式中的arg min，再进行一次策略迭代，而离线策略评估是非常耗时的。既然 $\pi$ 并非与 $Q$ 独立，而是通过 $Q$ 值最大的动作衍生出来的，那么我们完全可以用使 $Q$ 取最大值的 $\mu$ 去近似 $\pi$ ，即:

上面给出了函数 $Q$ 的迭代方法。CQL 属于直接在函数 $Q$ 上做限制的一类方法，对策略 $\pi$ 没有特殊要求，因此参考文献中分别给出了基于 DQN 和 SAC 两种框架的 CQL 算法。考虑到后者应用更广泛，且参考文献中大部分的实验结果都是基于后者得出的，这里只介绍基于 SAC 的版本。对 SAC 的策略迭代和自动调整熵正则系数不熟悉的读者，可以先阅读第 14 章的相关内容。

总结起来，CQL 算法流程如下：

- 初始化 $Q$ 网络 $Q_\theta$ 、目标 $Q$ 网络 $Q_{\theta^{\prime}}$ 和策略 $\pi_\phi$ 、熵正则系数 $\alpha$
- **for 训练次数 $t=1 \rightarrow T$ do**
- 更新熵正则系数 $\alpha_t$: $$\alpha_t \leftarrow \alpha_{t-1}-\eta_\alpha \nabla_\alpha \mathbb{E}_{s \sim \mathcal{D}, a \sim \pi_\phi(a \mid s)}\left[-\alpha_{t-1} \log \pi_\phi(a \mid s)-\alpha_{t-1} \mathcal{H}\right]$$
- 更新函数 $Q$ : $$\theta_t \leftarrow \theta_{t-1}-\eta_Q \nabla_\theta\left(\alpha \cdot \mathbb{E}_{s \sim \mathcal{D}}\left[\log \sum_a \exp \left(Q_\theta(s, a)\right)-\mathbb{E}_{a \sim \hat{\pi}_b(a \mid s)}\left[Q_\theta(s, a)\right]\right]+\frac{1}{2} \mathbb{E}_{(s, a) \sim \mathcal{D}}\left[\left(Q_\theta(s, a)-\mathcal{B}^{\pi_\phi} Q_\theta(s, a)\right)^2\right]\right)$$
- 更新策略 $\phi_t$ :$$\phi_t \leftarrow \phi_{t-1}-\eta_\pi \nabla_\phi \mathbb{E}_{s \sim \mathcal{D}, a \sim \pi_\phi(a \mid s)}\left[\alpha \log \pi_\phi(a \mid s)-Q_\theta(s, a)\right]$$
- **end for**


## CQL 代码实践

下面在倒立摆环境中实现基础的 CQL 算法。该环境在前面的章节中已出现了多次，这里不再重复介绍。首先导入必要的库。

为了生成数据集，在倒立摆环境中从零开始训练一个在线 SAC 智能体，直到算法达到收敛效果，把训练过程中智能体采集的所有轨迹保存下来作为数据集。这样，数据集中既包含训练初期较差策略的采样，又包含训练后期较好策略的采样，是一个混合数据集。下面给出生成数据集的代码，SAC 部分直接使用 14.5 节中的代码，因此不再详细解释。



下面实现本章重点讨论的 CQL 算法，它在 SAC 的代码基础上做了修改。


接下来设置好超参数，就可以开始训练了。最后再绘图看一下算法的表现。因为不能通过与环境交互来获得新的数据，离线算法最终的效果和数据集有很大关系，并且波动会比较大。通常来说，调参后数据集中的样本质量越高，算法的表现就越好。感兴趣的读者可以使用其他方式生成数据集，并观察算法效果的变化。




## 总结

本章介绍了离线强化学习的基本概念和两个与模型无关的离线强化学习算法——BCQ 和 CQL，并讲解了 CQL 的代码。事实上，离线强化学习还有一类基于模型的方法，如 model-based offline reinforcement learning （MOReL）和 model-based offline policy optimization（MOPO），本章由于篇幅原因不再介绍。这一类算法的思路基本是通过模型生成更多数据，同时通过衡量模型预测的不确定性来对生成的偏离数据集的数据进行惩罚，感兴趣的读者可以自行查阅相关资料。

离线强化学习的另一大难点是算法通常对超参数极为敏感，非常难调参。并且在实际复杂场景中通常不能像在模拟器中那样，每训练几轮就在环境中评估策略好坏，如何确定何时停止算法也是离线强化学习在实际应用中面临的一大挑战。此外，离线强化学习在现实场景中的落地还需要关注离散策略评估和选择、数据收集策略的保守性和数据缺失性等现实问题。不过无论如何，离线强化学习和模仿学习都是为了解决在现实中训练智能体的困难而提出的，也都是强化学习真正落地的重要途径。


