

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-10-03 00:59:05
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-03 01:08:08
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 调参

调参指调整参数以获得更好的效果，其目的在于得到更好的模型：修复出现的错误、提高神经网络训练精确度。

模型的最优参数依赖于许多场景，在模型评估和选择中，除了算法的选择，还需要对其参数进行设定，调参便是完成参数设定的过程。目前普遍做法是，对参数选择一个范围和变化步长，如在 [0 , 0.2] 之间以 0.05 为步长，这样可以得到的待选参数值有 5 个，理想值便会从这 5 个候选值中获得，虽然这样获得参数值非最佳值，但可在计算开销和性能估计之间折中。[2]

## 具体措施

1. 数据预处理：如果输入的state是图片，那么可以进行resize和变成灰度图。进行resize的时候，尽可能保持原来的尺寸，这样进行输出action的时候能够保持尺度一致性。至于要不要变成灰度图，需要看看输入的state是否有较好的边缘，颜色是否携带较多信息等等。
1. weight初始化：采用随机或者normal distribution，均值为0，方差为1，可以设得小一些。太大的话，收敛会比较慢。当然，最好的初始化是用已经训练好的模型进行初始化，这个也叫做迁移学习。
1. Optimizer的选择：一般使用Adam效果会比较好，也不需要自己设置参数。RMSProp和Adam效果差不多，不过收敛速度慢一些。
1. 训练过程：训练过程可以把episode的reward打出来看，要是很久都没有改进，基本上可以停了，马上开始跑新的。

## DRL里的DL并不够D

过大、过深的神经网络不适合DRL。

深度学习可以在整个训练结束后再使用训练好的模型。而强化学习需要在几秒钟的训练后马上使用刚训好的模型。这导致DRL只能用比**较浅的网络**来保证快速拟合（10层以下）并且强化学习的训练数据不如有监督学习那么稳定，无法划分出训练集测试集去避免过拟合，因此DRL也不能用太宽的网络（超过1024），避免参数过度冗余导致过拟合。

dropout：在DL中得到广泛地使用，可惜不适合DRL。如果非要用，那么也要选择非常小的 dropout rate（0~0.2），而且要注意在使用的时候关掉dropout。我不用dropout。好处：在数据不足的情况下缓解过拟合；像Noisy DQN那样去促进策略网络探索坏处：影响DRL快速拟合的能力；略微增加训练时间

批归一化：经过大量实验，DRL绝对不能直接使用批归一化，如果非要用，那么就要修改Batch Normalization的动量项超参数。[1]

[1]: https://blog.csdn.net/qq_40145095/article/details/126337455
[2]: https://hyper.ai/wiki/2680

TODO: https://cloud.tencent.com/developer/article/2119878
