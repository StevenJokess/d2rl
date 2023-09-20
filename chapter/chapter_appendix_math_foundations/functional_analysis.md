

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-09 20:45:12
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-20 11:18:19
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 泛函分析

泛函分析（Functional Analysis）是现代数学的一个分支，隶属于分析学，其研究的主要对象是函数构成的空间。 泛函分析是由对函数的变换（如傅立叶变换等）的性质的研究和对微分方程以及积分方程的研究发展而来的。 使用泛函作为表述源自变分法，代表作用于函数的函数。

## 有模型策略迭代的理论基础

本节介绍有模型策略迭代的理论基础：度量空间上的Banach不动点定理。度量空间和Banach不动点定理在一般的泛函分析教程中都会介绍。本节对必要的概念加以简要的复习，然后证明Bellman算子是压缩映射，可以用Banach不动点定理迭代求解Bellman方程。

## 度量空间及其完备性

度量 (metric，又称距离)，是定义在集合上的二元函数。对于集合 $x$ ，其上的度量 $d: \mathcal{X} \times \mathcal{X} \rightarrow \mathbb{R}$ ，需要满足：

- 非负性：对任意的 $x^{\prime}, x^{\prime \prime} \in x ，$ 有 $d\left(x^{\prime}, x^{\prime \prime}\right) \geq 0$;
- 同一性: 对任意的 $x^{\prime}, x^{\prime \prime} \in x ，$ 如果 $d\left(x^{\prime}, x^{\prime \prime}\right)=0$ ，则 $x^{\prime}=x^{\prime \prime}$ ；
- 对称性：对任意的 $x^{\prime}, x^{\prime \prime} \in X$ ，有 $d\left(x^{\prime}, x^{\prime \prime}\right)=d\left(x^{\prime \prime}, x^{\prime}\right)$;
- 三角不等式：对任意的 $x^{\prime}, x^{\prime \prime}, x^{\prime \prime \prime} \in x ，$ 有 $d\left(x^{\prime}, x^{\prime \prime}\right) \leq d\left(x^{\prime}, x^{\prime \prime}\right)+d\left(x^{\prime \prime}, x^{\prime \prime \prime}\right)$ 。

有序对 $(X, d)$ 又称为**度量空间** (metric space)。

我们来看一个度量空间的例子。考虑有限Markov决策过程状态函数 $v(s) \quad(s \in S)$ ，其所有可能的取值组成集合 $\mathcal{V}
=\mathbb{R}^{|S|}$ ，定义 $d_{\infty}$ 如下：

$$
d_{\infty}\left(v^{\prime}, v^{\prime \prime}\right)=\max _{s e \mathcal{S}}\left|v^{\prime}(s)-v^{\prime \prime}(s)\right|
$$

可以证明， $d_{\infty}$ 是 $\mathcal{V}$ 上的一个度量。（证明：非负性、同一性、对称性是显然的。由于对于 $\forall s \in \mathcal{S}$ 有

$$
\begin{aligned}
& \left|v^{\prime}(s)-v^{\prime \prime \prime}(s)\right| \\
& =\left[v^{\prime}(s)-v^{\prime \prime}(s)\right]+\left[v^{\prime \prime}(s)-v^{\prime \prime}(s)\right] \\
& \leqslant\left|v^{\prime}(s)-v^{\prime \prime}(s)\right|+\left|v^{\prime \prime}(s)-v^{\prime \prime \prime}(s)\right| \\
& \leqslant \max _{s a s}\left|v^{\prime}(s)-v^{\prime \prime}(s)\right|+\max _{s a s}\left|v^{\prime \prime}(s)-v^{\prime \prime \prime}(s)\right| \\
&
\end{aligned}
$$

可得三角不等式。) 所以， $\left(\mathcal{V}, \mathrm{d}_{\infty}\right)$ 是一个度量空间。

对于一个度量空间，如果Cauchy序列都收敛在该空间内，则称这个度量空间是完备的 (complete)。

