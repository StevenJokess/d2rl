<!--
 * @version:
 * @Author:  StevenJokess https://github.com/StevenJokess
 * @Date: 2021-02-04 20:30:32
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-13 02:18:46
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->

# 强化学习

2018年的围棋AlphaGO战胜李世石使RL（强化学习）大为闻名，证明了这种强化模型能有超人类的表现，故我们才关注它。对于下围棋这一任务，即使是专家也很难给出“正确”的动作，二是获取大量数据的成本往往比较高。对于下棋强化学习我们很难知道每一步的“正确”动作，但是其最后的结果（即赢输）却很容易判断。因此，如果可以通过大量的模拟数据，通过最后的结果（奖励）来倒推每一步棋的好坏，从而学习出“最佳”的下棋策略，这就是强化学习。

这种在复杂、不确定的环境中交互时不断做出选择（sequential decision making）边学习的行为，我们其实早就在进行了。当一个婴儿玩耍，挥动手臂或环顾四周时，他没有明确的老师，但他确实通过直接的感觉与环境联系。他可以通过这种联系获得大量关于因果关系、动作的结果以及如何实现目标的信息。 在我们的生活中，这种交互无疑是环境和自身知识的主要来源。无论我们是学习驾驶汽车还是进行交谈，我们都敏锐地意识到我们的环境如何响应我们的行为，并且我们试图通过我们的行为来影响所发生的事情。

## 强化学习的概念

强化学习（Reinforcement Learning，简称RL），也叫增强学习，是指一类智能体从与环境交互中不断学习以取得最大回报（Return）的问题以及解决这类问题的方法。强化学习问题可以描述为一个智能体（Agent）从与环境（Environment）的不断交互，每次交互包括观察（Observate）当前的环境状态（State）、根据这个状态选择某个动作（Action）、并由此获得奖励（Award）或收益（Reward），我们把惩罚（punishment）处理成负收益形式，通过多次交互，智能体学习到了如何使得总体收益（Return）最大化。其中，强化是增加行为的意思，即当某个行为在从环境中获得正奖励后就会倾向去增加这种行为。[1] 更多强化学习的相关历史，可见[2]

![强化学习示意](/img/rl.png)

## 监督学习、非监督学习、强化学习之间的区别[3]

|      | 监督学习  | 非监督学习  | 强化学习 |
| ---- | ---------------- | ------------- | ------ |
| 数据 | 一次性给定的supervisor提供正确且严格的标签  | 没有标签  | 没有标签、supervisor，属于semi supervised learning。在智能体与环境交互的过程中得到评价反馈 或 指导性反馈，这些反馈是时间序列数据（sequential data）。如果智能体不采取某个决策动作，那么该动作对应的数据就永远无法被观测到，所以当前智能体的训练数据来自之前智能体的决策结果。
| 输入要求 | 独立同分布(i.i.d.), 为了消除数据之间的相关性。    | 独立同分布(i.i.d.)     | 归一化的占用度量（occupancy measure）用于衡量在一个智能体决策与一个动态环境的交互过程中，采样到一个具体的状态动作对（state-action pair）的概率分布。|
| 动作 | exploration     | exploration | Trial-and-error，即存在exploration和exploitation的平衡 (不一定按照已知的最优做法去做)|
| 驱动 | 任务驱动，模型是单纯被动地获得样本并被教育(instruct)[34]      | 数据驱动     | Active learning，自驱的，有目标，从错误中学习[4]，这个错误是模型与目标的距离，通过奖励函数定量判断[5]|
| 反馈 | 有反馈      | 无反馈     | 无即时反馈，反馈是稀疏且延迟的奖励, 用结果的总reward用来判断这个行为是好是坏，不会明确告诉什么是正确的action（若强化信号r与Agent产生的动作A有明确的函数形式描述，可得到梯度信息r/A则可直接可以使用监督学习算法。正因为强化信号r与Agent产生的动作A没有明确的函数形式描述，所以Agent在可能动作空间中进行搜索并发现正确的动作。[6]  |
| 模型 | 建立新输入对应原标签的预测模型     | 自学习映射关系，以此为模型     | 学习到从环境状态到行为的映射即决策（决策往往会带来“后果”，因此决策者需要为未来负责，在未来的时间点做出进一步的决策。），使得智能体选择的该行为能够获得环境最大的奖励reward|
| 优化目标公式 | $\text {最优预测模型} =\arg \min _{\text {模型}} \mathbb{E}_{(\text {特征, 标签}) \sim \text {数据分布}}[\text {损失函数 (标签, 模型（特征）)]}$ | $p(\boldsymbol{x})$ 或带隐变量 $\boldsymbol{z}$ 的 $p(\boldsymbol{x} \mid \boldsymbol{z})$ [21]| $\text {最优策略} =\arg \max _{\text {策略}} \mathbb{E}_{(\text {状态, 动作}) \sim \text {策略的占用度量}}[\text {奖励函数 (状态, 动作)]}$|
| 解释损失函数 | 目的是使预测值和真实值之间的差距尽可能小 | 最小重构错误[21]| 目的是使总奖励的期望尽可能大|
| 任务 | 预测仅仅产生一个针对输入数据的信号，并期望它和未来可观测到的信号一致，这不会使未来情况发生任何改变。预测任务总是单轮的独立任务。| 基于数据结构的假设，去学习数据的分布模式[29] |决策往往会带来“后果”，因此决策者需要为未来负责，在未来的时间点做出进一步的决策。决策任务往往涉及多轮交互，即序贯决策[7]（多序列决策） |
| 上限（upper bound） | 传统的机器学习算法依赖人工标注好的数据，从中训练好的模型的性能上限是产生数据的模型（人类）的上限      | 可超人类     | 不受人类先验知识所限，表现可超人类 |
| 适用情况 | 任务(分类/回归) | 数据驱动(聚类) [8]   | “多序列决策问题”，或者说是对应的模型未知，需要通过学习逐渐逼近真实模型的问题。并且当前的动作会影响环境的状态，即具有马尔可夫性的问题。同时应满足所有状态是可重复到达的条件，即满足**可学习条件**。 [5]|
| 通俗理解 | 记忆任务，考试 |  找规律  | 更符合现实决策 |

## 基本概念

### 智能体 (Agent)

智能体 (Agent)：强化学习系统中的决策者和学习者，它可以接受观测信号、做出决策和接受奖励信号，我们并不需要对智能体本身进行建模，只需了解它在不同环境下可以做出的动作，并接受奖励信号。

#### 智能体数量

- 单智能体（single agent）：只有一个决策者，它能得到所有可以观察到的观测，并能感知全局的奖励值；
- 多智能体（multi-agent）：多智能体中有多个决策者，它们*只能知道自己的观测*，感受到环境给它的奖励。当然，在有需要的情况下，多个智能体间可以交换信息，如王者荣耀里5v5时，能互相知道彼此的观测。在多智能体任务中，不同智能体奖励函数的不同会导致它们有不同的学习目标（甚至是互相对抗的）。下面没有特别说明的情况下，一般都是指单智能体。

