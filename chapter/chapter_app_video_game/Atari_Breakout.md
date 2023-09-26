# Breakout

在本节中，我们将介绍消砖块游戏“Breakout”并实际尝试一下该游戏。本节内容将在本地PC上执行。消砖块游戏的正式名称叫作Atari Breakout。与CartPole一样，它可以在OpenAI的Gym上运行[2]。在这个游戏中，需要玩家左右移动屏幕下部的球拍（杆），打破屏幕上部的方块，并使球不会落到屏幕下方（见图7.1）。

该任务的状态是210像素高和160像素宽的RGB信息。CartPole的实现使用了诸如小车的位置和速度之类的信息作为其状态，而不是图像信息。Breakout中没有这样的物理信息，它将图像本身用作状态。因此，状态的维度非常高，状态有210×160×3=100800个，大约有100000维。在CartPole任务中状态是4维，因此很显然两者是不同的。该任务的动作有4种。NOOP是No-Operation的缩写，意思是什么也不做。FIRE意味着发射球。RIGHT和LEFT分别是将屏幕下方的横杆向右和向左移动。

图7.3 随机移动Breakout的结果这里解释一下屏幕顶部的数字[3]。最左边的数字是总分，每次的得分取决于消除的块的颜色。屏幕下方的蓝色和绿色块为1分，中间的黄色和土黄色块为4分，屏幕上方的橙色和红色块为7分。此外，当球在具有较高分数的块中反弹时，球加速并且反弹更快。中间的数字是生命值（剩余的尝试次数）。Breakout初始生命值为5。因此，如果失败5次，游戏将结束，变量done变为True。最右边的数字显示了球员/球队的数量，但在这里的环境中它是无关紧要的，可以忽略。