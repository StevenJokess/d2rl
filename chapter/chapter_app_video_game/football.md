

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-10-03 03:50:34
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-24 02:00:51
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# Football


## Google Research Football 环境：


Link：https://github.com/google-research/football

这个环境是google基于之前某个足球小游戏的环境进行改动和封装出来的，主要可以分为11v11 single-agent场景（控制一个active player在11名球员中切换）和5v5 multi-agent场景（控制4名球员+1个守门员）。该环境支持self-play，有三种难度内置AI可以打，你可以人肉去体验下，玩起来和实况，FIFA，绿茵之巅感觉都差不多。游戏状态基于vector的主要是球员的坐标/速度/角色/朝向/红黄牌等，也可以用图像输入，但需要打开render，估计会略慢，动作输出有二十多维，包括不同方向/长短传/加速等。此外环境还提供了所谓“football academy”，你可以自己进行游戏场景和球员坐标的初始化，相当于可以进行课程学习配置。Render如下：

## 绝悟WeKick于Kaggle夺冠

2020 年，Google 公司与英超曼城俱乐部在数据科学社区和数据科学竞赛平台Kaggle 上举办了首届Google 足球竞赛，这次竞赛基于Google Research Football强化学习环境，采取11vs11 的赛制，参赛团队需要控制其中1 个智能体与10 个内置智能体组成球队。经过多轮角逐，最终腾讯AI Lab 研发的绝悟WeKick 版本成为冠军球队。[4]

### 细节

2020 年 12 月，腾讯 AI Lab 绝悟团队借助「开悟」平台开发的足球 AI 「绝悟-WeKick 版本」在 Google Research 与英超曼城俱乐部联合举办的足球 AI Kaggle 竞赛上获得冠军。

该竞赛使用 Google Brain 基于开源足球游戏 Gameplay Football 开发的强化学习环境 Google Research Football。这场 Kaggle 竞赛也是首场相关竞赛。不同于《王者荣耀》，足球 AI 比赛涉及到 11 个智能体的相互配合以及与另外 11 个智能体的对抗，同时奖励相比于 MOBA 游戏还更稀疏。

WeKick 踢足球

即便如此，WeKick 依然以显著优于第二名的成绩获得了冠军。这体现了完全体「绝悟」底层技术和框架的通用性。

虽然都是 RTS （即时战略）游戏，星际争霸中需要控制多种不同类型不同数量的单位，这些单位又有各自的运动和攻击特点，因而动作空间更大、策略空间更丰富。

---

12 月 30 日，腾讯宣布其人工智能球队摘得首届谷歌足球 Kaggle 竞赛冠军。该冠军球队来自腾讯 AI Lab 研发的绝悟 WeKick 版本，凭借 1785.8 的总分在与全球顶级技术团队的竞技中胜出，本文介绍了绝悟 AI WeKick 版本的技术要点。



今年 11 月底，腾讯 AI Lab 与王者荣耀联合研发的策略协作型 AI 绝悟升级为完全体，首次让 AI 精通了所有英雄的全部技能。此次绝悟 WeKick 版本的整体设计正是基于绝悟完全体迁移得到，并对足球任务做了针对性的调整。



一直以来，足球运动团队策略以其复杂性、多样性和高难度，成为长期困扰世界顶尖 AI 研究团队的难题，更加稀疏的游戏激励也使得其成为比 MOBA 游戏更难攻克的目标。今年 Kaggle 首次针对足球 AI 领域发布赛题，为深度强化学习多智能体技术竞技和基准评测提供了平台。



本次比赛使用 Google Research Football 强化学习环境，基于开源足球游戏 Gameplay Football 开发，采取 11vs11 的赛制，参赛团队需要控制其中 1 个智能体与 10 个内置智能体组成球队。



GoogleFootball(on Kaggle)以流行的足球游戏为模型，就像是一款由 AI 操作的 FIFA 游戏，智能体控制足球队中的一个或所有足球运动员，学习如何在他们之间传球，并设法克服对手的防守以进球。其竞赛规则与普通足球比赛类似，比如目标都是将球踢入对方球门以及越位、黄牌和红牌规则。



不同于常见足球视频游戏的统一调控式 NPC 球队，在本次 Kaggle 竞赛中，每个球员都各由一个单独的智能体控制，而参赛的 AI 模型则根据比赛情况控制其中一个智能体，与其他 10 个内置智能体配合。这要求每个球员不仅需要观察对手的行为，还需要留意己方队员的情况，背后需要非常复杂的团队协作和竞争策略作为支撑。



举个例子，当对方球员控球时，己方智能体不仅要根据球场上双方球员的分布位置预测控球球员的下一步动作，还需要与己方其他球员协同如何合规地夺取足球的控制权。且由于球场动态瞬息万变，因此高速的实时决策能力也是必需的。



此外，从零开始完全采用强化学习方法来训练完整的足球 AI 实际上也相当困难。与 MOBA 游戏中不断有经济、血量、经验等实时学习信号不同，足球的游戏激励非常稀疏，基本只能依靠进球，而稀疏激励一直是目前强化学习一大难题。



