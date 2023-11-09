# FrozenLake环境

## 简介

FrozenLake 是典型的具有离散状态空间的 Gym 环境，在此环境中，智能体需要在网格中从起始位置移动到目标位置，同时应当避开陷阱。网格的尺寸为四乘四 ( FrozenLake-v0 ) 或八乘八 ( FrozenLake8x8-v0 )，网格中的格子包含以下四种类型：

- S：起始位置
- G：目标位置，该位置可用于结束一个回合
- F：结冰的格子，这是智能体可以移动到的位置
- H：包含陷阱的格子，该位置可用于结束一个回合

FrozenLake 环境中可以执行四个动作：向左移动 ( 0 )，向下移动 ( 1 )，向右移动 ( 2 ) 和向上移动 ( 3 )。如果智能体成功到达目标位置，则奖励为 +1 ，否则为 0 。此外，状态空间以 16 维整数数组表示。

需要注意的是，在 FrozenLake 环境中，由于冰面很滑，因此智能体不会始终按照指定的方向移动。例如，当执行向下移动的动作时，智能体也可能向左或向右移动。

## 模拟FrozenLake环境

本节中，我们将模拟一个 4x4 的 FrozenLake 环境。 首先，导入 gym 库并创建 FrozenLake 环境的实例，并重置环境，智能体从状态 0 开始：

```py

import gym
import torch
env = gym.make('FrozenLake-v0')
n_state = env.observation_space.n
print(n_state)
# 16
n_action = env.action_space.n
print(n_action)
# 4
env.reset()
```

渲染环境，可以看到一个4 * 4矩阵，它表示智能体所在的冰冻湖面网格和当前所在格子(背景为红色，此时状态为 0 )：


```py
env.render()
```



因为智能体当前可以继续移动，指定智能体向下移动，渲染动作执行后的环境。可以看到动作执行后的冰冻湖面环境网格，其中智能体向右移动到状态 1 ，看到智能体并不一定以指定的动作 ( 1 ，即向下)执行：


```py
new_state, reward, is_done, info = env.step(1)
env.render()
```



打印出所有返回的信息，可以看到智能体会以 33.33% 的概率进入状态 4 ：


```py
print(new_state)
print(reward)
print(is_done)
print(info)
```

打印出的结果如下：



> 1
> 0.0
> False
> {'prob': 0.3333333333333333}


得到的奖励为 0 ，因为尚未达到目标位置并且回合尚未结束，同时可能会看到智能体因格子光滑而移动到状态 1 、状态 4 或停留在状态 0 。 如果满足以下两个条件之一，则回合将终止：

智能体移动到 H 格子(状态 5 、 7 、 11 或 12 )：产生的总奖励为 0
智能体移动到 G 格子(状态 15 )：产生的总奖励为 +1

为了验证在冰冻的湖面上移动的困难性，使用随机策略并运行 1000 回合计算平均总报酬。首先，定义函数 run_episode 用于根据给定策略模拟给定一个 FrozenLake 回合并返回总奖励：


```py
def run_episode(env, policy):
    state = env.reset()
    total_reward = 0
    is_done = False
    while not is_done:
        action = policy[state].item()
        state, reward, is_done, info = env.step(action)
        total_reward += reward
        if is_done:
            break
    return total_reward
```

随机生成一个策略，并将在每个回合中使用该策略，共运行 1000 回合，并计算平均奖励：

```py
n_episode = 1000
total_rewards = []
for episode in range(n_episode):
    random_policy = torch.randint(high=n_action, size=(n_state,))
    total_reward = run_episode(env, random_policy)
    total_rewards.append(total_reward)
print('Average total reward under random policy: {}'.format(sum(total_rewards) / n_episode))
```

我们使用随机生成的策略由 16 个动作组成，其对应于 FrozenLake 中的 16 个状态，指定了在某一状态时应执行的动作。需要强调的是，在 FrozenLake 中，运动方向仅部分取决于所选的动作，这增加了控制的不确定性。打印出的平均奖励如下所示，我们可以认为，如果采用随机动作，那幺智能体平均只有 1.1% 的概率可以到达目标：



> Average total reward under random policy: 0.011

[1]: https://flashgene.com/archives/239976.html
