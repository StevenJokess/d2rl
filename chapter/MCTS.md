

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-12 21:27:17
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-13 00:59:38
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 蒙特卡洛树搜索算法（MCTS）

将UCB用于强化学习算法中，最成功的例子莫过于蒙特卡洛树搜索算法，该算法在计算机围棋中取得极大成功，如今强化学习技术如此火热也是因为加持了深度神经网络的蒙特卡罗树搜索成功地战胜了人类顶级棋手，晋级为“围棋上帝”。很多学强化学习的人常常会问，蒙特卡罗树搜索跟经典的强化学习算法有什么区别和联系？这一讲，我会从强化学习的视角讲述蒙特卡罗树搜索算法。

## 在线强化学习算法

**先给一个结论：蒙特卡罗树搜索可以看成是一种在线强化学习算法。**下面，我们将这句话分成几个关键词进行解释。

### 1. 蒙特卡罗树搜索是在线强化学习

第一个概念就是在线强化学习，英文名online reinforcement learning. 跟在线强化学习对应的是离线强化学习，即offline reinforcement learning. 在线强化学习是从当前状态开始，利用强化学习算法得到最优马尔科夫决策过程。而离线强化学习是在每个状态处都得到一个最优的策略。两者的区别非常明显：

1. 它们的目标不同。离线强化学习的目标非常有野心，它要求解在每个状态处的最优策略，它要求一个全局解；在线强化学习的目标则很局部，它只求从当前状态开始的最优策略是什么。
2. 在线强化学习的计算复杂度和难度远远低于离线强化学习。为什么呢？因为在线强化学习只关心从当前状态出发的最优策略，因此它的搜索空间远远小于离线强化学习。

蒙特卡罗树搜索是一个在线的算法，因为蒙特卡罗树搜索的目的是找到当前状态下最优的策略，比如在面对当前棋盘时，应该下哪个子（最优策略）。

### 2. 蒙特卡罗树搜索是一种强化学习算法

广义的强化学习算法包括策略评估和策略改善，蒙特卡罗树在构建和使用的过程中同样包括这两个过程，所以蒙特卡罗树也是一种强化学习算法。我在新书《深入浅出强化学习：原理入门》（京东、亚马逊、淘宝等均有销售）已经讲过，基于值函数的方法包括动态规划方法，蒙特卡罗模拟方法和时间差分方法。这三种方法的唯一不同点在于值函数的评估方法不同，动态规划方法通过模型来评估值函数，蒙特卡罗模拟的方法通过随机模拟，并求解经验平均来估计，时间差分方法则通过一步或多步bootstrap方法来评估值函数，而蒙特卡罗树搜索呢？

结论是，蒙特卡罗树搜索通过构造蒙特卡罗树来估计值函数。

## 蒙特卡罗树搜索概念

蒙特卡罗树搜索如何通过构造蒙特卡罗树来估计值函数呢？这就需要了解蒙特卡罗树搜索到底是什么了。

### （1） 首先蒙特卡罗树搜索是一种基于树的搜索方法

基于树的搜索算法历史悠长，可以追溯到计算机的诞生，并且基于树的搜索算法得到了广泛的应用，可以说当今文明的背后都是一个个基于树的搜索算法在起作用。比如最早的击败顶级人类国际象棋选手的“深蓝”用的是基于树的搜索算法；在机器人学中最常用的路径规划算法 $A^*$ 算法，也是基于树的搜索算法。

有人可能会问，为什么不能用深蓝的搜索技术来下围棋？

原因是“深蓝”的搜索算法用的是全宽度树搜索，而围棋的状态空间非常巨大，利用全宽度树进行搜索，计算时间，存储空间都不可行。

![全树示意图](../img/Full_Width_Tree.png)

如图2.2 为全宽度树（Full Width Tree）的示意图。所谓全宽度树是每个节点都需要进行扩展，节点的数目跟深度成指数增长。因此全宽度树对于大尺度空间，多步决策几乎不可行。既然全宽度树无法解决问题，那么能不能构造一种非全宽度树呢？

### （2） 蒙特卡罗树搜索是一种非对称树搜索

全宽度树是在每个结点考虑了所有可能的动作以及所有可能的后继状态。我们已经说过，这种构造树的方法对于大尺度空间和多步决策不可行。蒙特卡罗树搜索采用了两个技巧来构造非对称树。

第一个技巧：引入树策略（tree policy）, 所谓树策略是指在树中进行搜索的时候，当前节点应该如何选择下一个节点。如图2.3为蒙特卡罗树示意图。常用的树策略为贪婪策略，即采样当前值函数最大的动作。通过这种策略，可以将搜索的资源都集中在那些很有潜力的子结点上，因此构成了一种非对称树。

跟全宽度树相比，利用树策略进行采样可以消减搜索树的宽度，极大地减小了树的节点数。

第二个技巧：引入蒙特卡罗方法评估叶节点，如图2.3，当树搜索到叶节点的时候，蒙特卡罗树搜索不会继续一直不停地往深处扩展，而是通过蒙特卡罗方法评估该叶节点处的值函数。关于利用蒙特卡罗方法评估值函数，我在新书《深入浅出强化学习：原理入门》（京东，亚马逊，淘宝等网站均有出售）已经详细介绍过了。这里再啰嗦下，蒙特卡罗评估方法其实就是利用随机策略进行模拟，然后求经验平均。这里的随机策略又称为rollout policy或者默认策略。即，在蒙特卡罗模拟时，每步所采用的策略为rollout
policy. 为什么叫rollout policy？从使用上来看，该策略在模拟阶段使用，并且使用到出现一个结果（对于围棋来说，结果为输，赢，和局），因此这个策略可称为rollout 策略或者默认策略。

