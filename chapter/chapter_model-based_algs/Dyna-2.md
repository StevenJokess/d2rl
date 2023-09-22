

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-09 00:46:47
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-21 20:56:51
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# Dyna-2

在Dyna算法框架的基础上后来又发展出了Dyna-2算法框架。和Dyna相比，Dyna-2将和和环境交互的经历以及 模型的预测这两部分使用进行了分离。还是以Q函数为例，Dyna-2将记忆分为永久性记忆 (permanent memory) 和瞬 时记忆 (transient memory), 其中永久性记忆利用实际的经验来更新，瞬时记忆利用模型模拟经验来更新。
永久性记忆的Q函数定义为：

$$
Q(S, A)=\phi(S, A)^T \theta
$$

瞬时记忆的Q函数定义为：

$$
Q^{\prime}(S, A)=\bar{\phi}(S, A)^T \bar{\theta}
$$

组合起来后记忆的Q函数定义为：

$$
\bar{Q}(S, A)=\phi(S, A)^T \theta+\bar{\phi}(S, A)^{T^{-}} \bar{\theta}
$$

Dyna-2的基本思想是在选择实际的执行动作前，智能体先执行一遍从当前状态开始的基于模型的模拟，该模拟 将仿真完整的轨迹，以便评估当前的动作值函数。智能体会根据模拟得到的动作值函数加上实际经验得到的值函数共同 选择实际要执行的动作。价值函数的更新方式类似于 $SARSA(\lambda)$

以下是Dyna-2的算法流程：

TODO

#

再次回到Dyna算法中来，Dyna算法一边从实际经验中学习，一边从模拟的经验中 学习。它是基于样本的学习和基于样本的规划的结合。如果将基于样本的规划 (Sample-Based Planning) 换成基于样本的搜索（Sample-Based Search)， 就有了Dyna-2，这是Dyna-2和Dyna的第一个区别。Dyna-2是由David Silver和 Sutton于 2008年 提出的，它是在Dyna的基础上发展起来的。同Dyna一样， Dyna-2也是一个框架。

先来解释一下何为基于样本的规划，何为基于样本的搜索? 基于样本的规划首先根 据实际经验拟合出一个模型， $M_\eta=<P_\eta, R_\eta>$ ，然后基于这个模型生成模拟数 据。在这个任务中，规划的有效性严重依赖于模型的准确性。

基于样本的搜索也是通过模型去采样，不过这些样本均从同一个真实状态 $s$ 开始转 移。当我们从状态 $s$ 开始，进行了N次采样之后，就可以将其连成一棵以当前状态 $s$ 为根节点的树。基于样本的搜索就是基于这棵树进行动作选择，以识别出最优动作 的过程。其中，最简单有效的搜索方法要数蒙特卡罗模拟了。它从真实状态 $s$ 开 始，遵循一个随机策略，模拟生成多条轨迹，以便评估当前状态下所有状态行为对 的值函数。针对所有在真实状态 $s$ 下采取行为 生成的轨迹，求取经验平均回报，作 为状态行为对 $(s, a)$ 的值函数 $Q(s, a)$ 的估计。轨迹模拟反复进行，在计算资 源和计算时间允许的条件下，尽可能穷尽状态 $s$ 下的所有行为。模拟完成之后，智 能体将会选择 $\max Q(s, a)$ 对应的动作作为真实动作。接着，基于样本的搜索转移 到下一个真实状态。
总的来说，Dyna-2包含了一个学习模块和一个搜索模块。学习过程使用后向Sarsa （ $\lambda$ ）方法，在选择每一个真实动作之前，都会从当前状态开始，执行一次基于样 本的搜索。智能体会根据搜索得到的值函数（搜索模块）加上实际得到的值函数 (学习模块)，共同选择实际要执行的动作。这里要说明一下Dyna-2与Dyna的第 二个区别。Dyna是用实际经验和模拟经验对同一个值函数进行估计，整个框架中 仅涉及一个值函数。而Dyna-2框架则进行了细分，Dyna-2中有两个值函数，分别 方法) 的值函数 $\hat{q}_{\mathrm{mb}}$ 。实际经验对应的值函数表示在实际场景中所预估的期望回 报。而模拟经验对应的值函数则是模拟搜索中估计的预期回报。Dyna-2最终将两种场景下产生的价值综合起来进行决策，以期得到更优秀的策略，如图11-10所 示。

综上所述，Dyna-2涉及了两个值函数，因此需要维护两套特征权重。一套反映智能体的永久记忆 (Permanent Memory)，用脿示。永久记忆一般是普适性的， 要求比较准确，该记忆利用实际经验来更新，对应的值函数也叫永久值函数。另一 套反应智能体的瞬时记忆 (Transient Memory)，用 $\bar{\theta}$ 表示。瞬时记忆用于即时 决策，用拟合经验来更新，对应的值函数也叫瞬时值函数。无论是永久值函数还是 瞬时值函数，两者都使用Sarsa $(\lambda)$ 求解。

接下来介绍Dyna-2算法的算法流程。算法流程中， $Q(s, a)$ 表示永久值函数， $\bar{Q}(s, a)$ 表示联合值函数，它是永久值函数和瞬时值函数的和，使得僢时值函数 能够时刻对永久值函数进行局部修正。

$$
\begin{aligned}
& Q(s, a)=\phi(s, a)^{\mathrm{T}} \boldsymbol{\theta} \\
& \overline{\mathbf{Q}}(s, a)=\phi(s, a)^{\mathrm{T}} \boldsymbol{\theta}+\bar{\phi}(s, a)^{\mathrm{T}} \boldsymbol{\theta}
\end{aligned}
$$

同时，将环境模型分化为两个模型，分别为状态转移模型（也叫运动模型，或者环境动力学模型) $A$ 和回报模型 $B$ 。

Dyna-2体系框架包含了大量的学习和搜索算法，是瞬时记忆和永久记忆的结合。 如果Dyna-2没有瞬态记忆， $\bar{\phi}=\phi ，$ 那么搜索过程将不起作用，整个Dyna-2退化 为Sarsa $(\lambda)$ 算法。如果没有永久记忆， $\phi=\phi$ ，没有学习过程，那么Dyna-2简化 为基于样本的搜索算法，如蒙特卡罗模拟。Silver和Sutton于2008年将Dyna-2应 用到 $9 \times 9$ 的计算机围棋程序中，取得了很好的效果。当只使用瞬时记忆时，Dyna2的表现效果和UCT一样。当综合使用瞬时记忆和永久记忆时，Dyna-2显著优于 UCT方法（Upper Confidence Bound Apply to Tree，即上限置信区间搜索树算法。）[2]


[1]: https://cloud.tencent.com/developer/article/1398231
[2]: https://weread.qq.com/web/reader/57d321c0813ab6bf8g016fe1kfe932230253fe9fc289c8a3
