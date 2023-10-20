# Sarsa(λ)

对于控制问题，我们只需要将 $\mathrm{G}_{\mathrm{t}}$ 的定义改成状态-动作版本就行了，剩余的方法跟预测类方法是一样的。 根据之前的定义，我们有 $\mathrm{n}$ 步回报的动作价值函数形式：

$$
\begin{aligned}
\mathrm{G}_{\mathrm{t}: \mathrm{t}+\mathrm{n}} & :=\mathrm{r}_{\mathrm{t}+1}+\gamma \mathrm{r}_{\mathrm{t}+2}+\ldots+\gamma^{\mathrm{n}-1} \mathrm{r}_{\mathrm{t}+\mathrm{n}}+\gamma^{\mathrm{n}} \mathrm{q}\left(\mathrm{s}_{\mathrm{t}+\mathrm{n}}, \mathrm{a}_{\mathrm{t}+\mathrm{n}}, \boldsymbol{w}_{\mathrm{t}+\mathrm{n}-1}\right), \quad \mathrm{t}+\mathrm{n}<\mathrm{T} \\
\mathrm{G}_{\mathrm{t}} & :=\mathrm{r}_{\mathrm{t}+1}+\gamma \mathrm{r}_{\mathrm{t}+2}+\gamma^2 \mathrm{r}_{\mathrm{t}+3} \cdots
\end{aligned}
$$

对于 $\mathrm{t}+\mathrm{n} \geq \mathrm{T}$ ，令 $\mathrm{G}_{\mathrm{t}: \mathrm{t+n}}:=\mathrm{G}_{\mathrm{t}}$ 。利用 $\mathrm{G}_{\mathrm{t}}$ 我们就能得到 动作价值函数的离线 $\lambda$-回报方法。

$$
\boldsymbol{w}_{\mathrm{t}+1}:=\boldsymbol{w}_{\mathrm{t}}+\alpha\left[\mathrm{G}_{\mathrm{t}}^\lambda-\mathrm{q}\left(\mathrm{s}_{\mathrm{t}}, \mathrm{a}_{\mathrm{t}}, \boldsymbol{w}_{\mathrm{t}}\right)\right] \nabla_{w_{\mathrm{t}}} \mathrm{q}\left(\mathrm{s}_{\mathrm{t}}, \mathrm{a}_{\mathrm{t}}, \boldsymbol{w}_{\mathrm{t}}\right)
$$

对于动作价值版本的 $\mathrm{n}$-步截断 $\lambda$-回报方法 用 $\mathrm{G}_{\mathrm{t}: \mathrm{t}+\mathrm{n}}$ 就行了。另外，将资格迹引入动作价值的 $\lambda$-回报方法 （动作价值函数的时序差分）就是 Sarse( $\lambda)$ 方法，更新方法如下：

$$
\boldsymbol{w}_{\mathrm{t+1}}:=\boldsymbol{w}_{\mathrm{t}}+\alpha \delta_{\mathrm{t}} \boldsymbol{z}_{\mathrm{t}}
$$

其中

$$
\delta_{\mathrm{t}}:=\mathrm{r}_{\mathrm{t}+1}+\mathrm{v}\left(\mathrm{s}_{\mathrm{t}+1}, \boldsymbol{w}_{\mathrm{t}}\right)-\mathrm{v}\left(\mathrm{s}_{\mathrm{t}}, \boldsymbol{w}_{\mathrm{t}}\right)
$$

动作价值函数的资格迹更新方式如下：

$$
\begin{aligned}
\boldsymbol{z}_{-1} & :=\mathbf{0} \\
\boldsymbol{z}_{\mathrm{t}} & :=\gamma \lambda \boldsymbol{z}_{\mathrm{t}-1}+\nabla \mathrm{q}\left(\mathrm{s}_{\mathrm{t}}, \mathrm{a}_{\mathrm{t}}, \boldsymbol{w}_{\mathrm{t}}\right), \quad 0 \leq \mathrm{t} \leq \mathrm{T}
\end{aligned}
$$



