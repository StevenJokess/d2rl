

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-03 03:12:26
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-12 15:59:56
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# Bandit问题的核心

## MAB的变体

多臂赌博机包含多种变体，例如：Contextual bandit，Adversarial bandit，Infinite-armed bandit，Non-stationary bandit，Dueling bandit，Collaborative bandit，Combinatorial bandit 等。

## 核心问题

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

## 其他相关资源

主要就是微软研究院（Microsoft Research）的Aleksandrs Slivkins的这本关于MAB的draft book Introduction to Multi-Armed Bandits （还未正式出版，他最近还一直在完善）： 这是我个人认为目前市面上最适合入门的MAB教科书。整本书的逻辑都很清晰，且数学derivation也尽可能都是从first principles出发，对许多结果的proof都是我见过的写的最简洁的，Slivkins本人对这个领域贡献也非常大，他对很多细节都有很深入的思考，所以才能写出这么neat的教学向内容吧。个人非常推荐，并将主要在MAB的内容介绍他的思路。

Slivkins之前，另一位MAB领域的大神Sebastien Bubeck（也在MSR...）的讲义Regret Analysis of Stochastic and Nonstochastic Multi-armed Bandit Problems可能是市面上唯一找的到的系统性阐述各方面MAB理论的讲义。 Bubeck自然也是MAB领域一位极具创造力和洞察力的大牛，不过个人认为他的这本讲义并没有Slivkins的书适合入门，因为Bubeck的讲义风格一如他写博客的风格，很多地方非常handwavy，即不给证明细节而跟你“简单聊聊”这些个结果和背后的intuition。。。所以建议有一定基础的同学再去读他的内容，这样可能反而收获会更大一些。[4]


[1]: https://zhuanlan.zhihu.com/p/32502139
[2]: https://zhuanlan.zhihu.com/p/32382432
[3]: https://cloud.tencent.com/edu/learning/live-2060
[4]: https://zhuanlan.zhihu.com/p/54695750

> https://chat.openai.com ;prompt:强化学习 anti bandit
