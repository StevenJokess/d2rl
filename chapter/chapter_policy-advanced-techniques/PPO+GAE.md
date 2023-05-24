# PPO+GAE

PPO+GAE（Generalized Advantage Estimation），训练最稳定，调参最简单，适合高维状态 High-dimensional state，但是环境不能有太多随机因数。GAE会根据经验轨迹 trajectory 生成优势函数估计值，然后让Critic去拟合这个值。在这样的调整下，在随机因素小的环境中，不需要太多 trajectory 即可描述当前的策略。尽管GAE可以用于多种RL算法，但是她与PPO这种On-policy 的相性最好。

[1]: http://www.deeprlhub.com/d/166-muzerosacppotd3ddpgdqn
