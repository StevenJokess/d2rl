

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-06-01 00:19:11
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-06-01 00:19:18
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# VIDEO GAMES

Video games would be great testbeds for artificial general intelligence. Wu and Tian (2017) deployed A3C with CNN to train an agent in a partially observable 3D environment, Doom, from recent four raw frames and game variables, to predict next action and value function, following the curriculum learning (Bengio et al., 2009) approach of starting with simple tasks and gradually transition to harder ones. It is nontrivial to apply A3C to such 3D games directly, partly due to sparse and long term reward. The authors won the champion in Track 1 of ViZDoom Competition by a large margin, and plan the following future work: a map from an unknown environment, localization, a global plan to act, and visualization of the reasoning process.

Dosovitskiy and Koltun (2017) approached the problem of sensorimotor control in immersive environments with supervised learning, and won the Full Deathmatch track of the Visual Doom AI Competition. We list it here since it is usually a RL problem, yet it was solved with supervised
learning. Lample and Chaplot (2017) also discussed how to tackle Doom.

Peng et al. (2017b) proposed a multiagent actor-critic framework, with a bidirectionally-coordinated
network to form coordination among multiple agents in a team, deploying the concept of dynamic
grouping and parameter sharing for better scalability. The authors used StarCraft as the testbed.
Without human demonstration or labelled data as supervision, the proposed approach learned strategies for coordination similar to the level of experienced human players, like move without collision,
hit and run, cover attack, and focus fire without overkill. Usunier et al. (2017); Justesen and Risi
(2017) also studied StarCraft.

Oh et al. (2016) and Tessler et al. (2017) studied Minecraft, Chen and Yi (2017); Firoiu et al. (2017)
studied Super Smash Bros, and Kansky et al. (2017) proposed Schema Networks and empirically
studied variants of Breakout in Atari games.

See Justesen et al. (2017) for a survey about applying deep (reinforcement) learning to video games.
See Ontan˜on et al. ´ (2013) for a survey about Starcraft. Check AIIDE and CIG Starcraft AI Competitions, and its history at https://www.cs.mun.ca/˜dchurchill/starcraftaicomp/history.shtml. See Lin
et al. (2017) for StarCraft Dataset.

DRL首先应用于视频游戏领域[23]，主要的原因是 DRL 需要大量的采样和试错训练，而游戏环境能够提供充足的样本，并且避免了试错的成本。从目前的文献来看，研究 DRL 所采用的游戏环境可以分为两类：一类用来提升算法的通用性，如Atari 2600；另一类用来处理复杂的游戏场景，如ViZDoom、StarCraftII等。[2]

[1]: https://arxiv.org/abs/1701.07274
[2]: http://www.infocomm-journal.com/znkx/article/2020/2096-6652/2096-6652-2-4-00314.shtml
