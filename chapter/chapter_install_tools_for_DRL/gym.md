

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-10-06 20:22:13
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-13 04:27:42
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# Gym(nasium)

## 简介

Gym是一个开发和比较强化学习算法的工具包。它对代理的结构没有任何假设，并且兼容于任何数值计算库(如TensorFlow或Theano)。

Gym库中包含许多可以用于制定强化学习算法的测试问题（即环境），这些环境有共享接口，允许编写通用的算法。

## 背景：为什么有Gym？

强化学习(RL)是机器学习中涉及决策和电机控制的子领域。它研究代理如何在复杂、不确定的环境中学习如何实现目标。令人兴奋的原因有两个：

RL非常普遍，包括所有涉及到做出一系列决策的问题：例如，控制机器人的马达使其能够跑和跳；做出商业决策，如定价和库存管理；或玩视频游戏和棋盘游戏。RL甚至可以应用于具有顺序或结构化输出的监督学习问题。

RL算法已经开始在许多困难的环境中取得良好的效果。RL有着悠久的历史，但直到最近在深度学习方面取得的进展之前，它还需要许多针对特定问题的工程。DeepMind的Atari results、Pieter Abbeel小组的BRETT和AlphaGo都使用了深度RL算法，这些算法没有对环境做太多假设，因此可以应用于其他设置。

然而，RL的研究也受到两个因素的影响：

1. 需要更好的benchmas。在监督学习中，像ImageNet这样的大型标记数据集驱动了其进步。在RL中，类似的就是大量多样的环境集合。然而，现有的RL环境的开源集合没有足够的多样性，而且它们通常很难设置和使用。
2. 缺乏环境的标准化。在问题定义上的细微差别，如奖励函数或动作集合，可以极大地改变任务的难度。这个问题使得复制已发表的研究和比较不同论文的结果变得困难。

Gym正试图解决这两个问题。[2]

## Gym库包括的可用环境

Gym配有多种从难到易的环境，也包含多种不同类型的数据，full list of environments中可以查看概览。

1. Classic control和toy text：完整的小规模任务，大多来自于强化学习文献，适合于入门。
2. Algorithmic：执行计算例如多位数加法和反转序列。一般认为这些任务对于计算机来说很容易，但是挑战在于纯粹从例子中去学习这些算法。这些任务有一个很好的特性，即通过改变序列长度很容易改变难度。
3. Atari：玩经典的Atari游戏。我们以一种易于安装的形式集成了Arcade学习环境(这对强化学习研究产生了很大的影响)。
4. 2D and 3D robots：控制仿真机器人。这些任务使用MuJoCo物理引擎，用于快速准确的仿真。包含了一些来自由UC Berkeley研究人员提供的benchmark环境。MuJoCo是一款私有软件，但也提供了免费试用许可证。

Gym库具有Box2d、Atari等子库，可通过完整安装获得

## 安装

$ git clone https://github.com/openai/gym.git

$ cd gym

$ pip install -e '.[all]'

或

## 安装atari环境

$ pip install -e '.[atari]'

## 安装棋盘类游戏

$ pip install -e '.[board_game]'

## 安装Box2d控制类游戏

$ pip install -e '.[box2d]'

先到 http://www.swig.org/download.html 中下载 swigwin-3.0.12

下载完后，解压缩 d:/swigwin-3.0.12，然后打开 系统环境变量设置

打开：“我的电脑->属性->高级->环境变量->系统变量”

把 d:/swigwin-3.0.12 添加到 path 变量中

重启后执行
$ pip install box2d-py

## 安装经典控制类游戏

$ pip install -e '.[classic_control]'

## 注册

gym的主要目的是提供大量的环境集合，这些环境暴露了一个公共接口，并进行了版本控制以便进行比较。要列出已安装可用的环境，只需询问`gym.env .registry`：

```py
from gym import envs
print(envs.registry.all())
```

```py
#> [EnvSpec(DoubleDunk-v0), EnvSpec(InvertedDoublePendulum-v0),EnvSpec(BeamRider-v0), EnvSpec(Phoenix-ram-v0), EnvSpec(Asterix-v0),EnvSpec(TimePilot-v0), EnvSpec(Alien-v0), EnvSpec(Robotank-ram-v0),EnvSpec(CartPole-v0), EnvSpec(Berzerk-v0), EnvSpec(Berzerk-ram-v0),EnvSpec(Gopher-ram-v0), ...
```

这将给出EnvSpec对象的列表。这些对象定义了特定任务的参数，包括要运行的试验数量和最大步数。例如，EnvSpec(Hopper-v1)定义了一个环境，其中的目标是让一个2D模拟机器人跳跃：EnvSpec(Go9x9-v0)在9x9棋盘上定义了围棋游戏。
这些环境id被视为不透明的字符串。为了确保将来进行有效比较，环境永远不会以影响性能的方式更改，只会被更新的版本替换。我们现在给每个环境加上一个v0后缀，以便将来的替换可以自然地称为v1、v2等。
将自己的环境添加到注册表中非常容易，从而使它们对gym.make()可用。make():只需在加载时注册register()它们。[2]


