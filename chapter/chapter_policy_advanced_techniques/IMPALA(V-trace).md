

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-06-04 18:24:26
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-11 19:50:27
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# IMPALA

IMPALA：Importance Weighted Actor-Learner Architectures，模型与GA3C：基于GPU的并行强化学习算法很相似。
但是引入Value纠错模块V-trace，模型可以接受更多的延迟(policy-lag)，实现离线学习(off-policy)，拥有更大的吞吐量。

## 模型结构

IMPALA的基础模型结构与A2C/A3C：并行强化学习算法一致，经典的Actor-Critic结构，输出包含policy π(.|s) 和 state Value V(s)。


每一个Actor和Learner都有此网络结构，但是参数不同。

## 学习过程

1. 每个Actor单独定期地从Learner同步参数，然后进行数据收集(s, a, r, s')。
2. 所有Actor收集的数据都会即时存储到数据采样队列(queue)里。
3. 当队列的数据达到mini-batch size时，Learner开始梯度学习，并更新其参数。
4. ** Actor与Learner互不干扰**，Actor定期从Learner同步参数，Learner定量学习更新参数。
1. Learner也可以是分布式集群，这种情况下，Actor需要从集群同步参数。
2. Actor一般使用CPU，Learner使用GPU。


## 损失函数

损失函数与A3C类似，value loss、policy loss 和 熵正则三部分叠加在一起，构成整体的损失函数。
计算损失函数时一般前面加上系数超参数，注意策略梯度和熵是最大化，其相关系数为负值。

 impala paper

策略梯度中v_s的细节见V-trace，ρ_s为重要性权重：
 impala paper

## V-trace

因为Actor收集数据时的policy与Learner学习时的policy不一定一致;
V-trace就是针对不同采样数据时的policy，设计不同重要性权重，纠正此误差。
 impala paper

核心与PPO：近端策略优化深度强化学习算法类似，通过重要性权重π/μ控制数据贡献，同时通过超参数ρ和c_i来控制函数的收敛性：
 impala paper

一个特例就是当π = μ，超参数均为1，此时V-trace就是on-policy n-steps学习：
 impala paper

## 模型特点

1. 与A2C相比，Actor采集数据无需等待，并由GPU快速统一学习。
2. 与A3C相比 ，Actor无需计算梯度，只需收集数据，数据吞吐量更大。
3. 与GA3C相比，引入V-trace策略纠错，同时接受更大延迟，方便大规模分布式部署Actors。
4. 框架拓展方便，支持多任务学习。
5. 但是当场景包含很多终止条件的Episode，而又对这些终止(Terimal)敏感时，不管是在Actor收集数据时，还是在Learner梯度学习时，分段处理长短不一的Episode，都会降低IMPALA的性能，影响其流畅性；所以场景最好是Episode不会终止或者是对终止不敏感。

[1]: https://aistudio.baidu.com/projectdetail/548980

https://zhuanlan.zhihu.com/p/58226117
