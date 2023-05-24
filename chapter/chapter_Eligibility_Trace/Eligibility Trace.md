

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-17 05:15:29
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-05-25 00:09:44
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 资格迹

https://chengfeng96.com/blog/2020/02/24/%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0%EF%BC%88%E4%B8%83%EF%BC%89-%E8%B5%84%E6%A0%BC%E8%BF%B9/

### $TD(\lambda)$

$TD(\lambda)$ 是用于解决在n步TD方法中选择n值的问题，同时在不增加计算复杂度的情况下综合考虑所有步数的预测。

引入了参数 $\lambda$ 是为了便于调超参， $\lambda \in [0,1]$。通过调整 $\lambda$ 的取值可以在不同的任务和环境中平衡延迟和学习速度。
- 当  $\lambda$ 较小时，算法更加关注近期的经验，能够更快地更新价值函数，更好适应环境变化；
- 当 $\lambda$ 较大时，算法更加关注长期的经验，能够更好地考虑长期奖励。

在n步TD方法中选择n值的问题在传统的n步TD方法中，需要选择一个固定的n值来作为更新的步数。然而，这样的方法存在一个问题，即对于不同的环境和任务，不同的n值可能会导致性能差异。
- 如果选择的n值较小，可能会导致算法在处理长期依赖关系时预测不准确；
- 而选择的n值较大，可能会导致算法在更新价值函数前需等待时间长，降低学习效率。

任意一个 $n$-步收获的权重被设计为 $(1-\lambda) \lambda^{n-1}$ ，如图所示。通过这样的权重设计，可以得到sampling时，按随时间衰减的比例累加所有步的回报，得G(t)，其计算公式变为

$$
G_t^\lambda=(1-\lambda) \sum_{n=1}^{\infty} \lambda^{n-1} G_t^{(n)}
$$

首先随步数n的增加，系数成指数级衰减，距离当前状态越远，作用就应该越小。[3]

其次所有步的回报系数和为1，因为：

$$
\begin{aligned} \sum\limits_{n = 1}^\infty(1-\lambda)\lambda^{n-1}&=\sum\limits_{n = 1}^\infty\lambda^{n-1}-\lambda^{n} \\ &=1-\lambda+\lambda-\lambda^2+...+\lambda^n \\ &=1-\lambda^n \\ &\rightarrow 1 \end{aligned}
$$

![TD(lambda)图](../../img/TD(lambda).png)

$TD(\lambda)$ 的价值函数的迭代公式为：

$$
\begin{aligned} &V(S_t)\gets V(S_t)+\alpha(G_t^{(\lambda)}(S_t)-V(S_t)) \\ &Q(S_t,A_t)\gets Q(S_t,A_t)+\alpha(G_t^{(\lambda)}(S_t)-Q(S_t,A_t)) \end{aligned}
$$

### SARSA ($\lambda$)

SARSA($\lambda$)算法是在SARSA算法的基础上引入了「资格迹（eligibility trace）」，直观上解释就是让算法对于经历过的状态有一定的记忆性，如sutton书中所述，资格迹对所获取的轨迹起到了短期记忆的效果。从下图可以直观看出，经历过的状态不再是经历过之后直接删除，而是存在一个「平滑的衰减过渡」，保存了一定程度上的信息。也可以说，该算法增加了距离目标点最近的状态的权重，从而加快算法的收敛性（直观意义上的说法）。[2]

[2]: https://yuancl.github.io/2019/01/28/rl/%E4%B8%8D%E5%9F%BA%E4%BA%8E%E6%A8%A1%E5%9E%8B%E7%9A%84%E9%A2%84%E6%B5%8B/
[3]: https://www.zhihu.com/question/480946038/answer/2374280417
[4]:
