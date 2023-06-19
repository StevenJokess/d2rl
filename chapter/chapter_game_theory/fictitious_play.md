

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-06-07 14:47:37
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-06-17 02:24:10
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 虚拟对局

虚拟对局是博弈论中最早提出的几个博弈学习方法之一, 通常用来计算几类标准形式博弈的纳什均衡, 比如二人零和博弈、势博弈以及一般的 (generic) $2 \times m$ 矩阵博弈。

在虚拟对局中, 博弈重复进行, 在第 $t$ 轮博弈中, 任意个体 $i$ 根据对手的历史平均策略 $\sigma_{-i, t-1}$ 来执行最优响应, 即执行 $\operatorname{Br}_i\left(\sigma_{-i, t-1}\right)=\operatorname{argmax}_{a_i \in A_i} \overline{\mathcal{R}}_i\left(a_i, \sigma_{-i, t-1}\right)$ (如果存在多个最优响应, 则随机执行其中一个)。

记第 $t$ 轮博弈结束时, 联合平均策略为 $\sigma_t=\times_{i=1}^n \sigma_{i, t}$, 最优响应联合为 $\operatorname{Br}\left(\sigma_t\right)=\times_{i=1}^n \operatorname{Br}_i\left(\sigma_{-i, t-1}\right)$。

虚拟对局按照下式来更新平均策略

$$
\sigma_t=\frac{t-1}{t} \sigma_{t-1}+\frac{1}{t} \operatorname{Br}\left(\sigma_{t-1}\right) \#(12)
$$

随着虚拟对局的进行, 当联合平均策略收敛时, 该策略就是博弈的纳什均衡。

虚拟博弈(fictitious play, FP)是一类求解纳什均衡的方法，它根据对手的平均经验做出最优策略，将最优策略加入平均经验，并以此反复迭代的学习过程。如公式(3)所示，第 $n$ 次迭代的智能体平均经验为 $\sigma_n$ ，最优策略为 $b\left(\sigma_n\right)$ 。

$$
\sigma_{n+1} \in(1-1 /(n+1)) \sigma_n+1 /(n+1) \cdot b\left(\sigma_n\right)
$$
[2]

此外, 当博弈是二人零和博弈、势博弈或者一般的 (generic) $2 \times m$ 矩阵博弈时, 联合平均策略一定会收敛到纳什均衡 [25]。

Leslie 和 Collins 在 2006 年对以上虚拟对局进行了重要扩展, 提出了一般化且弱化的虚拟对局 (generalised weakened fictitious play) 。该工作的重要扩展在于两点: 一是每一轮个体不再需要精确求解最优响应, 这对应了“弱化"; 二是每一轮在 更新平均策略时允许存在扰动且更新的步长不一 定是 $1 / t$, 这对应了“一般化”。

对于近似最优响应 (即 $\epsilon$-最优响应, $\epsilon \geq 0$ ), 其定义为 $\operatorname{Br}_i^\epsilon\left(\sigma_{-i}\right)=\left\{\sigma_i \in\right.$ $\left.\Sigma_i \mid \overline{\mathcal{R}}_i\left(\sigma_i, \sigma_{-i}\right) \geq \overline{\mathcal{R}}_i\left(\operatorname{Br}_i\left(\sigma_{-i}\right), \sigma_{-i}\right)-\epsilon\right\}$。 在一般化且弱化的虚拟对局中, 式(12)修改为:

$$
\sigma_t=\left(1-\alpha_t\right) \sigma_{t-1}+\alpha_t\left(\operatorname{Br}^{\epsilon_{t-1}}\left(\sigma_{t-1}\right)+M_t\right) \#(13)
$$

其中 $\lim _{t \rightarrow \infty} \alpha_t=0, \lim _{t \rightarrow \infty} \epsilon_t=0, \sum_{t=1}^{\infty} \alpha_t=\infty$, 以及 对于任意的 $T>0$,

$$
\limsup _{t \rightarrow \infty}\left\{\left\|\sum_{m=t-1}^{k-1} \alpha_{m+1} M_{m+1}\right\| \mid \sum_{m=t-1}^{k-1} \alpha_{m+1} \leq T\right\}=0 \#(14)
$$

Leslie 和 Collins 同时也证明了当博弈是二人零和博弈、势博弈或者一般的 $2 \times m$ 矩阵博弈时, 一般化且弱化的虚拟对局最终会使得联合平均策略收敛到纳什均衡。

在一般化且弱化的虚拟对局基础上, Heinrich 等人将其进一步拓展到扩展形式博弈, 提出了 XFP 算法 (full-width extensive-form fictitious play), 并证明了 XFP 与一般化且弱化的虚拟对局拥有相同 的收玫性保证 [29]。在此基础上, 为了更高效地处理大规模博弈场景, 他们提出了虚拟自我对局算法 FSP, 该算法利用强化学习来求解近似最优响应, 利用监督学习来进行平均策略的更新。

[1]: http://cjc.ict.ac.cn/online/onlinepaper/zl-202297212302.pdf
[2]: https://www.china-simulation.com/article/2023/1004-731X/1004-731X-2023-35-4-671.shtml