#### 智能体的组成

通常将智能体分为两个部分：决策者和学习者。

1. 决策者是指智能体的决策部分，其任务是根据当前状态和学习者更新得到的价值函数或策略，选择一个最优的动作(decision)，以对环境产生影响并获取奖励。决策者可以采用不同的方法来选择动作，例如ε-greedy、Softmax等。
1. 学习者是指智能体的学习部分，其任务是根据环境的反馈信息和自身的经验，*更新*自己的价值函数或策略，以最大化累积奖励。学习者可以采用不同的算法来实现策略或价值函数的更新，例如Q-learning、SARSA、Actor-Critic等。

决策者和学习者在智能体的训练和使用过程中起着不同的作用。决策者则负责在实际运行时根据当前状态和策略选择最优的动作，对环境进行影响和控制。学习者通过不断地学习和更新，提高智能体的性能和适应性。而

需要注意的是，在某些情况下，学习者和决策者可能合并成一个整体，称为直接策略搜索（Direct Policy Search）或基于模型的强化学习（Model-Based Reinforcement Learning）。在这种情况下，智能体通过直接搜索或建模策略或环境，实现学习和决策的统一。

### 任务环境 PEAS

任务环境的描述，用PEAS来表示：

- Performance measure（性能度量）
- Environment（环境）
- Actuators（执行器）
- Sensors（感知器）

#### 性能度量（Performance Measure）

性能度量（Performance Measure）：用于描述任务中智能体行为成功的客观指标，回报（Return）

#### 环境 (Environment)及其状态（State）

- 环境 (Environment):强化学习系统中除智能体以外的所有事物，它是智能体交互的对象。交互的每一步会接收智能体的动作，发出状态和奖励。[30]环境可以是已知的，也可以是未知的，因此可以对环境建模，也可以不对环境建模。[9]
- 环境的模型是指一个预测状态转换和奖励的函数。[11]状态和奖励后面介绍。

##### 环境分类

按智能体和环境的交互方式：

- 离散时间环境（discrete time environment）：如果智能体和环境的交互是分步进行的，那么就是离散时间环境。
- 连续时间环境（continuous time environment）：如果智能体和环境的交互是在连续的时间中进行的，那么就是连续时间环境。[10]

按照环境是否具有随机性：

- 确定性环境（deterministic environment）：环境不具有随机性。例如，对于机器人走固定的某个迷宫的问题，只要机器人确定了移动方案，那么结果就总是一成不变的。这样的环境就是确定性的。
- 非确定性环境（stochastic environment）：环境具有随机性。例，如果迷宫会时刻随机变化，那么机器人面对的环境就是非确定性的。

按照环境是否具有开始、结束条件：

- 回合制环境（episodic/terminating environment）或阶段性环境：对于回合制环境，有明确的开始状态和终止状态(Terminal State)[21]。当到达终止状态时，一个智能体和环境的交互过程就结束了，这一轮交互的过程称为一个回合（Episode）或试验（Trial）。很多环境都是回合制的，如围棋等棋类、回合游戏。
- 连续性环境（continuing/non-terminating[32] environment）：对于连续性环境，没有明确的开始结束条件。即 $T=\infty$。

### 传感器（Actuators）

传感器（Actuators）去感知和观测环境，即接受输入为观察

### 执行器（Actuators）

执行器（Actuators）去作用于外部环境，即将动作（定义见后）的输出传递给环境。[25]


> - 像人类智能体: 眼睛、耳朵和其他感觉器官是传感器；手，腿和其他身体部分是执行器
> - 像机器智能体：摄像头和红外测距仪作为传感器，各种马达作为执行器。

### 例如：

Atari Breakout游戏的四大要素：

- 性能度量：分数最大化
- 环境：Atari Breakout
- 执行器：鼠标
- 传感器：人的眼睛

自动出租车智能体PEAS 的四大要素是：

- 性能度量：安全、快速、合法、舒适、利润最大化
- 环境：道路、其他交通工具、行人、顾客
- 执行器：方向盘、油门、刹车、信号、喇叭
- 传感器：摄像机、声纳、测速仪、GPS、里程表、发动机传感器、键盘

### 与环境的交互：

试验（trial）是指对一个智能体在一个环境中执行一次完整的交互的过程。这个过程通常包括以下几个步骤：

1. 环境初始化：初始化智能体的状态和环境状态。
1. 智能体与环境交互：智能体根据当前状态采取一个动作，环境根据这个动作返回一个奖励和下一个状态。
1. 状态更新：智能体更新自己的状态估计，例如状态值或动作值函数。
1. 判断是否结束：如果满足某个停止条件，例如达到预定的步数或目标状态，则结束试验，否则继续执行步骤2。

试验（trial）是强化学习中的一个重要概念，因为它是学习的基本单位。通过执行多个试验，智能体可以逐步改善自己的策略和价值估计，以实现更好的性能。试验的次数越多，智能体的性能就越好，但同时也需要注意防止过拟合。

> 注意区别
> 预演（rollout）是一种模拟智能体在**当前策略**下进行一系列动作的过程。预演的目的是为了评估当前策略的效果，以便对其进行改进。在预演中，智能体会在当前策略下选择一个动作并执行，然后根据环境的反馈信息（例如奖励信号）更新自己的状态和价值估计。然后，智能体会基于更新后的状态选择下一个动作并执行，重复这个过程直到达到预定的终止条件。预演通常被用于评估当前策略的表现，例如计算状态值或动作值函数。它也可以被用于生成训练数据，例如在蒙特卡罗树搜索中，预演可以用于生成候选动作序列，以便选择最优的动作。预演还可以被用于生成演示数据，例如在逆强化学习中，预演可以用于生成人类专家的行为轨迹，以便训练一个智能体来模仿人类行为。

### 具体交互过程

#### 第一步:感知——观察 (Observation)，随机初始化智能体

状态 (State): 一个关于这个世界状态的完整描述。这个世界除了状态以外没有别的信息。状态更新$S_{t}=f\left(H_{t}\right)$。完整的环境信息，包括所有可见和不可见的变量和参数。以汽车举例，状态是:引擎是否开启，车辆是否正在行驶等。状态默认指的就是环境状态。
  - 环境状态 $S_{t}^{e}$ (Environment State)：指智能体外部的状态，包括智能体所处的环境的所有特征，如周围的物体、声音、光线、温度、风向等等。环境的状态是智能体的感知输入，即智能体通过感知获取到的外部信息。环境状态的更新$S_{t}^{e}=f^{e}\left(H_{t}\right)$
  - 智能体的状态 $S_{t}^{a}$ (Agent State)： 指智能体当前的内部状态，包括其知识、信念、意图、规划等等，以及可能的外部状态，比如其位置、速度、姿态等等。智能体状态是智能体自身的一个描述，它反映了智能体的**内部和外部**环境的特征。智能体状态的更新$S_{t}^{a}=f^{a}\left(H_{t}\right)$

