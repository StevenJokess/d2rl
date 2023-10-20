对于机器人的运动控制，强化学习是广受关注的方法。本期技术干货，我们邀请到了小米工程师——刘天林，为大家介绍机器人（以足式机器人为主）强化学习中的sim-to-real问题及一些主流方法。

”

一、前言

设计并制造可以灵活运动的足式机器人，一直是工程师追逐的梦想。相比于轮式机器人，足式机器人凭借其腿部结构优势可以在离散非连续的路面行走。近年来，足式机器人技术发展迅速，涌现出了许多先进的足式机器人，如波士顿动力的Atlas/Spot机器人、麻省理工学院（MIT）的Cheetah系列机器人、瑞士苏黎世理工学院（ETH）的ANYmal系列机器人、宇树科技的A1/Go1机器人、小米的铁蛋机器人等。主流的传统运动控制方法，如模型预测控制（Model Predictive Control，MPC）和全身运动控制（Whole-Body Control，WBC），在足式机器人上得到了广泛的应用。

然而，这些方法往往需要复杂的建模和繁琐的人工调参，生成的动作在自然度和灵活性上也有所欠缺，这也使得研究者们把目光转向受生物启发的学习方法，强化学习（Reinforcement Learning，RL）就是其中最为广泛关注的方法。图1为四足机器人利用强化学习方法在不同路面行走的例子。



图1 基于强化学习的四足机器人不同路面行走

图片来源：https://ashish-kmr.github.io/rma-legged-robots/

强化学习是机器学习的一个分支。与监督学习不同，在强化学习中，智能体通过与环境不断交互进行试错学习，其目标是最大化累积回报。强化学习最早起源于 20 世纪 50 年代出现的“最优控制”，用于解决控制器的设计问题，其目标是使得动态系统能够随时间变化实现某种指标的最优（即最大或者最小）。

强化学习的另一个起源来自于对动物行为实验的观察。研究发现，动物在面对相同情景时会表现出不同的行为，它们更倾向于能够引起自身满足感的行为，而对于那些会给自己带来不适的行为则会尽量避免。换言之，动物们的行为在与环境的互动中通过不断试错来巩固，试错学习也是强化学习方法的核心思想。

强化学习通过与环境交互不断试错来学习，其代价是所需的样本量很大，这对于实体机器人来说往往不可行，因为过多的交互次数会对机器人硬件造成不可逆转的损耗，甚至损坏机器人，同时也需要大量时间。

基于物理引擎的仿真器，如Pybullet、Mujoco、Isaac Gym等，为获取大量机器人交互数据提供了一个有效的方式。研究者们可以先在仿真器中进行训练，之后再迁移到真实机器人上。然而，由于真实环境受到多种物理规律的约束，仿真器无法准确地建模真实环境，这也使得仿真中训练得到的策略在真实机器人上直接部署时往往会失效或性能下降。学术界将从仿真到真机的迁移称作sim-to-real，它们之间的差异称作sim-to-real gap或者reality gap。

二、sim-to-real问题

在介绍具体方法之前，首先带大家了解一下sim-to-real中需要考虑的一些问题，这也有助于大家理解解决sim-to-real问题背后的方法思想。图2为机器人感知控制框架的示意图，机器人处于一个环境中，根据自身传感器获取对环境的感知信息，之后根据这些信息进行决策，得到相应的动作并在环境中执行该动作，整个过程是一个闭环的控制过程。从这个过程也可以了解仿真和真实的一些差异：

▍（1）环境建模差异。
物理仿真器无法准确地捕捉真实世界的物理特性，如摩擦力、接触力、质量、地面反弹系数、地形方面的特性。

▍（2）感知差异。
真实世界中的感知往往是存在噪声的，易受到多种因素如光照方面的影响。而且，不同于仿真环境，真实世界中的感知是部分可观测的，sim-to-real时也需要考虑这方面的因素。

▍（3）机器人建模差异。
仿真中的机器人与真实机器人存在差异，无法准确地刻画真实机器人的运动学、动力学、电机模型等方面的特性。

▍（4）控制差异。
受通信传输和机械传动的影响，从机器人发出控制指令到真正执行指令之间存在延时，且控制信号存在噪声。当前的sim-to-real研究主要也是从这四方面差异展开。



