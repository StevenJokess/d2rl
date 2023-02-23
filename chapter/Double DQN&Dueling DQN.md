

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-02-24 00:03:50
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-02-24 00:04:19
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# DQN改进算法

## 简介

DQN 算法敲开了深度强化学习的大门，但是作为先驱性的工作，其本身存在着一些问题以及一些可以改进的地方。于是，在 DQN 之后，学术界涌现出了非常多的改进算法。本章将介绍其中两个非常著名的算法：Double DQN 和 Dueling DQN，这两个算法的实现非常简单，只需要在 DQN 的基础上稍加修改，它们能在一定程度上改善 DQN 的效果。如果读者想要了解更多、更详细的 DQN 改进方法，可以阅读 Rainbow 模型的论文及其引用文献。


## 总结

在传统的 DQN 基础上，有两种非常容易实现的变式——Double DQN 和 Dueling DQN，Double DQN 解决了 DQN 中对值的过高估计，而 Dueling DQN 能够很好地学习到不同动作的差异性，在动作空间较大的环境下非常有效。从 Double DQN 和 Dueling DQN 的方法原理中，我们也能感受到深度强化学习的研究是在关注深度学习和强化学习有效结合：一是在深度学习的模块的基础上，强化学习方法如何更加有效地工作，并避免深度模型学习行为带来的一些问题，例如使用 Double DQN 解决值过高估计的问题；二是在强化学习的场景下，深度学习模型如何有效学习到有用的模式，例如设计 Dueling DQN 网络架构来高效地学习状态价值函数以及动作优势函数。