> 特例——马尔科夫状态（Markov State）： 当且仅当：$P\left[S_{t+1} \mid S_{t}\right]=P\left[S_{t+1} \mid S_{1}, \ldots, S_{t}\right]$， $S_{t}$ 是马尔科夫状态。到这一步可以把历史丟掉了, 只要每一步的状态即可。历史 (History):是在截止某刻之前的所有时刻的一序列的观察、行动、奖励。  $H_{t}=A_{1}, O_{1}, R_{1}, \ldots A_{t}, O_{t}, R_{t_{0}}$。其长度取决于任务环境和任务要求。

观察 (Observation):$O_{t}, t$ 时刻对环境的观察。它是对于一个状态的部分描述，只包括智能体可以观测到的环境信息，可能漏掉一些信息。以汽车举例，观察则是看到汽车在行驶，听到汽车的发动机声音等。
      - 当能观测当前所有环境，即观察即是状态，叫做全观测环境 (Full observability) :$ O_{t}=S_{t}^{a}=S_{t}^{e}$.如国际象棋。
      - 部分观测环境 (Partial observability): $S_{t}^{e}$。如自动驾驶。

> 有时候用符号 s 代表状态，有些地方也会写作观测符号 o。 尤其是，当智能体在决定采取什么动作的时候，符号上的表示按理动作是基于状态的， 但实际上，动作是基于观测的，因为智能体并不能知道状态（只能通过观测了解状态）。[11]

在训练智能体之前，我们需要对其进行随机初始化。这样可以使智能体开始时对环境一无所知，从而避免其陷入局部最优解。

#### 第二步:决策——行动(Action):

行动(Action):$A_{t}, t$ 时刻采取的行动。

动作空间(Action Spaces)：动作空间是所有给定环境中智能体可以执行的所有可能的有效动作的**集合**。

- 离散动作空间（discrete action space）: 有些环境，比如说 Atari 游戏和围棋，属于 离散动作空间，这种情况下智能体只能采取有限的动作。
- 连续动作空间（continuous action space）: 其他的一些环境，比如智能体在物理世界中控制机器人，属于 连续动作空间。在连续动作空间中，动作是实数向量。

这种区别对于深度强化学习来说，影响深远。有些种类的算法只能一种情况下直接使用，而在另一种情况下则必须进行大量修改。

在训练智能体的过程中，很多时候我们也是通过正在学习的智能体与环境交互来得到数据的。所以如果在训练过程中，智能体不能保持稳定，就会使我们采集到的数据非常糟糕。我们通过数据来训练智能体，如果数据有问题，整个训练过程就会失败。所以在强化学习里面一个非常重要的问题就是，怎么让智能体的动作一直稳定地提升，具体见后。

#### 第三步：环境给智能体的即时反馈（feedback），即奖励(Reward)

- **奖励** $r$ (Reward)：它是由环境给的一种标量的反馈信号（scalar feedback signal）。在每个时间步骤（time step），环境向强化学习个体发送的单个数字称为 奖励。$r_{t}$ 是 $t$ 时刻的奖励。注意，在常用语境, 有益智能体的是奖励, 有害的是惩罚；而奖励在强化学习里包括有益智能体和有害智能体的。在生物系统中，我们可能会认为奖励类似于快乐或痛苦的经历。
- **奖励的意义**：奖励信号是个体所面临的问题的直接和明确的特征。奖励信号定义了强化学习问题的目标，个体的唯一目标是最大化其长期收到的总奖励。通过引入奖励机制, 这样就可以衡量任意序列的优劣, 即对序列决策进行评价。[29]奖励信号是改变智能体动作的主要依据，如果选择的动作之后是低奖励，则将来在该情况下选择其他动作。 [33]
- **奖励函数** $R$（Reward Functions）：
  - $S \times S \mapsto \mathbb{R}$, 其中 $R\left(S_t, S_{t+1}\right)$ 描述了从第 $t$ 步状态转移到第 $t+1$ 步状态所获得奖励。在一个序列决策过程中, 不同状态之间的转移产生了一系列的奖励 $\left(R_1, R_2, \cdots\right)$, 其中 **$R_{t+1}$** 为 $R\left(S_t, S_{t+1}\right)$ 的简便记法。它由当前状态、已经执行的动作和下一步的状态这个三个参数的奖励函数$R$决定。 $r_t = R(s_t, a_t, s_{t+1})$ 有时候这个公式会被改成只依赖当前的状态 $r_t = R(s_t)$，或者状态动作对 $r_t = R(S_t,A_t)$ 。

#### 第四步：下一个时间步 t+1，环境和智能体的状态更新，即二者的模型参数更新，循环前3步，即观察状态、得到反馈、不断地选择动作以获取较大的奖励，即优化

##### 相关术语:

环境的状态更新：

- 环境的状态转移（state transition）指的是环境从某一时间 $t$ 的状态 $s_t$ 到 另一时间 $t+1$ 的状态 $s_{t+1}$ 。它是由环境的自然法则确定的，并且只依赖于最近的动作 $a_t$ 。它们可以是确定性的：$s_{t+1} = f(s_t, a_t)$ 也可以是随机的：$s_{t+1} \sim P(\cdot|s_t, a_t)$

智能体的状态更新：

- 智能体的学习者的参数更新方式，包括回合更新和单步更新。
  - 回合更新: 在一个回合(episode)后才进行参数的更新。如:原始版Policy Gradients ， Monte-Carlo Learning
  - 单步更新: 不需要等回合结束，可以综合利用现有的信息和现有的估计进行更新学习。如:Temporal-Difference, Q-learning ，Sarsa ， 进阶版Policy Gradients
- 学习率是指模型在更新参数时所使用的步长。如果学习率设置过大，可能会导致模型在更新过程中出现不稳定的情况。因此，调整学习率可以帮助模型稳定地更新参数。

智能体的行动的相关序列：

- 行动轨迹 $\tau$(Trajectory) 是，$t$ 时刻前一系列观测和动作的序列，$\tau = (s_0, a_0, s_1, a_1, ..., s_t, a_t).$ 第一个状态 $s_0$ 是从 **开始状态分布** 中随机采样的，有时候表示为 $\rho_0$:$s_0 \sim \rho_0(\cdot).$
- 历史 $H$ (History)是$t$时刻前一系列观测、动作、奖励的序列：$H_t = (o_0, a_0, r_0, o_1, a_1, r_1, ..., o_t, a_t, r_t).$
- 最优控制线（optimal control line）是，在强化学习中，用于表示在每个状态下采取的最优动作序列的一条线。这条线可以帮助我们找到最优策略（后面介绍），即在每个状态下采取的最优动作序列。

##### 长期总奖励——回报（Return）

回报 $U$：从第 $t$ 时刻状态开始，直到终止状态时，所有奖励的某种方式之和称为回报（Return）。

计算回报公式(formulations of return):

