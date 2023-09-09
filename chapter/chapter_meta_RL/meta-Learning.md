

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-19 01:26:53
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-19 01:27:02
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 元学习

http://mapdic.com/categories/%E5%85%83%E5%AD%A6%E4%B9%A0

Meta Learning的概念
深度学习/机器学习性能强大，但是需要不断调整超参数，费时费力，那么是否可以让网络自己决定超参数？自己设计model？自己学会怎么学习呢？

Meta Learning 元学习，含义为学会学习（learn to learn），就是用来解决上述问题，让算法学会如何学习

回顾一下机器学习的流程：

确定一个function（例如线性回归模型、神经网络），该function有许多需要被学习的未知参数， 通常定义为变量 \theta
定义一个关于未知参数 \theta 的损失函数 L\left( \theta \right)
优化，找到最优参数 \theta^* ，使得损失函数最小
上面整个流程可以看作一个学习算法（learning algorithm），或者用“学习”来表示，这个流程的目标是找到一个参数确定的function。而学习算法（“学习”）本身又可以看作一个function，用“F”表示，它的输入是训练数据，输出是一个分类器（一个function）

F\left( training \ data \right)=function

“F”是怎么来的？通常是手设的（hand-crafted），那么是否可以直接学习这个function呢？也就学习如何“学习”


图1 元学习基本概念，F代表学习算法—引自参考1
知道了meta learning的基本概念，接下来就是如何实现？

Meta Learning的三大步骤
元学习一般通过三个步骤来学习 F (learning algorithm)

第一步：首先需要明确学习算法 F 中有哪些是要被学的东西，例如在deep learning中想要学网络的架构、初始化的参数、学习率等，这些原本是人自己决定的。设 \phi 是我们想要学习的东西（learnable components），将 F 写作 F_\phi ， \phi 代表 F 中的未知参数/components

第二步：定义一个损失函数 L\left( \phi \right) ，用来评估某组参数 \phi 有多好，即用来评估learning algorithm F_\phi 有多好，如果损失很小， F_\phi 就是一个好的学习算法；如果损失很大，F_\phi 就是一个糟糕的学习算法

如何定义 L 呢？在机器学习中，损失来自于训练数据，在meta learning中，损失是来自训练任务！例如我们想要训练一个二分类器，那么就要准备很多二元分类的任务，每个任务里面又分有训练数据和测试数据


图2 元学习的训练任务—引自参考1
准备好训练任务后，开始训练，将任务1交给学习算法 F_\phi ，训练得到任务1的分类器 f_{\theta ^{1*}} ， \theta ^{1*} 是通过学习算法 F_\phi 和任务1的训练数据，学习得到的。如果分类器 f_{\theta ^{1*}} 在测试数据上表现好，说明学习算法 F_\phi 的性能好，因此损失 L\left( \phi \right) 较小；如果分类器 f_{\theta ^{1*}} 在测试数据上表现差，说明学习算法 F_\phi 的性能糟糕，因此损失 L\left( \phi \right) 较大；


图3 元学习的训练任务的训练流程—引自参考1
一个任务不足以判断学习算法 F_\phi 的好坏。将任务2交给学习算法 F_\phi ，训练得到任务1的分类器 f_{\theta ^{2*}} ......，将所有训练任务的损失加起来

total\ loss:\ L\left( \phi \right)=l^1+l^2+\cdots=\sum_{n=1}^{N}{l^n}

这里需要强调，在机器学习中，损失的计算是在训练数据上的，而这里损失的计算用的是测试数据，为什么可以在训练阶段使用测试数据？在机器学习里这样做明显是不合理的，但是元学习训练资料的基本单位是“任务”，使用任务里的测试数据，实际上使用的还是“训练任务”，“测试任务”并没有动 。因此在meta learning中一定要讲清楚你所说的的训练数据和测试数据究竟是训练任务里的，还是测试任务里的

通常使用“support set”代表task中的训练数据，“query set”代表task中的训练数据

第三步：优化，找到最优参数 \phi^* 使得损失函数最小 \phi^*=argminL\left( \phi \right)

如何优化？如何可以计算 \frac{\partial L\left( \phi \right)}{\partial \phi} ，则可以使用梯度下降，但是通常无法计算，因为 \phi 可能是网络架构等非常复杂的component。无法计算微分，无法求得梯度，如何训练呢？（可以使用强化学习reinforcement learning硬做，或者使用evolutionary algorithm）假设我们可以进行优化，则可以学习到学习算法 F_{\phi^*} （a learned "learning algorithm"）