## 证明：递归形式的 $\lambda$-回报

$$
\begin{align}
& G_{t: t+h} \doteq R_{t+1}+\gamma G_{t+1: t+h} \\
& G_{t+1}^\lambda \doteq(1-\lambda) \sum^{\infty} n=1 \lambda^{n-1} G t+1: t+n+1 \\
& G_{t+1: t+1} \doteq \hat{v}\left(S_{t+1}, w_t\right) \\
\end{align}
$$

$$
\begin{aligned}
G_t^\lambda= & (1-\lambda) \sum^{\infty} n=1 \lambda^{n-1} G_{t: t+n} \\
= & (1-\lambda)\left[G_{t: t+1}+\lambda G_{t: t+2}+\lambda^2 G_{t: t+3}+\ldots\right] \\
= & (1-\lambda)\left[\left(R_{t+1}+\gamma G_{t+1: t+1}\right)+\lambda\left(R_{t+1}+\gamma G_{t+1: t+2}\right)+\lambda^2\left(R_{t+1}+\gamma G_{t+1: t+3}\right)+\ldots\right] \\
= & (1-\lambda)\left[\left(R_{t+1}+\lambda R_{t+1}+\lambda^2 R_{t+1}+\ldots\right)+\left(\gamma G_{t+1: t+1}+\lambda \gamma G_{t+1: t+2}+\lambda^2 \gamma G_{t+1: t+3}+\ldots\right)\right] \\
= & (1-\lambda)\left[\frac{R_{t+1}}{(1-\lambda)}+\gamma G_{t+1: t+1}+\gamma\left(\lambda G_{t+1: t+2}+\lambda^2 G_{t+1: t+3}+\ldots\right)\right] \\
= & R_{t+1}+(1-\lambda)\left[\gamma G_{t+1: t+1}+\gamma \lambda\left(G_{t+1: t+2}+\lambda G_{t+1: t+3}+\ldots\right)\right] \\
= & R_{t+1}+(1-\lambda) \gamma G_{t+1: t+1}+\gamma \lambda(1-\lambda) \sum^{\infty} n=1 \lambda^{n-1} G t+1: t+n+1 \\
= & R_{t+1}+(1-\lambda) \gamma G_{t+1: t+1}+\gamma \lambda G_{t+1}^\lambda \\
= & R_{t+1}+(1-\lambda) \gamma \hat{v}\left(S_{t+1}, w_t\right)+\gamma \lambda G_{t+1}^\lambda
\end{aligned}
$$

## Sarsa(λ)与原Sarsa的比较

![Sarsa(λ)伪代码](../../img/Sarsa(lambda).png)

其中 $E(S, A)$ 是一个矩阵，用来保存其经历过的所有状态的信息。参数 $\lambda$ 是一个值为 $[0,1]$ 的衰 减值，

$Sarsa(\lambda)$ 算法比 Sarsa 算法中多了一个矩阵E (eligibility trace)，$E(S, A)$ 它用来保存在路径中所经历的每一步，并其值会随时间不断地衰减；其是通过 $\lambda in [0,1]$ 对矩阵 $E(S, A)$ 进行更新，来增强离当前状态比较近的记忆，疏远那些太久之前的记忆。[5]该矩阵的所有元素在每个回 合的开始会初始化为 0 ，如果状态 $\mathrm{s}$ 和动作 $a$ 对应的 $\mathrm{E}(\mathrm{s}, \mathrm{a})$ 值被访问过，则会其值加一。并且矩阵 $\mathrm{E}$ 中所有元素的值在每步后都会进行衰减，这保证了离获得当 前奖励越近的步骤越重要，并且如果前期智能体在原地打转时，经过多次衰减后其 $\mathrm{E}$ 值就接近于 0 了，对应的 $\mathrm{Q}$ 值几乎没有更新。

