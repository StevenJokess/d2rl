

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-09-14 01:37:46
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-02 16:41:53
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# CQL

## 保守 Q-learning （CQL）算法

在BCQ章节，已经讲到，离线强化学习面对的巨大挑战是如何减少外推误差。实验证明，外推误差主要会导致在远离数据集的点上函数的过高估计，甚至常常出现值向上发散的情况。因此，如果能用某种方法将算法中偏离数据集的点上的函数保持在很低的值，或许能消除部分外推误差的影响，这就是保守 Q-learning（conservative Q-learning，CQL）算法的基本思想。CQL 在普通的贝尔曼方程上引入一些额外的限制项，达到了这一目标。接下来一步步介绍 CQL 算法的思路。

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

code

下面实现本章重点讨论的 CQL 算法，它在 SAC 的代码基础上做了修改。

code

接下来设置好超参数，就可以开始训练了。最后再绘图看一下算法的表现。因为不能通过与环境交互来获得新的数据，离线算法最终的效果和数据集有很大关系，并且波动会比较大。通常来说，调参后数据集中的样本质量越高，算法的表现就越好。感兴趣的读者可以使用其他方式生成数据集，并观察算法效果的变化。

## 相关推导

这里对 CQL 算法中 $\mathcal{R}(\mu)=-D_{K L}(\mu, \mathcal{U}(a))$ 的情况给出详细推导。对于一般的变量 $x \in \mathcal{D}$ 及其概率密度函数 $\mu(x)$ ，首先有归一化条件：

$$
\int_{\mathcal{D}} \mu(x) \mathrm{d} x=1
$$

把 $\mathrm{KL}$ 散度展开，由于 $\mathcal{U}(x)=1 /|\mathcal{D}|$ 是常数，因此得到

$$
\begin{aligned}
D_{K L}(\mu(x), \operatorname{Unif}(x)) & =\int_{\mathcal{D}} \mu(x) \log \frac{\mu(x)}{\mathcal{U}(x)} \mathrm{d} x \\
& =\int_{\mathcal{D}} \mu(x) \log \mu(x) \mathrm{d} x-\int_{\mathcal{D}} \mu(x) \log \mathcal{U}(x) \mathrm{d} x \\
& =\int_{\mathcal{D}} \mu(x) \log \mu(x) \mathrm{d} x-\log \frac{1}{|\mathcal{D}|} \int_{\mathcal{D}} \mu(x) \mathrm{d} x \\
& =\int_{\mathcal{D}} \mu(x) \log \mu(x) \mathrm{d} x+\log |\mathcal{D}|
\end{aligned}
$$

的常数 $\log |\mathcal{D}|$ 。综合起来，我们要求解如下的优化问题:

$$
\max _\mu \int_{\mathcal{D}} \mu(x) f(x)-\mu(x) \log \mu(x) \mathrm{d} x, \quad \text { s.t. } \int_{\mathcal{D}} \mu(x) \mathrm{d} x=1, \mu(x) \geq 0
$$

这一问题的求解要用到变分法。对于等式约束和不等式约束，引入拉格朗日乘数 $\lambda$ 和松弛函数 $\kappa(x)^2=\mu(x)-0$ ，得到相应的无约束优化问题:

$$
\max _\mu J(\mu), \quad J(\mu)=\int_{\mathcal{D}} F\left(x, \mu, \mu^{\prime}\right) \mathrm{d} x=\int_{\mathcal{D}} f(x) \mu(x)-\mu(x) \log \mu(x)+\lambda \mu(x) \mathrm{d} x
$$

其中， $\mu^{\prime}$ 是 $\frac{\mathrm{d} \mu}{\mathrm{d} x}$ 的简写。可以发现， $F\left(x, \mu, \mu^{\prime}\right)$ 事实上与 $\mu^{\prime}$ 无关，可以写为 $F(x, \mu)$ 。代入 $\mu(x)=\kappa(x)^2$ ，得到

