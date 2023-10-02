# Football

## Google Research Football 环境：


Link：https://github.com/google-research/football

这个环境是google基于之前某个足球小游戏的环境进行改动和封装出来的，主要可以分为11v11 single-agent场景（控制一个active player在11名球员中切换）和5v5 multi-agent场景（控制4名球员+1个守门员）。该环境支持self-play，有三种难度内置AI可以打，你可以人肉去体验下，玩起来和实况，FIFA，绿茵之巅感觉都差不多。游戏状态基于vector的主要是球员的坐标/速度/角色/朝向/红黄牌等，也可以用图像输入，但需要打开render，估计会略慢，动作输出有二十多维，包括不同方向/长短传/加速等。此外环境还提供了所谓“football academy”，你可以自己进行游戏场景和球员坐标的初始化，相当于可以进行课程学习配置。Render如下：

[1]: https://cloud.tencent.com/developer/article/2197037
