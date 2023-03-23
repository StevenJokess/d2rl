

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-02-26 18:16:10
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-02-26 18:16:37
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 元强化学习(Meta Reinforcement Learning)

除改善一个具体任务上的学习效率外，研究人员也在寻求能够提高在不同任务上整体学习表现的方法，这与模型的通用性(Generality)和多面性(Versatility)相关。因此，我们会问，如何让智能体基于它所学习的旧任务来在新任务上更快地学习？因此有了元学习(Meta Learning)这一概念。

元学习的最初目的是让智能体解决不同问题或掌握不同技能。然而，我们无法忍受它对每个任务都从头学习，尤其是用深度学习来拟合的时候。元学习，也称学会学习(Learn to Learn)，是让智能体根据以往经验在新任务上更快学习的方法，而非将每个任务作为一个单独的任务。通常一个普通的学习者学习一个具体任务的过程被看作是元学习中的内循环(Inner-Loop)学习过程，而元学习者(Meta-Learner)可以通过一个外循环(Outer-Loop)学习过程来更新内循环学习者。这两种学习过程可以同时优化或者以一种迭代的方式进行。三个元学习的主要类别为循环模型(Recurrent Model)、度量学习(Metric Learning)和学习优化器(Optimizer)。结合元学习和强化学习，可以得到元强化学习(Meta Reinforcement Learning)方法。一种有效的元强化学习方法像与模型无关的元学习(Finn et al., 2017) 可以通过小样本学习(Few-Shot Learning)或者几步更新来解决一个简单的新任务。可参见：https://www.cnblogs.com/kailugaji/tag/Meta%20Learning/

TODO:https://www.cnblogs.com/kailugaji/p/15592726.html

[1]: https://www.cnblogs.com/kailugaji/p/15354491.html#_label3_0_2_0
