

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-21 23:48:20
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-21 23:48:25
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# AlphaGo

AlphaGo 2016 版的训练分为三步：第一，随机初始化策略网络 $π(a|s; θ)$ 之后，用行为克隆(Behavior Cloning) 从人类棋谱中学习策略网络；第二，让两个策略网络自我博弈，用REINFORCE 算法改进策略网络；第三，基于已经训练好的策略网络，训练价值网络

**第一步：行为克隆**：一开始的时候，策略网络的参数都是随机初始化的。假如此时
直接让两个策略网络自我博弈，它们会做出纯随机的动作。它们得随机摸索很多很多次，
才能做出合理的动作。假如一上来就用REINFORCE 学习策略网络，最初随机摸索的过
程要花很久。这就是为什么 AlphaGo 2016 版基于人类专家的知识初步训练一个策略网络。


[1]: https://www.math.pku.edu.cn/teachers/zhzhang/drl_v1.pdf