值得注意的是，在更新 $\mathrm{Q}(\mathrm{s}, \mathrm{a})$ 和 $\mathrm{E}(\mathrm{s}, \mathrm{a})$ 时，是对“整个表”做更新，但是因为矩阵 $\mathrm{E}$ 的初始值是 0 ，只有智能体走过的位置才有值，所以并不是真正的对“整个表” 做更新，而是更新获得奖励值之前经过的所有步骤。而那些没有经过的步骤因为对应的 $\mathrm{E}(\mathrm{s}, \mathrm{a})$ 值为 0 ，所以 $Q(s, a)=Q(s, a)+\alpha \cdot \delta \cdot E(s, a)=Q(s, a)$ ， 会保持原值不变。

### 代码

与SARSA代码不同的地方也是在`learn`函数，这里需要再引入一个与Q表相同维度大小的表，用于表示eligbiltiy trace（下文用E表来表示），在检查已有Q表中是否有某个状态的时候，也需要对E表进行检查，如果没有对应状态，需要调用`append`函数。[3]

```py

class SARSALambdaAgent(SARSAAgent):
    def __init__(self, env, lambda_=0.6, beta=1.,
            gamma=0.9, learning_rate=0.1, epsilon=.01):
        super().__init__(env, gamma=gamma, learning_rate=learning_rate,
                epsilon=epsilon)
        self.lambda_ = lambda_
        self.beta = beta
        self.eligibility_trace = np.zeros((env.observation_space.n, env.action_space.n))

    def learn(self, state, action, reward, next_state, next_action):
        self.check_state_exist(next_state)
        q_predict = self.q_table.loc[s, a]
        if next_state != 'terminal':
            q_target = reward + self.gamma * self.q_table.loc[next_state, next_action]  # next state is not terminal
        else:
            q_target = reward  # next state is terminal

        # increase trace amount for visited state-action pair
        self.eligibility_trace.loc[state, action] = 1. + self.beta * self.eligibility_trace.loc[state, action]

        # Q update
        td_error = q_target - q_predict
        self.q += self.learning_rate * td_error * self.eligibility_trace

        # decay eligibility trace after update
        self.eligibility_trace *= (self.gamma*self.lambda_)
```

代码实现了SARSA(λ)算法智能体类SARSALambdaAgent类，它由SARSAAgent类派生而来。与SARSAAgent类相比，它多了需要控制衰减速度的参数lambd和**控制资格迹增加的参数beta**。值得一提的是，lambda是Python的关键字，所以这里不用lambda作为变量名，而是用lambda_作为变量名。由于引入了资格迹，所以SARSA(λ)算法的性能往往比单步SARSA算法要好。[4]

---