例如，实数集R就是一个著名的完备空间（事实上实数集就是由完备性定义出来的。有理数集不完备，加上无理数集就完 $\varepsilon>0$ ，存在正整数K使得任意的 $k^{\prime}, k^{\prime}>K$ ，均有 $d\left(v_{k^{\prime}}, v_{k^{\prime}}\right)<\varepsilon$ 。对于 $\forall s \in \mathcal{S},\left|v_{k^{\prime}}(s)-v_{k^{\prime}}(s)\right| \leqslant d\left(v_{k^{\prime}}, v_{k^{\prime}}\right)<\varepsilon$ ，所以 $\left\{v_k(s): k=0,1,2, \ldots\right\}$ 是Cauchy列。由实数集的完备性，可以知道 $\left\{v_k(s): k=0,1,2, \ldots\right\}$ 收玫于某个实数，记这个实数为 $v_{\infty}(s)$ 。所以，对于 $\forall \varepsilon>0$ ，存在正整数 $\kappa(s)$ ，对于任意 $k>\kappa(s)$ ，有 $\left|v_k(s)-v_{\infty}(s)\right|<\varepsilon$ 。.取 $\kappa(\mathcal{S})=\max _{s \in s} \kappa(s)$ ， 有 $d\left(v_k, v_{\infty}\right)<\varepsilon$ ，所以 $\left\{v_k: k=0,1,2, \ldots\right\}$ 收敛于 $v_{\infty}$ ，而 $v_{\infty} \in \mathcal{V}$ ，完备性得证）。

## 压缩映射与Bellman算子

本节介绍压缩映射的定义，并证明Bellman期望算子和Bellman最优算子是度量空间 $\left(\mathcal{V}, d_{\infty}\right)$ 上的压缩映射。 对于一个度量空间 ${ }^{(\mathcal{X}, d)}$ 和其上的一个映射 $t: \mathcal{X} \rightarrow \mathcal{X}$ ，如果存在某个实数 $\gamma \in(0,1)$ ，使得对于任意的 $x^{\prime}, x^{\prime \prime} \in \mathcal{X}$ ，都有

$$
d\left(t\left(x^{\prime}\right), t\left(x^n\right)\right)<d\left(x^{\prime}, x^{\prime \prime}\right)
$$

