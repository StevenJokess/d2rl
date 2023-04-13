

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-12 20:38:30
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-12 20:40:24
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 多智能体深度强化学习

随着深度学习的发展，利用神经网络的强大表达能力来搭建逼近模型（value approximation）和策略模型（常见于 policy-based 的 DRL 方法）。深度强化学习的方法可以分为基于值函数（value-based）和基于策略（policy-based）两种，在考虑多智能体问题时，主要的方式是在值函数的定义或者是策略的定义中引入多智能体的相关因素，并设计相应的网络结构作为值函数模型和策略模型，最终训练得到的模型能够适应（直接或者是潜在地学习到智能体相互之间的复杂关系），在具体任务上获得不错的效果。

2.1 policy-based 的方法

在完全合作的 setting 下，多智能体整体通常需要最大化全局的期望回报。前面提到一种完全集中式的方式：通过一个中心模块来完成全局信息的获取和决策计算，能够直接地将适用于单智能体的 RL 方法拓展到多智能体系统中。但通常在现实情况中，中心化的控制器（centralized controller）并不一定可行，或者说不一定是比较理想的决策方式。而如果采用完全分布式的方式，每个智能体独自学习自己的值函数网络以及策略网络、不考虑其他智能体对自己的影响，无法很好处理环境的不稳定问题。利用强化学习中 actor-critic 框架的特点，能够在这两种极端方式中找到协调的办法。