3.4 Sarsa $(\lambda)$
3.4.1 n-Step Sarsa
根据前面的 $n$-步收获，类似的可以引出一个 $n-\frac{1}{5}$ Sarsa 的概念。
表 5.1: n-步 Q 收获
\begin{tabular}{ccc}
\hline $\mathrm{n}=1$ & Sarsa & $q_t^{(1)}=R_{t+1}+\gamma Q\left(S_{t+1}, A_{t+1}\right)$ \\
$\mathrm{n}=2$ & & $q_t^{(2)}=R_{t+1}+\gamma R_{t+2}+\gamma^2 V\left(S_{t+2}, A_{t+2}\right)$ \\
$\ldots$ & $\ldots$ & $\ldots$ \\
$\mathrm{n}=\infty$ & $\mathrm{MC}$ & $q_t^{(\infty)}=R_{t+1}+\gamma R_{t+2}+\vdots+\gamma^{T-1} R_T 12$ \\
\hline
\end{tabular}
Episode结束，agent进入终止状态，获得终止状态的即时奖励。
定义n-步Q收获 (Q-return) :
$$
q_t^{(n)}=R_{t+1}+\gamma R_{t+2}+\ldots+\gamma^{n-1} R_{t+n}+\gamma^n Q\left(S_{t+n}, A_{t+n}\right)
$$
上式和之前的 $\mathrm{n}$-步 $\mathrm{G}$ 收获很相似，这里的 $\mathrm{n}-$ 步 $\mathrm{Q}$ 收获， $\mathrm{Q}$ 是包含行为的，也就是在当前策略下基于某一状态产生的行为。
有了如上定义，可以把n-步Sarsa用n-步Q收获来表示，如下式:
$$
Q\left(S_t, A_t\right) \leftarrow Q\left(S_t, A_t\right)+\alpha\left[q_t^{(n)}-Q\left(S_t, A_t\right)\right]
$$
类似于 $\mathrm{TD}(\lambda)$, 可以给 $\mathrm{n}-$ 步 $\mathrm{Q}$ 收获中的每一步收获分配一个权重，并按权重对每一步 $\mathrm{Q}$ 收获求和，那么将得到 $q^\lambda$ 收获，它结合了所有 $\mathrm{n}$-步 $\mathrm{Q}$ 收获:
$$
q_t^\lambda=(1-\lambda) \sum_{n=1}^{\infty} \lambda^{n-1} q_t^{(n)}
$$
3.4.2 Forward View Sarsa $(\lambda)$
如果用某一状态的 $q^\lambda$ 收获来更新状态行为对的 $Q$ 值，那么可以表示称如下的形式:
$$
Q\left(S_t, A_t\right) \leftarrow Q\left(S_t, A_t\right)+\alpha\left(q_t^{(\lambda)}-Q\left(S_t, A_t\right)\right)
$$
这是 $\operatorname{Sarsa}(\lambda)$ 的前向认识，使用它更新 $\mathrm{Q}$ 价值需要遍历完整的状态序列。
3.4.3 Backward View Sarsa( $\lambda)$
与 $\mathrm{TD}(\lambda)$ 的反向认识一样，引入效用追踪 (Eligibility Trace) 概念，不同的是这次的E值针对的不是一个状态，而是一个状态行为对:
$$
\begin{aligned}
& E_0(s, a)=0 \\
& E_t(s, a)=\gamma \lambda E_{t-1}(s, a)+1\left(S_t=s, A_t=a\right)
\end{aligned}
$$
它体现的是一个结果与某一个状态行为对的因果关系，与得到结果最近的状态行为对，以及那些在此之前频繁发生的状态行为对对得到这个结果的影响最大。
下式是引入 $\mathrm{ET}$ 概念的 $S A R S A(\lambda)$ 之后的Q值更新描述:
$$
\begin{aligned}
& \delta_t=R_{t+1}+\gamma Q\left(S_{t+1}, A_{t+1}\right)-Q\left(S_t, A_t\right) \\
& Q(s, a) \leftarrow Q(s, a)+\alpha \delta_t E_t(s, a)
\end{aligned}
$$
引入 ET概念，同时使用 $S A R S A(\lambda)$ 将可以更有效的在线学习，因为不必要学习完整的Episode，数据用完即可丢弃。ET通常也是更多应用在在线学习算法中(online algorithm)。