$$
J(\kappa)=\int_{\mathcal{D}} F\left(x, \kappa^2\right) \mathrm{d} x
$$

写出欧拉-拉格朗日方程 $\frac{\partial F}{\partial \kappa}-\frac{\mathrm{d}}{\mathrm{d} x} \frac{\partial F}{\partial \kappa^{\prime}}=0 ，$ 分别计算

$$
\frac{\partial F}{\partial \kappa}=\frac{\partial F}{\partial \mu} \frac{\partial \mu}{\partial \kappa}+\frac{\partial F}{\partial \mu^{\prime}} \frac{\partial \mu^{\prime}}{\partial \kappa}=2 \kappa \frac{\partial F}{\partial \mu}+2 \kappa^{\prime} \frac{\partial F}{\partial \mu^{\prime}}=2 \kappa \frac{\partial F}{\partial \mu}
$$

由于第二项 $F$ 和 $\kappa^{\prime}$ 无关，直接等于 0 。最终拉格朗日方程简化为:

$$
2 \kappa \frac{\partial F}{\partial \mu}=0
$$

两项的乘积等于零，因此在每一点上，要么 $\alpha(x)=0$ ，即 $\mu(x)=\kappa(x)^2=0$ ，要么 $/ \mu(x)$ 满足 $\frac{\partial F}{\partial \mu}=0$ 。先来解后面的方程，直接计算得到

$$
\frac{\partial F}{\partial \mu}=f(x)+\lambda-\log \mu(x)+1=0 \quad \Rightarrow \quad \mu(x)=e^{\lambda+1} e^{f(x)}
$$

最终的解 $\mu(x)$ 应是 $\mu(x)=\kappa(x)^2=0$ 和 $\mu(x)=e^{\lambda+1} e^{f(x)}$ 两者的分段组合。由于指数函数必定大于需，为了使目标泛函取到最大值，应当全部取 $\mu(x)=e^{\lambda+1} e^{f(x)}$ 的部分。代回归一化条件，就得到原问题的最优解:

$$
\mu^*(x)=\frac{1}{Z} e^{f(x)}
$$

其中， $Z=\int_{\mathcal{D}} e^{f(x)} \mathrm{d} x$ 是归一化系数。此时，优化问题取到最大值，为

$$
\begin{aligned}
J^* & =\int_{\mathcal{D}} \mu^*(x) f(x)-\mu^*(x) \log \mu^*(x) \mathrm{d} x \\
& =\int_{\mathcal{D}} \frac{1}{Z} e^{f(x)} f(x)-\frac{1}{Z} e^{f(x)}(f(x)-\log Z) \mathrm{d} x \\
& =\frac{\log Z}{Z} \int_{\mathcal{D}} e^{f(x)} \mathrm{d} x \\
& =\log Z=\log \int_{\mathcal{D}} e^{f(x)} \mathrm{d} x
\end{aligned}
$$

对照原迭代方程，将 $f(x)$ 改为 $f(a)=\mathbb{E}_{s \sim \mathcal{D}}[Q(s, a)]$ ，积分改为在动作空间上求和，上式就变为:

$$
J^*=\log \sum_a \exp \left(\mathbb{E}_{s \sim \mathcal{D}}[Q(s, a)]\right)
$$

此处的期望是对 $s$ 而言的，与 $a$ 无关，因此可以把期望移到最前面，得到

$$
J^*=\mathbb{E}_{s \sim \mathcal{D}}\left[\log \sum_a \exp (Q(s, a))\right]
$$

至此，式中已经不含 $\mu$ ，完成化简。

[1]: https://hrl.boyuai.com/chapter/3/%E7%A6%BB%E7%BA%BF%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0/#186-%E6%89%A9%E5%B1%95%E9%98%85%E8%AF%BB
[2]: TODO:https://zhuanlan.zhihu.com/p/496103195
