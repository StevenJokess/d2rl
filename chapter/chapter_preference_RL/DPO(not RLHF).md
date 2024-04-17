

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-09-12 16:00:42
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-12 16:05:38
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# DPO(not RLHF)

## 不同于RLHF

大语言模型目前的调优策略一般是在大规模的无监督训练之后，通过人类偏好的策略将期望的行为融入到语言模型中。虽然最直接的偏好学习方法是基于高质量的示范进行监督微调，但最成功的方法类别是通过人类（或 AI）反馈进行强化学习，即 RLHF。

RLHF 方法将奖励模型适应到人类偏好的数据集上，然后使用强化学习优化语言模型策略，以产生被分配高奖励的回应，同时不过度偏离原始模型。RLHF 和奖励模型的迭代训练，语言模型可以不断改进自己的文本生成能力。然而，RLHF 的流程远比监督学习复杂得多，其中涉及到训练多个语言模型。同时，该方法还需要相当大的计算成本，用来在训练过程中从语言模型策略中抽样。此外，RLHF 主要受到了数据的限制。它需要大量的人工反馈和偏好数据，不仅会消耗大量的人力资源，还可能会引入人为的偏见。

 ｜RLHF vs DPO optimizing for human preferences

为了寻求更简单有效的大语言模型优化策略，斯坦福大学的团队提出了一种新的算法 Direct Preference Optimization（DPO）。该方法可以通过直接优化语言模型来实现对其行为的精确控制，而无需使用复杂的强化学习。DPO 将奖励函数和最优策略之间的映射联系起来，从而把约束奖励最大化问题转化为一个单阶段的策略训练问题。这种算法不仅不用拟合奖励模型，还避免了在微调过程中从语言模型中采样或调整重要超参数的需要。实验结果表明，DPO 算法可以与现有 RLHF 方法一样有效地从人类偏好中学习，甚至在某些任务中表现更好，比如情感调节、摘要和单轮对话。

DPO 是一种隐式优化策略，与现有的 RLHF 方法具有相同的目标，但更容易实现且易于训练。DPO 虽然增加了首选回复与其他回复之间的相对对数概率，但单纯的相对概率作为目标会引发模型退化。为了解决这个问题，DPO 使用了一个动态的权重表示每个示例回复的重要性。与现有的算法一样，DPO 同样依赖一个理论上的偏好模型，用于衡量给定的奖励函数和实际的偏好数据之间的对齐程度。然而与其他算法不同的是，DPO 使用变量的变化将偏好损失直接定义为一个策略函数。因此，DPO 能根据给定的偏好数据和模型回复，用简单的二进制交叉熵作为目标进行策略优化。

DPO 针对人类偏好进行了优化，同时避免了强化学习。在大语言模型微调中，现有的基于人类反馈的方法都会首先将奖励模型拟合到一个包含提示和人类偏好的数据集上，然后使用对比学习来找到一个策略最大化学习到的奖励。相比之下，DPO 只通过简单的分类目标，就能直接针对最满足人类偏好的策略进行优化，无需明确的奖励函数或者强化学习。

相关资料：

论文地址：[Direct Preference Optimization: Your Language Model is Secretly a Reward Model](https://arxiv.org/pdf/2305.18290.pdf)

##

提问：DPO公式是由PPO的objective公式推导过来的，为什么DPO是off-policy算法，而PPO是on-policy算法，到底哪一步推导出了问题？

回答：在DPO公式推导中，由目标公式：

$max_{\pi_\theta}E_{x \sim D, y \sim \pi_{theta}}[r(x, y)] - \beta D_{KL}[\pi_\theta(y|x) || \pi_{ref}(y|x)]$

推导出optimal policy

$\pi_r(y | x) = \frac{1}{Z(x)}\pi_{ref}(y|x)exp(\frac{1}{\beta}r(x, y))$

在公式中其实 $\pi_{ref}$ 应该是随着模型更新而一直改变的，但是真正实现的时候一般使用 $\pi_{sft}$ 代替。那么就导致了DPO从on-policy变成了off-policy的方法。DPO面临着RL领域经典的state distribution shift的问题，从而效果会不如PPO。除此之外由于DPO中 $\pi_{ref}$ 和 $\pi_{sft}$ 有KL散度的限制，所以state distribution shift的问题不会像传统RL中那么大，所以整体上还是work的。

补：经修正，相比于不加KL散度或者传统bandit算法算是分布差异小，但整体分布差异仍然很大（约25左右），如图：[2]


[1]: https://my.oschina.net/u/4209276/blog/10088196
[2]: https://zhuanlan.zhihu.com/p/685948009

TODO: 【手撕RLHF-DPO】step-by-step公式推导及实验分析 - 小冬瓜AIGC的文章 - 知乎
https://zhuanlan.zhihu.com/p/692991235
