

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-26 01:10:37
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-05-26 22:52:57
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# PILCO

基于模型强化学习方法最大的挑战：模型误差

基于模型强化学习方法最大的缺点是通过数据学习到的模型存在模型误差。尤其是刚开始的时候，数据很少，利用很少的数据学到的模型必定不准确。利用不准确的模型去预测未知状态的值便会产生更大的误差。所以模型误差是基于模型强化学习算法最大的挑战。

如何解决这个挑战呢？PILCO

将模型误差考虑进去的基于模型的强化学习算法是PILCO。PILCO一般只需要几次到几十次便可以成功实现对单摆等典型非线性系统的稳定性控制，而对于同样的问题，基于无模型的强化学习则需要上万次。

PILCO的成功关键是：

PILCO解决模型偏差的方法不是集中于一个单独的动力学模型，而是建立了概率动力学模型，即动力学模型上的分布。也就是说，PILCO建立的模型并不是具体的某个确定性函数，而是建立一个可以描述一切可行模型（所有通过已知训练数据的模型）上的概率分布。该概率模型有两个目的：

第一， 它表达和表示了学习到的动力学模型的不确定性

第二， 模型不确定性被集成到了长期的规划和决策中。

接下来会详细推导PILCO算法及其发展。参考文献为：

1. PILCO： A Model-Based and Data-Efficient Approach to Policy Search（PILCO提出）

2. Efficient Reinforcement Learning using Gaussian Processes(PILCO一作的博士论文)

3. Data-Efficient Reinforcement Learning in Continuous-State POMDPs(PILCO扩展到部分可观马尔科夫)

4. Improving PILCO with Bayesian Neural Network Dynamics Models

基于这些参考文献，我会给大家详细介绍下PILCO的思想，敬请期待。

[1]: https://zhuanlan.zhihu.com/p/27537744
