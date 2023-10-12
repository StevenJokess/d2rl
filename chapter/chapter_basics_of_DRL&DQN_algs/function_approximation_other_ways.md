

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-10-13 02:17:25
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-13 02:18:27
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 函数近似强化学习

虽然基于表格的经典算法在小规模离散空间的强化学习任务上表现不错,但更多的实际问题中状态数量很多,甚至是连续状态空间,这时经典强化学习方法难以有效学习.特别对于连续状态空间离散化的方法,当空间维度增加时,离散化得到的状态数量指数增加,可见基于表格值的强化学习算法不适用于大规模离散状态或者连续状态的强化学习任务[2],Bellman用"维度灾难"来描述这一困难.

研究人员从不同的角度试图克服维度灾难.例如,分层强化学习(Hierarchical reinforcement learning)采用了"分而治之"的思想,把一个强化学习问题分解成一组具有层次的子强化学习问题,降低了强化学习问题的复杂度[10-20];迁移强化学习(Transfer learning in reinforcement learning)侧重于如何利用一个已学习过的强化学习问题的经验提高另一个相似但不同的强化学习问题的学习性能[17,21-30];函数近似(Function approximation)则将策略或者值函数用一个函数显示描述[4,31-39]等.这三类方法分别从不同角度求解维度灾难问题,其中函数近似方法是最直接的求解方法,受到了很多关注.

根据函数近似的对象不同划分,可以分为策略搜索(Policy search)和值函数近似(Value function approximation).

1)策略搜索指直接在策略空间进行搜索,又分为基于梯度(Gradient-based)的方法和免梯度(Gradient-free)方法.

基于策略梯度的方法:从一个随机策略开始,通过策略梯度上升的优化方法不断地改进策略,例如通用策略梯度方法(Policy gradient method)[40]、自然策略梯度方法(Natural policy gradient)[41]、自然演员--评论员方法(Natural actor-critic)[42];

免梯度方法:从一组随机策略开始,根据优胜劣汰的原则,通过选择、删除和生成规则产生新的一组策略,不断迭代这个过程以获取最优策略,例如遗传算法(Genetic algorithm)[43]、交叉熵方法(Cross entropy methods)[44-45]、蚁群优化算法(Ant colony optimizaition)等.

2)值函数近似指在值函数空间进行搜索,又分为策略迭代(Policy iteration)和值迭代(Value iteration).

策略迭代:从一个随机策略开始,通过策略评估(Policy evaluation)和策略改进(Policy improvement)两个步骤不断迭代完成,例如最小二乘策略迭代(Least squares policy iteration)[35,46]、 λ最小二乘策略迭代(Least squares λ policy iteration)[47-48]、改进的策略迭代(Modified policy iteration)[49-50];

值迭代:从一个随机值函数开始,每步迭代更新改进值函数.由于天然的在线学习(Online learning)特性,值迭代是强化学习研究中的最重要的研究话题.

而根据函数模型的不同划分,函数近似包括基于线性值函数近似的强化学习、基于核方法的强化学习、基于加性模型的强化学习和基于神经网络的强化学习.下面我们从函数模型的角度介绍相关工作.

2.1   基于线性值函数近似的强化学习
线性值函数近似通过一组特征 ϕ
 和对应权重θ的线性乘积来估计某个状态S的值:

Vθ(s)=∑i=1nθiϕi(s)

(6)
在强化学习函数近似中,线性函数近似因其简便实现以及简易分析的特点,引起了强化学习研究者的广泛关注,并得到了深入研究.其相关工作主要从两个方面展开:梯度法和最小二乘法.

梯度法: 1988年,Sutton首次提出了线性时间差分学习(Linear temporal difference,linear TD)以及TD(λ)算法[4],并证明了线性TD(0)在最小化均方差(Mean square error,MSE)意义下的收敛性[4].1997年,Tsitsiklis等证明了线性TD(λ)的收敛性,并给出了误差界[51].然而当学习率α或者资格跟踪参数e选得不合适时,线性TD(λ)甚至会发散[32].Bradtke等在1995年提出的"归一化线性TD(λ)"(Normalized TD(λ))[32],它不修改调整的方向,而是限定调整值的大小,以此减少TD不稳定行为的几率.1995年,Baird等提出了一种的残差法(Residual algorithms),在保证残差梯度法的收敛性的同时,提高了收敛速度[52].2008年,Sutton等提出了梯度时间差分学习(Gradient temporal difference,GTD)算法,该算法的复杂度为O(n),适用于离策略的学习,并且被证明可以收敛到最小二乘解[53].不过,GTD算法与传统的线性TD方法比,收敛速度要慢很多.上述几种算法的目标是最小化Bellman均方差(Mean square Bellman error,MSBE),与此不同,2009年,Sutton等提出了两种具有里程碑意义的新型算法,GTD二代(GTD2)以及TDC,这两种算法的目标是最小化投影Bellman均方差(Mean square projected Bellman error,MSPBE)[39].Sutton等人揭示了这两种算法才是真正的梯度下降方法(换序二次偏导值相等),它们的计算复杂度为O(n),并且收敛的速度比GTD要快很多,但比直接梯度法和残差梯度法慢.2010年,Maei通过最小化λ权重的MSPBE,得到了一个学习预测算法GQ(λ)[54],并且将之扩展为一个学习控制算法Greedy-GQ[55].