与大多数参赛队伍一样，绝悟 WeKick 版本也主要采用了强化学习和自博弈（Self-Play）来从零开始训练模型的方法。其训练的基础架构是基于绝悟完全体的架构迁移得到的，详情参阅《腾讯绝悟AI完全体限时开放体验，研究登上国际顶会与顶刊》。基于此，腾讯 AI Lab 又针对足球任务对框架做了针对性改进，使其能适应 11 智能体足球游戏训练环境。



为此，腾讯 AI Lab 部署了一种异步的分布式强化学习框架。虽然该异步架构牺牲了训练阶段的部分实时性能，但灵活性却得到提升，而且还支持在训练过程中按需调整计算资源。此外，由于 MOBA 游戏和足球游戏任务目标的差异，团队还在特征与奖励设计上进行了扩展和创新。这些改进加上关键性的生成对抗模拟学习（GAIL）方案和 League 多风格强化学习训练方案，最终使绝悟夺冠。




架构概况



具体来说，该模型由一些密集层（每层 256 维）和一个 LSTM 模块（32 步，256 隐藏单元）构成。训练过程采用了改进版的近端策略优化（PPO）强化学习算法。学习率固定为 1e-4。参数更新则采用了 Adam 优化器。这套方案能实现非常快速的适应和迭代，且内存占用也较为合理。



在算法上，绝悟总体上采用了一种改进版 PPO 强化学习算法，这与不久之前发布的绝悟完全体的架构一致。简单来说，PPO 算法的思路在每个步骤计算更新时不仅会保证成本函数尽可能小，而且还会确保与之前策略的偏差相对较小。这一策略能克服强化学习难以调试的缺点，在实现难度、样本复杂度和调试难度之间取得合适的平衡。



在价值估计上，采用了绝悟完全体的多头价值（MHV）估计方案，即奖励会被分解为多个头，然后再使用不同的折现因子聚集到一起。采用这一方案的原因是某些事件仅与近期的动作相关，比如拦截、越位和铲球；另一些事件则涉及一系列决策，比如进球。因此不同事件的奖励会具有不同的权重。



在特征设计上，研究者对标准的 115 维向量进行了扩展，使之包含了更多特征，比如队友与对手的相对姿态（位置与方向）、活动球员与足球之间的相对姿态、标记可能越位的队友的越位标签、红/黄牌状态等特征。这些扩展为训练速度带来了 30%的效率增益。



除了人工设计的奖励，绝悟 WeKick 版本还采用了生成对抗模拟学习（GAIL），该方案利用了生成对抗训练机制来拟合专家行为的状态和动作分布，使其可以从其它球队学习。比如某个 AI 球队展现出的「反攻（counter attack）」策略就给研究者留下了深刻印象，即接球后退-传到守门员-守门员高传到前场。这是一种相对复杂的序列动作，难以通过人工方法定义其奖励；但使用 GAIL，绝悟 WeKick 版本可以成功地基于回放（replay）进行学习。然后，再将 GAIL 训练的模型作为固定对手进行进一步自博弈训练，绝悟 WeKick 版本的稳健性得到了进一步提升。




GAIL 的优势（WeKick 的奖励设计综合了 Reward Shaping 和 GAIL 两种方案）



通过自博弈强化学习得到的模型有一个天然的缺点：很容易收敛到单一风格；在实际比赛时，单一风格的模型很容易发生由于没见过某种打法而表现失常，最终导致成绩不佳。于是为了提升策略的多样性和稳健性，绝悟 WeKick 版本还采用了针对多智能体学习任务的 League 多风格强化学习训练方案。






其主要流程可简单总结为：



1. 训练一个基础模型，具备一定程度竞技能力，比如运球过人、传球配合、射门得分；



2. 基于基础模型训练出多个风格化模型，每个模型专注一种风格打法；在风格化模型训练的过程中会定期加入主模型作为对手，避免过度坚持风格，丢失基本能力；



3. 基于基础模型训练一个主模型，主模型除了以自己的历史模型为对手以外，还会定期加入所有风格化对手的最新模型作为对手，确保主模型的策略具备鲁棒性，能够适应风格完全不同的对手。



内部能力评分系统显示，加入对手池训练以后的主模型，可以在基础模型的上提高 200 分，比最强的风格化打法高 80 分。






研究者认为，基于 League 的多风格强化学习和基于 GAIL 的风格学习方法是保证 WeKick 最终获胜的关键。当然，在绝悟框架基础上针对足球任务的一些改进设计也必不可少。



附赛事技术介绍：



https://www.kaggle.com/c/google-football/discussion/202232

## Deepmind

来自DeepMind 的科学家与利物浦足球俱乐部合作，对人工智能帮助球员和教练分析对战数据进行了探索[86]。举例来讲，他们分析了过去几年的点球数据，发现球员在踢向自己最强的一侧更容易得分。他们还在一个可以仿真球员关节动作且决策间隔精确到毫秒的足球环境中训练了AI 模型，发现AI 可以自发地形成一个队伍进行合作来取得胜利[49]。[4]


[1]: https://cloud.tencent.com/developer/article/2197037
[2]: https://www.sohu.com/a/442547985_473283
[3]: https://www.infoq.cn/article/F77eX25RjDyspDkPlx1E?utm_source=related_read_bottom&utm_medium=article
[4]: https://personal.ntu.edu.sg/boan/Chinese/%E5%88%86%E5%B8%83%E5%BC%8F%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD%E7%AE%80%E4%BB%8B.pdf
