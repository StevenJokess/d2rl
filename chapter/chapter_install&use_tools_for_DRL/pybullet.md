# pybullet

Bullet (网站: https://pybullet.org) 是一个物理动力开源库，它实现了三维物体 的运动、碰撞检测、渲染绘制等功能。它的Python API称为PyBullet。有许多游戏 和电影是利用Bullet开发的。很多强化学习的研究也使用了Bullet和PyBullet。
本节将介绍PyBullet的使用方法，并且利用PyBullet提供的环境训练智能体。

PyBullet作为一个Python扩展库，是需要额外安装的。可以使用pip来安装 PyBullet:

```py
pip install-- upgrade pybullet
```

安装时会自动安装依赖库。
安装完PyBullet后，可以用下列语句导入它:

```py
import gym
import pybullet_envs
```

[1]: https://weread.qq.com/web/reader/85532b40813ab82d4g017246k2b4324802732b44928aee17