得到 F_{\phi^*} 之后，将测试任务的训练数据送入 F_{\phi^*} ，得到一个分类器 f_{\theta ^{*}} ，利用该分类器对测试任务中的测试数据进行预测，输出预测类别。测试任务是我们真正关心的，希望有好的预测结果


图4 meta learning的框架—引自参考1
如上图，得到学习算法 F_{\phi^*} （a learned "learning algorithm"）后，在测试任务上，我们只需要很少的训练数据（few-shot learning）就可以得到一个高性能的分类器

Meta Learning和机器学习的对比
对比机器学习和meta learning的目标


图5 meta learning和机器学习的目标对比—引自参考1
总之，meta learning的任务是找到学习算法 F_{\phi^*} （a learned "learning algorithm"）

对比机器学习和meta learning的训练过程，用“across-task training”代表meta learning的整个训练过程；“within-task training”代表机器学习的训练过程（meta learning的“across-task training”中又包含多个“within-task training”）


图6 meta learning和机器学习的训练过程对比—引自参考1
对比机器学习和meta learning的测试过程，用“across-task testing”代表meta learning的测试过程；“within-task testing”代表机器学习的测试过程


图7 meta learning和机器学习的测试过程对比—引自参考1
一个“episode”代表一个“within-task training”+一个“within-task testing”

对比机器学习和meta learning的损失函数


图8 meta learning和机器学习的损失函数对比—引自参考1
注意meta learning虽然是“learn to learn”，但是在实际使用时仍然需要调参数，例如解优化问题 \phi^*=argminL\left( \phi \right) ，找到一个好的learning algorithm，找到之后，这个learning algorithm可以用在一个新的任务上（此时不需要调参数，已经学到了最优算法）

Meta Learning的具体应用
1 MAML (Model-Agnostic Meta-Learning)

MAML要做的是学习如何初始化（learning to initialize），同样需要调参数

在MAML中，把“across-task training”叫“outer loop”；把“within-task training”叫“inner loop”，因为在“across-task training”中，有很多“within-task training”，类似一种嵌套关系，前者是外循环，后者是内循环

对比MAML和self-supervised learning，两者很类似，目标都是找到一组好的初始化参数，但是self-supervised learning中没有用到标注数据


图8 MAML和self-supervised learning的损失函数对比—引自参考1
self-supervised learning之前，常使用multi-task learning，即把多个task的数据放在一起，当做一个task


multi-task learning
MAML为什么会有效？一种假设是outer loop学习到的初始化参数可以让inner loop的训练很快收敛；另一种假设是outer loop学习到的初始化参数本身就和各个task的最优解非常接近


具体参考（年份+会议+应用量+论文名）：

2019-ICLR-250-how to train your maml

2 学习optimizer

具体参考：

2016-NIPS-1200-Learning to learn by gradient descent by gradient descent

3 学习网络架构——Network Architecture Search (NAS)

显然无法计算微分，可以使用reinforcement learning硬做，具体参考：

2017-ICLR-3000-Learning Transferable Architectures for Scalable Image Recognition

2018-CVPR-3000-Learning Transferable Architectures for Scalable Image Recognition

2018-ICML-1500-Efficient Neural Architecture Search via Parameter Sharing


NAS
也可以使用evolution algorithm做，具体参考：

2018-ICLR-600-hierachical representations for efficient architecture search

2017-ICML-1000-Large-Scale Evolution of Image Classifiers

2019-AAAI-1500-Regularized Evolution for Image Classifier Architecture Search

硬把网络结构改一下，让它可以微分：

2019-ICLR-1800-DARTS: differentiable architecture search


4 学习data process


5 学习给每个data不同的权重（sample reweighting）


6 beyond gradient descent

彻底抛弃梯度下降的学习框架


7 尝试将训练阶段和测试阶段合成一个网络，不再分训练和测试（learning to compare/metric-based approach）


8 few shot image classification

meta learning的真实应用——few shot image classification

每个类别都只有很少的数据


few shot image classification
如何构造很多个N-wags K-shot的任务呢？常用的数据集：Omniglot



[2]: https://zhuanlan.zhihu.com/p/407958310#Meta%20Learning%E7%9A%84%E6%A6%82%E5%BF%B5
