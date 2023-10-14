

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

强化学习已经取得了像AlphaGo这样的成就。强化学习落地怎么样？

这篇博客讨论上下文老虎机，尤其是微软的决策服务(Decision Service). 这方面理论研究深入，实践结果丰富；在近期几个实际问题上，取得双位百分数的性能提升；可以说是强化学习落地的成熟技术。上下文老虎机适用于广泛的应用场景；在推荐上有成功应用；值得开发其它方面的应用。

上下文老虎机基于上下文信息有效地选择动作；比如，在新闻推荐中，通过用户的历史活动、内容的描述信息和分类等用户和新闻的上下文信息，帮用户选择新闻。上下文老虎机是一种多臂老虎机(Multi-Arm Bandits)，是简化版的强化学习：没有状态转移。

下表列了几个上下文老虎机的应用例子。


不过这里有两个挑战。1）部分反馈。只有选择过的动作有奖赏信息，没有探索过的动作则没有。2）延迟的奖赏。执行一个动作后，奖赏信息可能很久才出现。这样，实现上下文老虎机一般会面对如下问题：1）部分反馈和偏差；2）不正确的数据收集；3）环境的改变；以及，4）脆弱的监视和调试。

A/B测试是一种解决部分反馈的探索方法。它按照一定比例在随机的用户流量上运行、测试两个策略。这样，对数据的需求就随着策略的个数线性增长。上下文老虎机则可以用相同的数据测试、优化指数个策略。同时，它并不要求先部署策略再测试，这样可以节省大量的商业和工程上的开销。这样的能力我们称之为多情景测试(multiworld testing, 缩写为MWT). 监督学习不支持探索，也就不支持上下文的情况，以及部分反馈和偏差。

决策服务定义了四个系统抽象模块：用于收集数据的探索(Explore)模块、用于正确记录数据的记录(Log)模块、用于学习好模型的学习(Learn)模块、以及用于在应用程序中部署模型的部署(Deploy)模块。通过上下文老虎机和策略评估等技术，提供多情景测试MWT，以完成有效的上下文学习环路。


上图展示了决策服务的体系结构。客户函数库(Client Library)实现了多种探索策略用以实现探索抽象模块。它与应用程序(App)接口，接受上下文特征和事件关键字作为输入，然后输出动作。它把数据传送给记录模块，进而由连接服务(Join Service)在决策时正确地记录数据。记录模块把探索数据传送给线上学习器(Online Learner)和存储器(Store)。探索和记录模块用来解决问题1）部分反馈和偏差以及问题2）不正确的数据收集。线上学习器根据上下文老虎机的探索数据提供线上学习，用以实现学习抽象模块。它连续地把数据包括进来，并以可预设的频率为部署模块记录检查点。这用来解决问题3）环境的改变以及问题4）脆弱的监视和调试。存储器用来存储数据和模型，用以实现部署抽象模块。离线学习器(Offline Learner)利用数据进行离线实验，用反事实(counterfactual)正确的方式，完成超参数调优、评估其它学习算法或策略类型、更改奖赏度量等等。特征生成器(Feature Generator)为某类内容自动生成特征。

通过这样的设计而部署的系统，具有反应灵敏、可复制、可扩展、容错、灵活等特性。决策服务提供几种部署方案：1) 部署在用户自己的Azure账号里；2) 部署在微软的Azure账号里，以便与其他用户共享资源；3) 以本地模式部署在用户的机器里。

下面列举几个决策服务的应用场景。MSN基本是新闻推荐问题；其前端服务器根据用户的请求把主页的新闻排序。实验表明有超过25%的点击率(click through rate，缩写为CTR)提升。Complex为新闻推荐视频，也推荐最受欢迎的新闻，并取得了超过30%的性能提升。TrackRevenue为应用程序里面的广告推荐登陆页面，从而最大化回报，并取得了18%的性能提升。Toronto为技术支持问题提供答案以减轻负载。云服务中不反应的虚拟机可能需要重启；Azure Compute优化等待时间，并减少19%的浪费时间。

这篇博客基于Agarwal et al. (2016). Li et al. (2010)把个性化新闻推荐定义为基于上下文老虎机问题，并给出算法。更多强化学习应用参考Li (2019)及博客《强化学习应用场景》。

Decision Service获得了SIGAI首届Industry Award; 网址为https://sigai.acm.org/awards/industry_award.html. 微软在ICML 2019 Expo Day组织了Real World Reinforcement Learning Workshop; 网址为https://vowpalwabbit.github.io/ icml2019/.

参考文献

Agarwal, A., Bird, S., Cozowicz, M., Hoang, L., Langford, J., Lee, S., Li, J., Melamed, D., Oshri, G., Ribas, O., Sen, S., and Slivkins, A. (2016). Making contextual decisions with low technical debt. ArXiv.

Li, L., Chu, W., Langford, J., and Schapire, R. E. (2010). A contextual-bandit approach to personalized news article recommendation. In WWW.

Li, Y. Reinforcement Learning Applications. ArXiv, 2019.[5]

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
[5]: https://zhuanlan.zhihu.com/p/82566742

> https://chat.openai.com ;prompt:强化学习 anti bandit
