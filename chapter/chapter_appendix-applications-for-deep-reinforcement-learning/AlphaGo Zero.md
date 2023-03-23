

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-22 00:05:23
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-22 00:10:47
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# AlphaGo Zero

$$
\boldsymbol{\theta}_{\text {new }} \leftarrow \boldsymbol{\theta}_{\text {now }}-\beta \cdot \frac{1}{n} \sum_{t=1}^n \nabla_{\boldsymbol{\theta}} H\left(\boldsymbol{p}_t, \pi\left(\cdot \mid s_t \boldsymbol{\theta}_{\text {now }}\right)\right) .
$$

此处的 $\beta$ 是学习率。


更新价值网络：训练价值网络的方法与 AlphaGo 2016版本基本一样, 都是让 $v\left(s_t ; \boldsymbol{w}\right)$ 拟合回报 $u_t$ 。定义回归问题：

$$
\min _{\boldsymbol{w}} \frac{1}{2 n} \sum_{t=1}^n\left[v\left(s_t ; \boldsymbol{w}\right)-u_t\right]^2
$$

设价值网络 $v$ 当前参数是 $\boldsymbol{w}_{\text {now }}$ 。用价值网络做预测: $\widehat{v}_t=v\left(s_t ; \boldsymbol{w}_{\text {now }}\right), \forall t=1, \cdots, n$ 。做 一次梯度下降更新 $w$ :

$$
\boldsymbol{w}_{\text {new }} \leftarrow \boldsymbol{w}_{\text {now }}-\alpha \cdot \frac{1}{n} \sum_{t=1}^n\left(\widehat{v}_t-u_t\right) \cdot \nabla_{\boldsymbol{w}} v\left(s_t ; \boldsymbol{w}_{\text {now }}\right) .
$$

训练流程：随机初始化策略网络参数 $\theta$ 和价值网络参数 $w$ 。然后让 MCTS 自我溥 恋, 玩很多局游戏; 每完成一局游戏, 更新一次 $\theta$ 和 $w$ 。训练的具体流程就是重复下面 三个步骤直到收敛：

1. 让 MCTS 自我博恋, 完成一局游戏, 收集到 $n$ 个三元组: $\left(s_1, \boldsymbol{p}_1, u_1\right), \cdots,\left(s_n, \boldsymbol{p}_n, u_n\right)$ 。
2. 按照公式 (18.2) 做一次梯度下降, 更新策略网络参数 $\boldsymbol{\theta}$ 。
3. 按照公式 (18.3) 做一次梯度下降, 更新价值网络参数 $w$ 。


[1]: https://www.math.pku.edu.cn/teachers/zhzhang/drl_v1.pdf 18.3.2