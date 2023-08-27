

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-28 21:20:34
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-06-01 00:39:00
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# poker

## hold'em（HUNL）

德州扑克与围棋的区别在于德州扑克属于非完备信息博弈问题，是计算机博弈的另一分支。非完备信息机器博弈（不完全信息博弈）问题已被证明是一个NP 难问题[153]，一对一有限注德州扑克的状态复杂度约为3.16×10^17,包含其中的状态大多是无法确认的，有极大的随机性和不确定性，因此，德州扑克也是人工智能领域非常具有挑战性和代表性的博弈课题。图7-1 展示了德州扑克的牌局实例。

对于2 人有限下注的德州扑克，可能出现的不同牌面状态是10^14[4]

## 历史

- 2008 年，德州扑克博弈系统Polaris 首次战胜了职业扑克选手。
- 2009 年，蒙特卡洛方法被引用于无限注德州扑克，并开始普遍应用。
- Boris Iolis[154]提出了一种适用于扑克牌问题的选择策略，该策略以决策行为被选择的概率大小为依据，取得了较好效果；
- JohannesHeinrich[155]提出了一种Kuhn poker 的近似纳什均衡策略；
- 2011 年，文献[156]中首次应用了模式匹配算法研究德州扑克游戏。
- 2015 年，加拿大阿尔伯特大学发表了关于一对一有限注德州扑克系统的研究成果，得到了该博弈问题的理论解。该研究小组开发的系统采用了反现实悔恨值最小化（Counterfactual regret minimization,简称CFR）算法，该算法通过多次的自对弈与评估过程，通过迭代得到近似的纳什均衡。
- 2017 年，阿尔伯特大学在Science 发表了关于一对一无限注德州扑克的DeepStack 算法研究[157]，DeepStack 是首个打败职业扑克玩家的计算机程序

纸牌游戏作为典型的不完美信息游戏，长期以来一直是人工智能的挑战。DeepStack 和 Libratus 是在 HUNL 中击败职业扑克玩家的两个典型 AI 系统。

## 神经虚拟自我博弈 (NFSP)

Heinrich 和 Silver (2016) 提出了神经虚拟自我博弈 (NFSP)，将虚拟自我博弈与深度强化学习相结合，以学习不完全信息博弈的近似纳什均衡无需先验领域知识的可扩展端到端方法。 NFSP 在两人游戏中进行了评估零和游戏。在 Leduc poker 中，NFSP 接近纳什均衡，而普通的 RL 方法分歧。在 Limit Texas Hold'em 中，一种真实世界规模的不完全信息游戏，NFSP 执行类似于基于重要领域专业知识的最先进的超人算法。[5]

## Libratus

### 三个关键模块

（1）赛前纳什均衡近似（Nash  equilibrium  approximation  before competition）：这个模块把最重要的博弈信息（例如针对某一手牌对应的战略）进行抽取，然后再应用强化学习等方法，继续寻求提高和改进。这里使用了一个新的算法：蒙特卡洛反事实遗憾最小化。在这个模型的帮助下，Libratus 自己学会了德州扑克，而且比以前速度更
快。

（2）残局解算（Endgame  solving）。这是 Libratus 最重要的部分，因为一局德扑只需要几个回合，耗费时间短。因此Libratus 的开发者们选择从下往上构建博弈树，这样最下面节点的状态是比较容易算出来的，用这个状态反过来指导设计上面的博弈树，并使用蒙特卡罗方法，每次选一些节点去更新它们上面的策略。也就是说，Libratus不仅仅是在比赛前学习，而且还能在比赛中学到东西。

（3）持续自我强化（Continual self-improvement）。比赛中人类高手会寻找Libratus 的漏洞，并展开有针对性的攻击。这个模块的作用就是发现问题所在，找到更多细节进行自我强化，然后得到一个更好的纳什均衡。[2]

### CFR 算法

2007 年, 加拿大阿尔伯塔大学的 Zinkevich 和 Johanson 提出了基 Regret Minimization, 其中, Regret Minimization 即为悔恨值最小化。 算法的核心在于博変中的纳什均衡探寻。

$$
R_i^T=\frac{1}{T} \max _{\sigma_i \Sigma_i} \sum_{i=1}^T\left(u_i\left(\sigma_i^*, \sigma_{-i}^t\right)-u_i\left(\sigma^t\right)\right)
$$

悔恨值是在线学习中的概念。在扩展式博变中, 平均悔恨值的计 算方法如公式 (2)。其中, $\sigma_i^t$ 是玩家 $\mathrm{i}$ 在第 $\mathrm{t}$ 轮游戏中所使用的策 略, $\mathrm{u}$ 为玩家收益。悔恨值最小化算法就是将每步策略的收益与平均 收益相比较, 得到差值, 并根据差值大小选择下一次的相应策略。在 零和游戏中, 如果双方玩家的平均悔恨值均小于 $\varepsilon$, 则可以看作达到 了一个 $2 \varepsilon$ 均衡。

CFR 算法与普通悔恨值最小化算法的不同之处在于其将平均悔 恨值分解为一系列的可加悔恨值项, 即反现实悔恨值 （counterfactual regret）， 因此可以分别进行最小化。反现实悔恨值定义在独立的信 息集上，而平均悔恨值受限于反现实悔恨值之和。

$$
R_i^T(I, a)=\frac{1}{T} \sum_{t=1}^T \pi_{-i}^{\sigma^{\prime}}(I)\left(u_i\left(\left.\sigma^t\right|_{l \rightarrow a}, I\right)-u_i\left(\sigma^t, I\right)\right)
$$

