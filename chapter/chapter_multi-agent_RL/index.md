

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-22 02:22:18
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-12 20:55:28
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# MARL_index

介绍一些CTDE比较典型的算法有COMA [2]，VDN [3]，QMIX [4]等

先MADDPG, 再VDN, QMIX, 从目前我的经验来看，QMIX和MATD3(TD3 CTDE版本，很容易改)或者MADDPG(离散加连续)已经能解决大部分问题了，包括不限于这一堆MA Benchmark。我个人也更喜欢这类样本更加高效的off policy算法。

关于MARL的入门个人感觉主要有以下几个方面：

首先是强化学习的基本知识，DP、MC、TD，以及Q-learning，SARSA，PG，AC这些。这些要搞清楚，不能停留在似懂非懂，不然的话DRL是搞不懂的，更别说MARL了。

然后在此基础上，要对深度强化学习里value-based的算法，比如DQN非常熟悉；以及对actor-critic based算法，比如DDPG和PPO等算法。要搞清楚每个算法最重要的点在哪里，并且对这些经典的DRL算法最好自己复现过关键代码，这样后面才能更好的理解MARL算法，毕竟MARL很多思想是由single-agent拓展到multi-agent的。

对于MARL领域，首先推荐看一两篇综述，个人感觉下面这篇综述是不错的，里面对MARL进行了基本的分类，并对每一类介绍了部分前期的经典工作。大概浏览一下，就能对MARL存在的一些关键问题和部分经典工作的思想有个直观的感受。

Is multiagent deep reinforcement learning the answer or the question? A brief survey
然后就是按照类别阅读一些经典论文了：

对于value-decomposition based的文章，有VDN、QMIX、Qatten、QPLEX、ROMA、RODE等，其中部分文章存在争议，但还是可以了解一下基本思想的，其中对于QMIX这个算法，最好能把核心代码自己复现一下，对提升MARL的认知有很大帮助！对于这个大方向似乎很难再有突破性的工作了，PyMARL2这个框架里的QMIX的性能已经非常不错了，尤其TD(λ)对算法性能的提升是非常大的。
对于actor-critic based的文章，MADDPG、COMA、MAAC、MAPPO等。其中建议把MADDPG的核心代码复现一下。MAPPO的全文建议都看一下（挺长的），虽然文章里有些实验效果可能不太好复现，但里面有些trick是值得学习的。
对于communication的文章，经典的有RIAL/DIAL、CommNet、BiCNet、ATOC、NCC等。不过通信的文章近来好像比较少见了。个人感觉，在通信里很关键的一个研究点是如何对智能体进行合适的动态的分组通信，也就是如何学习一种动态变化的通信拓扑结构，来尽可能提升multi-agent之间的协作效果。有文章做过相关的工作，比如汪军老师挂名的一篇LSC等，但目前似乎没有很好的动态通信分组的方式。
这些是一些基本的分类及其经典工作，同样还有很多其他研究类别，比如前两年比较多的基于GNN、GAT的一些MARL的研究工作，包括后来细化出来的role-based的相关工作。此外，还有multi-task、few-shot learning、迁移性、课程学习等等一些细分的研究方向，每个方向都有一些延续性的工作在做。最近，offline RL在整个RL领域都比较火，offline MARL也是值得关注的一个点。最后，基于transformer的决策大模型目前是学术界比较火的，MARL领域似乎还没有相关的工作，这块工作可能需要很高的算力才能做好，目前可能华为的诺亚决策推理实验室等大公司的研究部门正在做吧。

下面是对MARL领域的一些个人看法：

个人感觉MARL领域短时间内难有落地，而且这两年领域内的有价值的文章似乎在变少。一直以来都流行的CTDE框架，在工业界似乎少有类似的落地场景。自动驾驶对RL目前还在探索阶段，可能只有很少的公司的小部分业务会落地；游戏领域似乎在对MARL逐渐失去兴趣，这也是通过今年秋招得出来的结论。目前MARL可能会在某些场景用作优化，还有就是推荐系统的机制设计可能会探索一下MARL。总而言之，MARL的落地目前非常有限；虽然还有很多研究难题亟待解决，但这两年的研究似乎也遇到了瓶颈。

以上结论，是根据我近一年多对MARL领域的研究，以及今年秋招过程中与工业界各个方向工程师的交流得出的。这些结论可能存在某些偏差，同时本人很乐意接受同行前辈们的交流和指正。[3]

```toc
:maxdepth: 2
MAS
multi-agent_DRL_intro

MARL
MiniMax-Q
NashQ
IPPO
MADDPG
COMA
VDN
QMIX
self-play
Applications
```

[3]: https://zhuanlan.zhihu.com/p/587993058
