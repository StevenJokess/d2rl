

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-02 02:10:07
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-04 19:09:44
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

## 马尔可夫过程

马尔科夫过程是一个具有马尔科夫性 (无后效性) 的随机过程。其末来的状态，只与当前状态有关，而与过去的所有状态无关。

随机过程 $\{X(t), t \in T\}, t_0, \cdots, t_n \in T$ 且 $t_0<\cdots<t_n$ ，若对任意自然数 $\mathbf{n}$ ，随机过程 $\{X(t), t \in T\}$ 满足如下马尔科夫性：

$$
\begin{aligned}
& P\left\{X\left(t_{n+1}\right) \leq x_{n+1} \mid X\left(t_n\right)=x_n, X\left(t_{n-1}\right)=x_{n-1}, \cdots, X\left(t_0\right)=x_0\right\}= \\
& P\left\{X\left(t_{n+1}\right) \leq x_{n+1} \mid X\left(t_n\right)=x_n\right\}
\end{aligned}
$$

则称 $\{X(t), t \in T\}$ 为马尔枓夫过程。

在已知现在状态的情况下，具备马尔科夫性系统的末来状态只与现在状态有关，而与过去的所有状态无关。

状态离散的马尔科夫过程被称为**马尔科夫链**。

## 马尔科夫链

马尔科夫链 $\{X(n, s), n \in T, s \in S\}$ 通常被简记为 $\left\{X_n\right\}$ ，其时间参数集 $T=\{1, \cdots, n, \cdots\}$ 是离散的时间集合，状态空间 $S=\left\{s_1, \cdots\right\}$ 是离散的状态集合。

为了方便介绍，下面将状态空间中的状态值简记为 $\{1,2, \cdots\}$ 。若马尔科夫链 $\left\{X_n\right\}$ 在第 $n$ 次随机试验 (通常被称为第 $n$ 步) 处于状态 $i(i \in N)$ ，则记为 $x_n=i$

马尔科夫链 $\{X_n\}$ ，从第 $n$ 步的状态 $i$ 转移到第 $n+1$ 步的状态 $j$ 的转移概率定义为

$$
p_{i j}(n) \doteq P\left\{X_{n+1}=j \mid X_n=i\right\}, \forall m, n \in T, \forall i, j \in S
$$

### 马尔科夫链的状态分类

**齐次马尔科夫链**：

若 $\forall m, n \in T, \forall i, j \in S$ ，马尔科夫链 $\left\{\mathbf{X}_n\right\}$ 的转移概率 $\boldsymbol{p}_{i j}(\mathbf{n})$ 与所处时刻 $\mathbf{n}$ 无关，即：

$$
P\left\{X_{n+1}=j \mid X_n=i\right\}=P\left\{X_{m+1}=j \mid X_m=i\right\}
$$

齐次马尔枓夫链 $\left\{X_n, n \geq 1\right\}$ ，其状态空间 $\mathbf{S}=\{1,2 \cdots\}$ ，转移概率矩阵为 $\mathbf{P}=\left[p_{i j}\right], i, j \in S$, 依据转移概率的性质可对齐次马尔科夫链的状态进行分类。

#### 平稳分布

$\forall j \in S$ ，若 $\pi_j \geq 0, \sum_{j \in S} \pi_j=1$, 且 $\pi_j=\sum_{i \in S} \pi_i p_{i j}$ ，则称概率分布 $\left\{\pi_j, j \in S\right\}$ 为齐次马尔枓夫链 $\left\{X_n\right\}$ 的平稳分布 (或极限分布)。

$\pi_j \geq 0, \sum_{j \in S} \pi_j=1$, 因此平稳分布也是一个随机矩阵。

对平稳分布的两点理解：

1 ) 无论初始状态如何，经过足够长的时间后，处于状态 $\mathrm{j}$ 的概率为 $\pi_j$ ；
2 ) 无论初始状态如何，经过足够长的时间后，到达状态 j的次数占总数的比例。

##### 平稳分布的性质：

若 $\left\{\pi_j, j \in S\right\}$ 为平稳分布，

1. $\pi_j=\sum_{i \in S} \pi_i p_{i j}^{(n)}$;
1. $\pi=\pi \mathrm{P}$;
1. $\pi_j=\lim _{n \rightarrow \infty} p_j^{(n)}=\frac{1}{\mu_j}$ 。

- 性质 (2) 是性质 (1) 的矩阵形式。
- 性质 (3) 中， $u_j$ 是从状态 $j$ 出发再次返回到状态 $j$ 的平均返回时间，则 $\frac{1}{\mu_j}$ 表示从状态 $j$ 出发每単 位时间返回状态 $j$ 的平均次数。

##### 平稳分布示例

例题 马尔科夫链的状态空间 $S=\{1,2,3\}$ ，一步转移概率矩阵为：
$\mathbf{P}=\left[\begin{array}{ccc}0.5 & 0.4 & 0.1 \\ 0.3 & 0.4 & 0.3 \\ 0.2 & 0.3 & 0.5\end{array}\right]$, 试求其平稳分布。

答案 :

设其平稳分布 $\pi=\left[\pi_1, \pi_2, \pi_3\right]$, 依据 $\pi=\pi \mathrm{P}$ 有：

$$
\left[\pi_1, \pi_2, \pi_3\right]\left[\begin{array}{lll}
0.5 & 0.4 & 0.1 \\
0.3 & 0.4 & 0.3 \\
0.2 & 0.3 & 0.5
\end{array}\right]=\left[\pi_1, \pi_2, \pi_3\right]
$$

因此，有： $0.5 \pi_1+0.3 \pi_2+0.2 \pi_3=\pi_1 ， 0.4 \pi_1+0.4 \pi_2+0.3 \pi_3=\pi_2, 0.1 \pi_1+0.3 \pi_2+0.5 \pi_3=\pi_3$

且有平稳分布的性质： $\pi_1+\pi_2+\pi_3=1$ 。

计算可得 : $\pi_1=\frac{21}{62}, \pi_2=\frac{23}{62}, \pi_3=\frac{18}{62}$ 。因此，该沵枓夫锥的平稳分布 $\pi=\left[\frac{21}{62}, \frac{23}{62}, \frac{18}{62}\right]$ 。




[1]: https://cloud.tencent.com/developer/article/2091514?areaSource=&traceId=

马尔科夫链：http://www.jingxuanyang.com/2021/01/27/Markov-Chains/