对于信息集 $\mathrm{I}$ 中的每一个 $\pi_{-i}^{\sigma^*}$ 可选行动 $\mathrm{a}$, 玩家 $\mathrm{i}$ 在时间 $\mathrm{T}$的反现实悔恨值如公式 (3) 所示。其中, 表示除玩家 $\mathrm{i}$ 外其他玩家 依据策略 $\sigma$ 达到当前信息集的概率。图 7-2 展示了 CFR 算法的迭代求 解过程。





在近年来，CFR 算法及其变形广泛应用于扑克游戏中近似纳什均衡解的计算。

### CFR+ 算法

在2015 年，阿尔伯塔大学的Bowling，Burch 与Johanson等研究人员以CFR 算法为基础，提出了一种叫做CFR+的新算法[159]，完成了一对一有限注德州扑克的求解。CFR 算法截取博弈过程的一部分进行迭代，而CFR+算法对整棵博弈树迭代，且规定悔恨值必须为正。


### DeepStack 算法

DeepStack 算法是于2017 年由CFR+算法的研究团队提出的又一新算法。与CFR 算法不同的是，DeepStack 算法解决的是一对一无限注德州扑克问题[157]。相对于一对一有限注德州扑克，无限注德州扑克的复杂度更高，因此也更难解[160]。

DeepStack 算法由三个部分组成：针对当前公共状态的本地策略计算（local strategy computation）[161]，使用任意扑克状态的学习价值函数实现有限深度的前瞻（depth-limited lookahead）[162]，以及预测动作的受限集合[163]。

此外，DeepStack 还采用了深度神经网络（Deep neural networks  ，DNNs）[164]分别训练了在发下三张公共牌后（flop network）、发下第四张公共牌后（turn network）价值的估计。深度神经网络使用了七个
全连接隐含层，每层 500 个节点。训练样本分别为 1,000,000 盘与10,000,000 盘游戏。网络得到的输出为各玩家在各种手牌情况下评估值组成的向量

图7-3（a）中，在每一个公共状态中，DeepStack 使用有限深度
的前瞻估计当前局面，前瞻时子树的估值使用训练好的深度神经网络
（b）计算。而（b）中神经网络的训练样本为由（c）随机生成的扑
克局面。


---

冷扑大师 Libratus[30]为卡耐基梅隆大学开发的二人无限注德州扑克智能体，在正式比赛中战胜了顶级人类选手.  按照博弈学习框架，在此应用中，二人无限注德州扑克被建模为二人零和不完美信息扩展形式博弈，博弈解概念是近似纳什均衡，博弈解计算基于反事实遗憾最小化算法并在求解过程中通过约简问题的离线求解与更细粒度的子博弈安全实时求解实现策略的优化。

Libratus 以反事实遗憾最小化算法为基本框架求解博弈的近似纳什均衡，包括三个求解模块：

离线的蓝图策略模块给出约简后的博弈中基于反事实遗憾最小化算法得到的博弈策略（用于博弈的前两个阶段preflop 和flop，约简过程考虑下注约简与牌面约简）；
实时的子博弈安全求解模块优化蓝图策略模块在博弈的后两个阶段（turn 与river）的行动（不进行牌面约简）；
蓝图策略自主提升模块依据与人类选手的对抗情况补全约简丢失的重要决策点（博弈树的分支，依据对手实际行动构建）。

具体地，蓝图策略模块首先对完整的二人无限注德州扑克博弈进行约简，约简包括下注约简与牌面约简.

- 前者采用一种应用无关的参数优化算 法 （ application-independent parameter-optimization algorithm）[61]获得局部较优的下注集合；
- 后者则依赖一定的领域知识，将博弈后两个阶段的牌面构型分别从5500 万种与240万种约简为250 万种与125 万种。约简后的博弈变得可求解（约简后博弈的决策点数目远小于约简前的决策点数目），然后采用离线计算的方式通过蒙特卡洛反事实遗憾最小化算法获得约简后博弈的近似纳什均衡策略。

实时的子博弈求解模块采用安全嵌套子博弈求解算法，用于在turn 和river阶段替换粗糙的蓝图策略，其中安全性体现在子博弈求解时采用了估计最大边际子博弈求解方法（Estimated-Maxmargin subgame solving），嵌套则表示每个后续的决策点都会重复上述子博弈求解算法。
- 通过对非蓝图行动构建独立的解，安全嵌套子博弈求解算法得到了相比于蓝图策略更优的子博弈解。蓝图策略自主提升模块与蓝图策略模块一样，也采用了离线计算的方式，其目的在于将观察到的未出现在蓝图策略上的分支进行补全，以得到更全面的近似纳什均衡解，从而消除比赛过程中博弈策略潜在的弱点。

## Pluribus


## AlphaHoldem

[6]

[1]: https://zhuanlan.zhihu.com/p/73268685
[2]: https://www.ambchina.com/data/upload/image/20220226/2017%E4%B8%AD%E5%9B%BD%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD%E7%B3%BB%E5%88%97%E7%99%BD%E7%9A%AE%E4%B9%A6--%E6%99%BA%E8%83%BD%E5%8D%9A%E5%BC%88-2017.pdf
[3]: https://www.ambchina.com/data/upload/image/20220226/2017%E4%B8%AD%E5%9B%BD%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD%E7%B3%BB%E5%88%97%E7%99%BD%E7%9A%AE%E4%B9%A6--%E6%99%BA%E8%83%BD%E5%8D%9A%E5%BC%88-2017.pdf
[4]: https://personal.ntu.edu.sg/boan/Chinese/%E5%88%86%E5%B8%83%E5%BC%8F%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD%E7%AE%80%E4%BB%8B.pdf
[5]: http://cjc.ict.ac.cn/online/onlinepaper/zl-202297212302.pdf
[6]: https://zhiqianghe.blog.csdn.net/article/details/126858696
