

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-05-31 23:24:48
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2024-07-22 01:05:34
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 马里奥

超级马里奥兄弟是情节性任务（episodic tasks），一个剧情开始于新马里奥人物的出生点出来并到结束：当马里奥被杀或者达到了关卡的末尾。

## 建模

- 智能体是玩家操控的马里奥
- 环境就是我们玩的“超级玛丽”的等级。画面中的敌人和方块构成了这个世界。时间在流逝，分数在上升（至少我们希望如此！）。

- 动作：上下左右、AB；构成跳跃、闪避和前进等动作。
- 奖励：分数 + 保持活着；智能体的目标是与环境进行交互，从而获得奖励。


代码：Pytorch笔记（9）——强化学习应用之超级马里奥详解 - XuanAxuan的文章 - 知乎
https://zhuanlan.zhihu.com/p/402519441

[1]: https://zhuanlan.zhihu.com/p/54439020
[2]: https://zhuanlan.zhihu.com/p/53939137
