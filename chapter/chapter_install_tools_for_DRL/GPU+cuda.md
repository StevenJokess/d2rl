

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-10-06 22:11:43
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-13 01:49:54
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# GPU与CUDA

深度学习是一门以实验结果而非理论为指导的工程科学。只有当有合适的硬件来尝试新的想法（或者像通常的情况那样升级旧的想法）时，算法的进步才成为可能。在计算机视觉或语音识别中使用的典型深度学习模型需要的计算能力比你的笔记本电脑所能提供的计算能力大几个数量级。在 21 世纪初至今，像 NVIDIA 和 AMD 这样的公司持续投资数十亿美元来开发快速、大规模并行芯片（图形处理单元，或说 GPU），为越来越逼真的视频游戏图形提供廉价、单用途的超级计算机，设计用于实时在屏幕上呈现复杂的 3D 场景。

2007 年，NVIDIA 推出了 CUDA（Compute Unified Device Architecture，简称“计算统一设备体系结构”），这是一款通用的 GPU 编程接口，这项投资使科学界受益匪浅。从物理建模开始，少量的 GPU 开始在各种高度并行化的应用中取代大量的 CPU 集群。深度神经网络，主要由许多矩阵乘法和加法组成，也具有高度的并行性。

2011 年前后，一些研究人员开始编写 CUDA 的神经网络实现方式，Dan Ciresan 和 Alex Krizhevsky 是其中的第一批研究人员。今天，当训练深层神经网络时，一个高端的 GPU 可以提供比一个典型的 CPU 能力高数百倍的并行计算能力。如果没有现代 GPU 强大计算能力，就不可能训练出许多最先进的深层神经网络。


## 安装显卡驱动和CUDA

### 先检查本地安装的显卡驱动支持的CUDA版本

桌面>>右键>>NAVIDIA控制面板 打开如下：


 

如果检查的CUDA版本过低，请到下方显卡驱动下载地址下载新驱动

[1]: https://www.bilibili.com/read/cv21030152/?from=search&spm_id_from=333.337.0.0
