

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-02-26 17:20:19
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-02 16:31:10
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 模型预测控制

本章节主要介绍模型预测控制方法的思路、带有轨迹采样的概率集成（PETS）以及其代码实践。

## 简介

之前几章介绍了基于值函数的方法 DQN、基于策略的方法 REINFORCE 以及两者结合的方法 Actor-Critic。它们都是无模型（model-free）的方法，即没有建立一个环境模型来帮助智能体决策。而在深度强化学习领域下，基于模型（model-based）的方法通常用神经网络学习一个环境模型，然后利用该环境模型来帮助智能体训练和决策。利用环境模型帮助智能体训练和决策的方法有很多种，例如可以用与之前的 Dyna 类似的思想生成一些数据来加入策略训练中。本章要介绍的**模型预测控制**（model predictive control，MPC）算法并不构建一个显式的策略，只根据环境模型来选择当前步要采取的动作。

## 打靶法

首先，让我们用一个形象的比喻来帮助理解模型预测控制方法。假设我们在下围棋，现在根据棋盘的布局，我们要选择现在落子的位置。一个优秀的棋手会根据目前局势来推演落子几步可能发生的局势，然后选择局势最好的一种情况来决定当前落子位置。

模型预测控制方法就是这样一种迭代的、基于模型的控制方法。值得注意的是，MPC 方法中不存在一个显式的策略。具体而言，MPC 方法在每次采取动作时，首先会**生成一些候选动作序列**，然后根据当前状态来确定每一条候选序列能得到多好的结果，最终选择结果最好的那条动作序列的第一个动作来执行。因此，在使用 MPC 方法时，主要在两个过程中迭代，一是根据历史数据学习环境模型，二是在和真实环境交互过程中用环境模型来选择动作。

## PETS 算法

**带有轨迹采样的概率集成**（probabilistic ensembles with trajectory sampling，PETS）是一种使用 MPC 的基于模型的强化学习算法。在 PETS 中，环境模型采用了集成学习的方法，即会构建多个环境模型，然后用这多个环境模型来进行预测，最后使用 CEM 进行模型预测控制。接下来，我们来详细介绍模型构建与模型预测的方法。

在强化学习中，与智能体交互的环境是一个动态系统，所以拟合它的环境模型也通常是一个动态模型。我们通常认为一个系统中有两种不确定性，分别是**偶然不确定性**（aleatoric uncertainty）和**认知不确定性**（epistemic uncertainty）。偶然不确定性是由于系统中本身存在的随机性引起的，而认知不确定性是由“见”过的数据较少导致的自身认知的不足而引起的，如图 16-1 所示。

pics

在 PET 算法中，环境模型的构建会同时考虑到这两种不确定性。首先，我们定义 环境模型的输出为一个高斯分布，用来捕捉偶然不确定性。令环境模型为 $\hat{P}$ ，其 参数为 $\theta$ ，那么基于当前状态动作对 $\left(s_t, a_t\right)$ ，下一个状态 $s_t$ 的分布可以写为

$$
\hat{P}\left(s_t, a_t\right)=\mathcal{N}\left(\mu_\theta\left(s_t, a_t\right), \Sigma_\theta\left(s_t, a_t\right)\right)
$$

这里我们可以采用神经网络来构建 $\mu_\theta$ 和 $\Sigma_\theta$ 。这样，神经网络的损失函数则为

$$
\mathcal{L}(\theta)=\sum_{n=1}^N\left[\mu_\theta\left(s_n, a_n\right)-s_{n+1}\right]^T \Sigma_\theta^{-1}\left(s_n, a_n\right)\left[\mu_\theta\left(s_n, a_n\right)-s_{n+1}\right]+\log \operatorname{det} \Sigma_\theta\left(s_n, a_n\right)
$$

这样我们就得到了一个由神经网络表示的环境模型。在此基础之上，我们选择用集成（ensemble）方法来捕捉认知不确定性。具体而言，我们构建个网络框架一样的神经网络，它们的输入都是状态动作对，输出都是下一个状态的高斯分布的均值向量和协方差矩阵。但是它们的参数采用不同的随机初始化方式，并且当每次训练时，会从真实数据中随机采样不同的数据来训练。

有了环境模型的集成后，MPC 算法会用其来预测奖励和下一个状态。具体来说，每一次预测会从个模型中挑选一个来进行预测，因此一条轨迹的采样会使用到多个环境模型，如图 16-2 所示。

pics

## PETS算法实践

有了环境模型之后，我们就可以定义一个`FakeEnv`，主要用于实现给定状态和动作，用模型集成来进行预测。该功能会用在 MPC 算法中。

code

定义经验回放池的类`Replay Buffer`。与之前的章节对比，此处经验回放缓冲区会额外实现一个返回所有数据的函数。

code

大功告成！让我们在倒立摆环境上试一下吧，以下代码需要一定的运行时间。

code

可以看出，PETS 算法的效果非常好，但是由于每次选取动作都需要在环境模型上进行大量的模拟，因此运行速度非常慢。与 SAC 算法的结果进行对比可以看出，PETS 算法大大提高了样本效率，在比 SAC 算法的环境交互次数少得多的情况下就取得了差不多的效果。

## MPC的评价：优点和缺点

- 优点：MPC是 MIT 之前开源的主要算法。算法对环境进行建模后，在每个时间步求解优化问题以找到最优的控制信号。
- 缺点：其效果依赖于环境模型的建模准确度，并且在实际部署过程中需要耗费比较大的算力去求解最优的控制信号。[2]

## 总结

通过学习与实践，我们可以看出模型预测控制（MPC）方法有着其独特的优势，例如它不用构建和训练策略，可以更好地利用环境，可以进行更长步数的规划。但是 MPC 也有其局限性，例如模型在多步推演之后的准确性会大大降低，简单的控制策略对于复杂系统可能不够。MPC 还有一个更为严重的问题，即每次计算动作的复杂度太大，这使其在一些策略及时性要求较高的系统中应用就变得不太现实。

[1]: https://hrl.boyuai.com/chapter/3/%E6%A8%A1%E5%9E%8B%E9%A2%84%E6%B5%8B%E6%8E%A7%E5%88%B6/
[2]: https://ai.baidu.com/support/news?action=detail&id=2612