## 结合UCB的算法

蒙特卡罗树搜索真正强大起来，还需要结合UCB的算法。

2006年是神奇的一年，在这一年，Hinton等人在science发表论文，开启了深度学习的研究热潮。同年，在强化学习领域，来自不同国家的科学家将UCB引入到蒙特卡罗树算法中， 如Kocsis等将UCB结合蒙特卡罗树提出UCT算法, Gelly等将UCB引入到围棋中，开发了强大的围棋程序MoGo。UCB的引入以及多臂赌博机的进一步发展最终促进了围棋上帝AlphaGo Zero的出现。

## UCB如何用于蒙特卡罗树搜索中

在蒙特卡罗树的选择阶段，利用UCB策略来替换树策略即可。这个想法是这样来的：每个结点有很多子结点，在进行树搜索时，你需要从很多子结点中一个节点进行搜索，这个选择的过程十分类似多臂赌博机，在多臂赌博机中你需要面对的是有很多臂，你需要选择哪一个，而在蒙特卡罗树搜索中，在树结点处，你需要选择的是哪个子结点。

当UCB策略作为树策略时，每个结点既考虑了利用也考虑了探索，实现了利用和探索的平衡。**因为探索的存在，使得构造出的蒙特卡罗树不会遗漏最优的策略，因为利用的存在使得蒙特卡罗树不会过于庞大，因此UCB的引入极大地提升了蒙特卡罗树搜索算法的能力。**蒙特卡罗树搜索算法一经提出，便在很多领域得到广泛应用。当然，围棋从2006年的UCT算法到2017年的AlphaGo Zero，又经过十多年无数科学家的研究。这个技术如何演化的也是一个很有意思的课题，如果大家感兴趣可以找相关的论文来看。

另外：蒙特卡罗树适用的场合是：模型已知，可以进行模拟。每次模拟需要很短的时间。因此，蒙特卡罗树搜索可以看成是基于模型的强化学习算法。对于那些模型未知的场合，还得依靠时间差分，A3C，DDPG，PPO等无模型的强化学习算法。

那么这些无模型的强化学习算法是否也可以利用UCB呢？我们下次再更新，敬请期待！

## 代码

```python
class MCTS(object):
    """An implementation of Monte Carlo Tree Search."""

    def __init__(self, policy_value_fn, c_puct=5, n_playout=10000):
        """
        policy_value_fn: a function that takes in a board state and outputs
            a list of (action, probability) tuples and also a score in [-1, 1]
            (i.e. the expected value of the end game score from the current
            player's perspective) for the current player.
        c_puct: a number in (0, inf) that controls how quickly exploration
            converges to the maximum-value policy. A higher value means
            relying on the prior more.
        """
        self._root = TreeNode(None, 1.0)
        self._policy = policy_value_fn
        self._c_puct = c_puct
        self._n_playout = n_playout

    def _playout(self, state):
        """Run a single playout from the root to the leaf, getting a value at
        the leaf and propagating it back through its parents.
        State is modified in-place, so a copy must be provided.
        """
        node = self._root
        while(1):
            if node.is_leaf():
                break
            # Greedily select next move.
            action, node = node.select(self._c_puct)
            state.do_move(action)

        # Evaluate the leaf using a network which outputs a list of
        # (action, probability) tuples p and also a score v in [-1, 1]
        # for the current player.
        action_probs, leaf_value = self._policy(state)
        # Check for end of game.
        end, winner = state.game_end()
        if not end:
            node.expand(action_probs)
        else:
            # for end state，return the "true" leaf_value
            if winner == -1:  # tie
                leaf_value = 0.0
            else:
                leaf_value = (
                    1.0 if winner == state.get_current_player() else -1.0
                )

        # Update value and visit count of nodes in this traversal.
        node.update_recursive(-leaf_value)

    def get_move_probs(self, state, temp=1e-3):
        """Run all playouts sequentially and return the available actions and
        their corresponding probabilities.
        state: the current game state
        temp: temperature parameter in (0, 1] controls the level of exploration
        """
        for n in range(self._n_playout):
            state_copy = copy.deepcopy(state)
            self._playout(state_copy)

        # calc the move probabilities based on visit counts at the root node
        act_visits = [(act, node._n_visits)
                      for act, node in self._root._children.items()]
        acts, visits = zip(*act_visits)
        act_probs = softmax(1.0/temp * np.log(np.array(visits) + 1e-10))

        return acts, act_probs

    def update_with_move(self, last_move):
        """Step forward in the tree, keeping everything we already know
        about the subtree.
        """
        if last_move in self._root._children:
            self._root = self._root._children[last_move]
            self._root._parent = None
        else:
            self._root = TreeNode(None, 1.0)

    def __str__(self):
        return "MCTS"
```

[1]: https://zhuanlan.zhihu.com/p/33578829
[2]: https://github.com/junxiaosong/AlphaZero_Gomoku/blob/master/mcts_alphaZero.py
