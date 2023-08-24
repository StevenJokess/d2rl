

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-06-17 01:46:48
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-06-17 01:47:12
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# ACKTR

actor-critic using Kronecker-factored trust region

ACKTR 是以 actor-critic 框架为基础，引入 TRPO 使算法稳定性得到保证，然后加上 Kronecker 因子分解以提升样本的利用效率并使模型的可扩展性得到加强。ACKTR 相比于 TRPO 在数据利用率和训练鲁棒性上都有所提升，因而训练效率更高。PPO 和 TRPO 一样以可信域算法为基础，以策略梯度算法作为目标更新算法，但 PPO 相比于 TRPO，只使用一阶优化算法，并对代理目标函数简单限定约束，实现过程更为简便但表现的性能更优。基于策略的深度强化学习发展历程如表 4 所示。

[1]: https://www.eefocus.com/article/402315.html