图2 机器人感知控制框图

‍

三、主流方法

强化学习在仿真控制中取得了很大的成功，也促使研究者们将这些“成功”应用到真实机器人中。本节介绍用于解决sim-to-real问题的前沿方法，包括更好的仿真、域随机化、域适应等。

 >>>> 3.1 更好的仿真
从仿真到真机迁移的一个直观想法是，构造更真实的物理模拟器，使得仿真中的环境及其生成的数据更接近于真实环境。

比如，在视觉感知方面，通过调节仿真器中的渲染参数，使得仿真中得到的图像数据更接近于真实环境的数据。在运动控制方面，一个经典的例子是ETH 2019年发表在Science Robotics中的工作[1]。为了更好地模拟真实关节电机的驱动效果，ETH的研究人员利用神经网络建模了从PD误差到关机电机的输出扭矩，其中，PD误差包括关节位置误差和关节速度。该神经网络也被称作执行器网络（Actuator Net），如图3右上角所示。在实现时，为了更好地捕捉关节电机的动态执行特性，Actuator Net的输入包括了过去多个时刻的关节位置误差和关节速度。



图3 仿真中训练ANYmal机器人的控制策略

图片来源：https://www.science.org/doi/10.1126/scirobotics.aau5872

整个sim-to-real过程如图4所示，共分为四步：

（1）识别出机器人的物理参数，并对机器人进行刚体运动学/动力学建模；

（2）收集真实的关节电机执行数据，训练一个Actuator Net；

（3）在仿真中，利用Actuator Net建模关节电机，并结合第一步中的刚体运动学/动力学建模，进行强化学习；

（4）将第3步中训练得到的策略部署到真机上。



图4 从仿真到真机迁移框图

图片来源：https://www.science.org/doi/10.1126/scirobotics.aau5872

除了视觉感知和运动控制方面，仿真速度也是大家关注的指标。2021年，英伟达的研究人员开发了Isaac Gym强化学习仿真环境[2][3]，该环境运行在英伟达自家生产的RTX系列显卡上。Isaac Gym充分利用了GPU多核并行计算的优势，使得在同一个GPU中可以同时进行数千个机器人的仿真训练学习，这也加快了数据采集的时间。视频1为ETH和英伟达的研究人员利用Isaac Gym进行强化学习行走的例子。

视频1 利用大规模并行强化学习方法学习行走

视频来源：https://www.youtube.com/watch?v=8sO7VS3q8d0

>>>> 3.2 域随机化
从仿真到真机迁移的差异中，有很大一部分是仿真和真实之间的物理参数差异。域随机化（Domain Randomization）方法的主要思路是，在训练过程中随机化仿真环境的物理参数。它背后的思想是，如果这些参数足够多样化，并且模型能够适应这些不同的参数，那么真实环境也可以看作是仿真环境中的一个特例。

域随机化常见的一种方法是随机化视觉特征参数，这种方法在基于视觉的机器人策略中经常被使用。例如，OpenAI和UC Berkeley的研究人员利用随机化视觉特征参数后渲染得到的图像训练物体检测器，并将得到的物体检测器用在真实机器人上进行抓取控制 [5] ，如图5所示。除了随机化视觉特征参数外，随机化动力学参数也是一种常见的方法。例如，OpenAI的研究人员利用强化学习在仿真环境中训练 Shadow 机器人灵巧手的操作策略，并将得到的策略迁移到实体 Shadow 机器人灵巧手上[6]，如视频2所示。在仿真环境中，他们同时随机化了系统的动力学参数（如摩擦力、质量等）和视觉特征参数。



图5 利用图像域随机化实现从仿真到真机迁移

图片来源：https://arxiv.org/pdf/1703.06907

视频2 学习灵巧手操作策略

视频来源：https://www.youtube.com/watch?v=jwSbzNHGflM

域随机化的常见难点是，很多时候需要人为指定参数随机化的范围。这些范围的确认需要一些领域的知识或洞见，如果选择不当可能会导致从仿真到真机迁移时性能下降明显。随着自动机器学习（Automated Machine Learning，AutoML）技术的发展，一些研究人员也开始探索自动学习域随机化的参数范围，如Chebotar等人的工作[7]。

