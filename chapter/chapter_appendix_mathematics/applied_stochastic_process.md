

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-02 02:10:07
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-04 17:02:28
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 应用随机过程

随机过程为强化学习的序贯决策问题提供了模型分析基础。

## 随机过程

- 随机变量是将随机试验结果咉射为数值的函数，随机变量的数学表示为 : $X: \Omega \rightarrow R$
- 随机过程可简单地理解为随时间变化的随机变量，其取值还时间而变化。

例如，某商店在某一时间接待顾客人数是随机变量 $X$ ，在不同的时间 $t ， X(t)$ 的取值不同，这里的 $X(t)$ 就是一个随机过程。当给定一个具体时间 $t_0$ 时，随机过程 $X(t)$ 退化为一个随机变量。

也就是说，以时间 $t$ 为自变量的随机函数 $X(t)$ 就是随机过程，随机过程的数学表示头 $\{X(t), t \in T\}$ 。

### 基本概念

给定概率空间 $(\Omega, \mathcal{F}, P)$ 和实数参数集 $\boldsymbol{T}$ ，对任意一个给定的 $t \in T, X(t, \omega)$ 是定义在概率空间 $(\Omega, \mathcal{F}, P)$ $\{X(t), t \in T\}$

- 在实际应用中，实数参数集T通常为时间，下面统一将参数 $\mathrm{t}$ 称为时间。
- 若固定时间 $t_0 \in T, X\left(t_0, \omega\right)$ 就是一个定义在概率空间 $(\Omega, \mathcal{F}, P)$ 上的随机变量。
- 若固定状态 $\omega_0 \in \Omega, X\left(t, \omega_0\right)$ 就是一个关于参数 $t \in T$ 的函数，通常被称为样本函数。
- 根据参数集和状态空间的不同类型，随机过程可以以分为以下四类：

1. 离散时间离散状态随机过程；
1. 离散时间连续状态随机过程；
1. 连续时间离散状态随机过程；
1. 连续时间连续状态随机过程。

除了上述分类方法外，随皈过程还可以根据其统计特征或概率特征进行分类，比如平稳程，独 立增量过程，平稳增量过程和马尔科夫过程等。

### 基本类型

#### 严平稳过程

随机过程 $\{X(t), t \in T\}, t_0, \cdots, t_n \in T$ 且 $t_0<\cdots<t_n$ ，若对任意实数 $h, t_0+h, \cdots, t_n+h \in T$, $\left(X\left(t_0\right), \cdots, X\left(t_n\right)\right)$ 与 $\left(X\left(t_0+h\right), \cdots, X\left(t_n+h\right)\right)$ 具有相同的联合分布函数，即：

$$
\begin{aligned}
F\left(t_0, \cdots, t_n ; x_0, \cdots, x_n\right) & =P\left\{X\left(t_0\right) \leq x_0, \cdots, X\left(t_n\right) \leq x_n\right\} \\
& =P\left\{X\left(t_0+h\right) \leq x_0, \cdots, X\left(t_n+h\right) \leq x_n\right\} \\
& =F\left(t_0+h, \cdots, t_n+h ; x_0, \cdots, x_n\right)
\end{aligned}
$$

则称 $\{X(t), t \in T\}$ 为严平稳过程，或狭义平稳过程。
当随机过程为严平稳过程时，其有限维分布不䃛时间的隹移而发生变化。

#### 平稳增量过程

随机过程 $\{X(t), t \in T\}, t_0, \cdots, t_n \in T$ 且 $t_0<\cdots<t_n$ ，若对任意 $t_1, t_2, X\left(t_1+h\right)-X\left(t_1\right)$ 与 $X\left(t_2+h\right)-X\left(t_2\right)$ 具有相同的分布函数，则称 $\{X(t), t \in T\}$ 为平稳增量过程。

#### 独立增量过程

随机过程 $\{X(t), t \in T\}, t_0, \cdots, t_n \in T$ 且 $t_0<t_1<\cdots<t_{n-1}<t_n$ ，若对任意正整数 $\mathbf{n}$ ，随机 变量 $X\left(t_1\right)-X\left(t_0\right), X\left(t_2\right)-X\left(t_1\right), \cdots, X\left(t_n\right)-X\left(t_{n-1}\right)$ 是相互独立的，则称 $\{X(t), t \in T\}$ 为独立增量过程。

若随机过程 $\{X(t), t \in T\}$ 同时满足平稳增量和独立增量的条件，则称其为平稳独立增量过程。


