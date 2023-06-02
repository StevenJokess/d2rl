

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-05 19:49:51
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-05 20:29:17
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# vizdoom

## Doom

毁灭战士（Doom：  Defend  Line）：这是一款仿3D的第一人称射击游戏。游戏场景是在一个密闭的空间里，尽可能多地杀死怪物和保全自己，杀死的怪物越多，奖励就越多。AI玩家所能观察的，同人类玩家一样，只是一个第一人称的视野。

## vizdoom

VizDoom 是让强化学习模型进行Doom游戏的平台。清华大学人工智能研究院在Doom上的CrowdAI比赛上获得了 Visual Doom AI Competition 2018 Track1 的冠军，同时在 Visual Doom AI Competition 2018 Track2 以微弱劣势屈居第二。

相关视频：[2]



复杂游戏环境的状态和动作空间显著增大，更多地考虑 3D 环境的情况，并且涉及单智能体的多任务学习，或者多智能体之间的交互。Kempka M等人[25]提出将 ViZDoom 作为 DRL 的测试平台。ViZDoom采用第一视角的3D游戏环境，且游戏环境可以自由设计，以测试算法在不同任务上的性能。Lample G等人[26]提出了一种改进的DQN算法并将其应用于ViZDoom，该算法主要解决游戏中的两个任务，一个是在地图中导航搜寻敌人或者弹药，另一个是在发现敌人时采取射击行动，并且对每个任务分别采用一个网络，两个网络交替训练能够取得很好的效果。Dosovitskiy A等人[27]将一种有监督学习的方法应用于ViZDoom，使用高维感知流和低维测量流分别处理高维的原始图像以及与智能体当前状态有关的信息，并且训练好的模型可以被用于动态的指定目标。Pathak D等人[28]引入好奇心机制来解决 ViZDoom 游戏环境中奖励的稀疏性问题，将当前状态特征和动作输入前向模型中，对下一时刻的状态特征进行预测，并将预测误差作为内部奖励，鼓励智能体探索新奇的环境状态，同时还使用逆模型来提取只与当前动作有关的环境特征。针对 ViZDoom 游戏的复杂性，Wu Y 等人[29]提出了一种主从式课程 DRL 的方法，该方法引入了主从智能体的概念，其中一个主智能体被用来处理目标任务，多个从智能体被用来处理子任务，并且主从智能体可以使用不同的动作空间，同时引入课程学习，大大提高了算法的训练速度，显著地提高了智能体在游戏中的表现性能。

[1]: https://ml.cs.tsinghua.edu.cn/thuai/#/ai_frameworks
[2]: https://www.bilibili.com/video/BV1vV4y1p7R1/?spm_id_from=333.999.0.0&vd_source=bca0a3605754a98491958094024e5fe3
[3]: https://pdf-1307664364.cos.ap-chengdu.myqcloud.com/%E6%95%99%E6%9D%90/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0/%E3%80%8A%E7%99%BE%E9%9D%A2%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%AE%97%E6%B3%95%E5%B7%A5%E7%A8%8B%E5%B8%88%E5%B8%A6%E4%BD%A0%E5%8E%BB%E9%9D%A2%E8%AF%95%E3%80%8B%E4%B8%AD%E6%96%87PDF.pdf
[4]: http://www.infocomm-journal.com/znkx/article/2020/2096-6652/2096-6652-2-4-00314.shtml

[25]: KEMPKA M , WYDMUCH M , RUNC G ,et al. Vizdoom:a doom-based AI research platform for visual reinforcement learning[C]// 2016 IEEE Conference on Computational Intelligence and Games (CIG). Piscataway:IEEE Press, 2016: 1-8.[本文引用: 1]
[26]: LAMPLE G , CHAPLOT D S .Playing FPS games with deep reinforcement learning[C]// The 31st AAAI Conference on Artificial Intelligence.[S.l.:s.n.], 2017.[本文引用: 1]
[27]: DOSOVITSKIY A , KOLTUN V .Learning to act by predicting the future[J]. arXiv preprint, 2016,arXiv:1611. 01779.[本文引用: 1]
[28]: PATHAK D , AGRAWAL P , EFROS A A ,et al.Curiosity-driven exploration by self-supervised prediction[C]// The 34th International Conference on Machine Learning. New York:ACM Press, 2017.[本文引用: 1]
[29]: WU Y , ZHANG W , SONG K .Master-slave curriculum design for reinforcement learning[C]// The 28th International Joint Conference on Artificial Intelligence. New York:ACM Press, 2018: 1523-1529.[本文引用: 1]