>>>> 3.3 域适应
机器人在现实环境中成功部署需要它们能够适应不可见的场景，比如不断变化的地形、不断变化的负载、机械磨损等。与域随机化对应的另一种sim-to-real方法是域适应（Domain Adaptation）。它旨在将仿真环境中 (源域) 训练得到的策略在现实环境中 (目标域) 进行再适应。这种方法背后的假设是，不同域之间具有相同的特征，智能体在一个域中学习得到的行为和特征能够帮助其在另一个域中学习。

在sim-to-real过程中，域随机化常常也与域适应一起使用。近年来，机器人领域一个经典的域适应工作是2021年UC Berkeley和CMU的研究人员发表在RSS机器人会议上的工作[8]。针对机器人实时在线适应问题，他们提出了RMA（Rapid Motor Adaptation）方法，使得四足机器人可以在不同地形下实现快速适应，实验结果示例如图1所示。图6和图7为RMA方法的系统框图。RMA由两个子模块组成，包括基础策略 π 和适应模块 Φ 。下面介绍如何在仿真中训练RMA，以及如何在真机中部署RMA。

•仿真中训练RMA（图6） 共分为两个阶段

（1）在第一个阶段中，利用模型无关（Model-free）的强化学习方法（如PPO[9]）训练基础策略 π 。其中，基础策略 π 的输入包括当前时刻状态 xt 、上一时刻动作 at-1 、经过环境特征编码器 μ 编码得到的隐变量 zt 。环境特征编码器 μ 的输入包括质量、质心、摩擦力、地形高度等，其中很大一部分信息在实际部署时很难获取，仅在仿真时训练使用，这些信息也被称为特权信息（Privileged Information）。

（2）在第二个阶段中，利用监督学习训练适应模块 Φ ，以取代第一阶段中的环境特征编码器 μ ，这也是RMA方法的主要创新点所在。需要注意的是，在这个阶段中基础策略 π 保持不变。适应模块 Φ 的输入为过去多个时刻的状态和动作，输出为环境信息的隐变量 žt。它背后的思想是，系统当前状态是机器人在特定环境下的产物，根据过去的状态和动作信息可以推断出当前的环境信息。第二阶段训练的适应模块 Φ 也解决了第一阶段中训练得到的环境特征编码器 μ 无法在实际环境中部署的问题。这种训练方式也被称为Teacher-Student学习，后续很多工作也采用了该方式。



图6 RMA方法系统框图 -- 在仿真中训练

图片来源：https://arxiv.org/pdf/2107.04034

真机部署RMA（图7）

真机部署时与仿真训练中的第二阶段类似，使用的是训练后的基础策略 π 和适应模块 Φ 。其中，基础策略 π 以100Hz运行，适应模块 Φ 以更低的频率（10Hz）异步运行。基础策略 π 输出的动作 at 为关节期望角度，最终通过机器人的PD控制器转换成扭矩。适应模块 Φ 的运行过程相当于一个在线的系统辨识过程，类似于卡尔曼滤波器通过先前的观测状态进行状态估计。



图7 RMA方法系统框图 -- 真机部署

图片来源：https://arxiv.org/pdf/2107.04034

除了四足机器人，UC Berkeley和CMU的研究人员也将RMA方法成功部署到双足机器人上[10]，如视频3所示。

视频3 双足机器人上应用RMA方法

视频来源：https://www.youtube.com/watch?v=HSdFHX0qQqg

>>>> 3.4 其他
除了前面提到的三种方法，近年来也有研究者使用其它方法来解决sim-to-real的问题。例如，通过元学习（即学习如何学习）[11]来学习机器人的本体设计[12][13]（视频4），通过扩展随机力注入（Extended Random Force Injection，ERFI）学习鲁棒的机器人运动控制策略[14]（视频5），通过对抗运动先验（Adversarial Motion Priors，AMP）从动捕数据中学习机器人动作[15][16]（视频6）。

视频4 学习四足机器人的平行弹性执行器设计及控制

视频来源：https://twitter.com/i/status/1615291830882426883

视频5 通过ERFI学习鲁棒的运动控制策略

视频来源：https://www.youtube.com/watch?v=kGkOoJ_DAwQ

视频6 四足机器人上应用AMP模仿学习方法

视频来源：https://www.youtube.com/watch?v=Bo88rwUQbrM&t=4s

