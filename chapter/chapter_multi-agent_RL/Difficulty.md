# 难点

在多智能体强化学习的系统中，每个智能体通过与环境进行交互获取奖励值（reward）来学习改善自己的策略，从而获得该环境下最优策略。

但在多智能体在应用的过程中，有很多困难，有以下几点：

维度爆炸：在单体强化学习中，需要存储状态值函数或动作-状态值函数。在多体强化学习中，状态空间变大，联结动作空间（联结动作是指每个智能体当前动作组合而成的多智能体系统当前时刻的动作，联结动作随智能体数量指数增长，因此多智能体系统维度非常大，计算复杂。
目标奖励确定困难：多智能体系统中每个智能体的任务可能不同，但是彼此之间又相互耦合影响。奖励设计的优劣直接影响学习到的策略的好坏。
不稳定性：在多智能体系统中，多个智能体是同时学习的。当同伴的策略改变时，每个智能体自身的最优策略也可能会变化，这将对算法的收敛性带来影响。
探索-利用：探索不光要考虑自身对环境的探索，也要对同伴的策略变化进行探索，可能打破同伴策略的平衡状态。每个智能体的探索都可能对同伴智能体的策略产生影响，这将使算法很难稳定，学习速度慢。
以上问题部分也在普通强化学习应用中存在，这里整理一些解决问题的方法和一些人的思考。

对于维度爆炸存在的问题一般解决从两个方面解决

1、状态空间优化
2、联合动作空间优化
对于第一点，状态空间简化一般由特征工程来处理，一般方法有PCA（Principal Component Analysis） ，一种常见的数据分析方式，常用于高维数据的降维，可用于提取数据的主要特征分量。以及LDA(线性判别算法)算法等，根据具体问题的不同算法也不尽相同。

联合动作空间优化有Mean Field MARL， MFMARL算法主要解决的是集中式多智能体强化学习中，联合动作


的维度随智能体数量n的增多极速扩大的情况。因为每个智能体是同时根据联合策略估计自身的值函数，因此当联合动作空间很大时，学习效率及学习效果非常差。为了解决这个问题，算法将值函数


转化为只包含邻居之间相互作用的形式






其中


表示智能体j邻居智能体的标签集，


表示邻居节点的个数。上式(4)对智能体之间的交互作用进行了一个近似，降低了表示智能体交互的复杂度，并且保留了部分主要的交互作用（近似保留邻居之间的交互，去掉了非邻居之间的交互）。虽然对联合动作


做了近似化简，但是状态信息


依然是一个全局信息。但这样也丧失了对各个智能体的精准刻画。还有一些靠对动作编码来解决动作多问题，试图在有限的位数的限制下表达更多的信息，但并没有实际减少联合动作的优化问题。还有就是通过

对于第二点对于目标奖励确定困难，工学中多是对具体问题具体分析，设计对应问题的奖励函数，但在奖励稀疏的环境下，有一些特别的算法以下是一些算法的举例：

Curiosity Driven：好奇心驱动是使用内在奖励鼓励agent探索更陌生的状态，平衡探索与利用，本质上是提高了样本的利用效率，主要分为两类，分别是基于状态计数的和基于状态预测误差的方法，前者比如这两篇文章Count-based exploration with neural density models、Exploration: A study of count-based exploration for deep reinforcement learning，后者比如Incentivizing exploration in reinforcement learning with deep predictive models、ICM（Curiosity-driven Exploration by Self- supervised Prediction）。

Hindsight Experience Replay（HER）：一般的强化学习方法对于无奖励的样本几乎没有利用，HER的思想就是从无奖励的样本中学习。HER建立在多目标强化学习的基础上，将失败的状态映射为新的目标


，使用


替换原目标


就得到了一段“成功”的经历（达到了目标


）。论文地址https://arxiv.org/abs/1707.01495。

Priority Experience Replay：PER是DQN中最有效的改进手段之一，通过改变样本采样的概率来提高样本利用率和训练速度。Prioritized DQN中使用与td-error大小成正比的采样概率，在稀疏奖励中还可以对正样本采用更高的采样概率，防止在大量无奖励样本中“淹没”。

对于第三点不稳定性，有一个在实践编程中最有效的方法就是cliping，就是限制td-error的范围，很好用。有论文研究此方面的问题（Deep Reinforcement Learning and the Deadly Triad https://arxiv.org/abs/1812.02648），但此多在术的层面上。在解决复杂环境的影响中

