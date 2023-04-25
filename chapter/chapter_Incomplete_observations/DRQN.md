# 结合RNN

对于时间序列信息, 深度Q网络的处理方法是加入经验回放机制. 但是经验回放的记忆能力有限, 每个决策点需要获取整个输入画面进行感知记忆。

Hausknecht等将长短时记忆网络与深度Q网络结合，提出深度递归Q 网络 (deep recurrent Q network, DRQN), 在部分可观测马尔科夫决策过程(partially observable Markov decision process, POMDP)中表现出了更好的鲁棒性, 同时在缺失若干帧画面的情况下也能获得很好的实验结果[1]


最终我们可以这样总结RNN在强化学习的潜力： RNN，作为一个动力学系统，本身表达了过去，现在和将来的联系。 这可以看作是部分的， 或者全部的世界模型。 而强化学习， 作为一个对未来收益的优化， 可以看作一个序列决策问题， 你对系统的过去现在和将来了解的越透彻，这个决策能力就越强， 因此RNN天生和强化学习有某种契合。 RNN的这个动力系统， 可以说部分的，或者全部的表达了世界模型，因此， 它非但是解决局部马尔科夫问题的利器，更在免模型和有模型的强化学习当中构建了一个桥梁。

## 拓展阅读

几个大家可以阅读的文章如下：

Bakker B. Reinforcement learning with long short-term
memory[C]//Advances in neural information processing systems. 2002

最早在强化学习里引入RNN的尝试， 主要是强调RNN可以解POMDP

Hausknecht, Matthew,
and Peter Stone. "Deep recurrent q-learning for partially observable mdps." CoRR, abs/1507.06527 7.1
(2015).

这一篇接着2002的文章， 主要是承接了2015 deepmind 在DQN的突破，强调那些信息并不全面的Atari Game， 可以通过RNN（LSTM）得到性能突破

Mirowski,
Piotr, et al. "Learning to navigate in complex environments." arXiv preprint arXiv:1611.03673 (2016)

导航领域的牛文， 介绍了在RNN（LSTM）下的深度强化学习里如何进一步加入监督学习， 获得性能突破

Wang J X, Kurth-Nelson Z, Tirumala D, et al. Learning to reinforcement
learn[J]. arXiv preprint arXiv:1611.05763, 2016.

小众的神文， wang xiao jing 大神介绍了一种基于RNN的强化元学习能力， 一种举一反三的能力。

Banino, Andrea, et
al. "Vector-based navigation using grid-like representations in artificial
agents." Nature 557.7705 (2018): 429.

最新的Nature文章， 介绍了通过监督学习引导RNN（LSTM）产生空间栅格细胞的能力

[1]: http://pg.jrj.com.cn/acc/Res/CN_RES/INDUS/2023/2/9/27c20431-8ed3-4562-83b5-5c82706f28a5.pdf
