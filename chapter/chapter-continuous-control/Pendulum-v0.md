

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-09 18:08:59
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-09 18:09:29
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# Pendulum-v0

本节使用 Gym 库中的倒立摆控制问题 (Pendulum-v0) 。该问题的对象是一根一端固定 的棍子，以固定点为原点，垂直向上为 $x x$ 轴方向，水平向右为 $y y$ 轴方向；观测值为棍 子活动端的坐标 $\left(X_t, Y_t\right)=\left(\cos \Theta_t, \sin \Theta_t\right), \Theta_t \in[-\pi, \pi)$ $\left(X_t, Y_t\right)=\left(\cos \Theta_t, \sin \Theta_t\right), \Theta_t \in[-\pi, \pi)$ 和角速度 $\dot{\Theta}_t \in[-8,+8] \dot{\Theta}_t \in[-8,+8]$ ；动作为连续值，是一个施加在活动端的力矩 $A_t \in[-2,+2] A_t \in[-2,+2]$ ；奖励值 也是一个和状态与动作有关的连续值 $R_{t+1} \in\left[-\pi^2-6.404,0\right]$
$R_{t+1} \in\left[-\pi^2-6.404,0\right]$ ； 任务是在给定的时间内 (200 步) 总收益越大越好，即相 当于尽可能的保持木棍静止直立。其他更详细的数据可参阅源代码。

该问题的状态空间、动作空间、奖励空间都是连续的空间，问题整体的空间变得相当大；而且最大的力矩也无法将倒立摆从单边直接推至直立状态，因此智能体需要学会利 用重力将倒立摆荡上至直立位置。
代码中类 OrnsteinUhlenbeckProcess 实现了 Ornstein Uhlenbeck 过程; 智能体类 DDPG 实现了深度确定性策略梯度智能体类，以及智能体类 TD3 实现了双重延迟深度确定性策 略梯度智能体类；前两者代码与书中基本一致，后者稍作修改，减少了一些代码量，但 实现逻辑不变，此处不再展示。

[1]: https://anesck.github.io/M-D-R_learning_notes/RLTPI/notes_html/9.chapter_nine.html
[2]: https://github.com/openai/gym/blob/master/gym/envs/classic_control/pendulum.py