- 即时奖励（immediate / instantaneous [28] reward），即当前 $t$ 时刻的奖励 $r_t$ 或$r(s_t,a_t)$。
- 远期奖励（long-term reward），又叫延迟奖励（delayed reward），是指在一次行动后在一定时间$k$后（或者是一系列动作后）才获得回报。这种回报可能是一次, 也可能是多次。单个时间步的奖励 $r_{t+k}$ 或 $r(s_{t+k}, a_{t+k})$ 可用奖励函数 $R(s_{t+k}, a_{t+k})$算得。
- 注：实际奖励用 $r$ ，而后面的奖励函数用 $R$ 。

不同的回报公式可以用来计算在不同任务环境中的回报值:

- 累积奖励是指在一次行动轨迹中所有奖励值的总和。公式是 $G(\tau) = \sum_{t=0}^T r_t$
- 在实际问题中，智能体可能会面临如下的困境：选择一些立即的奖励可能会影响未来获得的奖励，而放弃即时回报可能会带来更多的长期奖励。所以通常存在着即时奖励和远期奖励之间的权衡。使用折扣因子能去平衡即时奖励和远期奖励，具体见下。
- 折扣累积奖励是指在一次行动轨迹中所有奖励值按时间折扣的总和。
   - 折扣因子（discount factor）是一个用来平衡未来奖励的价值衰减因子，表示在未来的每个时刻，奖励会以一定的比例进行衰减。使用时间折扣是为了使强化学习智能体更好地处理长期决策问题，同时能够适应不同的环境和任务。
   - 折扣因子通常表示为 $\gamma$ ，其中 $0 \leq \gamma \leq 1$ 。由于 $\gamma \leq 1$ ，因未来的奖励价值会以指数级别的速度进行衰减，这表达出了我们更加关注立即可获得的奖励，而不怎么关注远期可能获得的奖励。$\gamma$ 越接近1，越接近原累计奖励公式，越远视；越接近0，越短视。
   - 从当前时间步$t$开始到未来有限视野T的所有时间步的累积奖励可以表示为：$$G_t = r_{t} + \gamma r_{t+1} + \gamma^2 r_{t+2} + \cdots = \sum_{k=0}^{T} \gamma^k r_{t+k}$$
   - 而无限视野(infinite-horizon) 是$$G_t = r_{t} + \gamma r_{t+1} + \gamma^2 r_{t+2} + \cdots = \sum_{k=0}^{\infty} \gamma^k r_{t+k}$$
   - 其中，$r_t$ 表示在时间步 $t$ 时获得的奖励，$\gamma$ 是时间折扣因子，$G_t$ 表示从时间步 $t$ 开始的累积奖励。[12]

##### 优化问题(Optimization  problem)

最大化回报是强化学习系统的学习目标，即，强化学习的优化问题是指为了最大化智能体的累积（折扣）奖励而进行的。

当我们用期望回报来表示预测的未来的回报，那么就变成最大化期望回报问题。

再由回报公式推导，那么强化学习中的核心优化问题可以表示为最大化未来累计（折扣）期望奖励。

