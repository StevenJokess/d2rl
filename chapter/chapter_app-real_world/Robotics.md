

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-06-01 00:16:33
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-06-01 00:17:20
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# ROBOTICS

Robotics is a classical area for reinforcement learning. See Kober et al. (2013) for a survey of RL in
robotics, Deisenroth et al. (2013) for a survey on policy search for robotics, and Argall et al. (2009)
for a survey of robot learning from demonstration. See the journal Science Robotics. It is interesting
to note that from NIPS 2016 invited talk, Boston Dynamics robots did not use machine learning.
In the following, we discuss guided policy search (Levine et al., 2016a) and learn to navigate (Mirowski et al., 2017). See more recent robotics papers, e.g., Chebotar et al. (2016; 2017);
Duan et al. (2017); Finn and Levine (2016); Gu et al. (2016a); Lee et al. (2017); Levine et al.
(2016b); Mahler et al. (2017); Perez-D’Arpino and Shah ´ (2017); Popov et al. (2017); Yahya et al.
(2016); Zhu et al. (2017b).

We recommend Pieter Abbeel’s NIPS 2017 Keynote Speech, Deep Learning for Robotics, slides at,
https://www.dropbox.com/s/fdw7q8mx3x4wr0c/

## GUIDED POLICY SEARCH

Levine et al. (2016a) proposed to train the perception and control systems jointly end-to-end, to map
raw image observations directly to torques at the robot’s motors. The authors introduced guided
policy search (GPS) to train policies represented as CNN, by transforming policy search into supervised learning to achieve data efficiency, with training data provided by a trajectory-centric RL
method operating under unknown dynamics. GPS alternates between trajectory-centric RL and supervised learning, to obtain the training data coming from the policy’s own state distribution, to
address the issue that supervised learning usually does not achieve good, long-horizon performance.
GPS utilizes pre-training to reduce the amount of experience data to train visuomotor policies. Good
performance was achieved on a range of real-world manipulation tasks requiring localization, visual
tracking, and handling complex contact dynamics, and simulated comparisons with previous policy
search methods. As the authors mentioned, ”this is the first method that can train deep visuomotor
policies for complex, high-dimensional manipulation skills with direct torque control”.

## LEARN TO NAVIGATE

Mirowski et al. (2017) obtained the navigation ability by solving a RL problem maximizing cumulative reward and jointly considering un/self-supervised tasks to improve data efficiency and task
performance. The authors addressed the sparse reward issues by augmenting the loss with two
auxiliary tasks, 1) unsupervised reconstruction of a low-dimensional depth map for representation
learning to aid obstacle avoidance and short-term trajectory planning; 2) self-supervised loop closure classification task within a local trajectory. The authors incorporated a stacked LSTM to use
memory at different time scales for dynamic elements in the environments. The proposed agent
learn to navigate in complex 3D mazes end-to-end from raw sensory input, and performed similarly
to human level, even when start/goal locations change frequently.
In this approach, navigation is a by-product of the goal-directed RL optimization problem, in contrast to conventional approaches such as Simultaneous Localisation and Mapping (SLAM), where
explicit position inference and mapping are used for navigation. This may have the chance to replace
the popular SLAM, which usually requires manual processing.

[1]: https://arxiv.org/abs/1701.07274
