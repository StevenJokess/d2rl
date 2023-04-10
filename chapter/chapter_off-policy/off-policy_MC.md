

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-20 00:47:39
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-09 11:54:01
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 异策略（off-policy）&同策略（on-policy）

## 定义

- **异策略**（off-policy）：在执行【策略评估】和【策略提升】的时候，使用的不是相同的策略。off表示在策略评估时，我们使用的策略（我们叫做behavior policy）【偏离】了我们的目标策略（target policy）。
- **同策略**（on-policy）：在执行【策略评估】和【策略提升】的时候，使用的是相同的策略。

## 重要性采样（Importance Sampling）

考虑一个场景，假如正在尝试计算函数 $f(x)$ 的期望值，其中 $x \sim p(x)$ 服从某种分布。则对 $E(f(x))$ 有以下估计:

$$
E_{x \sim p}[f(x)]=\int f(x) p(x) d x \approx \frac{1}{n} \sum_i f\left(x_i\right)
$$

蒙特卡洛抽样方法是简单地从分布 $p(x)$ 中抽出 $x$ ，然后取所有样本的平均值来得到期望值的估 计。那么问题来了，如果 $p(x)$ 非常难取样怎么办? 是否能够根据一些已知的、容易抽样的分布来 估计期望值？

答案是肯定的。公式的一个简单转换就可以做到

$$
E_{x \sim p}[f(x)]=\int f(x) p(x) d x=\int f(x) \frac{p(x)}{q(x)} q(x) d x=E_{x \sim q}\left[f(x) \frac{p(x)}{q(x)}\right]
$$

其中 $x$ 从分布 $q(x)$ 中采样， $q(x)$ 不应为 0。通过这种方式，估计期望能够从另一个分布 $q(x)$ 中 采样， $p(x) / q(x)$ 是称为采样率或采样权重，它作为校正权重以抵消来自不同分布的概率采样。

该公式称为普通重要性采样（ordinary importance sampling）。

### 重要性采样的缺陷

虽然重要性采样保证了期望的一致，但是这里来计算一下方差是否一致

方差的计算：

$$
\operatorname{Var}[X]=E\left[X^2\right]-(E[X])^2
$$

分别计算方差：

$$
\begin{gathered}
\operatorname{Var}_{x \sim p}[f(x)]=E_{x \sim p}\left[f(x)^2\right]-\left(E_{x \sim p}[f(x)]\right)^2 \\
\operatorname{Var}_{x \sim q}\left[f(x) \frac{p(x)}{q(x)}\right]=E_{x \sim q}\left[\left(f(x) \frac{p(x)}{q(x)}\right)^2\right]-\left(E_{x \sim q}\left[f(x) \frac{p(x)}{q(x)}\right]\right)^2 \\
=E_{x \sim p}\left[f(x)^2 \frac{p(x)}{q(x)}\right]-\left(E_{x \sim p}[f(x)]\right)^2
\end{gathered}
$$

可以发现两者虽然期望相等但方差并不一致，甚至方差有可能无穷大（unbound）。为了解决普通重要性采样的方差无穷大问题，我们会经常使用另一种【加权重要性采样（weighted importance sampling）】

### 加权重要性采样

令公式（1）中 $\rho(x_i^`)=\frac{p(x_i^`)}{q(x_i^`)}$ ，期望公式改为：

$\hat E[f]=\frac{\sum^m_{i=1}[\rho(x_i^`)f(x_i^`)]}{\sum^m_{i=1}\rho(x_i^`)}$

## 蒙特卡洛的异策略形式

我们令执行策略评估时的策略为 $\mu (a_t|s_t)$ ，目标策略，也就是执行策略提升时候的策略为 $\pi(a_t|s_t)$ ，对于一条给定的轨迹 $[s_0,a_0,r_0,s_1,a_1,r_1,...,s_T]$ ，使用策略 $\pi$ 和策略 $\mu$ 产生该轨迹的概率分别是

P^{\pi}=\prod_{t=0}^{T-1}\pi(a_t|s_t)p(s_{t+1}|s_t,a_t)\tag{3}

P^{\mu}=\prod_{t=0}^{T-1}\mu(a_t|s_t)p(s_{t+1}|s_t,a_t)\tag{4}

由公式（3）（4）可得

