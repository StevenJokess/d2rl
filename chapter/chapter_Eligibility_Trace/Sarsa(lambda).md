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

[1]: https://kelincc.cn/post/zi-ge-ji-eligibility-traces/
[2]: http://zuzhiang.cn/2019/10/10/sarsa/
[3]: https://zhuanlan.zhihu.com/p/262019592
[4]: https://www.guyuehome.com/33225
[5]: https://www.jianshu.com/p/de8b3fded19c

TODO:http://mapdic.com/archives/%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%85%AD--%E8%B5%84%E6%A0%BC%E8%BF%B9
