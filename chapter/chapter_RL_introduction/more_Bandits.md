# Bandit问题的核心

Bandit的研究总是需要回答3个核心问题：

如何预测点击率 p
Contextual Bandits使用了线性模型： p=x^T\theta
我们当然也可以使用非线性模型，比如决策树、神经网络
如何衡量 p 的不确定性 \Delta ，按照 \tilde{p} \in [p-\Delta, p + \Delta] 对物品进行排序
UCB算法是Frequentist学派的代表，用置信区间来刻画
Thompson Sampling是Bayesian学派的代表，用概率分布来刻画
抓住了这个核心，我们看看之前的问题

冷启动有多冷：一条新闻只被推荐过几次，它的不确定性 \Delta 是很大的， \Delta 很大表示这个新闻还很冷，按照 \tilde{p} \in [p-\Delta, p + \Delta] 对物品进行排序是很有可能把新闻推荐出来的
算法和用户反馈的关系：用户只会点击算法选中的新闻，
利用已有历史信息(Exploitation)：推荐高质量的新闻，确保用户当前的体验，也就是 p 值较高的那些新闻
勇于探索 (Exploration)：有些新闻才出来，或者用户以前没点击过，不确定性高，但如果推荐出来用户也有可能会喜欢，也就是 \Delta 高的那些新闻
如何平衡Exploitation和Exploration：万变不离其宗，我们是 \tilde{p} \in [p-\Delta, p + \Delta] 对物品进行排序 ， [p-\Delta, p + \Delta] 是一个区间，如何在这个区间取值反映了我们对Exploitation v.s. Exploration的偏好[1]

# 上下文赌博机（Context Bandits）

Context Bandits（上下文赌博机）是一种在强化学习中使用的算法。它是一种多臂赌博机算法，能够根据不同的上下文条件对不同的臂（即策略）进行选择，从而使得总体收益最大化。

具体来说，Context Bandits算法通过学习每个臂在不同上下文条件下的收益情况，来更新每个臂的选择概率。这种方法可以适应不同的上下文条件，并在每个上下文条件下选择最佳的策略，从而获得最大的收益。

相比于传统的多臂赌博机算法，Context Bandits算法能够更好地处理现实中的多样性和复杂性，因为它能够自适应地选择不同的策略来适应不同的上下文条件。这使得它在许多应用中都表现出色，例如推荐系统、广告投放、搜索引擎等等。[3]



# anti-bandit

反弈者问题，是指在一个多臂赌博机（multi-armed bandit）场景中，智能体需要在多个选择中做出决策，以最大化累积奖励。与传统弈者问题不同的是，反弈者问题中，环境可能会根据智能体的行为进行调整，从而影响智能体的决策。智能体需要根据当前环境的变化，实时调整其行为策略，以应对环境的变化，并在不断的探索和利用之间进行平衡，以获得最优的累积奖励。

反弈者问题在实际应用中具有广泛的应用，例如在线广告投放、金融投资决策、医疗治疗方案选择等。在解决反弈者问题时，通常会使用各种强化学习算法，如Q学习、SARSA、深度Q网络（DQN）等，来帮助智能体在动态环境中做出最优的决策。[3]




[1]: https://zhuanlan.zhihu.com/p/32502139
[2]: https://zhuanlan.zhihu.com/p/32382432
[3]: https://cloud.tencent.com/edu/learning/live-2060

> https://chat.openai.com ;prompt:强化学习 anti bandit
