

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-02-23 20:10:35
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-02 16:34:34
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 基于模型的策略优化

## 简介

第 16 章介绍的 PETS 算法是基于模型的强化学习算法中的一种，它没有显式构建一个策略（即一个从状态到动作的映射函数）。回顾一下之前介绍过的 Dyna-Q 算法，它也是一种基于模型的强化学习算法。但是 Dyna-Q 算法中的模型只存储之前遇到的数据，只适用于表格型环境。而在连续型状态和动作的环境中，我们需要像 PETS 算法一样学习一个用神经网络表示的环境模型，此时若继续利用 Dyna 的思想，可以在任意状态和动作下用环境模型来生成一些虚拟数据，这些虚拟数据可以帮助进行策略的学习。如此，通过和模型进行交互产生额外的虚拟数据，对真实环境中样本的需求量就会减少，因此通常会比无模型的强化学习方法具有更高的采样效率。本章将介绍这样一种算法——MBPO 算法。

## MBPO 算法

基于模型的策略优化 (model-based policy optimization，MBPO）算法是加州大学伯克利分校的研究员在 2019 年的 NeurIPS 会议中提出的。随即 MBPO 成为深度强化学习中最重要的基于模型的强化学习算法之一。

MBPO 算法基于以下两个关键的观察： (1) 随着环境模型的推演步数变长，模型累积的复合误差会快速增加，使得环境模型得出的结果变得很不可靠； (2) 必须要权衡推演步数增加后模型增加的误差带来的负面作用与步数增加后使得训练的策略更优的正面作用，二者的权衡决定了推演的步数。

MBPO 算法在这两个观察的基础之上，提出只使用模型来从之前访问过的真实状态开始进行较短步数的推演，而非从初始状态开始进行完整的推演。这就是 MBPO 中的分支推演（branched rollout）的概念，即在原来真实环境中采样的轨迹上面推演出新的“短分支”，如图 17-1 所示。这样做可以使模型的累积误差不至于过大，从而保证最后的采样效率和策略表现。


图17-1 分支推演示意图

MBPO 与第 6 章讲解的经典的 Dyna-Q 算法十分类似。Dyna-Q 采用的无模型强化学习部分是 Q-learning，而 MBPO 采用的是 SAC。此外，MBPO 算法中环境模型的构建和 PETS 算法中一致，都使用模型集成的方式，并且其中每一个环境模型的输出都是一个高斯分布。接下来，我们来看一下 MBPO 的具体算法框架。MBPO 算法会把真实环境样本作为分支推演的起点，使用模型进行一定步数的推演，并用推演得到的模型数据用来训练模型。


分支推演的长度 $k$ 是平衡样本效率和策略性能的重要超参数。接下来我们看看 MBPO 的代码，本章最后会给出关于 MBPO 的理论推导，可以指导参数 $k$ 的选取。

## MBPO 代码实践

code

对于不同的环境，我们需要设置不同的参数。这里以 OpenAI Gym 中的 Pendulum-v0 环境为例，给出一组效果较为不错的参数。读者可以试着自己调节参数，观察调节后的效果。

code

可以看到，相比无模型的强化学习算法，基于模型的方法 MBPO 在样本效率上要高很多。虽然这里的效果可能不如 16.3 节提到的 PETS 算法优秀，但是在许多更加复杂的环境中（如 Hopper 和 HalfCheetah），MBPO 的表现远远好于 PETS 算法。

## 小结

MBPO 算法是一种前沿的基于模型的强化学习算法，它提出了一个重要的概念——分支推演。在各种复杂的环境中，作者验证了 MBPO 的效果超过了之前基于模型的方法。MBPO 对于基于模型的强化学习的发展起着重要的作用，不少之后的工作都是在此基础上进行的。

除了算法的有效性，MBPO 的重要贡献还包括它给出了关于分支推演步数与模型误差、策略偏移程度之间的定量关系，进而阐明了什么时候我们可以相信并使用环境模型，什么样的环境导出的最优分支推演步数为 0，进而建议不使用环境模型。相应的理论分析在 17.5 节给出。

## 拓展阅读：MBPO 理论分析

### 性能提升的单调性保障

基于模型的方法往往是在环境模型中提升策略的性能，但这并不能保证在真实环境中策略性能也有所提升。因此，我们希望模型环境和真实环境中的结果的差距有一 定的限制，具体可形式化为:

$$
\eta[\pi] \geq \hat{\eta}[\pi]-C
$$

其中， $\eta[\pi]$ 表示策略在真实环境中的期望回报，而 $\hat{\eta}[\pi]$ 表示策略在模型环境中的期望回报。这一公式保证了在模型环境中提高策略性能超过 $C$ 时，就可以在真实环境中 取得策略性能的提升。

在 MBPO 中，根据泛化误差和分布偏移估计出这样一个下界:

$$
\eta[\pi] \geq \hat{\eta}[\pi]-\left[\frac{2 \gamma r_{\max }\left(\epsilon_m+2 \epsilon_\pi\right)}{(1-\gamma)^2}+\frac{4 r_{\max } \epsilon_\pi}{(1-\gamma)}\right],
$$

其中， $\epsilon_m=\max _t \mathbb{E}_{s \sim \pi_{D, t}}\left[D_{T V}\left(p\left(s^{\prime}, r \mid s, a\right) \| p_\theta\left(s^{\prime}, r \mid s, a\right)\right)\right]$ 刻画了模型泛化误差，而 $\epsilon_\pi=\max _s D_{T V}\left(\pi \| \pi_D\right)$ 刻画了当前策略 $\pi$ 与数据收集策略 $\pi_D$ 之间的策略转移 (policy shift)。

### 模型推演长度

在上面的公式里，如果模型泛化误差很大，就可能不存在一个使得推演误差 $C$ 最小的正数推演步数 $k$ ，进而无法使用模型。因此作者提出了分支推演（branched rollout) 的办法，即从之前访问过的状态开始进行有限制的推演，从而保证模型工作时的泛化误差不要太大。

