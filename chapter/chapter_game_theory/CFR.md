

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-06-17 20:45:27
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-06-17 20:46:20
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# CFR

CFR 是目前最先进的能够在大规模智能博弈对抗中生成高效策略的技术之一, 是一种在两人零和博弈中收敛到纳什均衡的迭代算法, 收敛性具有理论保证。卡内基梅隆大学与阿尔伯塔大学在近十年不断致力于智能博弈研究 ${ }^{[16,61-66]}$, 研究思路收敛聚焦到反事实后悔值最小化算法, 取得了一系 列令人瞩目的成果并发表于《Science》杂志 , 如 DeepStack ${ }^{[24]}$, Libratus $^{[25]}$ 和 Pluribus ${ }^{[10]}$ 等高水平德州扑克 $A I$ 在人机对抗中能够匹敌人类职业选手, 引起了美国国防军事机构的高度重视,并着手研究相关技术在军事中的应用。

反事实后悔值和平均策略的更新步骤如下。

Step1 反事实值计算。定义反事实值 (Counterfactual Utility) 为: 所有其他 (除了玩家 $i$ 以外的) 玩家都遵循策略组合 $\sigma$ 选择动作并到达节点 $h$ 的期望效益值(不考虑玩家 $i$ 到达节点 $h$ 的概率)。即:

$$
v_i^\sigma(h)=\sum_{h \in z, z \in Z} \pi_{-i}^\sigma(h) \pi^\sigma(h, z) u_i(z)
$$

其中, $u_i(z)$ 为玩家 $i$ 到达终点节点 $z \in Z$ 获得的博弈收益。

Step2 后悔值更新。首先考虑一个指定信息集 $I \in I_i$ 和 玩家 $i$ 在该信息集上的选择,定义 $u_i(\sigma, h)$ 为所有玩家遵循策略 $\sigma$ 进行游戏并到达了动作序列 $h$ 时的期望效益值, 执行 $a$ 的动作反事实值为 $v_i^\sigma(a \mid h)=v_i^\sigma(h a)$, 执行该动作的后悔值为 $r_i^\sigma=v_i^\sigma(a \mid h)-v_i^\sigma(h)$ 。同样,信息集 $I_i$ 上的反事实值为 $v_i^\sigma\left(I_i\right)=\sum_{h \in I_i} v_i^\sigma(h)$, 后悔值为:

$r_i^t\left(a \mid I_i\right)=\sum_{h a \subseteq z} \pi_i^\sigma(h a, z) u_i(z)-\sum_{h \subseteq z} \pi_i^\sigma(h, z) u_i(z)$

$T$ 次迭代的后悔值的定义如下:

$R_i^T(I, a)=\sum_{t=1}^T r_i^t(I, a)$

如果产生某一玩家的策略使得当 $T \rightarrow \infty$ 时, $R_i^{T,+} / T \rightarrow 0$ (其中, $R_i^{T,+}=\max \left\{R_i^T, 0\right\}$ 表示后悔值都为非负)，则称这个策略是后悔值最小的。根据博弈历史数据中选取动作的后悔程度来更新将来的动作选择策略。

Step3 策略组合生成。以后悔值匹配 ( Regret Matching, RM) 为代表的后悔最小化的简单迭代算法适用于正则博弈。后悔匹配依据一个正比于正后悔值的动作概率分布, 随机采样动作。对于可选动作集 $A_i$ 中的每一个动作 $a$, 存储该动作的每轮迭代计算得到的后悔值, 在接下来的第 $T+1$ 轮迭代中, 更新策略的计算式如下:

$$
\sigma_i^{T+1}(a)=\frac{R_i^{T,+}(a)}{\sum_{b \in A_i} R_i^{T,+}(b)}
$$

从式(4)中可以看出, 动作 $a$ 的后悔值表示在过去 $T$ 轮游戏中, 玩家 $i$ 没有采取该动作而产生的累加后悔值。在理想化的情况下, 可以通过正比于所有动作正后悔值之和来更新下一时刻的策略, 以减少之后博弈中动作的后悔值。值得注意的是, 若对手也理解后悔匹配算法并在对局过程中察觉 到了己方策略的某种概率性偏向, 则可以利用这种偏向针对性地选择动作。因此, 后悔匹配算法采用虚拟自博弈的方式, 通过自博弈 (Self-play) 反复迭代直到后悔值最小化。

$T+1$ 次迭代的当前策略更新公式如下：

$$
\sigma_i^{T+1}(a)= \begin{cases}\frac{R_i^{T,+}(I, a)}{\sum_{a \in A(I)} R_i^{T,+}(I, a)}, & \text { if } \underset{a \in A(I)}{ } R_i^{T,+}(I, a)>0 \\ \frac{1}{|A(I)|}, & \text { others }\end{cases}
$$

式 (5) 揭示了后悔值与更新下一轮策略中的动作概率分 布之间的关系, 即每种动作被选择的概率与其在过去已经进 行的 $T$ 轮博弈中所有动作对应的正后悔值之和成正比。若 不存在正后悔值, 则给每个动作分配均等的采样概率。

Step4 平均策略更新。对于 $T$ 次迭代的平均策略定义为:

$$
\bar{\sigma}_i^T\left(a \mid I_i\right)=\frac{\sum_{t=1}^T \pi_i^t\left(I_i\right) \sigma_i^t\left(a \mid I_i\right)}{\sum_{t=1}^T \pi_i^t\left(I_i\right)}
$$

Step5 循环。重复 Step1－Step4, 直到收敛。通过最小 化即时后悔值可以使平均整体后悔值最小化, 即仅仅通过最 小化即时后悔值就可以找到近似纳什均衡。寻找博弈中近似 纳什均衡的关键点就是如何最小化每个信息集上的即时后悔值, 即时后悔值的关键特征是可以通过仅控制 $\sigma_i(I)$ 来实现最 小化。然而, 算法迭代的终止条件需要根据解的质量来设置。 仅用迭代次数来控制解的质量略显盲目,终止迭代的条件与 可利用度密切相关。在叶子节点的收益值标准化为区间 $[0$, $1]$ 时, 通常可利用度参考阈值 ${ }^{[36]}$ 为 0.5 。

存储每个信息集动作对 $(I, a)$ 上的反事实值、后悔值和平均策略需要 $\sum_i\left(\left|I_i\right|\left|A_i\right|\right)$ 的存储空间。对于大规模扩展式博弈, 计算并最小化平均整体后悔值 $R_i^T$ 是不现实的。后悔值最小化算法的基本思想是将整体后悔分解为一组可独立的信息集, 并在每个独立的信息集上引人后悔的概念, 通过不断迭代最小化每个信息集上的后悔值来最小化平均整体后悔, 此时得到的平均策略达到近似纳什均衡。后悔值在所有信息集 $I$ 上的有界收敛的实质是在每个信息集 $I$ 上的最小化“组合 (Composability)”。2019 年, 卡内基梅隆大学的 Tuomas 等 从最优控制的视角进一步提出了后悔值回路 (Regret Circuit) 的概念, 揭示了一般性 “组合”的深层次含义 ${ }^{[67]}$, 通过将复合 凸集的后悔值最小化归纳表示为在简单集上的一系列保凸运算, 证明了简单集合的局部 CFR 最小化器可以与附加 CFR 最小化器组合成复合集合的 CFR 最小化器, 为 CFR 向具有一般凸策略约束的、可以跨越决策点的广义决策问题泛化提供了理论支撑。


[1]: https://www.jsjkx.com/CN/article/openArticlePDF.jsp?id=20967