最小二乘法: 1996年,Bradtke等提出了最小二乘时间差分算法(Least square temporal difference,LSTD)[33].Boyan于2002年将LSTD扩展到LSTD(λ)[34].2003年Lagoudakis提出了最小二乘策略迭代算法(Least square policy iteration,LSPI),以获得更好的稳定性[35].Bradtke等于1996年根据增量求逆技巧,提出了在线的递归LSTD算法(Recursive LSTD,RLSTD)[33].RLSTD每步的计算复杂度依然是O(n2),这对于很多具有大量特征的应用(例如围棋的特征有100多万)而言是不现实的[36-37].2006年,Geramifard提出了增量最小二乘时间差分学习(iLSTD)算法,其计算复杂度为O(n),空间复杂度为O(n2),相比于传统的TD方法,具有更好的数据有效性,相比于LSTD方法具有更好的计算有效性[56].同年,Geramifard又提出了带资格跟踪的增量最小二乘时间差分学习(iLSTD with eligibility traces),并给出了收敛性证明[38].此外,还有众多研究者提出了基于正则化方法的最小二乘迭代算法[57-61].2010年,为了解决高维度特征的强化学习问题,尤其当特征数目超过样本数目时,Ghavamzadeh提出了基于随机投影的LSTD方法(LSTD with random projections,LSTD-RP)[62];2011年,为了解决在关联矩阵求逆中出现的近乎奇异问题,Bertsekas提出了新的时间差分算法[63].

2.2   基于核方法的强化学习
1998年,Sutton等人提出了一类基于径向基函数网络(Radial basis function,RBF)的强化学习方法[2].2002年,Ormoneit等人明确提出了基于核方法的强化学习(Kernel-based reinforcement learning,KBRL),确立了KBRL的研究方向[64].近年,KBRL吸引了众多的国内外强化学习研究者.在2006年的International Conference on Machine Learning(ICML)会议上,专门设立了一个Workshop on Kernel Machines and Reinforcement Learning来讨论KBRL的问题.国际著名期刊IEEE Transactions on Neural Networks and Learning Systems于2012年专门组织了Special Issue on Online Learning in Kernel Methods.

根据表示定理,基于核方法的强化学习的值函数通过一组核函数 k(⋅,⋅)
 和对应权重θ的线性乘积来估计某个状态S的值:

Vθ(s)=∑i=1nθik(s,si)

(7)
其中,集合{si}称为字典(Dictionary/D).基于核方法的强化学习需要考虑以下三个问题:核函数 k(⋅,⋅)
 如何选择、字典D能否稀疏化构造、以及值函数的参数怎样估计.

核函数 k(⋅,⋅)
 的选择:不同的核函数适用于不同的强化学习问题.对于复杂或困难的强化学习问题,单个核函数并不有效、甚至无法求解.此外,在目前关于KBRL的研究中,核函数都是根据经验或者实验人员的试错决定的.因此,针对任意的强化学习问题,如何找到一个普适的方法或原则来自动选择核函数,将成为KBRL研究的热点.

字典D的稀疏化构造:基于核方法的值函数有一对矛盾: 1)字典D中元素(si)个数越多,值函数的表达能力越强;2)字典D中元素(si)个数越多,则值函数的复杂度越大,越不利于参数学习.

在KBRL的研究初期,字典D是预先设定好的,如文献[2,64-73].此后,字典的自动稀疏化构造方法得到了关注: 1)近似线性依赖(Approximate linear dependence,ALD)方法[74-77],该方法的单步计算复杂度为O(n2);2)核界定感知方法(Bounded kernel-based perceptron)[78-80];3)基于选择性集成学习(Selective ensemble learning)的字典稀疏化,如基于核距离的在线稀疏化方法[81].

值函数的参数估计:基于单个核函数的值函数可以通过多种方法来进行参数估计,如高斯过程时间差分学习(Gaussian process temporal difference,GPTD)[74-75]、基于核方法的奖赏回归(Kernel rewards regression,KRR)[65]、基于核方法的优先排序遍历方法(Kernel-based prioritized sweeping,KBPS)[66]、基于核方法的稀疏最小二乘时间差分学习方法(Kernel-based LS-TD,KLSTD)[76]、基于核方法的最小二乘策略迭代方法(Kenel-based least-squares policy iteration,KLSPI)[77]、 Bellman残差最小化(Bellman residual minimization)[69]、 Bellman残差消除算法BRE(SV)[70]、基于核密度估计的无参动态规划(Non-parametric dynamic programming,NPDP)[73]、以及基于核方法的在线选择时间差分学习(Online selective kernel-based temporal difference learning,OSKTD)[81].