## 环境介绍

在Gym包中内置了众多模拟环境, 包括经典的控制模型(如倒立摆、爬山小车), Atrai 2600像素游戏, Mujoco仿真环境和一些简单的机械臂模型. 实验采用了Gym包中的CartPole-v1、Acrobot-v1、LunarLander-v2和Qbert-ram-v0共4个环境.

1) CartPole-v1: CartPole-v1 (CP)是一个经典的倒立摆模型, 如图2(a)所示, 一根杆连接在小车上, 而小车在光滑的水平面上. 系统通过对小车施加正向或负向的力来进行控制, 杆每保持一个时间单位的直立就获得一分, 而当杆偏离竖直的角度或是小车距起始点的距离超过了一定范围, 当次实验就结束. 最大的时间单位为500
, 即该模型的最大得分也为500
.


图 2  实验环境
Fig. 2  Experimental environments
下载: 全尺寸图片 幻灯片
2) Acrobot-v1: Acrobot-v1 (AB)如图2(b)所示, 该系统包括两个关节和两个连杆, 其中两个连杆之间的关节被驱动. 最初, 连杆是向下悬挂的, 目标是将较低连杆的末端向上摆动到给定的高度.

3) LunarLander-v2: LunarLander-v2 (LL)是一个登月模型, 如图2(c)所示, 登月器从屏幕顶部开始移动, 最终以零速度到着陆台的奖励大约是在100
到140
分之间, 坠毁或成功着陆都将停止学习并分别获得额外的−100
或+100
分, 每条腿着陆+10
分, 每次发动引擎−0.3
分, 一般将实验成功的标准定为200
分.

4) Qbert-ram-v0: Qbert-ram-v0 (Qbert)是一个雅达利2600像素游戏, 如图2(d)所示, 玩家控制主角在一个由正方体构成的三角立面上来回跳跃, 每一次地面接触都会改变方块表层的颜色, 只要将所有色块踩成规定的颜色即告胜利. 状态观测选用雅达利的随机存储器(Random access memory, RAM)状态.

## 升级

强化学习环境升级 – 从gym到Gymnasium
作为强化学习最常用的工具，gym一直在不停地升级和折腾，比如gym[atari]变成需要要安装接受协议的包啦，atari环境不支持Windows环境啦之类的，另外比较大的变化就是2021年接口从gym库变成了gymnasium库。让大量的讲强化学习的书中介绍环境的部分变得需要跟进升级了。

不过，不管如何变，gym[nasium]作为强化学习的代理库的总的设计思想没有变化，变的都是接口的细节。

step和观察结果
总体来说，对于gymnasium我们只需要做两件事情：一个是初始化环境，另一个就是通过step函数不停地给环境做输入，然后观察对应的结果。

初始化环境分为两步。
第一步是创建gymnasium工厂中所支持的子环境，比如我们使用经典的让一个杆子不倒的CartPole环境：[5]

import gymnasium as gym
env = gym.make("CartPole-v1")


## 常见问题

### 不显示画面

在早期版本gym中，调用env.render()会直接显示当前画面，但是现在的新版本中这一方法无效。现在有一下几种方法显示当前环境和训练中的画面：

1. render_model = “human”

env = gym.make("CartPole-v1", render_mode = "human")
Python
显示效果：





问题：

该设置下，程序会输出所有运行画面。但是这一步会带来一个问题，因为画面渲染需要时间，导致训练变的非常慢。强化学习的前期是一个一直试错的部分，显然我们并不是每次都想花费时间去观察模型试错，并且多数时候我们想要观察我们想观察的训练阶段。对此我们可以使用下一个方法；

2. render_model = “rgb_array”

env = gym.make("CartPole-v1", render_mode = "rgb_array")
Python
该方法会让env.render()返回一个 rgb_array， 这一rgb_array 表示当前step下的环境画面，当我们需要显示的时候可以使用cv2来进行渲染。方法如下：

# RGB 转化为BGR， cv2显示格式为BGR
img = cv2.cvtColor(env.render(), cv2.COLOR_RGB2BGR)

# 显示画面，test为窗口名称
cv2.imshow("test",img)

# 给cv2一定时间完成渲染，否则无法显示
cv2.waitKey(1)


[1]: https://blog.csdn.net/QFire/article/details/91490383
[2]: https://mp.weixin.qq.com/s?__biz=MzU1OTkwNzk4NQ==&mid=2247484108&idx=1&sn=0c9ff7488185c6287fbe56a3fa24a286&chksm=fc115732cb66de24dab450f458cc39effea9ffe4441010d5d3e00078badcdf132a54eb5388ba&token=366879770&lang=zh_CN#rd
[3]: http://www.aas.net.cn/cn/article/doi/10.16383/j.aas.c220103?viewType=HTML
[4]: https://aitechtogether.com/python/137108.html
[5]: https://aitechtogether.com/python/114618.html
