

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-17 17:24:02
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-17 17:25:13
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 写《动手学强化学习》的一些感想

最近出版了《动手学强化学习》，不少同行已经拿到了书，给我拍了照片。但由于上海疫情的原因，我自己现在都还没拿到书…

海峰老师让我在RLChina社区里写一个关于本书的帖子，为RLChina的赠书活动推波助澜。我很感谢RLChina大家庭对我们这本作品的大力支持。

不少感想，AI科技评论的访谈中已经有回答。这里我基于访谈内容，总结一下自己的感想。

1. “数学好，编程好，还要品格好”

我工作所在的上海交通大学APEX数据和知识管理实验室（简称APEX实验室）有一个30人的强化学习研究组。强化学习的研究其实门槛比较高，一方面它对数理统计基础要求高，另一方面它的实验总是比较难做成功，很多时候需要付出很多努力才能复现论文实验结果或者做出新的实验突破。因此我也经常开玩笑说：要做好强化学习研究，你需要“数学好，编程好，还要品格好”。最后的‘品格好’是指需要具备实事求是的态度和持之以恒的韧性，在强化学习实验调不出来时还能细心检查bug，在实验跑了一周还没起色时，愿意再坚持几天，在最终意识到自己方法确实不work时，能坦然面对，重新设计算法。

我们实验室还有一个很大数据挖掘组，做信息检索、智慧教育和图数据挖掘。普遍来讲，数据挖掘组出论文的速度是明显快于强化学习组的。平均下来，强化学习的课题一般要开展1年才能取得成果，而数据挖掘的课题往往在4个月内则可以投出去论文。旁边有这样一群总是发论文的同学在，强化学习组的同学需要抗住心理压力，潜心完成手里的课题。因此，我认为要做好强化学习的研究，实属不易。

2. 营造RL学术氛围，共同趟坑

强化学习组的师生们相互帮助，促进研究效率提升，也为带刚刚进组的新同学“避坑”，就慢慢沉淀出了一份强化学习算法的代码。而真正想到把强化学习代码整理公布出来，是有一位外校的研究生跟我讨论时说，他们实验室只有2位同学做强化学习的研究课题，问我如何才能做好强化学习的研究和实验。我当时想了想，觉得他的情况可能确实比较难一点，因为没有足够的同学一起研究强化学习，很多强化学习的理论可能会理解不够深入，很多实验方面的“坑”没有被趟过，于是就比较难以入门，进入研究深水区。

因此，如果能有一本材料，能把强化学习的理论讲透，并且把相关的实现代码就穿插在理论算法讲解中，那么学习起来可能就会更加容易体会强化学习的原理。更重要的是，这些代码要能够直接跑通，实验结果可以复现，这样就能体会到强化学习算法是如何work的。

3. Jupyter Notebook作为RL的学习形式

作为入门的材料，我们尝试探索使用Jupyter Notebook来看看它是不是可以成为一种好的学习入门强化学习的载体。一个MDP的学习材料的例子请见Link。通过APEX实验室和强化学习课堂的学生们的反馈来看，这种Jupyter Notebook的学习材料是可以有效帮助提升对强化学习原理和代码理解效率的形式。希望这本书能够帮助更多人入门强化学习。

4. 强化学习的入门材料

对于来我们APEX实验室的强化学习初学者，我建议的学习路线是：

- 先学习UCL David Silver的强化学习课程[Link](https://www.davidsilver.uk/teaching/)
  这是强化学习的基础知识，不太包含深度强化学习的部分，但- 对后续深入理解深度强化学习十分重要。
- 然后学习UC Berkeley的深度强化学习课程[Link](http://rail.eecs.berkeley.edu/deeprlcourse/)
- 最后可以可以挑着看OpenAI 的夏令营内容[Link](https://www.boyuai.com/elites/course/xVqhU42F5IDky94x)

当然，如果希望学习中文的课程，我推荐的是：
我本人在上海交通大学的强化学习课程[Link](https://www.boyuai.com/elites/course/xVqhU42F5IDky94x)
周博磊老师的强化学习课程[Link](https://www.bilibili.com/video/BV1LE411G7Xj)

可以配合《动手学强化学习》一起看的强化学习原理讲解的书如下：

- Richard S. Sutton and Andrew G. Barto. - “Reinforcement Learning: An Introduction (Second - Edition) .” MIT Press, 2018.
- 俞凯[译].《强化学习（第2版）》. 电子工业出版社，2019.
王琦、杨毅远、江季.《Easy RL 强化学习教程》. 人民邮电出版社，2022.
1. 更上一层楼

要知道本书只是为了帮助大家入门强化学习的原理理解和初步的代码实践。本书的代码只是皮毛。要成为强化学习高手，各位同学们还需要保持一个不断进取学习的心，多看论文，多动手跑实验，多多讨论。祝各位学有所成！

[1]: http://rlchina.org/topic/398
TODO:
https://www.6aiq.com/article/1535425620363