> 特别地，当基于策略（后面讲），强化学习的目标是学习到一个策略 $\pi_\theta(a \mid s)$ 来是大化期望回报(Expected Return)
>
> $$
> \mathcal{J}(\theta)=\mathbb{E}_{\tau \sim p_\theta(\tau)}[G(\tau)]=\mathbb{E}_{\tau \sim p_\theta(\tau)}\left[\sum_{t=0}^{T-1} \gamma^t r_{t+1}\right]
> $$
>
> 其中 $\theta$ 为**策略函数的参数**.
>
> **再特别地**，当MDP时[24]，$$p_\theta(\tau) = p_\theta\left(\mathbf{s}_1, \mathbf{a}_1, \ldots, \mathbf{s}_T, \mathbf{a}_T\right) = p\left(\mathbf{s}_1\right) \prod_{t=1}^T \pi_\theta\left(\mathbf{a}_i \mid \mathbf{s}_t\right) p\left(\mathbf{s}_{t+1} \mid \mathbf{s}_t, \mathbf{a}_t\right)$$
>
> $$
> \theta^{\star}=\arg \max _\theta \underbrace{E_{\tau \sim p_\theta(\tau)}\left[\sum_t r\left(\mathrm{~s}_t, \mathbf{a}_t\right)\right]}_{J(\theta)}
> $$
> $$
> J(\theta)=E_{\tau \sim p_\theta(\tau)}\left[\sum_t r\left(\mathbf{s}_t, \mathbf{a}_t\right)\right] \approx \frac{1}{N} \sum_i \sum_t r\left(\mathbf{s}_{i, t}, \mathbf{a}_{i, t}\right)
> $$ $\sum_i$ represents sum over samples from $\pi_\theta$
>
> - 无限视野情况：$$ \theta^{\star} = \arg \max _\theta E_{\left(\mathbf{s}, \mathbf{a}, \sim p_\theta(\mathbf{s},mathbf{a})\right.}[r(\mathbf{s}, \mathbf{a})]  $$
> - 有限视野情况：$$ \theta^{\star}=\arg \max _\theta \sum_{t=1}^T E_{\left(\mathbf{s}_t, \mathbf{a}_i\right) \sim p_\theta\left\{\mathbf{s}_t, \mathbf{a}_t\right)}\left[r\left(\mathbf{s}_t, \mathbf{a}_t\right)\right]$$

### 细说智能体

其学习者的常见结构[17]：

> 奖励（Reward）、奖励函数（Reward Functions）：描述当前状态到下一个状态过程中的奖励实际值

- 价值（Value）、价值函数（Value Functions）：描述当前状态到下一个状态过程中的的奖励预测值；优势函数（Advantage Functions）：去描述预测该状态相对其他状态有多好。
- 策略（Policy）、策略函数（Policy Functions）、策略概率（Policy Probability）：行动的函数。

其决策者选择动作的方式：

- 要以试错探索（trial-and-error exploration）的方式选择动作去获取奖励的过程。
  - 利用（exploitation）：利用指采取已知的可以获得最多奖励的动作，重复执行这个动作，因为我们知道这样做可以获得一定的奖励。如果智能体过于偏向于利用已知的最优策略，那么它可能会错过更好的策略，从而无法获得更高的累计奖励。这种现象被称为“局部最优”（local optimum）。
  - 探索（exploration）：指尝试一些新的动作，这些新的动作有可能会使我们得到更多的奖励，也有可能使我们“一无所有”。如果智能体过于偏向于探索新的动作，那么它可能会花费大量的时间和资源在次优的策略上，从而导致学习过程的效率低下。
- 事实上，“探索”和“利用”两者是矛盾的，因为尝试次数有限，加强了一方则会自然削弱另一方，这就是强化学习所面临的探索-利用窘境（Exploration-Exploitation dilemma）。
- 决策者的具体决策方式：之后会以多臂老虎机N-Armed Bandit (N = 10)问题为例介绍几种具体决策方式，如 $\epsilon-greedy$算法、UCB算法，见[下一章：多臂老虎机](MAB.md)

模型（Model）：智能体对环境的表征（representation），已知的状态转移函数、状态转移概率和由状态-动作对为输入的奖励函数。

#### 价值（Value）

**价值（Value）** 或 **效用（Utility）**: 价值 $V$ 是对未来折扣累积奖励的预测值，是估计该状态的期望回报（即从这个状态出发的未来累积奖励的期望）[18]。

##### 价值函数(Value Functions)

所有状态的价值就组成了**价值函数（Value Functions）**，简称值函数。价值函数，是一个将状态或状态-行动对映射到预期的未来的**累计折扣奖励**的函数。价值函数的值是对未来累计折扣奖励的预测，我们用它来评估状态的好坏。

价值函数通常有两种形式：

- **状态价值函数**（state value function）表示在某个状态下之后每一步行动都按照策略 $\pi$ 执行后每个状态的价值函数。下面公式，只由一个状态s确定V，是由于此时是[马尔可夫决策过程 MDP](MDP_MC.md)。
  $$V_{\pi}(s) = \mathbb{E}_{\pi} \left[ G_t | S_t = s \right] = r_0 + \mathbb{E}_{\pi}\left[R_{t+1}+\gamma R_{t+2}+\gamma^{2} R_{t+3}+\ldots \mid S_{t}=s\right]$$
  其中，$V_{\pi}$ 是在状态 $s$ 下，根据策略 $\pi$ （具体介绍见后）执行后的预期累积奖励，$G_t$ 是从时刻 $t$ 开始的累积奖励（可分有限视野$\sum_{k=0}^{T} \gamma^k R_{t+k+1}$ 和无限视野 $\sum_{k=0}^{\infty} \gamma^k R_{t+k+1}$），$\mathbb{E}\pi$ 是在策略 $\pi$ 下的期望。
> 预测下一个即时的奖励: $\mathbf{R}_{}= \mathbb{E}_{\pi}\left[R_{t+1} \mid S_{t}=s, A_{t}=a\right]_{\circ}$
- **动作价值函数**（action value function）表示从某个状态开始，先随便执行一个行动 $a$ (有可能不是按照策略走的），之后每一步都按照固定的策略 $\pi$ 执行后的每个状态-行为对下的价值函数，又叫Q函数。[11]下面公式，只由一个状态s和一个动作a确定V，是由于此时是[马尔可夫决策过程 MDP](MDP_MC.md)。
$$Q_{\pi}(s, a) = \mathbb{E}_{\pi} \left[ G_t \mid S_t = s, A_t = a \right] = \mathbb{E}_{\pi}\left[R_{t+1}+\gamma R_{t+2}+\gamma^{2} R_{t+3}+\ldots \mid S_t = s, A_t = a \right]$$ 其中，$Q_{\pi}$ 是在状态 $s$ 下执行动作 $a$，根据策略 $\pi$ 执行后的预期累积奖励。$G_t$ 是从时刻 $t$ 开始的累积奖励（可分有限视野 $\sum_{k=0}^{T} \gamma^k R_{t+k+1}$ 和无限视野 $\sum_{k=0}^{\infty} \gamma^k R_{t+k+1}$ ） 是在策略 $\pi$ 下的期望。注意，动作价值函数 $Q^\pi(s, a)$ 是状态 $s$ 和动作 $a$ 的函数。可以通过 Q 函数得到进入某个状态要采取的最优动作。
- 由二者的定义得，$V^\pi(s)=\mathbb{E}_{a \sim \pi}\left[Q^\pi(s, a)\right]$,

> 注：不管策略如何，某个状态的价值是不变的，因为在算期望的时候已经就考虑了所有情况的奖励。策略变了，R是变了，但期望是所有的可能轨迹去平均或者怎样，所以跟策略无关，确定性策略只是去选择一条轨迹。 (？X)

**最优价值函数**：

- **最优值函数**（最优策略的状态价值函数）：表示在某个状态下之后每一步行动都按照最优策略 $*$ 执行后每个状态的价值函数。$$V_{*}(s) = \max_{*} \left[ G_t | S_t = s \right] = \max_{*}\left[R_{t+1}+\gamma R_{t+2}+\gamma^{2} R_{t+3}+\ldots \mid S_{t}=s\right]$$
- **最优行动-值函数**（最优策略的动作价值函数）：表示从某个状态开始，先随便执行一个行动 $a$ (有可能不是按照策略走的），之后每一步都按照*最优策略* $\pi$ 执行后的每个状态-行为对下的价值函数。$$Q_{*}(s, a) = \max_{*} \left[ G_t \mid S_t = s, A_t = a \right] = \max_{*}\left[R_{t+1}+\gamma R_{t+2}+\gamma^{2} R_{t+3}+\ldots \mid S_t = s, A_t = a \right]$$
- 由二者的定义得，$$V_{*}(s) = \max_{a}Q_{*}(s, a)$$

##### 优势函数（Advantage Functions)

强化学习中，有些时候我们不需要描述一个行动的绝对好坏，而只需要知道它相对于平均水平的优势。也就是说，我们只想知道一个行动的相对优势 。这就是优势函数的概念。[11]

一个服从策略 $\pi$ 的优势函数，描述的是它在状态 $s$ 下采取行为 $a$ 比随机选择一个行为好多少（假设之后一直服从策略 $\pi$ ）。数学角度上，优势函数的定义为：[11]

$$A^\pi(s, a) = Q^\pi(s, a) - V^\pi(s, a)$$

> 我们之后会继续谈论优势函数，它对于策略梯度方法非常重要。

#### 策略（Policy）

##### 策略（Policy）：

策略（Policy）或 策略函数（Policy Functions）：指智能体怎么选择动作。它对应于心理学中所谓的一组刺激-反应规则或关联。策略是强化学习个体的核心，因为它本身就足以确定行为。

- 不利用动作价值函数时：观察（Observation）到 行动（Action）的一个映射   或 全观察情况下的状态（fully observated State) [23] 到 行动（Action） 的一个映射。    。以雅达利游戏为例子，策略函数的输入就是游戏的任一帧，它的输出决定智能体向左移动或者向右移动。强化学习通过学习来改进策略来最大化总奖励。
- 利用动作价值函数时：动作价值函数Q 到 行动（Action）的一个映射。

根据策略固定与否分为不同的学习模式：

- 被动学习中智能体的策略是固定的，任务是学习状态（或状态 - 行动配对）的价值，也可能涉及到学习环境模型。这种固定的策略叫做**确定性策略（deterministic policy）** [21]，通常智能体直接采取最有可能（最大概率）的动作，策略$\mu(s)= a^* =\underset{a}{\arg \max } \pi(a \mid s)$
- 主动学习主要涉及的问题是探索：智能体必须尽可能多地体验其环境，以便学习如何表现。这种不同的策略是随机性策略（stochastic policy），具体定义是在给定环境状态下，智能体选择一个从概率分布采样[28]得到的动作，这个输出为特定观察下的行动的概率的函数用$\pi$表示，$\pi(a \mid s) \equiv p(a \mid s) = P\left[A_{t}=a \mid S_{t}=s\right]$）其中，$\Sigma \pi(a \mid s)=1$ 为使记法没那么复杂（less cumbersome），我们会经常用 $\pi(s)$ 代替 $\pi(a \mid s)$ 。 动作从概率分布去采样：$a\sim \pi\left(a \mid s\right)$
  - 注意：*因为$\pi$能表示一种特定的观察到行动的映射关系，很多中文的相关资料也就把$\pi$说是策略。而从严谨来说，从概率分布随机采样得到动作 $a\sim \pi\left(a \mid s\right)$这个整体才是策略*
  - 通常情况下，强化学习*一般使用随机性策略*，随机性策略有很多优点。比如，在学习时可以通过引入一定的随机性来更好地探索环境； 随机性策略的动作具有多样性，这一点在多个智能体博弈时非常重要。采用确定性策略的智能体总是对同样的状态采取相同的动作，这会导致它的策略很容易被对手预测。[19]

强化学习的策略在训练中会不断更新，其对应的数据分布（即占用度量）也会相应地改变。因此，强化学习的一大难点就在于，智能体看到的数据分布是随着智能体的学习而不断发生改变的。

由于奖励建立在状态 - 动作对之上，一个策略对应的价值其实就是一个占用度量下对应的奖励的期望，因此寻找最优策略对应着寻找最优占用度量。[5]

##### 最优策略

- 最优确定性策略：$\pi^*(s)=\underset{a \in \mathcal{A}}{\operatorname{argmax}}\left[r(s, a)+\gamma \sum_{s^{\prime} \in \mathcal{S}} P\left(s^{\prime} \mid s, a\right) V^*\left(s^{\prime}\right)\right]$
- 最优随机性策略：其能达到最大平均回报 $\pi^* = \underset{\pi}{\operatorname{argmax}} V^\pi\left(s_0\right)$ 。
- 记此时最优策略的价值函数和动作价值函数 $V^* \equiv V^{\pi^*}$ 和 $Q^* \equiv Q^{\pi^*}$

#### 模型（Model）

模型是对环境的模拟，或者更一般地说，它对环境的行为做出推断。例如，给定状态和动作，模型可以预测结果的下一状态和下一个奖励。 模型用于 规划，我们指的是在实际行动前对未来进行预判。[33]

用以描述智能体述与环境交互时状态改变的四个概念：

- **状态转移**（state transition）：描述当前状态下执行某个动作后转移到下一个状态的过程。
- 我们试图用这样一个函数形式去描述当前状态下执行某个动作后转移到下一个状态的过程。它就是**状态转移函数**（state transition function）：其输入是当前状态、执行的动作、下一特定状态，而输出是一个数。而在[马尔可夫过程 MDP](MDP.md)，由于下一步的状态只取决于当前的状态以及当前采取的动作，故可以将状态转移函数 T ，用概率的形式表示，即 $T(s,a,s') = P(s'|s,a)$，其中，s是当前状态，a是采取的行动，s'是下一个状态，$P(s'|s,a)$表示在状态s下采取行动a转移到状态s'的概率，将这个概率定义为状态转移概率。
- **状态转移概率**（State Transition Probability）是指当前状态到下一个特定状态的概率，即 $p_{s s^{\prime}}=p\left(s_{t+1}=s^{\prime} \mid s_t=s \right)$。状态转移概率的作用是用于计算值函数或策略的期望收益。
- **状态转移矩阵**（state transition matrix）。状态转移矩阵 $\mathcal{P}$ 定义了所有状态对之间的转移概率。假设一共有 $n$ 个状态， 此时 $\mathcal{S}=\left\{s_1, s_2, \ldots, s_n\right\}$ ，那么
$$
\mathcal{P}=\left[\begin{array}{ccc}
P\left(s_1 \mid s_1\right) & \cdots & P\left(s_n \mid s_1\right) \\
\vdots & \ddots & \vdots \\
P\left(s_1 \mid s_n\right) & \cdots & P\left(s_n \mid s_n\right)
\end{array}\right]
$$

用以描述每一步奖励的一个概念：

- 奖励函数是指我们在当前状态采取了*某个动作*，可以得到多大的奖励，输入是状态动作对，输出是奖励值。即 $R(s, a)=\mathbb{E}\left[r_{t+1} \mid s_t=s, a_t=a\right]$[27]

所以当我们说模型时，意味着状态转移概率和奖励函数都已知了。

### 根据学习/优化方法分类：

- 不基于模型，即免模型学习(Model-Free)方法：该方法放弃了对环境的建模，直接与真实环境进行交互，所以其通常需要较多的数据或者采样工作来优化策略，这也使其对于真实环境具有更好的泛化性能[5]；由于这种方式更加容易实现，也容易在真实场景下调整到很好的状态。所以免模型学习方法更受欢迎，得到更加广泛的开发和测试。
  1. **基于价值函数**(value-based)：该方法是智能体通过学习价值函数(value function)（如状态值函数或动作值函数）来隐式地构建最优策略，即选择具有最大值的动作。包括，采取回合更新的蒙特卡洛方法（MC）、采取单步或多步更新的时间差分方法（TD）{使用表格学习的 Q-learning、Sarsa算法以及一系列基于Q-learning的算法（具体见off-policy）}。此时我们在训练 $Q_\theta$ 以满足自洽方程，间接地优化智能体的表现，即训练的是一个主要完成任务的Actor。 优点：Value-based算法因为其更能有效地重用历史，所以样本利用率高、价值函数估值方差小, 不易陷入局部最优；缺点：此类算法只能解决离散动作空间问题, 容易出现过拟合, 且可处理问题的复杂度受限. 同时, 由于动作选取对价值函数的变化十分敏感, value-based算法收敛性质较差。[22]
     1. 在线控制 或 **同策学习**（on-policy）是指直接对策略进行建模和优化的方法，其目标是找到一个能够最大化期望累积奖励的最优策略。要优化的策略网络（更新参数时使用的策略）恰也是行动策略（生成样本的策略），即学习者与决策者统一。包括Sarsa，Sarsa（λ）[13]。on-policy方法更加稳定但收敛速度较慢。例如，SARAS是基于当前的策略直接执行一次动作选择，然后用动作和对应的状态更新当前的策略，因此生成样本的策略和学习时的策略相同，所以SARAS算法为on-policy算法。该算法会遭遇探索-利用窘境，仅利用目前已知的最优选择，可能学不到最优解，不能收敛到局部最优，而加入探索又降低了学习效率。ϵ - 贪心算法是这种矛盾下的折衷，其优点是直接了当、速度快，缺点是不一定能够找到最优策略。[20]
     2. 离线控制 或 **异策学习**（off-policy）则是指在训练过程中使用一个不同于当前策略的策略进行采样和更新，也就是说，智能体在执行动作时可以采用任意策略生成的动作进行训练。常见的off-policy方法包括Q-learning，Deep-Q-Network，Deep Deterministic Policy Gradient (DDPG)等方法。通过之前的历史（可是自己的也可以是别人的）进行学习，要优化的策略网络（更新参数时使用的策略）与行动策略（生成样本的策略）不同，即学习者和决策者不需要相同。[14]而off-policy方法则更容易出现不稳定性但收敛速度较快。包括Q-Learning ， Deep Q Network。例如，Q-learning在计算下一状态的预期奖励时使用了最大化操作，直接选择最优动作，而当前策略并不一定能选择到最优的动作，因此这里生成样本的策略和学习时的策略不同，所以Q-learning为off-policy算法。[20]
  2. **基于策略**(policy-based) or 反射(reflex)：该方法是跨越价值函数, 直接搜索最佳策略。[22]包括无梯度方法(Gradient-Free)、策略梯度方法Policy Gradient及其衍生的 REINFORCE算法、带基准线的REINFORCE算法。此时我们直接再优化你想要的奖励[24]，即训练的是不完成任务的一个Critic。优点：相比value-based算法, policy-based算法能够处理离散/连续空间问题, 并且具有更好的收敛性；policy-based方法轨迹方差较大、样本利用率低, 容易陷入局部最优的困境。
     1. Gradient-Free：能够较好地处理低维度问题。[22]Cross-Entropy Method的DQN演化而成的QT-Opt、Evolution Strategy的SAMUEL
     2. Gradient-Based：基于策略梯度算法仍然是目前应用最多的一类强化学习算法, 尤其是在处理复杂问题时效果更佳, 如AlphaGo 在围棋游戏中的惊人表现。算法在Hopper问题的效果对比，Policy Gradient、VPG（如REINFORCE）、TRPO/PPO、ACKTR。SAC=TD3＞DDPG=TRPO=DPG＞VRG。[22]
  3. **基于执行者/评论者**（actor-critic）：该方法是智能体结合了值函数和策略的思想。它包含一个执行者（actor）网络和一个评论者（critic）网络，执行者网络用于生成动作，而评论者网络用于估计值函数。![在Actor-Critic 基础上扩展的 DDPN (Deep Deterministic Policy Gradient)、A3C (Asynchronous Advantage Actor-Critic)、DPPO (Distributed Proximal Policy Optimization)。优点：actor-critic算法多是off-policy，能够通过经验重放(experience replay)解决采样效率的问题；缺点：策略更新与价值评估相互耦合, 导致算法的稳定性不足, 尤其对超参数极其敏感。Actor-critic算法的调参难度很大, 算法也难于复现, 当推广至应用领域时, 算法的鲁棒性也是最受关注的核心问题之一。[15] ![执行者/评论者的智能体](..\img\A+C.png)
- **基于模型** （model-based）或 基于效用（utility-based）[31] ：该“模型”特指环境，即环境的动力学模型。[21]该类智能体尝试学习环境的动态模型，即预测从给定状态和动作转移到下一个状态的概率。然后，智能体可以使用学习到的环境模型来提前（look-ahead）规划决策。（model + policy and/or + value function）但缺点是如果模型跟真实世界不一致，那么限制其泛化性能，即在实际使用场景下会表现的不好。预测从给定状态和动作转移到下一个状态的概率: $$\mathbf{P}_{s}^{a}=P\left[S_{t+1}=s^{\prime} \mid S_{t}=s, A_{t}=a\right], \mathbf{R}$$ 环境模型一般可以从数学上抽象为状态转移函数 P (transition function) 和奖励函数 R (reward function)。 在学习R和P之后，所有环境元素都已知，理想情况下，智能体可以不与真实环境进行交互，而只在模拟的环境中，通过RL算法（值迭代、策略迭代等规划方法）最大化累积折扣奖励，得到最优策略。[16]包括：动态规划算法（策略迭代算法、值迭代算法），已给定的模型（Given the Model） MCTS（AlphaGo/**AlphaZero**），学习这模型（Learn the Model） I2A 和 World Model。其优点是可以大幅度提升采样效率；有最大的缺点就是智能体往往不能获得环境的真实模型。如果智能体想在一个场景下使用模型，那它必须完全从经验中学习，这会带来很多挑战。最大的挑战就是，智能体探索出来的模型和真实模型之间存在误差，而这种误差会导致智能体在学习到的模型中表现很好，但在真实的环境中表现得不好（甚至很差）。基于模型的学习从根本上讲是非常困难的，即使你愿意花费大量的时间和计算力，最终的结果也可能达不到预期的效果。[11]
- 更多相关：模仿学习智能体（imitation learning agent）：该类智能体不是直接学习环境奖励，而是尝试模仿人类或其他专家的决策。模仿学习可以提供一种简单而有效的方式，使智能体学习到正确的行为。

> 在这个项目中选取了能够呈现强化学习近些年发展历程的核心算法。目前，在 可靠性 (stability)和 采样效率 (sample efficiency)这两个因素上表现最优的策略学习算法是 PPO 和 SAC。从这些算法的设计和实际应用中，可以看出可靠性和采样效率两者的权衡。[11]

## 附录——RL里程碑：Alpha Go

阿尔法围棋（AlphaGo）是第一个击败人类职业围棋选手、第一个战胜围棋世界冠军的人工智能机器人，由谷歌（Google）旗下DeepMind公司戴密斯·哈萨比斯领衔的团队开发。其主要工作原理是“深度学习”。

### 历史：

- 2016年3月，阿尔法围棋与围棋世界冠军、职业九段棋手李世石进行围棋人机大战，以4比1的总比分获胜；
- 2016年末2017年初，该程序在中国棋类网站上以“大师”（Master）为注册账号与中日韩数十位围棋高手进行快棋对决，连续60局无一败绩；
- 2017年5月，在中国乌镇围棋峰会上，它与排名世界第一的世界围棋冠军柯洁对战，以3比0的总比分获胜。围棋界公认阿尔法围棋的棋力已经超过人类职业围棋顶尖水平，在GoRatings网站公布的世界职业围棋排名中，其等级分曾超过排名人类第一的棋手柯洁。

机器同样是使用强大的算力以数倍、数十倍、数百倍的训练时间去击败人类（通常人类训练十年的时间，机器可以模拟训练几百年），为什么Alpha Go的取胜这么重要、这么引人关注（世界各地媒体疯狂报道，一股狂潮如炒作一般）呢？

原因有两个：

1. AlphaGo解决的围棋问题比之前的都要复杂，西洋双陆棋只有 $10^20$ 种不同的“棋位”空间配置，深蓝打败人类的国际象棋有 $10^43$ 种不同的“棋位”空间配置，而围棋却有 $10^170$ 种不同的“棋位”空间配置，这种量级的数字人类已经无法处理（意思是对于这么多种不同的状态，就是目前算力最强的计算机也无能为力）。举个例子，$10^170$ 这个数字比宇宙中存在的原子数还多。为什么AlphaGo可以在围棋上击败人类就如此重要呢？因为机器如果可以解决这个大的状态空间的问题，那么在机器学习也应该能解决很复杂的现实世界中的问题。这意味着机器真正融入我们的劳动力市场，为我们的日常生活提供便利的日子已经不远啦（真的吗？）！
1. AlphaGo解决的围棋问题不可能通过纯粹的、暴力计算的方式来学习出很好的模型，这就需要为AlphaGo设计一个更加“智能、聪明”的算法。AlphaGo引起热潮的另一个原因就是，其训练算法是一个通用算法，而不是一个专门为解决某项任务特别设计的算法，这与97年IBM的深蓝计算机程序完全不同，因为深蓝只能用于学习下国际象棋，在中国象棋中就不适于训练。此前，AlphaGo的前身已经能够在Atari 49个不同规则、不同游戏模式中使用相同的通用训练算法训练出比人类还厉害的模型，AlphaGo的成功意味着不仅在虚拟环境可以使用这一套学习方法训练模型，而且可以在不同的现实世界问题中使用这一套学习方法、代码结构。

有能力解决状态空间非常大的问题和通用学习算法是使AlphaGo红极一时的两个主要原因，这也解释了为什么这场比赛在媒体上引起了轰动。有些人认为李世石的失败是机器占据人类劳动力市场的先兆，也有些人认为这预示着人工智能迎来了黄金时代，实际上我们距离真正的人工智能还有很长的路要走，就算机器可以在某项非常复杂的任务中超过人类的表现能力，其也没有真正的思维方式，不会进行思考，说到底也只是曲线的拟合罢了，但是，只有基础做好了，才能向上研究人工智能。

构建AlphaGo和其前身（应用于Atari游戏）的学习算法的设计思路、计算架构在一系列论文和视频中都可以获得，而没有被Google（收购了英国公司DeepMind）私藏。为什么他不私藏呢？这么厉害的代码、设计思路没必要公开出来嘛，因为Google想把自己打造为基于云的机器学习和大数据的领导者，而它在2016年是全球第三大云服务提供商，排在微软和亚马逊之后，它需要把客户从其他平台引流到自己的平台上。由此可见，大公司们之间的竞争反而可以使我们平民获益。

## 参考文献

[1]: https://zh.wikipedia.org/wiki/%E6%93%8D%E4%BD%9C%E5%88%B6%E7%B4%84
[2]: https://blog.sciencenet.cn/blog-2374-1351757.html
[3]: https://github.com/NLP-LOVE/ML-NLP/tree/master/Deep%20Learning/14.%20Reinforcement%20Learning
[4]: https://leovan.me/cn/2020/05/introduction-of-reinforcement-learning/
[5]: https://www.cnblogs.com/kailugaji/p/16140474.html
[6]: https://baike.baidu.com/item/%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0/2971075
[7]: https://hrl.boyuai.com/chapter/1/%E5%88%9D%E6%8E%A2%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0
[8]: https://baike.baidu.com/item/%E6%97%A0%E7%9B%91%E7%9D%A3%E5%AD%A6%E4%B9%A0/810193?fromModule=lemma_search-box
[9]: https://easyai.tech/ai-definition/reinforcement-learning/
[10]: https://www.huoban.com/news/post/2237.html
[11]: https://spinningup.qiwihui.com/zh_CN/latest/spinningup/rl_intro.html
[13]: https://baike.baidu.com/item/%E9%A9%AC%E5%B0%94%E5%8F%AF%E5%A4%AB%E9%93%BE/6171383?fromModule=search-result_lemma-recommend
[13]: https://echenshe.com/class/ml-intro/4-02-RL-methods.html
[14]: https://anesck.github.io/M-D-R_learning_notes/RLTPI/notes_html/1.chapter_one.html
[15]: https://blog.csdn.net/Hansry/article/details/80808097
[16]: https://opendilab.github.io/DI-engine/02_algo/model_based_rl_zh.html
[17]: https://cloud.tencent.com/developer/article/1692318
[18]: https://hrl.boyuai.com/chapter/1/%E9%A9%AC%E5%B0%94%E5%8F%AF%E5%A4%AB%E5%86%B3%E7%AD%96%E8%BF%87%E7%A8%8B
[19]: https://datawhalechina.github.io/easy-rl/#/chapter1/chapter1?id=_123-%e5%ba%8f%e5%88%97%e5%86%b3%e7%ad%96
[20]: https://www.cnblogs.com/kailugaji/p/16140474.html
[21]: https://www.cnblogs.com/kailugaji/p/15354491.html#_lab2_0_7
[22]: http://www.c-s-a.org.cn/html/2020/12/7701.html#outline_anchor_19
[23]: http://rail.eecs.berkeley.edu/deeprlcourse/static/slides/lec-2.pdf
[24]: https://spinningup.readthedocs.io/zh_CN/latest/spinningup/rl_intro2.html
[25]: https://www.zhihu.com/question/574829510/answer/2891438632
[26]: https://blog.csdn.net/qq_40990057/article/details/125750328
[27]: https://zhuanlan.zhihu.com/p/493257376
[28]: https://www.d2l.ai/chapter_reinforcement-learning/value-iter.html#value-function
[29]: https://aistudio.baidu.com/aistudio/education/preview/3103363
[30]: https://www.youtube.com/watch?v=QDzM8r3WgBw&list=PLrAXtmErZgOeiKm4sgNOknGvNjby9efdf
[31]: https://www.jiqizhixin.com/graph/technologies/ee1a8f69-3170-4ddf-b2b6-47d91c844425
[32]: https://www.bilibili.com/video/BV1UT411a7d6?p=35&vd_source=bca0a3605754a98491958094024e5fe3
[33]: https://github.com/qiwihui/reinforcement-learning-an-introduction-chinese/blob/master/source/chapter1/introduction.rst
[34]: https://zhuanlan.zhihu.com/p/52727881


其上很多涉及到的网站已被Markdown渲染，这些网站也被参考到了，但在文章的哪个具体位置忘了：

> https://spinningup.readthedocs.io/zh_CN/latest/spinningup/rl_intro.html#bellman-equations
> https://weread.qq.com/web/reader/62332d007190b92f62371aek92c3210025c92cc22753209
> http://rail.eecs.berkeley.edu/deeprlcourse/static/slides/lec-1.pdf
> https://zhuanlan.zhihu.com/p/316339517
> https://rl.qiwihui.com/zh_CN/latest/chapter1/introduction.html#id4
> https://github.com/applenob/rl_learn/blob/master/class_note.ipynb
> https://blog.csdn.net/weixin_40056577/article/details/104109073
> https://tianshou.readthedocs.io/zh/latest/docs/2-impl.html#id31
> https://nndl.github.io/ 的ch14
> https://echenshe.com/class/ml-intro/4-02-RL-methods.html
> https://blog.csdn.net/weixin_42022175/article/details/99676753
> http://www.deeprlhub.com/d/722/42
> https://baike.baidu.com/item/%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0/2971075
> https://baike.baidu.com/item/%E6%97%A0%E7%9B%91%E7%9D%A3%E5%AD%A6%E4%B9%A0/810193?fromModule=lemma_search-box
> https://blog.sciencenet.cn/blog-3189881-1122463.html
> https://blog.csdn.net/qq_38962621/article/details/103951014
> https://ai.stackexchange.com/questions/21628/is-there-any-difference-between-reward-and-return-in-reinforcement-learning