则称映射t是压缩映射 (contraction mapping，或Lipschitzian mapping）。其中的实数 Y被称为Lipschitz常数。 第2章中介绍了Bellman期望方程和Bellman最优方程。这两个方程都有用动作价值表示动作价值的形式。根据这个形式， 我们可以为度量空间 $\left(\mathcal{V}, d_{\infty}\right)$ 定义Bellman期望算子和Bellman最优算子。

给定策略 $\pi(a \mid s)(s \in \mathcal{S}, a \in \mathcal{A}(s))$ 的Bellman期望算子 ${ }^t: \mathcal{V} \rightarrow \mathcal{V} ：$

$$
t_n(v)(s)=\sum_a \pi(a \mid s)\left[r(s, a)+\gamma \sum_{s^{\prime}} p\left(s^{\prime} \mid s, a\right) v\left(s^{\prime}\right)\right], \quad s \in \mathcal{S}
$$

Bellman最优算子 ${ }^t: \mathcal{V} \rightarrow \mathcal{V}:$

$$
t_*(v)(s)=\max _{a \in \mathcal{A}}\left[r(s, a)+\gamma \sum_{s^{\prime} \in \mathcal{S}} p\left(s^{\prime} \mid s, a\right) v_*\left(s^{\prime}\right)\right], \quad s \in \mathcal{S}
$$

下面我们就来证明，这两个算子都是压缩映射。

$$
t_n\left(v^{\prime}\right)(s)-t_n\left(v^{\prime \prime}\right)(s)=\gamma \sum_a \pi(a \mid s) \sum_{s^{\prime}} p\left(s^{\prime} \mid s, a\right)\left[v^{\prime}\left(s^{\prime}\right)-v^{\prime \prime}\left(s^{\prime}\right)\right]
$$

所以

$$
\left|t_\pi\left(v^{\prime}\right)(s)-t_\pi\left(v^{\prime \prime}\right)(s)\right| \leqslant \gamma \sum \pi(a \mid s) \sum_s p\left(s^{\prime} \mid s, a\right) \max _{s^{\prime}}\left|v^{\prime}\left(s^{\prime}\right)-v^{\prime \prime}\left(s^{\prime}\right)\right|=\gamma d\left(v^{\prime}, v^{\prime \prime}\right)
$$

考虑到 $v^{\prime}, v^{\prime \prime}$ 是任取的，所以有

$$
d\left(t_\pi\left(v^{\prime}\right), t_\pi\left(v^{\prime \prime}\right)\right) \leqslant \gamma d\left(v^{\prime}, v^{\prime \prime}\right)
$$

当 $\gamma<1$ 时， $t_\pi$ 就是压缩映射。

$$
\left|\max _a f^{\prime}(a)-\max _a f^{\prime \prime}(a)\right| \leqslant \max _a\left|f^{\prime}(a)-f^{\prime \prime}(a)\right|
$$

其中f'和f"是任意的以 $a$ 为自变量的函数。（证明：设 $a^{\prime}=\arg \max _a f^{\prime}(a)$ ，则

$$
\max _a f^{\prime}(a)-\max _a f^{\prime \prime}(a)=f^{\prime}\left(a^{\prime}\right)-\max _a f^{\prime \prime}(a) \leqslant f^{\prime}\left(a^{\prime}\right)-f^{\prime \prime}\left(a^{\prime}\right) \leqslant \max _a\left|f^{\prime}\left(a^{\prime}\right)-f^{\prime \prime}\left(a^{\prime}\right)\right|
$$

同理可证 $\max _a f^{\prime \prime}(a)-\max _a f^{\prime}(a) \leqslant \max _a\left|f^{\prime}\left(a^{\prime}\right)-f^{\prime \prime}\left(a^{\prime}\right)\right|$ ，于是不等式得证。)

利用这个不等式，对任意的 $v^{\prime}, v^{\prime \prime} \in \mathcal{V}$ ，有

$$
\begin{aligned}
& t_*\left(v^{\prime}\right)(s)-t_*\left(v^{\prime \prime}\right)(s) \\
& \quad=\max _{a \in \mathcal{A}}\left[r(s, a)+\gamma \sum_{s^{\prime} \in \mathcal{S}} p\left(s^{\prime} \mid s, a\right) v^{\prime}\left(s^{\prime}\right)\right]-\max _{a \in \mathcal{A}}\left[r(s, a)+\gamma \sum_{s^{\prime} \in \mathcal{S}} p\left(s^{\prime} \mid s, a\right) v^{\prime \prime}\left(s^{\prime}\right)\right] \\
& \quad \leqslant \max _{\sigma^{\prime} \in \mathcal{A}}\left|\gamma \sum_{s^{\prime} \in S} p\left(s^{\prime} \mid s, a^{\prime}\right)\left(v^{\prime}\left(s^{\prime}\right)-v^{\prime \prime}\left(s^{\prime}\right)\right)\right| \\
& \quad \leqslant \gamma\left|v^{\prime}\left(s^{\prime}\right)-v^{\prime \prime}\left(s^{\prime}\right)\right|
\end{aligned}
$$

进而易知 $\left|t_*\left(v^{\prime}\right)(s)-t_*\left(v^{\prime \prime}\right)(s)\right| \leqslant \gamma\left|v^{\prime}\left(s^{\prime}\right)-v^{\prime \prime}\left(s^{\prime}\right)\right| \leqslant \gamma d\left(v^{\prime}, v^{\prime \prime}\right)$ ，所以是压缩映射。

## Banach不动点定理

巴拿赫不动点定理，又称为压缩映射定理或压缩映射原理，是度量空间理论的一个重要工具。它保证了度量空间的一定自映射的不动点的存在性和唯一性，并提供了求出这些不动点的构造性方法。[3]

