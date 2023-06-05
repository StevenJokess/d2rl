

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-05-23 22:23:20
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-05-23 22:23:31
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 框架



Google于2018年开源的深度强化学习框架——多巴胺（Dopamine），旨在为入门或资深的深度强化学习研究人员提供具备灵活性、稳定性和可重复性的研究平台。[2]

谷歌TensorFlow Agents ---TensorFlow的加强版,它提供许多工具，通过强化学习可以实现各类智能应用程序的构建与训练。这个框架能够将OpenAI Gym接口扩展至多个并行环境，并允许各代理立足TensorFlow之内实现以执行批量计算。其面向OpoenAI Gy环境的批量化接口可与TensorFlow实现全面集成，从而高效执行各类算法。该框架还结合有BatchPPO，一套经过优化的近端策略优化算法实现方案。其核心组件包括一个环境打包器，用于在外部过程中构建OpenAI Gym环境; 一套批量集成，用于实现TensorFlow图步并以强化学习运算的方式重置函数; 外加用于将TensorFlow图形批处理流程与强化学习算法纳入训练特内单一却步的组件。

Roboschool：Roboschool 提供开源软件以通过强化学习构建并训练机器人模拟。其有助于在同一环境当中对多个代理进行强化学习训练。通过多方训练机制，您可以训练同一代理分别作为两方玩家（因此能够自我对抗）、使用相同算法训练两套代理，或者设置两种算法进行彼此对抗。Roboschool由OpenAI开发完成，这一非营利性组织的背后赞助者包括Elon Musk、Sam Altman、Reid Hoffman以及Peter Thiel。其与OpenAI Gym相集成，后者是一套用于开发及评估强化学习算法的开源工具集。OpenAI Gym与TensorFlow、Theano以及其它多种深度学习库相兼容。OpenAI Gym当中包含用于数值计算、游戏以及物理引擎的相关代码。Roboschool基于Bullet物理引擎，这是一套开源许可物理库，并被其它多种仿真软件——例如Gazebo与Virtual Robot Experimentation Platform（简称V-REP）所广泛使用。其中包含多种强化学习算法，具体以怨报德 异步深度强化学习方法、Actor-Critic with Experience Replay、Actor- Critic using Kronecker-Factored Trust Region、深度确定性策略梯度、近端策略优化以及信任域策略优化等等。

Coach：英特尔公司的开源强化学习框架，可以对游戏、机器人以及其它基于代理的智能应用进行智能代理的建模、训练与评估。Coach 提供一套模块化沙箱、可复用组件以及用于组合新强化学习算法并在多种应用领域内训练新智能应用的Python API。该框架利用OpenAI Gym作为主工具，负责与不同强化学习环境进行交换。其还支持其它外部扩展，具体包括Roboschool、gym-extensions、PyBullet以及ViZDoom。Coach的环境打包器允许用户向其中添加自定义强化学习环境，从而解决其它学习问题。该框架能够在桌面计算机上高效训练强化学习代理，并利用多核CPU处理相关任务。其能够为一部分强化学习算法提供单线程与多线程实现能力，包括异步优势Actor-Critic、深度确定性策略梯度、近端策略优化、直接未来预测以及规范化优势函数。所有算法皆利用面向英特尔系统作出优化的TensorFLow完成，其中部分算法亦适用于英特尔的Neon深度学习框架。Coach 当中包含多种强化学习代理实现方案，具体包括从单线程实现到多线程实现的转换。其能够开发出支持单与多工作程序（同步或异步）强化学习实现方法的新代理。此外，其还支持连续与离散操作空间，以及视觉观察空间或仅包含原始测量指标的观察空间。

## 环境模拟器

强化学习不同于监督学习，它不依赖预先收集好的数据集。强化学习算法通过与环境进行交互，通过试错和反馈（奖励）来学习如何行动以最大化奖励。这种交互过程中生成的数据（状态，动作，奖励）通常是在训练过程中动态生成的，并且是依赖于当前的策略的，因此强化学习不同于监督学习或者无监督学习，它不通常使用静态的、预先定义的数据集。
不过，尽管强化学习不使用传统意义上的数据集，但是我们还是可以使用一些环境（Environment）或者模拟器来进行训练和测试。这些环境提供了一种方式来模拟 agent 和环境的交互，并给出对 agent 行为的反馈。以下是一些常见的强化学习环境和模拟器：

- OpenAI Gym: 这是一个常用的强化学习环境集合，提供了一系列的标准化环境，包括模拟机器人控制、Atari 游戏、棋类游戏等等。
- DeepMind Lab: 这是 DeepMind 提供的一个 3D 学习环境，提供了一系列富有挑战性的 3D 导航和解谜任务。
- MuJoCo: 一个用于模拟复杂物理环境的工具，尤其是涉及到机器人的任务。
- Microsoft Project Malmo: 这是基于 Minecraft 的强化学习环境，允许在复杂、丰富的 3D 世界中进行研究。
- Unity ML-Agents: 这是 Unity 提供的一个强化学习环境，提供了丰富的游戏模拟环境。
- StarCraft II Learning Environment: 这是一个允许研究者使用星际争霸 II 游戏来研究强化学习的环境。
- CARLA: 一个开源的自动驾驶汽车模拟器，用于开发和测试自动驾驶系统。[3]

---

MARL：
https://blog.csdn.net/sinat_39620217/article/details/117589067

[1]: https://github.com/scutan90/DeepLearning-500-questions/blob/master/ch10_%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0/%E7%AC%AC%E5%8D%81%E7%AB%A0_%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0.md
[2]: https://www.epubit.com/articleDetails?id=NN8d3150d1-097c-4dc9-8715-adae9f3fd09a
[3]: https://www.zhihu.com/question/277325426/answer/3057391213
