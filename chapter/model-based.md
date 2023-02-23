

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-02-23 20:09:19
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-02-23 20:12:49
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 模型预测控制

## 简介

之前几章介绍了基于值函数的方法 DQN、基于策略的方法 REINFORCE 以及两者结合的方法 Actor-Critic。它们都是无模型（model-free）的方法，即没有建立一个环境模型来帮助智能体决策。而在深度强化学习领域下，基于模型（model-based）的方法通常用神经网络学习一个环境模型，然后利用该环境模型来帮助智能体训练和决策。利用环境模型帮助智能体训练和决策的方法有很多种，例如可以用与之前的 Dyna 类似的思想生成一些数据来加入策略训练中。本章要介绍的模型预测控制（model predictive control，MPC）算法并不构建一个显式的策略，只根据环境模型来选择当前步要采取的动作。

[1]: https://hrl.boyuai.com/chapter/3/%E6%A8%A1%E4%BB%BF%E5%AD%A6%E4%B9%A0
