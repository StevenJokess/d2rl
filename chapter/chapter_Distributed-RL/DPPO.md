

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-12 23:24:11
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-05-27 20:33:48
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# DPPO

谷歌的科学家在了解PPO后，认可了PPO确实是一个优秀的算法，他们结合自身强大的并行计算能力推出了分布式近端策略优化（DPPO， Distributed PPO[2]），也就是分布式PPO算法。由它的名字可知，它是对PPO算法的改进。这个算法讨论的一个重点就是如何只利用简单的回报函数，通过丰富多变的环境来学习到稳定的行为

原始的PPO算法通过完整的回报和估计策略优势，而为了便于使用分步更新的神经网络，DPPO使用了K阶回报的思想来估计策略优势。在DPPO算法中，数据的收集和梯度的计算被分布到多个任务对象中，思想类似于A3C算法。

我们先回忆一下PPO算法的主要思想，总的来说PPO是一套Actor-Critic结构，Actor想最大化JPPO（θ），而Critic想最小化LBL(φ)，其中

Actor就是在旧策略的基础上根据优势函数修改出新策略，优势大的时候，修改幅度大，让新策略更有可能发生。而且它附加了一个KL惩罚项，如果新策略和旧策略差太多，那么KL系数也会越大，从而避免了难以收敛的问题。DPPO算法分为两部分：“执行者”和“工作者”。“工作者”在每次迭代中依次做M步策略参数和B步值函数参数的更新。“执行者”部分从“工作者”部分收集梯度，当收集指定个数后，将它们的均值更新到总的参数中。对于每个“工作者”，每轮迭代中按当前策略执行T步，然后把它们按K个数据一份份分好。对于每K步样本，估计优势函数，然后分别通过梯度[插图]和[插图]更新相应参数。另外它会根据当前策略和之前策略的KL距离是否超出区域调节目标函数中的系数λ。

具体到代码的实现层面，可以对DPPO的分布式思想做以下总结：

- 实现裁剪的代理目标函数；
- 使用多个“工作者”平行在不同的环境中收集数据；
- “工作者”共享同一个中心PPO；
- “工作者”不会自己算PPO的梯度，不会像A3C那样推送梯度给中心网络，而只推送自己采集的数据给中心PPO；
- 中心PPO拿到多个“工作者”一定批量的数据后进行更新（更新时“工作者”停止采集）；
- 更新后，“工作者”使用最新的策略采集数据。

有实验将DPPO算法与TRPO和A3C算法在几个控制任务中的性能作了对比，显示DPPO比后两者有更好的表现，同时它还有很好的伸缩性。在所有情况下，DPPO都实现了与TRPO相同的性能，并且能够很好地适应不同数量的“工作者”，且它可以用于递归网络。

TODO:

代码：https://mofanpy.com/tutorials/machine-learning/reinforcement-learning/DPPO#%E7%AE%80%E5%8D%95%20PPO%20%E7%9A%84%E4%B8%BB%E7%BB%93%E6%9E%84

[1]: https://weread.qq.com/web/reader/62332d007190b92f62371ae?
[2]: https://mofanpy.com/tutorials/machine-learning/reinforcement-learning/DPPO#%E7%AE%80%E5%8D%95%20PPO%20%E7%9A%84%E4%B8%BB%E7%BB%93%E6%9E%84
