

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-10-06 20:22:13
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-10 00:00:16
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# Gym

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

[1]: https://blog.csdn.net/QFire/article/details/91490383
[2]: https://mp.weixin.qq.com/s?__biz=MzU1OTkwNzk4NQ==&mid=2247484108&idx=1&sn=0c9ff7488185c6287fbe56a3fa24a286&chksm=fc115732cb66de24dab450f458cc39effea9ffe4441010d5d3e00078badcdf132a54eb5388ba&token=366879770&lang=zh_CN#rd
