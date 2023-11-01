

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-11-02 01:25:52
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-11-02 01:35:30
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# Lunar Lander

## Lunar Lander简介：

Lunar Lander 的环境更具挑战性。 特工必须将月球着陆器降落在着陆垫上。 月亮的表面会发生变化，着陆器的方位也会在每个剧集发生变化。 该智能体将获得一个八维数组，用于描述每个步骤中的世界状态，并且可以在该步骤中执行四个操作之一。 智能体可以选择不执行任何操作，启动其主引擎，启动其左向引擎或启动其右向引擎。

## Keras-RL的帮助

感谢 Keras-RL，我们用于 Lunar Lander 的智能体几乎与 CartPole 相同，除了实际的模型架构和一些超参数更改外。 Lunar Lander 的环境有八个输入而不是四个输入，我们的智能体现在可以选择四个操作而不是两个。

如果您受到这些示例的启发，并决定尝试构建 Keras-RL 网络，请记住，超参数选择非常非常重要。 对于 Lunar Lander 智能体，对模型架构的最小更改导致我的智能体无法学习环境解决方案。 使网络正确运行是一项艰巨的工作。

## Lunar Lander 网络架构

我的 Lunar Lander 智能体程序的架构仅比 CartPole 的架构稍微复杂一点，对于相同的三个隐藏层仅引入了几个神经元。 我们将使用以下代码来定义模型：

def build_model(state_size, num_actions):
    input = Input(shape=(1, state_size))
    x = Flatten()(input)
    x = Dense(64, activation='relu')(x)
    x = Dense(32, activation='relu')(x)
    x = Dense(16, activation='relu')(x)
    output = Dense(num_actions, activation='linear')(x)
    model = Model(inputs=input, outputs=output)
    print(model.summary())
    return model
在此问题的情况下，较小的架构会导致智能体学习控制和悬停着陆器，但实际上并未着陆。 当然，由于我们要对每个剧集的每个步骤进行小批量更新，因此我们需要仔细权衡复杂性与运行时和计算需求之间的关系。

记忆和策略
CartPole 的内存和策略可以重复使用。 我相信，通过进一步调整线性退火策略中的步骤，可能会提高智能体训练的速度，因为该智能体需要采取更多的步骤来进行训练。 但是，为 CartPole 选择的值似乎可以很好地工作，因此这是留给读者的练习。

智能体
从以下代码中可以看出，Lunar Lander DQNAgent再次相同，只是学习率小得多。

dqn = DQNAgent(model=model, nb_actions=num_actions, memory=memory, nb_steps_warmup=10, target_model_update=1e-2, policy=policy)
dqn.compile(Adam(lr=0.00025), metrics=['mae'])
训练
在训练该特工时，您会注意到它学会做的第一件事是将着陆器悬停，并避免着陆。 当着陆器最终着陆时，它会收到非常高的奖励，成功着陆时为 +100，坠毁时为 -100。 这种 -100 的奖励是如此之强，以至于智能体一开始宁愿因悬停而受到小额罚款。 我们的探员要花很多时间才能得出这样的提示：良好的着陆总比没有良好着陆好，因为坠机着陆非常糟糕。

可以塑造奖励信号来帮助座席更快地学习，但这超出了本书的范围。 有关更多信息，请查看奖励塑造。

由于这种对坠机着陆的极端负面反馈，网络需要花费相当长的一段时间才能学会着陆。 在这里，我们正在运行五十万个训练步骤，以传达我们的信息。 我们将使用以下代码来训练智能体：

callbacks = build_callbacks(ENV_NAME)

dqn.fit(env, nb_steps=1000000,
        visualize=False,
        verbose=2,
        callbacks=callbacks)
您可以通过调整参数gamma（默认值为 0.99）来进一步改进此示例。 如果您从Q函数中调用，此参数会减少或增加Q函数中将来奖励的影响。

结果
我在 Git 一章中包含了 Lunar Lander 的权重，并创建了一个脚本，该脚本在启用可视化的情况下运行这些权重dqn_lunar_lander_test.py。 它加载经过训练的模型权重并运行 10 集。 在大多数情况下，特工能够以惊人的技能和准确率将月球着陆器降落在其着陆板上，如以下屏幕截图所示：

希望这个例子可以说明，尽管深层 Q 网络并不是火箭科学，但仍可用于控制火箭。

[1]:
