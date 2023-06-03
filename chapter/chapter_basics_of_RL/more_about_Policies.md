

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-06-02 23:16:48
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-06-03 23:12:41
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 策略（Policies）




在 DRL 中，我们处理参数化策略（parameterized policies）：其输出是依赖于一组参数（例如神经网络的权重和偏差）的 可计算函数的策略，我们可以通过一些优化算法调整这些参数以改变行为。

我们经常用 $\theta$ 或 $\phi$ 来表示这种策略的参数，然后将其作为下标写在策略符号上以突出连接:

$$
\begin{aligned}
& a_t=\mu_\theta\left(s_t\right) \\
& a_t \sim \pi_\theta\left(\cdot \mid s_t\right) .
\end{aligned}
$$

## 确定性策略

示例：确定性策略。以下是使用 torch.nn 包在 PyTorch 中为连续动作空间构建简单确定性策略的代码片段：

```py
pi_net = nn.Sequential(
              nn.Linear(obs_dim, 64),
              nn.Tanh(),
              nn.Linear(64, 64),
              nn.Tanh(),
              nn.Linear(64, act_dim)
            )
```

这将构建一个多层感知器 (MLP) 网络，其中包含两个大小为 64 的隐藏层和 \tanh 激活函数。如果 obs 是包含一批观察值的 Numpy 数组， pi_net 可用于获取一批操作，如下所示：

```py
obs_tensor = torch.as_tensor(obs, dtype=torch.float32)
actions = pi_net(obs_tensor)
```

### 分类策略

分类策略就像对离散动作的分类器。您为分类策略构建神经网络的方式与为分类器构建的方式相同：输入是观察结果，然后是一些层（可能是卷积层或密集连接层，具体取决于输入的类型），然后您有最后一个线性层为您提供每个动作的 logits，然后是 softmax 将 logits 转换为概率。

采样。给定每个动作的概率，PyTorch 和 Tensorflow 等框架具有用于采样的内置工具。例如，请参阅 PyTorch 、 torch.multinomial 、 tf.distributions.Categorical 或 tf.multinomial 中分类分布的文档。

对数似然。将最后一层概率表示为。它是一个向量，有多少个条目就有多少个动作，所以我们 可以将动作视为向量的索引。然后可以通过对向量进行索引来获得动作 $a$ 的对数似然:

$$
\log \pi_\theta(a \mid s)=\log \left[P_\theta(s)\right]_a
$$

### 对角高斯策略

多元高斯分布（或多元正态分布，如果您愿意）由均值向量 $\mu$ 和协方差矩阵 $\Sigma$ 描述。对角高斯分布是一种特殊情况，其中协方差矩阵仅在对角线上具有条目。因此，我们可以用向量来表示它。

对角线高斯策略总是有一个神经网络，将观察映射到平均动作，$\mu_\theta(s)$。协方差矩阵通常有两种不同的表示方式。

第一种方式：有一个单一的对数标准差向量 ，它不是状态的函数：它们是独立的参数。 （您应该知道：我们的 VPG、TRPO 和 PPO 实现就是这样做的。）

第二种方式：有一个神经网络从状态映射到对数标准差，$\log \sigma_\theta(s)$。它可以选择与平均网络共享一些层。

采样。给定平均动作和标准差，以及来自球形高斯 () 的噪声向量 z ，可以使用以下公式计算动作样本

$a = \mu_{\theta}(s) + \sigma_{\theta}(s) \odot z$

其中 $\odot$ 表示两个向量的元素乘积。标准框架具有生成噪声向量的内置方法，例如 torch.normal 或 tf.random_normal 。或者，您可以构建分布对象，例如通过 torch.distributions.Normal 或 tf.distributions.Normal ，并使用它们生成样本。 （后一种方法的优点是这些对象还可以为您计算对数似然。）

对数似然。 $k$ 维度动作 $a$ 的对数似然，对于具有均值和标准差的对角线高斯，由下式给出

$$
\log \pi_\theta(a \mid s)=-\frac{1}{2}\left(\sum_{i=1}^k\left(\frac{\left(a_i-\mu_i\right)^2}{\sigma_i^2}+2 \log \sigma_i\right)+k \log 2 \pi\right) .
$$



## 对比

- 确定性策略的优点在于：需要采样的数据少，算法效率高随机策略的梯度计算公式:

$$
\nabla_\theta J\left(\pi_\theta\right)=E_{s \sim \rho} \pi_{, a \sim \pi_\theta}\left[\nabla_\theta \log \pi_\theta(a \mid s) Q^\pi(s, a)\right]
$$

其中的 $Q^\pi(s, a)$ 是状态-行为值函数。可见，策略梯度是关于状态和动作的期望，在求期望时，需要对状态分布和动作分布求积分，需要在状态空间和动作空间内大量采样，这样求出来的均值才能近似期望。

而确定性策略的动作是确定的，所以，如果存在确定性策略梯度，其求解不需要在动作空间采样，所以需要的样本 数更少。对于动作空间很大的智能体（如多关节机器人），动作空间维数很大，有优势。

- 随机策略的优点: 随机策略可以将探索和改善集成到一个策略中

随机策略本身自带探索，可以通过探索产生各种数据（有好有坏），好的数据可以让强化学习算法改进当前策略。

而确定性策略给定状态和策略参数时，动作是固定的，所以无法探索其他轨迹或者访问其他状态。

确定性策略无法探索环境，所以需要通过异策略（off-policy）方法来进行学习，即行动策略和评估策略不是同一个策略。行动策略采用随机策略，而评估策略要用确定性策略。而整个确定性策略的学习框架采用的是AC方法。




[1]: https://spinningup.openai.com/en/latest/spinningup/rl_intro.html#policies
[2]: https://daiwk.github.io/posts/rl-stepbystep-chap9.html
