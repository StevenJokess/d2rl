

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-29 23:30:00
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-30 02:03:27
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# MuZero

2016年，我们推出了AlphaGo，这是第一个能够在围棋比赛中击败人类的人工智能（AI）程序。两年后，它的继任者AlphaZero从零开始学习并掌握了围棋、国际象棋和将棋。现在，在《自然》杂志上发表的一篇论文中，我们描述了MuZero，这是在通用算法追求中的一大步。MuZero可以在未知环境中规划获胜策略，而不需要事先告诉它规则，因此可以掌握围棋、国际象棋、将棋（shogi）和Atari等多种游戏。

> 不知道规则：[6]
>
> 严谨的理论描述和定义：
> - 不知道：在深度神经网络和蒙特卡罗树搜索运行的过程当中，没有使用游戏的规则
> - 规则：（V）代表知道，（X）代表不知道，（VX）代表知道一半
>   - **围棋规则**：
>   - 一、终局和胜负的判断（X）
>   - 二、提子【没气】（X）
>   - 三、由谁落子（V），以及允许的落子（VX）
>   - 额外的：开始局面没子（也是设定好的），不能悔棋（根本算法里没悔棋的选项）
>
>   - **-> 强化学习规则**：
>   - 一、终局和胜负的判断 ->最终状态以及奖励的判断（X）
>   - 二、提子（变成特例）（X） ->状态、行动、下一个状态 $f(s_t, a_t) = s_{t+1}$
>   - 三、由谁落子，以及允许的落子 -> 采取行动的Agent（V）& 允许的行动（VX）

多年来，研究人员一直寻求一种方法，既可以学习一个解释它的环境的模型，又可以利用该模型规划最佳行动方案。但迄今为止，大多数方法在规划复杂且未知的领域（如Atari）中都面临着困难。

MuZero首次在2019年的初步论文中提出，通过学习一个仅关注环境中最重要方面以进行规划的模型，解决了这个问题。通过将这个模型与AlphaZero强大的前瞻搜索树结合起来，MuZero在Atari基准测试中创造了一个新的最高水平的结果，同时在围棋、国际象棋和将棋的经典规划挑战中与AlphaZero的表现相匹配。这样一来，MuZero展示了强化学习算法能力的重大飞跃（a significant leap forward）。

![AlphaGo -> MuZero](../../img/MuZero_domain_knowledge.png)

**主要创新点**：介绍了隐含状态（hidden state）的概念，来代替游戏的动态模型。这个模型被称为"学习的动态模型（learned dynamics model）"。我估计隐含状态就是s^i=h()

> 普通的状态（state）定义为代理当前所观察到的完整信息，
> 隐藏状态（hidden state）则是指一个经过编码或者压缩的状态，其中包含了代理当前状态的重要信息。与普通的状态不同，隐藏状态并不需要完整地包含所有代理观察到的信息，而是只需包含与当前策略和价值预测相关的信息即可。隐藏状态的设计旨在提高强化学习算法的计算效率和泛化能力，因为它可以**减少输入信息的维度并抓住关键的特征**。

## 泛化到“不知道的模型”（Generalising to unknown models）

规划能力是人类智能的重要组成部分，它使我们能够解决问题并做出关于未来的决策。例如，如果我们看到天空中出现了乌云（dark clouds forming），我们可能会预测会下雨，并决定在出门前带上一把雨伞。人类能够快速学习这种能力，并能够将其推广到新的情境（scenarios），这是我们也希望算法具备的特征。

研究人员一直试图通过使用两种主要方法来解决人工智能中的这个重大挑战：前瞻搜索（lookahead search）或基于模型的规划。

使用前瞻搜索的系统，例如AlphaZero，在古典游戏（如跳棋、国际象棋和扑克）中取得了显著的成功，但是它们依赖于对环境动态的知识，例如游戏规则或准确的模拟器。这使得将它们应用于杂乱的真实世界问题变得困难，这些问题通常是复杂的，并且很难归纳为简单的规则。

基于模型的系统旨在通过学习一个准确的环境动态模型来解决这个问题，然后使用该模型进行规划。然而，模拟环境的各个方面的复杂性意味着这些算法无法在图像丰富的领域（如Atari）中竞争。直到现在，Atari上的最佳结果都来自无模型的系统，例如DQN、R2D2和Agent57。顾名思义，无模型算法不使用学习的模型，而是估计下一步应该采取的最佳行动。

MuZero采用了一种不同的方法来克服以前方法的局限性。MuZero不是试图对整个环境建模，而是只对**对决策过程重要的方面**（ arxiv:directly relevant for planning改成了）进行建模。毕竟，知道雨伞能让你保持干燥比模拟空气中雨滴的模式更有用（knowing an umbrella will keep you dry is more useful to know than modelling the pattern of raindrops in the air）。

具体而言，MuZero模拟环境的三个关键要素是：

- 价值：当前局面（current position）有多好？
- 策略：应该采取哪种行动？
- 奖励：上一次行动有多好？

这些要素都是使用深度神经网络学习的，并且这些要素是MuZero理解采取某种行动会发生什么以及相应地进行规划所需的全部内容。