如果我们使用当前策略 $\pi_t$ 而非本轮之前的数据收集策略 $\pi_{D, t}$ 来估计模型误差（记为 $\epsilon_m^{\prime}$ ），那么有:

$$
\epsilon_m^{\prime}=\max _t \mathbb{E}_{s \sim \pi_t}\left[D_{T V}\left(p\left(s^{\prime}, r \mid s, a\right) \| p_\theta\left(s^{\prime}, r \mid s, a\right)\right)\right]
$$

并且对其进行线性近似:

$$
\epsilon_m^{\prime} \approx \epsilon_m+\epsilon_\pi \frac{\mathrm{d} \epsilon_m^{\prime}}{\mathrm{d} \epsilon_\pi}
$$

结合上 $k$ 步分支推演，我们就可以得到一个新的策略期望回报界:



$$
\eta[\pi] \geq \eta^{\text {branch }}[\pi]-2 r_{\max }\left[\frac{\gamma^{k+1} \epsilon_\pi}{(1-\gamma)^2}+\frac{\gamma^k \epsilon_\pi}{(1-\gamma)}+\frac{k}{1-\gamma} \epsilon_{m^{\prime}}\right]
$$

其中， $\eta^{\text {branch }}[\pi]$ 表示使用分支推演的方法得到的策略期望回报。通过以上公式，我们就可以得到理论最优的推演步长，即：

$$
k^*=\operatorname{argmin}_k\left[\frac{\gamma^{k+1} \epsilon_\pi}{(1-\gamma)^2}+\frac{\gamma^k \epsilon_\pi}{(1-\gamma)}+\frac{k}{1-\gamma} \epsilon_{m^{\prime}}\right]
$$

在上式中可以看到，对于 $\gamma \in(0,1)$ ，当推演步数变大时， $\frac{\gamma^{k+1} \epsilon_\pi}{(1-\gamma)^2}+\frac{\gamma^k \epsilon_\pi}{(1-\gamma)}$ 变小，而 $\frac{k}{1-\gamma} \epsilon_{m^{\prime}}$ 变大。更进一步，如果 $\epsilon_m^{\prime}$ 足够小的话，由 $\epsilon_m^{\prime} \approx \epsilon_m+\epsilon_\pi \frac{\mathrm{d} \epsilon_m^{\prime}}{\mathrm{d} \epsilon_\pi}$ 的关系可知， 如果 $\frac{\mathrm{d} \epsilon_m^{\prime}}{\mathrm{d} \epsilon_\pi}$ 足够小，那么最优推演步长 $k$ 就是为正，此时分支推演（或者说使用基于模型的方法）就是一个有效的方法。

$\mathrm{MBPO}$ 论文中展示了在主流的机器人运动环境 Mojoco 的典型场景中， $\frac{\mathrm{d} \epsilon_m^{\prime}}{\mathrm{d} \epsilon_\pi}$ 的数量级非常小，大约都在 $\left[10^{-4}, 10^{-2}\right]$ 区间，而且可以看出它随着训练数据的增多的而不 断下降，说明模型的泛化能力逐渐增强，而我们对于推演步长为正数的假设也是合理的。但要知道，并不是所有的强化学习环境都可以有如此小的 $\frac{\mathrm{d} \epsilon_m^{\prime}}{\mathrm{d} \epsilon_\pi}$ 。例如在高随 机性的离散状态环境中，往往环境模型的拟合精度较低，以至于 $\frac{\mathrm{d} \epsilon_m^{\prime}}{\mathrm{d} \epsilon_\pi}$ 较大，此时使用基于分支推演的方法效果有限。




[1]: https://hrl.boyuai.com/chapter/3/%E5%9F%BA%E4%BA%8E%E6%A8%A1%E5%9E%8B%E7%9A%84%E7%AD%96%E7%95%A5%E4%BC%98%E5%8C%96/