对于度量空间 $(\mathcal{X}, d)$ 上的映射 $t: \mathcal{X} \rightarrow \mathcal{X}$ ，如果 $x \in \mathcal{X}$ 使得 $t(x)=x$ ，则称 $\mathrm{x}$ 是映射t的不动点 (fix point)。 $v_*(s)(s \in \mathcal{S})_{\text {满足Bellman最优方程，是Bellman最优算子 }} t_*$ 的不动点。

完备度量空间上的压缩映射有非常重要的结论：Banach不动点定理。Banach不动点定理（Banach fixed-point theorem， 缩映射，则映射t在 $x$ 内有且仅有一个不动点 ${ }^x+\infty$ 。更进一步，这个不动点可以通过下列方法求出: 从 $x$ 内的任意一个元素 $x$ 0开始，定义迭代序列 $x_k=t\left(x_{k-1}\right)(k=1,2,3, \ldots)$ ，这个序列收敛，且极限为 $x_{+\infty}$ 。（证明: 考虑任取 $\times$ 的及其确定的列 $\left\{x_k: k=0,1, \ldots\right\}$ ，我们可以证明它是Cauchy序列。对于任意的 $k^{\prime}, k^{\prime \prime}$ 且 $k^{\prime}<k^{\prime \prime}$ ，用距离的三角不等式和非负性可知，

$$
d\left(x_{k^{\prime}}, x_{k^{\prime}}\right) \leqslant d\left(x_{k^{\prime}}, x_{k^{\prime}+1}\right)+d\left(x_{k^{\prime}+1}, x_{k^{\prime}+2}\right)+\cdots+d\left(x_{k^{\prime}-1}, x_{k^{\prime}}\right) \leqslant \sum_{k=k^{\prime}}^{+\infty} d\left(x_{k+1}, x_k\right)
$$

再反复利用压缩映射可知，对于任意的正整数 $k$ 有 $d\left(x_{k+1}, x_k\right) \leqslant \gamma^k d\left(x_1, x_0\right)$ ，代入得：

$$
d\left(x_{k^{\prime}}, x_{k^{\prime}}\right) \leqslant \sum_{k=k^{\prime}}^{+\infty} d\left(x_{k+1}, x_k\right) \leqslant \sum_{k=k^{\prime}}^{+\infty} \gamma^k d\left(x_1, x_0\right)=\frac{\gamma^{k^{\prime}}}{1-\gamma} d\left(x_1, x_0\right)
$$

由于 $\gamma \in(0,1)$ ，所以上述不等式右端可以任意小，得证。）

Banach不动点定理给出了求完备度量空间中压缩映射不动点的方法：从任意的起点开始，不断迭代使用压缩映射，最终 就能收敛到不动点。并且在证明的过程中，还给出了收敛速度，即迭代正比于 $\gamma^k$ 的速度收敛 (其中是 $\mathrm{i}$ 迭代次数) 。在射，那么就可以用迭代的方法求Bellman期望算子和Bellman最优算子的不动点。由于Bellman期望算子的不动点就是策略 价值，Bellman最优算子的不动点就是最优价值，所以这就意味着我们可以用迭代的方法求得策略的价值或最优价值。在后面的小节中，我们就来具体看看求解的算法。



[1]: https://baike.baidu.com/item/%E6%B3%9B%E5%87%BD%E5%88%86%E6%9E%90/4151#:~:text=%E6%B3%9B%E5%87%BD%E5%88%86%E6%9E%90%EF%BC%88Functional%20Analysis,%E4%BD%9C%E7%94%A8%E4%BA%8E%E5%87%BD%E6%95%B0%E7%9A%84%E5%87%BD%E6%95%B0%E3%80%82
[2]: https://developer.aliyun.com/article/726187?spm=a2c6h.12873639.article-detail.5.20c06a2ewJKXn1#slide-4
[3]: https://zh.wikipedia.org/zh-cn/%E5%B7%B4%E6%8B%BF%E8%B5%AB%E4%B8%8D%E5%8A%A8%E7%82%B9%E5%AE%9A%E7%90%86
[4]: https://cread.jd.com/read/startRead.action?bookId=30513215&readType=1