多核学习方法是当前机器学习领域的一个新的热点[82].在监督学习中,多核学习方法是将多个核函数进行组合,可以用于求解数据异构、数据不规则、样本规模巨大、样本不均匀分布等问题[83-84].因此,将多核学习引入KBRL有利于求解复杂或困难的强化学习问题.

2.3   基于加性模型的强化学习
加性模型在监督学习中的使用较为常见,例如Boosting方法得到的模型都是加性模型,这一类模型将多个现有模型结合起来,有很强的表示能力,而在强化学习中基于加性模型的方法还很少.目前已有的基于加性模型的强化学习方法都是策略梯度方法,因此这些方法直接用加性模型来表示策略,而不表示值函数:

π(s)=∑i=1kθifi(s)

其中,fi可以是任意现有监督学习回归模型,例如决策树或者神经网络.加性模型的学习,不涉及每个基模型fi的学习,通常假设fi可由现有学习方法来解决,例如线性回归算法、随机森林等经典机器学习算法.加性模型的学习,是基于损失函数(长期累积奖赏)的泛函梯度,对fi一个一个的顺序学习,已减少损失函数残差.

NPPG方法是第一个基于加性模型的策略梯度算法[85].然而后来发现,NPPG仅在简单问题上有效,问题复杂时会出现过拟合问题,影响了对策略的搜索[86],由此提出了PolicyBoost方法[86].需要注意的是,加性模型的训练每一轮会增加一个基模型,当迭代次数很多时,加性模型自身的计算开销就会很大,由此提出了Napping方法来将线性增加的模型数量降到了常数大小[87].

2.4   基于神经网络的强化学习
基于神经网络的强化学习顾名思义采用神经网络作为函数近似的模型.

多层神经网络:克服了单层感知器不能进行非线性分类问题,多层神经网络于上世纪80年代强势回归.神经网络用于求解强化学习问题稍晚一些,早期的代表作是1995年前后轰动一时的TD-Gammon.它采用了三层神经网络模型(即输入层、隐含层、输出层),结合TD(λ)学习、自我博弈、梯度下降的误差反向传播法则以及多步约简搜索.其中,TD-Gammon 1.0版本并未采用任何西洋双陆棋戏(Backgammon)的领域知识,达到了当时电脑程序的最佳水平;TD-Gammon 2.0版本添加了基于领域知识的特征作为神经网络的输入,并结合两步搜索,达到了人类顶级专家的水平[88-89].

基于三层神经网络模型的强化学习算法,其性能(不考虑参数更新方式)依赖于首层的网络输入,一旦使用了专家级的领域知识,问题将变得容易求解,效果也会很好.然而在实际应用中,专家级的领域知识并不容易获取,如何在没有领域知识的情况下获得高性能成为了近年来强化学习的研究热潮.

深度神经网络:深度学习模型是深层的神经网络,即有多个(三个以上)隐含层.自2006年开始,深度学习在语音识别、手写数字识别等图像、视频、语音和音频处理取得了突破性进展[90-91].深度学习的成功在于,它把原始数据通过一些简单的但是非线性的模型转变成为更高层次的,更加抽象的表达.这个过程不需要利用人工工程来设计的,而是使用一种通用的学习过程从数据中学习的.因此,深度学习实际上是一种特征学习方法.Conference on Neural Information Processing Systems(NIPS)从2015年起开始举办深度强化学习研讨会,预示着深度强化学习正在迅速发展.

深度学习用于强化学习的研究同样滞后于监督学习.2010年,Deep fitted Q-iteration(DFQ)采用基于深度自动编码器的神经网络学习图像的特征,结合批量Q学习,获取了路径寻优策略[92].其中,与常见深度学习模型多层受限玻尔兹曼机(Restricted Boltzmann machine,RBM)不同,DFQ采用了深层感知器模型,使用RProp规则[93]进行逐层预训练和微调获得编码器,并使用该编码器构造特征.在同样的图像数据集上的对比实验,结果表明深度学习只需要两个维度就能重现出比主成分分析方法(Principal component analysis,PCA)好的图像.由于深度神经网络在状态表示上显示出的优势,基于深度神经网络的"深度强化学习"研究呈现井喷涌现的势态,研究论文主要出现在ICML、NIPS等会议上.[1]

[1]: http://www.aas.net.cn/cn/article/doi/10.16383/j.aas.2016.y000003?viewType=HTML
