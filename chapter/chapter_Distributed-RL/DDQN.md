

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-10-13 02:02:11
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-13 02:02:42
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 分布式深度Q网络

在训练过程中，DQN使用单个机器进行训练，这导致在实际中训练时间较长。为了充分利用计算资源，Nair A等人[17]提出一种分布式架构来加快算法的训练速度。它主要包括4个部分：①并行的行动者，算法采用N个不同的行动者，每个行动者复制一份 Q 网络，并在同一个环境中执行不同的动作，从而得到不同的经验；②经验回放存储机制，它将N个行动者与环境交互的经验存储到经验池中；③并行的学习者，算法采用N个学习者使用经验池存储的经验数据来计算损失函数的梯度，并发送到参数服务器中，从而对 Q 网络的参数进行更新；④参数服务器，用来接收学习者发送的梯度，并通过梯度下降的方式对Q网络参数进行更新。在49个Atari 2600游戏中，有41个游戏的基于分布式DQN算法的性能超过了DQN，并且在多种Atari 2600游戏中，分布式DQN算法的训练时间大大减少。[1]

[1]: https://www.infocomm-journal.com/znkx/article/2020/2096-6652/2096-6652-2-4-00314.shtml
