# 物理机器人

对强化学习的进一步研究可能遵循有监督深度学习的路径，并为我们提供更加稳健的算法和如何做出这些选择的系统指导。但有一件事令我担心。在监督学习中，基准数据集使全球研究人员能够针对同一数据集来调整算法，并在彼此的工作基础上进行构建。在强化学习中，更常用的基准是模拟环境，如 OpenAI Gym。但让强化学习算法在模拟机器人上运行要比让它在物理机器人上运行容易得多。

许多在模拟任务中表现出色的算法与物理机器人进行斗争。即使是同一机器人设计的两个副本也会有所不同。此外，为每一位有抱负的强化学习研究人员提供他们自己的机器人副本是不可行的。虽然研究人员在模拟机器人（以及玩视频游戏）的强化学习方面取得了快速进展，但应用于非模拟环境中应用中的桥梁往往缺失。许多优秀的研究实验室正在研究物理机器人。但由于每个机器人都是独一无二的，一个实验室的结果可能很难被其他实验室复制，这阻碍了发展的速度。

目前我还没有找到解决这些棘手问题的办法。但我希望所有人工智能领域的人们都能共同努力，使这些算法更加稳健且广泛有效。

加州大学伯克利分校的Levine等结合卷积神经网络和强化学习, 研制出了能够以纯视觉输入来抓取物体的机器人[119]. 最近, 该团队与谷歌合作, 通过大规模的数据训练, 使机器人能够从一堆物体中识别并抓取某件特定物体[120]. 在美国新成立了一家深度强化学习公司Osaro7, 该公司致力于使用深度强化学习技术, 融合多传感器信息来提供包括工业机器人、无人机、自动驾驶汽车和物联网等应用领域的解决方案.

[1]: https://zhuanlan.zhihu.com/p/550103973

[199]LEVINE S, WAGENER N, ABBEEL P. Learning contact-rich ma-
nipulation skills with guided policy search [C] //Proceedings of the
IEEE International Conference on Robotics and Automation. Seat-
tle: IEEE, 2015: 156 – 163.
[120] LEVINE S, PASTOR P, KRIZHEVSKY A, et al. Learning hand-eye
coordination for robotic grasping with deep learning and large-scale
data collection [EB/OL] //arXiv preprint. 2016. arXiv:1603.02199
