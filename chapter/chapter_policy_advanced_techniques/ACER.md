# ACER

ACER（Sample Efficient Actor-Critic with Experience Replay[2]）方法要比PPO方法复杂得多，需要额外添加代码来修正off-policy和重构缓冲器，但它在Atari基准上仅仅比PPO好一点点[1]

TODO:
第十章 Off-policy Policy gradient - 臭皮匠的文章 - 知乎
https://zhuanlan.zhihu.com/p/363657687

Retrace估计的是Q函数

[1]: https://daiwk.github.io/posts/rl-distributed-rl.html#2-a3c
[2]: https://arxiv.org/abs/1611.01224
