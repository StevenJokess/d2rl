

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-02 17:44:28
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-02 18:27:52
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 在运筹学中的应用

## 背景

假设有一个客服排班的任务，我们需要为 100 个人安排一个星期的排班问题，并且有以下约束条件：

- 一天被划分为 24 个时间段，即每个时间段为 1 个小时；
- 每个客服一个星期需要上七天班，每次上班八小时（万恶的资本主义）；
- 每个客服两次上班时间需要间隔 12 小时。

为了简化模型（虽然已经很简化了），我们再一个约束：

- 客服值班时，一个星期最早是 0，最晚 24*7 - 1。

- 评判标准：

现在有每个时间段所需客服人数，我们希望每个时段排班后的人数与实际人数尽量相近。

最优化问题可以使用启发式算法来做，现在想尝试一下强化学习。

## 代码实战

以 10 个客服为例，介绍下思路：

- 初始化客服状态：每个客服都排在每天的 0 点 [0, 24, 48, 72, 96, 120, 144]（注意上班八小时）；
- Agent 可以移动某个客服某天的排班，左移或者右移，当然也可以不动。所以共有 141 个动作【10 人 * 7 天 * 2 动作 + 1（不动）】；
- 将客服的排班结果视为一个状态；
- 利用加权求和方法计算匹配程度并作为奖励，取符号保证递增性。

```py
import random
import numpy as np
from copy import deepcopy
from collections import defaultdict

person_n = 10  # 先排 10 个人的班
```


[1]: https://cloud.tencent.com/developer/article/1736297