该算法同时还针对每一次状态序列维护一个关于状态行为对 $(S, A)$ 的 $E$ 表，初始时 $E$ 表值均为 0 。当agent首次在起点 $S_0$ 决定移动一步 $A_0$ (假设向右) 时，它被环境告知新位置为 $S_1$ ，此时发生如下事情:
首先，agent会做一个标记，使 $E\left(S_0, A_0\right)$ 的值增加 1，表明agent刚刚经历过这个事件 $\left(S_0, A_0\right)$;
其次，它要估计这个事件的对于解决整个问题的价值，也就是估计TD误差，此时依据公式结果为 0 ，说明agent认为在起点处向右走没什么价值，这个“没有什么价值”有两层含义：不仅说明在 $S_0$ 处往右目前对解决问题没有积极帮助，同时表明agent认为所有能够到达 $S_0$ 状态的状态行为对的价值没有任何积极或消极的变化。
随后，agen将要更新该状态序列中所有已经经历的 $Q(S, A)$ 值，由于存在 $E$ 值，那些在 $\left(S_0, A_0\right)$ 之前近期发生或频繁发生的 $(S, A)$ 的 $Q$ 值将改变得比其它 $Q$ 值明显些，此外 agent还要更新其 $E$ 值，以备下次使用。对于刚从起点出发的agent，这次更新没有使得任何 $Q$ 值发生变化，仅仅在 $E\left(S_0, A_0\right)$ 处有了一个实质的变化。随后的过程类似，agent有意义的发现就是对路径有一个记忆，体现在 $E$ 里，具体的 $Q$ 值没发生变化。这一情况直到agent到达终点位置时发生改变。此时agent得到了一个即时奖励 1，它会发现这一次变化 (从 $S_H$ 采取向上行为 $A_{u p}$ 到达 $\left.S_G\right)$ 价值明显，它会计算这个 TD误差为 1，同时告诉整个经历过程中所有 $(S, A)$ ，根据其与 $\left(S_H, A_{u p}\right)$ 的密切关系更新这些状态行为对的价值 $Q$ ，agent在这个状态序列中经历的所有状态行为对的 $Q$ 值都将得到一个非 0 的更新，但是那些在个体到达 $S_H$ 之前就近发生以及频繁发生的状态行为对的价值提升得更加明显。
在图示的例子中没有显示某一状态行为频发的情况，如果agent在寻路的过程中绕过一些弯，多次到达同一个位置，并在该位置采取的相同的动作，最终agent到达终止状态时，就产生了多次发生的 $(S, A)$ ，这时的 $(S, A)$ 的价值也会得到较多提升。也就是说，agent每得到一个即时奖励，同时会对所有历史事件的价值进行依次更新，当然那些与该事件关系紧密的事件价值改变的较为明显。这里的事件指的就是状态行为对。在同一状态采取不同行为是不同的事件。
当agent重新从起点第二次出发时，它会发现起点处向右走的价值不再是 0 。如果采用贪婪策略更新，agent将根据上次经验得到的新策略直接选择右走，并且一直按照原路找到终点。如果采用 $\epsilon$-贪婪策略更新，那么agent还会尝试新的路线。由于为了解释方便，做了一些约定，这会导致问题并不要求agent找到最短一条路径，如果需要找最短路径，需要在每一次状态转移时给agent一个负的奖励。
$\operatorname{Sarsa}(\lambda)$ 算法里在状态每发生一次变化后都对整个状态空间和行为空间的 $Q$ 和 $E$ 值进行更新，而事实上在每一个Episode里，只有agent经历过的状态行为对的 $E 才$ 可能不为 0 ，为什么不仅仅对该Episode涉及到的状态行为对进行更新呢? 理论上是可以仅对Episode里涉及的状态行为对的E和Q进行更新的，不过这要额外维护一个表，而往这个额外的表里添加新的状态行为对的E和 $Q$ 值比更新总的状态行为空间要麻烦，特别是在早期agent没有一个较好的策略的时候需要花费很长很长时间才能找到终点位置，这在一定程度上反而没有更新状态空间省时。不过随着学习深入、策略得到优化，此表的规模会变小。[5]

[1]: https://kelincc.cn/post/zi-ge-ji-eligibility-traces/
[2]: http://zuzhiang.cn/2019/10/10/sarsa/
[3]: https://zhuanlan.zhihu.com/p/262019592
[4]: https://www.guyuehome.com/33225
[5]: https://www.jianshu.com/p/de8b3fded19c
[6]: https://blog.csdn.net/weixin_37904412/article/details/81025539?spm=1001.2014.3001.5502

TODO:http://mapdic.com/archives/%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%85%AD--%E8%B5%84%E6%A0%BC%E8%BF%B9