\begin{align} \rho(T)&=\frac{\prod_{t=0}^{T-1}\pi(a_t|s_t)p(s_{t+1}|s_t,a_t)}{\prod_{t=0}^{T-1}\mu(a_t|s_t)p(s_{t+1}|s_t,a_t)}\\ &=\frac{\prod_{t=0}^{T-1}\pi(a_t|s_t)}{\prod_{t=0}^{T-1}\mu(a_t|s_t)} \end{align}\tag{5}

可以看到，状态转移概率被消掉了

于是，我们对Q函数期望的估计更新为下面的形式

由公式1的普通重要性采样得：

\begin{align} \bar q_N(s,a) = \frac{1}{N}\sum^N_{i=1}\rho_iq_i(s,a) \end{align}\tag{6}

或者，由公式2的加权重要性采样得：

\begin{align} \bar q_N(s,a) = \frac{\sum^N_{i=1}[\rho_iq_i(s,a)]}{\sum^N_{i=1}\rho_i} \end{align}\tag{7}

由于加权的重要性采样应用更广泛，下面推导重要性采样公式的递增形式：

加权重要性采样的递增形式（Incremental Implementation Of Weighted Importance Sampling)
令C_N= \sum^N_{i=1}\rho_i=C_{N-1}+\rho_N\\

$$
\begin{align} q_N(s,a) &= \frac{\sum^N_{i=1}[\rho_iq_i(s,a)]}{C_N}\\ &=\frac{\sum^{N-1}_{i=1}\rho_i}{C_N}\frac{\sum_i^{N-1}[\rho_iq_i(s.a)]+\rho_Nq_N(s,a)}{\sum^{N-1}_{i=1}\rho_i}\\ &=\frac{C_N-\rho_N}{C_N}q_{N-1}(s,a) + \frac{\rho_Nq_N(s,a)}{C_N}\\ &=q_{N-1}(s,a)+\frac{\rho_N}{C_N}[q_N(s,a)-q_{N-1}(s,a)] \end{align}\tag{8}
$$

并且注意，由公式（5）， $\rho_N = \rho_{N-1}\frac{\pi(a_N|s_N)}{\mu(a_N|s_N)}$ ，也是一种递归的表达。

以下这里不太理解

要注意的是，由于我们的策略是确定性策略，也就是 $\pi(a_t|s_t)$ 对于 $a_t=\pi(a_t|s_t)$ 始终为1。于是上式 $\rho$ 的迭代形式简化为：

$\rho_N = \rho_{N-1}\frac{1}{\mu(a_N|s_N)}$

## 异策略蒙特卡洛（递增形式）算法（every visit）

- 初始化:
  - $Q(s, a) \leftarrow$ arbitrary
  - $C(s, a) \leftarrow 0$
  - $\mu(a \mid s) \leftarrow$ an arbitrary soft behavior policy
  - $\pi(a \mid s) \leftarrow$ an arbitrary target policy
- for $\min [1,2,3, \ldots], \mathrm{m}$ 是总的迭代次数
  - 与环境交互获取状态-动作序列: $\left[s_0, a_0, r_0, \ldots, s_T, a_T, r_T, s_{T+1}\right]$
  - $G(S, A) \leftarrow 0$
  - $\rho \leftarrow 1$
  - for $t=T, T-1, \ldots, 0$ :
    - $g\left(s_t, a_t\right)=\gamma g\left(s_t, a_t\right)+r_t$
    - $C\left(s_t, a_t\right) \leftarrow C\left(s_t, a_t\right)+\rho$
    - $q\left(s_t, a_t\right) \leftarrow q\left(s_t, a_t\right)+\frac{\rho}{C\left(s_t, a_t\right)}\left[g\left(s_t, a_t\right)-q\left(s_t, a_t\right)\right]$ 对应公式 (8)
    - $\rho \leftarrow \rho \frac{1}{\mu\left(a_t \mid s_t\right)}$
    - $\pi\left(s_t\right) \leftarrow \operatorname{argmax}_a q\left(s_t, a_t\right)$ ，由于序列已经采样好，所以策略在for循环内更新更 在for循环外再加一层for更新策略没有区别

[1]: https://zhuanlan.zhihu.com/p/360265418

code:https://github.com/CHH3213/chhRL/blob/master/02-chh_MonteCarlo/MC_OffPolicy.py
