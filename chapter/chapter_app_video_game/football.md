

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-10-03 03:50:34
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-17 01:06:26
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# Football

## Google Research Football 环境：


Link：https://github.com/google-research/football

这个环境是google基于之前某个足球小游戏的环境进行改动和封装出来的，主要可以分为11v11 single-agent场景（控制一个active player在11名球员中切换）和5v5 multi-agent场景（控制4名球员+1个守门员）。该环境支持self-play，有三种难度内置AI可以打，你可以人肉去体验下，玩起来和实况，FIFA，绿茵之巅感觉都差不多。游戏状态基于vector的主要是球员的坐标/速度/角色/朝向/红黄牌等，也可以用图像输入，但需要打开render，估计会略慢，动作输出有二十多维，包括不同方向/长短传/加速等。此外环境还提供了所谓“football academy”，你可以自己进行游戏场景和球员坐标的初始化，相当于可以进行课程学习配置。Render如下：


2020 年 12 月，腾讯 AI Lab 绝悟团队借助「开悟」平台开发的足球 AI 「绝悟-WeKick 版本」在 Google Research 与英超曼城俱乐部联合举办的足球 AI Kaggle 竞赛上获得冠军。

该竞赛使用 Google Brain 基于开源足球游戏 Gameplay Football 开发的强化学习环境 Google Research Football。这场 Kaggle 竞赛也是首场相关竞赛。不同于《王者荣耀》，足球 AI 比赛涉及到 11 个智能体的相互配合以及与另外 11 个智能体的对抗，同时奖励相比于 MOBA 游戏还更稀疏。

WeKick 踢足球

即便如此，WeKick 依然以显著优于第二名的成绩获得了冠军。这体现了完全体「绝悟」底层技术和框架的通用性。

虽然都是 RTS （即时战略）游戏，星际争霸中需要控制多种不同类型不同数量的单位，这些单位又有各自的运动和攻击特点，因而动作空间更大、策略空间更丰富。

[1]: https://cloud.tencent.com/developer/article/2197037
[2]: https://www.sohu.com/a/442547985_473283
