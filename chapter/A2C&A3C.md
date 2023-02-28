

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-02-26 02:11:01
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-02-28 19:51:01
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# AC演化

## A2C 算法



## A3C 算法

A3C就是**异步**优势演员-评论家方法（Asynchronous Advantage Actor-Critic）：评论家学习值函数，同时有多个演员并行训练并且不时与全局参数同步。A3C旨在用于并行训练，是 on-policy 的方法。

在A3C算法中，有多个并行的环境，每个环境中都有一个智能体执行各自的动作和并计算累计的参数梯度。在一定步数后进行累计，利用累计的参数梯度去更新所有智能体共享的全局参数。


不同环境中的智能体可以使用不同的探索策略，会导致经验样本之间的相关性较小，可以提高学习效率。

A3C可根据critic所采用的算法进行同步/异步训练, 能适用于同步策略、异步策略。

使用在线Critic整合策略梯度, 降低训练样本的相关性, 在保证稳定性和无偏估计的前提下, 提升了采样效率和训练速度.[3]



## 算法大纲：

- 定义全局参数 $\theta$ 和 $w$ 以及特定线程参数 $\theta^{\prime}$ 和 $w^{\prime}$ 。
- 初始化时间步 $t=1$ 。
- 当 $T<=T_{\max}$ :
- 重置梯度: $d \theta=0$ 并且 $d w=0$ 。
- 将特定于线程的参数与全局参数同步: $\theta^{\prime}=\theta$ 以及 $w^{\prime}=w$ 。
- 令 $t_{s t a r t}=t$ 并且随机采样一个初始状态 $s_t$ 。
- 当 $\left(s_{t} !=\right.$ 终止状态 $)$ 并 $t-t_{\text {start }}<=t_{\max }$ :
- 根据当前线程的策略选择当前执行的动作 $a_t \sim \pi_{\theta^{\prime}}\left(a_t \mid s_t\right)$ ，执行动作后接 收回报 $r_t$ 然后转移到下一个状态 $s_{t+1}$ 。
- 更新 $t$ 以及 $T: t=t+1$ 并且 $T=T+1$ 。
- 初始化保存累积回报估计值的变量
- 对于 $i=t_1, \ldots, t_{\text {start }}$ :
$-r \leftarrow \gamma r+r_i ;$ 这里 $r$ 是 $G_i$ 的蒙特卡洛估计。
- 累积关于参数 $\theta^{\prime}$ 的梯度: $d \theta \leftarrow d \theta+\nabla \theta^{\prime} \log \pi \theta^{\prime}\left(a_i \mid s_i\right)\left(r-V w^{\prime}\left(s_i\right)\right)$;
- 累积关于参数 $w^{\prime}$ 的梯度:
$d w \leftarrow d w+2\left(r-V w^{\prime}\left(s_i\right)\right) \nabla w^{\prime}\left(r-V w^{\prime}\left(s_i\right)\right)$.
- 分别使用 $d \theta$ 以及 $d w$ 异步更新 日以及 $w$ 。

## 优势函数?

$A(s, a)=Q(s, a)-V(s)$ 是为了解决基于价值方法具有高变异性。 它代表着与该状态下采取的平均行动相比所取得的进步。

- 如果 $A(s, a)>0$ : 梯度被推向了该方向
- 如果 $A(s, a)<0$ : (我们的action比该状态下的平均值还差) 梯度被推向了反方


但是这样就需要两套价值函数，所以可以使用**时序差分方法**做估计： $A(s, a)=r+\gamma V\left(s^{\prime}\right)-V(s)$ 。



[2]: https://www.cnblogs.com/kailugaji/p/16140474.html
[3]: http://www.c-s-a.org.cn/html/2020/12/7701.html#outline_anchor_19
[4]: https://aistudio.baidu.com/aistudio/projectdetail/54249