这是是Monte Carlo Tree Search如何与MuZero神经网络进行规划的示意图。从游戏中的当前位置开始（动画顶部的示意围棋棋盘），MuZero使用**表示函数**（h）将观察结果映射到神经网络使用的嵌入（s0）。使用动态函数（g）和预测函数（f），MuZero可以考虑可能的未来行动序列（a），并选择最佳行动。



MuZero利用与环境交互时收集的经验来训练其神经网络。这种经验包括来自环境的观察结果和奖励，以及在确定最佳行动时执行的搜索结果。



在训练期间，模型随着收集到的经验展开（unrolled），每一步都预测先前保存的信息：值函数v预测观察到的奖励总和（u），策略估计（p）预测先前的搜索结果（π），奖励估计r预测最后观察到的奖励（u）。



这种方法还有另一个重大优点：MuZero可以**重复使用**其学习的模型来改进其规划，而不是从环境中收集新的数据。例如，在对Atari套件进行测试时，这种变体 - 称为MuZero Reanalyze - 90％的时间使用学习的模型重新规划过去的事件。



## MuZero的表现

我们选择了四个不同的领域来测试MuZero的能力。Go、棋类游戏和将棋被用于评估其在具有挑战性的规划问题上的性能，而我们使用Atari套件作为更具视觉复杂性问题的基准。在所有情况下，MuZero都刷新了强化学习算法的最新成果，并在Atari套件上胜过了所有先前的算法，同时在Go、棋类游戏和将棋上与AlphaZero的超人表现相匹配。

TODO:pic

这是使用每个训练运行的200M或20B帧时，在Atari套件上的表现。MuZero在两种设置中都刷新了最新的强化学习算法成果。所有分数都以人类测试者的表现（100％）为标准，每种设置的最佳结果都以粗体突出显示。

我们还对MuZero如何使用其学习的模型进行更详细的规划进行了测试。我们从具有挑战性的精度规划问题开始，这个问题出现在围棋中，其中一个单独的行动可以决定胜负。为了确认规划更多时间是否会导致更好的结果的直觉，我们测量了MuZero的完全训练版本在为每个行动规划更多时间的情况下可以变得更强的程度（见下图左侧）。结果表明，当我们将每个行动的时间从0.1秒增加到50秒时，游戏的强度可以增加超过1000个Elo（这是一个评估玩家相对技能的指标）。这类似于强业余玩家和最强职业选手之间的差距。

左：当每个行动的规划时间增加时，围棋中的游戏强度显着提高。请注意，MuZero的扩展几乎完美地匹配AlphaZero的扩展，后者可以访问完美的模拟器。右：在训练期间，Ms Pac-Man中的得分也随着每个行动的规划次数增加而增加。每个图表显示了不同的训练运行，MuZero被允许考虑每个行动的不同模拟次数。

为了测试规划是否在训练期间也带来了优势，我们在Atari游戏 Ms Pac-Man上进行了一系列实验（上图右侧），使用单独训练的MuZero实例。每个实例都可以考虑不同数量的规划模拟，范围从五到50。结果证实，增加每次移动的规划模拟量可以让MuZero学习更快，并实现更好的最终性能。

有趣的是，当MuZero只允许在每次移动时考虑六到七个模拟时 - 这个数字太小，无法覆盖Ms Pac-Man中所有可用的动作时 - 它仍然能够实现良好的性能。这表明MuZero能够在动作和情境之间进行泛化，并且不需要详尽地搜索所有可能性来有效地学习。

## 新的视野

MuZero既能够学习环境的模型，又能够成功地利用它进行规划，这证明了强化学习和通用算法的显著进步。其前身AlphaZero已经被应用于化学、量子物理学等复杂问题。MuZero强大的学习和规划算法背后的思想可能为解决机器人、工业系统和其他杂乱的真实世界环境中的新挑战铺平道路，这些环境的“游戏规则”不为人所知。

[1]: https://www.deepmind.com/blog/muzero-mastering-go-chess-shogi-and-atari-without-rules
[2]: https://www.nature.com/articles/s41586-020-03051-4.epdf?sharing_token=kTk-xTZpQOF8Ym8nTQK6EdRgN0jAjWel9jnR3ZoTv0PMSWGj38iNIyNOw_ooNp2BvzZ4nIcedo7GEXD7UmLqb0M_V_fop31mMY9VBBLNmGbm0K9jETKkZnJ9SgJ8Rwhp3ySvLuTcUr888puIYbngQ0fiMf45ZGDAQ7fUI66-u7Y%3D
[3]: https://www.bilibili.com/video/BV147411i7tM/?spm_id_from=333.337.search-card.all.click
[4]: https://en.wikipedia.org/wiki/MuZero
[5]: https://arxiv.org/pdf/1911.08265.pdf
[6]: https://www.bilibili.com/video/BV1Ci4y1A7Dn/?spm_id_from=333.337.search-card.all.click&vd_source=bca0a3605754a98491958094024e5fe3

> 1. https://chat.openai.com/chat; prompt:MuZero⇒ Remove assumption of a given gynamics model, introduce hidden state in arder to do MCTS with a learned dynamics model, starting from a learned root state initiolization
