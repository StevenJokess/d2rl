

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-23 23:05:23
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-26 23:21:22
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 智能驾驶(Intelligent driving)

## 强化学习

自动驾驶问题通过控制方向盘、油门、刹车等设备完成各种运输目标（见图1-4）。自动驾驶问题既可以在虚拟环境中仿真（比如在电脑里仿真），也可能在现实世界中出现。有些任务往往有着明确的目标（比如从一个指定地点到达另外一个指定地点），但是每一个具体的动作却没有正确答案作为参考。这正是强化学习所针对的任务。基于强化学习的控制策略可以帮助开发自动驾驶的算法。[2]

自动驾驶（本图截取自仿真平台AirSimNH）

## 深度强化学习

传感器技术的进步使得智能驾驶得到了快速发展。目前, 国内外众多研究团队已经将智能驾驶作为重要研究方向并取得了初步的研究成果. 然而类似激光雷达这类传感器的使用大大增加了智能车成本, 极大地阻碍了智能车的普及. 近年来, 基于摄像头的先进驾驶员辅助系统(advanced driver assistance systems, ADAS)逐渐成为智能驾驶的关键技术之一. 该技术基于摄像头获取图像信息, 通过深度学习实时提取路况特征, 设计相应的智能控制算法, 实现稳定可靠的智能驾驶. 深度强化学习作为一类自学习智能控制算法, 可用来解决车辆的复杂非线性系统控制问题. 根据现有深度强化学习在TORCS赛车平台的研究可以预测, 深度强化学习将在智能驾驶领域发挥巨大的作用, 成为降低智能车成本的一个可行方案。[1]

[1]: http://pg.jrj.com.cn/acc/Res/CN_RES/INDUS/2023/2/9/27c20431-8ed3-4562-83b5-5c82706f28a5.pdf
[2]: https://developer.aliyun.com/article/718967
