# SACwA

SACwA算法是对SAC算法的改进版本，主要在于增加了一个自适应的温度参数（alpha），用于动态地调整策略优化中的熵项权重。这使得SACwA可以在不同的环境中自适应地调整探索和利用的权衡，从而在不同任务和环境中表现更加灵活和高效。SACwA还引入了一个新的目标网络更新策略，通过使用经验池中的数据进行目标网络的更新，从而提高了训练的稳定性和收敛速度。



[1]: https://github.com/ZhiqingXiao/rl-book/tree/master/en2022