四、结语

随着人工智能技术的不断发展，强化学习作为实现机器人智能运动控制的有效途径成为了大家的共识。借助现代物理仿真器技术，研究人员可以先在虚拟世界中训练机器人，之后再迁移到现实世界中。

这篇文章讨论了解决从仿真到真机迁移问题的主流方法，这些方法都各有自己的优缺点，在实际部署时一般都会结合起来使用。近年来，机器人顶级会议CoRL、RSS等也开始举办针对sim-to-real的学术研讨会[17][18][19][20]，未来sim-to-real将朝着更鲁棒策略、更少经验调参、更多维度感知的方向发展。伴随着强化学习技术的不断前进，相信在不久的将来，强化学习在实体机器人上的应用落地也将迎来蓬勃发展的春天，为人类生产生活带来便利。

参考文献
[1] Hwangbo J, Lee J, Dosovitskiy A, et al. Learning agile and dynamic motor skills for legged robots[J]. Science Robotics, 2019, 4(26): eaau5872.

[2] Makoviychuk V, Wawrzyniak L, Guo Y, et al. Isaac gym: High performance gpu-based physics simulation for robot learning[J]. arXiv preprint arXiv:2108.10470, 2021.

[3] https://github.com/NVIDIA-Omniverse/IsaacGymEnvs

[4] Rudin N, Hoeller D, Reist P, et al. Learning to walk in minutes using massively parallel deep reinforcement learning[C]//Conference on Robot Learning. PMLR, 2022: 91-100.

[5] Tobin J, Fong R, Ray A, et al. Domain randomization for transferring deep neural networks from simulation to the real world[C]//2017 IEEE/RSJ international conference on intelligent robots and systems (IROS). IEEE, 2017: 23-30.

[6] Andrychowicz O A I M, Baker B, Chociej M, et al. Learning dexterous in-hand manipulation[J]. The International Journal of Robotics Research, 2020, 39(1): 3-20.

[7] Chebotar Y, Handa A, Makoviychuk V, et al. Closing the sim-to-real loop: Adapting simulation randomization with real world experience[C]//2019 International Conference on Robotics and Automation (ICRA). IEEE, 2019: 8973-8979.

[8] Kumar A, Fu Z, Pathak D, et al. Rma: Rapid motor adaptation for legged robots[J]. arXiv preprint arXiv:2107.04034, 2021.

[9] Schulman J, Wolski F, Dhariwal P, et al. Proximal policy optimization algorithms[J]. arXiv preprint arXiv:1707.06347, 2017.

[10] Kumar A, Li Z, Zeng J, et al. Adapting rapid motor adaptation for bipedal robots[C]//2022 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS). IEEE, 2022: 1161-1168.

[11] Finn C, Abbeel P, Levine S. Model-agnostic meta-learning for fast adaptation of deep networks[C]//International conference on machine learning. PMLR, 2017: 1126-1135.

[12] Belmonte-Baeza Á, Lee J, Valsecchi G, et al. Meta reinforcement learning for optimal design of legged robots[J]. IEEE Robotics and Automation Letters, 2022, 7(4): 12134-12141.

[13] Bjelonic F, Lee J, Arm P, et al. Learning-based Design and Control for Quadrupedal Robots with Parallel-Elastic Actuators[J]. IEEE Robotics and Automation Letters, 2023.

[14] Campanaro L, Gangapurwala S, Merkt W, et al. Learning and Deploying Robust Locomotion Policies with Minimal Dynamics Randomization[J]. arXiv preprint arXiv:2209.12878, 2022.

[15] Escontrela A, Peng X B, Yu W, et al. Adversarial motion priors make good substitutes for complex reward functions[C]//2022 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS). IEEE, 2022: 25-32.

[16] Vollenweider E, Bjelonic M, Klemm V, et al. Advanced skills through multiple adversarial motion priors in reinforcement learning[J]. arXiv preprint arXiv:2203.14912, 2022.

[17] https://sites.google.com/view/corl-22-sim-to-real

[18] https://sim2real.github.io/

[19] https://sim2real.github.io/rss2020

[20] https://sim2real.github.io/rss2019

[1]: https://blog.csdn.net/pengzhouzhou/article/details/129273164?spm=1001.2014.3001.5502
