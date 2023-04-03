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
如何平衡Exploitation和Exploration：万变不离其宗，我们是 \tilde{p} \in [p-\Delta, p + \Delta] 对物品进行排序 ， [p-\Delta, p + \Delta] 是一个区间，如何在这个区间取值反映了我们对Exploitation v.s. Exploration的偏好

[1]: https://zhuanlan.zhihu.com/p/32502139

