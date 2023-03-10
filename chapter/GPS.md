

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-13 01:09:10
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-13 02:06:35
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# GPS（Guided policy search）

Guided policy search的中文表达为指导策略搜索。从字面意思来理解，GPS是个策略搜索算法，但它不是普通的策略搜索，而是有指导的策略搜索。

一个很自然的问题是：谁来指导它进行策略搜索呢？

答案是：基于模型的最优控制器。

## 如何指导？

在回答这个问题之前，我们先理解下什么是策略搜索。通读过本专栏入门和进阶课程的知友应该对策略搜索这个词很熟悉。上节也讲过无模型的强化学习算法分为基于值函数的强化学习算法和基于直接策略搜索的强化学习算法。所谓直接策略搜索是指将策略进行参数化表示，然后在参数空间进行搜索，得到最优的策略。无模型的策略搜索靠的实实在在的与环境的交互数据，然后利用随机梯度的方法进行搜索。而GPS呢？它对参数的搜索依靠的是最优控制器。



下面，开始回答GPS如何指导策略网络进行参数搜索（即参数优化），从以下几个方面进行理解。


如图1.3 为Guided Policy Search方法的结构图。从图中，我们很容易看出GPS分为两个模块：左边的模块为最优控制器模块，右边的模块为策略搜索模块。

可以说，GPS=最优控制器+监督学习。

先说最优控制器模块：

在该模块中，控制器会运行当前的控制策略，并产生数据，然后利用这些产生的数据利用机器学习的方法，如回归的方法拟合控制方法。有了控制方程就可以利用经典的最优控制的方法来求解当前的最优控制率。经典的最优控制的方法包括变分法、庞特里亚金最大值原理和动态规划的方法。在GPS中，最常用的是动态规划的方法，如：

1. LQR：即线性二次型调节器
1. LQG：即线性二次高斯调节器
1. iLQG：即迭代线性二次高斯调节器
1. DDP：即微分动态规划


[1]: https://zhuanlan.zhihu.com/p/31084371