有Hierarchical Reinforcement Learning：分层强化学习，使用多层次的结构来学习不同层次的策略。把分层次解决复杂问题，把复杂劳动分解成多个简单劳动，使得算法更好收敛。也有优化操作会传播高估误差：DDQN（Double DQN）更加稳定，因为最优化操作会传播高估误差，所以同时训练两个Q network并选择较小的Q值用于计算TD-error，降低高估误差。还有Soft target update（软更新），用来稳定训练的方法，公式是


，其中 theta是使用梯度进行更新的网络参数，theta' 是使用了软更新的目标网络target network参数，tau略小于1。软更新让参数的更新不至于发生剧变，从而稳定了训练。还有其他算法，多是解决特定环境下的问题，并没有很强普适性。

对于第四点探索-利用，在探索不足的情况下有鼓励探索的好奇心机制paper: de Abril, Ildefons Magrans, and Ryota Kanai. "Curiosity-driven reinforcement learning with homeostatic regulation." f="https://arxiv.org/abs/1801.07440">arXiv preprint arXiv:1801.07440 (2018).也有论文讨论（Exploration Strategies in Deep Reinforcement Learning，https://lilianweng.github.io/lil-log/2020/06/07/exploration-strategies-in-deep-reinforcement-learning.html）这里贴一下原文的翻译，很有启发，可用在解决对应的问题。

探索策略可以分为两类，有向的和无向的。有向的探索策略考虑之前的历史，而无向的策略随机探索，不考虑之前的学习。这一节讨论三个部分，一是无向的探索，二是基于观测学习过程的有向探索，三是其他基于学习模型的有向探索。

1、Undirected Exploration——动作通过随机分布生成，不考虑学习过程。Thrun认为无向的探索不如有向的探索。这里介绍三种无向探索策略，即随机探索（random exploration），半均匀分布探索（semi-uniform distributed exploration）和玻尔兹曼分布探索（Boltzmann distributed exploration）。

用来衡量动作：






随机探索是指以均匀的概率随机生成动作。这种方法可能在不需要考虑探索成本的学习中使用，虽然这种情况实际中几乎不存在。

半均匀分布探索的生成动作的概率分布和随机探索的均匀分布不同，它的分子是一个0到1之间的数（均匀分布的分子是1），并且根据action的utility给概率加上项预先定义的概率


。


为0时就是完全随机探索，为1时就是完全利用。半均匀分布探索要比完全随机探索和玻尔兹曼分布探索都更好。






半均匀分布探索根据action的utility来决定将


分配到哪个action上，而玻尔兹曼分布探索采用另外的计算方式。


时就是完全利用（pure exploitation），


时就是随机探索。






2、Directed Exploration——在这里介绍四种基本的有向探索策略：Counter-based exploration基于计数的探索, Counter-based exploration with decay基于计数的带衰减的探索, Counter/Error-based exploration和 Recency-based exploration基于近因的探索

Counter-based exploration：动作用利用值和探索项的和来评估，这里的探索项是当前状态的计数和状态的期望计数的商。






Counter-based exploration with decay加了一个小于等于1的参数






Counter/Error-Based Exploration：






Recency-based exploration：






3、其他有向探索——Competence Maps、Interval Estimation、Multiple-Hypothesis Heuristics等

Competence Maps：使用一个辅助的数据结构来估计agent相信的程度，相信在哪个区域自己有足够的知识来做出好的决策。这个估计用来探索世界，选择的动作让agent去它认为有最小competence的区域。

Interval Estimation：学习的是动作价值以置信概率落在上下界中，但这种方法在实际中是有问题的，因为方法的复杂以及统计假设不能满足。

Multiple-Hypothesis Heuristics：对Q-learning算法的一个修正，是计算可行的。

BTW知乎的公式编辑太烦了

来源： https://zhuanlan.zhihu.com/p/133334392​

来源： https://zhuanlan.zhihu.com/p/56049023

来源： https://zhuanlan.zhihu.com/p/53474965

来源：https://zhuanlan.zhihu.com/p/342919579

来源：https://zhuanlan.zhihu.com/p/366174473

[1]: https://zhuanlan.zhihu.com/p/419924345
