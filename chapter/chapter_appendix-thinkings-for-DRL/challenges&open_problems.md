

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-26 00:52:59
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-05-23 23:34:33
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->


# 挑战

## 问题是什么？

关于**核心算法**的挑战：

- 稳定性：您的算法会融合吗？
- 效率：融合需要多长时间？（多少个样本）
- 泛化性：收敛后，它会推广吗？

关于*假设*的挑战：

- 这甚至是正确的问题表达吗？
- 监督的来源是什么？

### 关于**核心算法**的挑战：

#### 稳定性和超参数调整

- 设计稳定的RL算法非常困难
- Q学习/值函数估计
  - 具有深网络功能估计器的拟合Q/拟合值方法通常不是收缩（contractions），因此不能保证收敛
  - 为了稳定性的大量参数：目标网络延迟、重播缓冲尺寸（replay buffer size）、剪裁、对学习率的敏感性等等。
- 策略梯度/似然比/REINFORCE
  - 非常高的差异梯度估计器
  - 大量样本，复杂的基线等等。
  - 参数：批次大小，学习率，设计率，设计基线（baseline）
- 基于模型的RL算法
  - 模型类和拟合方法
  - 随时间反向传播的重要的（non-trivial）模型
  - 更微妙的问题：策略倾向于*利用*模型

#### 样本复杂度（sample complexity）的挑战

- 需要等待很长时间才能完成运行
- 现实世界的学习变得困难或不切实际（impractical）
- 排除（Precludes）使用昂贵的高保真模拟器的使用
- 将适用性限制在现实世界中问题

#### 扩大深度RL跟概括（generalization）

Imagenet：

- 大规模
- 强调多样性
- 对概括进行评估

Spider：

- 小规模
- 强调掌握
- 对性能进行评估
- 概括在哪里？


## 重新思考问题形成过程

- 我们应该如何定义问题？
  - 数据是什么？
  - 目标是什么？
  - 什么是监督？
  - 可能与目标不一样...•

- 考虑适合您问题设定的假设！
- 不要假设基本的RL问题是一成不变（set in stone）

## 一些展望

- 强化学习作为工程工具
- 强化学习和现实世界
- 强化学习作为“通用”学习

### 强化学习作为工程工具

- 强化学习 = 能理性地做决策（can reason about decision making）
- 深层模型 = 允许RL算法学习并表示复杂的输入输出映射（input-output mappings）

## 强化学习和现实世界

### Moravec悖论（ paradox）

Moravec悖论似乎是关于人工智能的陈述，但实际上是关于物理宇宙的陈述。

We are all prodigious olympians inperceptual and motor areas, so good that we make the difficult look easy. Abstract thought, though, is a new trick, perhaps less than 100 thousand years old. We have not yet mastered it. It is not all that intrinsically difficult; it just seemsso when we do it. - Hans Moravec

The main lesson of thirty-five years of AI research is that the hard problems are easy and the easy problems are hard. The mental abilities of a four-year-old that we take for granted –recognizing a face, lifting a pencil,walking across a room, answering a question – in fact solve some of the hardest engineering problems ever conceived. - Steven Pinker

### 这跟强化学习又有什么关联？

我们如何设计一种能够应对意外情况的系统？

- 尽量少的外部监督（minimal external supervision）告诉系统该做什么
- 需要适应的意外情况
- 必须自主（autonomously）发现解决方案
- 必须“存活”足够长的时间才能发现解决方案！

- 人类在这方面非常擅长
- 当前的人工智能系统在这方面表现极差
- 原则上，强化学习可以做到这一点，其他方法则无法做到

### 所以问题是？

- RL should be really good in the “hard” universes!
- But we **rarely** study this kind of setting in RL research!

“简单”宇宙：

- 成功=高奖励（“最优控制”）
- 封闭的世界，规则已知
- 有大量模拟
- 主要问题：强化学习算法能否真正优化

“困难”宇宙：

- 成功=“生存”（“足够好的控制”）
- 开放世界，一切必须来自数据
- 没有模拟（因为规则未知）
- 主要问题：强化学习能否泛化（generalize）和适应（adapt）

#### 真实世界的问题

1. 如何告诉强化学习智能体我们想让它们做什么？
2. 如何在连续的环境中进行完全自主的学习？
3. 在环境变化时如何保持鲁棒性？
4. 使用经验和先前数据进行泛化的正确方法是什么？
5. 使用先前经验如何启动探索的正确方法？

这并不是关于机器人的问题 机器人是我们最自然的想法，因为它们像我们一样具有实体性。

##### 还有哪些传达目标的方式？

Reward predictor <-- Human feedback

Paul Christiano, Jan Leike, Tom B. Brown, Miljan Martic, Shane Legg, Dario Amodei. Deep reinforcement learning from human preferences. 2017

##### 如何在连续的环境中进行完全自主的学习？

Nagabandi, Konolige, Levine, Kumar. Deep Dynamics Models for Learning Dexterous Manipulation. CoRL 20

Task 1: put cup in coffee machine
Task 2: pick up cup
Task 3: replace cup
Task 4: clean up spill from cup...

