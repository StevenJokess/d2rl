

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-13 02:57:51
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-13 22:58:02
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# GAIL

Finally, we would like to include a bonus fifth project: Generative Adversarial Imitation Learning (code), in which Jonathan Ho and colleagues present a new approach for imitation learning. Jonathan Ho is joining us at OpenAI as a summer intern. He did most of this work at Stanford but we include it here as a related and highly creative application of GANs to RL. The standard reinforcement learning setting usually requires one to design a reward function that describes the desired behavior of the agent. However, in practice this can sometimes involve expensive trial-and-error process to get the details right. In contrast, in imitation learning the agent learns from example demonstrations (for example provided by teleoperation in robotics), eliminating the need to design a reward function.[1]

Popular imitation approaches involve a two-stage pipeline: first learning a reward function, then running RL on that reward. Such a pipeline can be slow, and because it’s indirect, it is hard to guarantee that the resulting policy works well. This work shows how one can directly extract policies from data via a connection to GANs. As a result, this approach can be used to learn policies from expert demonstrations (without rewards) on hard OpenAI Gym environments, such as Ant and Humanoid.

[1]:https://openai.com/research/generative-models
中文版：https://cloud.tencent.com/developer/article/1150413?areaSource=&traceId=
