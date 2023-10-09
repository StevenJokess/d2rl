# 课程

## 1. CS294

CS 294: Deep Reinforcement Learning, Fall 2017，是加州大学伯克利分校的强化学习课程，该课程往期内容前沿，组织较为随意，主要以实验室paper为主，但是，但是，但是！在Fall 2017的课程组织上有了非常重大的改进！（以前喵喵是不会直接推荐这门课程的，现在强烈推荐！）

CS294 Fall 2017 基本可以分为DRL介绍+模仿学习、model free、model based、Exploration+迁移+多任务+Meta-learning等四大部分。课程需要有一点强化学习和机器学习基础，建议先看完David Silver的视频，对强化学习概念与方法有一个基本的了解。尤其是bellman方程等内容，CS294并没有讲解，建议内容上互相补充。另外，这门课后半model based部分恰好是对david silver等其他课程、书籍里讲的比较少的内容的非常大的补充，不过这部分难度比较高（控制优化理论）。

CS294一共有四个很有趣的assignment，分别是：模仿学习（行为克隆和DAgger）、策略梯度（AC算法）、DQN和基于模型的Model Predictive Control(MPC)。assignment代码量不大，但是很具有探索性，能够在帮助你理解相关算法的同时让你对强化学习环境OpenAI Gym以及MuJoCo有一个简单的认识。

## 2. CS234

CS234: Reinforcement Learning，是斯坦福大学的强化学习课程，该课程从强化学习介绍与基础知识（MDP、MC、TD）开始，主要讲解了model free、Exploration和策略梯度。前半部分和David Silver的视频内容可以相互对照学习。有个遗憾是没有视频。

CS234有三个assignment，分别是：值迭代和策略迭代、DQN和R-max。其中assignment 1恰好是CS294 assignment所没有体现的部分，可以作为补充。assignment 2 的DQN框架写的很漂亮（虽然一部分是借鉴CS294的2333），值得详细阅读！[1]


[1]: https://zhuanlan.zhihu.com/p/33442519