= = = = =

Gupta, Yu, Zhao, Kumar, Rovinsky, Xu, Devlin, Levine. Reset-Free Reinforcement Learning via Multi-
Task Learning: Learning Dexterous Manipulation Behaviors without Human Int

### 如何从自举（bootstrap）探索？

> 行为先验（behavioral prior）是指一个智能体在与环境交互之前已经具备的行为知识和经验。这些行为知识和经验通常是从历史数据或以前的交互中获得的，并可以用来指导智能体在新环境中的行为决策。在强化学习中，行为先验通常用于探索和加速学习过程，以更快地学习到有效策略。

从零开始探索（from scratch） 变成 从行为先验探索

Singh*, Hui*, Zhou, Yu, Rhinehart, Levine. Parrot: Data-driven behavioral priors for reinforcement learn

## 这似乎真的很难，那有什么意义？

为什么这很有趣？

- 看到智慧的智能体提出了哪些解决方案，这是令人兴奋的。
- 如果他们想出一些我们没想到（expect）的东西，这是最令人兴奋的。
- 这需要他们所居住的世界接受新颖的解决方案
- 这意味着世界必须足够复杂！

- 要看到有趣的涌现（emergent）行为，我们必须在实际需要有趣的涌现行为的环境中训练我们的系统！
- 现实世界中的rl可能很困难，但也很有意义

## 强化学习作为“通用”学习


### 大规模机器学习

大模型、服务器、钱、大数据（标注）、目标检测、实时翻译、

### 减少监督者负担

- small labeled dataset
- giant unlabeled garbage dataset (aka the Internet)-》self-supervised

顺带一提：也许这就是为什么给大型语言模型“提示”是一门艺术！




### 往回退一点

>题外话：
> Daniel Wolpert：我们拥有大脑，只有一个原因，那就是产生适应性和复杂的运动。运动是我们唯一能够影响周围世界的方式……我相信，要理解运动就是要理解整个大脑。

一个假设（postulate）：我们需要机器学习，只有一个原因，那就是产生适应性和复杂的决策。

决策（decision）：

- 如何移动关节（move my joints）
- 如何驾驶汽车（steer the car）

- 在目标检测中啥是决策？是图片标签？通常不是
- 而是该标签后来将发生什么：
  - 它被用于给用户照片打标签吗？
  - 在照相机陷阱（camera trap 满足条件自动拍照）中检测濒危动物吗？
- 这些都是决策，它们会产生后果。

## 强化学习+数据问题

离线强化学习（offline reinforcement learning）是一种基于历史经验数据进行训练的强化学习方法。与在线强化学习不同，离线强化学习不需要在现实世界中进行交互，因此避免了现实世界中的代价高昂的交互。

使用天真（naive）的强化学习，在现实世界中进行这个过程是代价高昂的交互过程！

但是，自监督学习是关于使用我们已经拥有的廉价（cheap)数据（在垃圾堆中）！

## 配方

多样（但可能低质量 low-quality）行为的大数据集 --offline reinforement learning -->学习下游任务（downstream task）

这里有几个不同的选择：

- 人定义的技能
- 目标条件（goal-conditioned）强化学习
- 自监督技能发现

### 我们可以从没有定义明确（well-defined）的任务的离线数据中学习吗？

- 根本没有奖励函数，完全使用**目标图像**来完全定义任务
- 使用保守的离线RL方法，基于CQL
- 无需训练预处理目标就可以很好地工作

Chebotar, Hausman, Lu, Xiao, Kalashnikov, Varley, Irpan, Eysenbach, Julian, Finn, Levine.
Actionable Models: Unsupervised Offline Reinforcement Learning of Robotic Skills. 2021.

1. 使用离线强化学习训练目标条件 Q 函数
1. 使用任务奖励和有限数据进行微调

### 可以用离线RL训练大型语言模型?

Das et al. Learning Cooperative Visual Dialog Agents with
Deep Reinforcement Learning. 2017

Human interaction dataset
              | offline RL finetuning
              \/
Pretrained LM -> Value Model

## 信号来自哪里？

- Yann LeCun 的蛋糕
  - 无监督或自监督学习
  - 模型学习（预测未来）
  - 生成模型的世界
  - 在实现目标之前，还有很多事情要做！
- 模仿和理解其他智能体
  - 我们是社交动物，我们有文化 - 有其原因！
- 巨大的价值备份
  - All it takes is one +1
- 上述所有

## 我们应该如何回答这些问题？

- 选择正确的问题！
  - 问自己：这个问题有解决重要问题的机会吗？
  - 在不确定性面前保持乐观是一个好的探索策略！
- 不要害怕去改变问题陈述
  - 许多这些挑战不会通过对现有基准测试进行迭代（iterating on existing benchmarks）来解决！
- 应用很重要
  - 有时将方法应用于现实且具有挑战性的实际领域可以教给我们许多关于缺失的重要事情
  - 强化学习长期以来一直忽视了这一事实
- 要有远大的目标，从小处着手开始






[1]: http://rail.eecs.berkeley.edu/deeprlcourse/static/slides/lec-23.pdf
