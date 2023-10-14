

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-22 03:05:05
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-13 03:42:46
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# PyTorch

## Conda 安装说明

目前支持Python 3.7和Gym 0.25.2版本。

### 创建Conda环境（需先安装Anaconda）

```bash
conda create -n joyrl python=3.7
conda activate joyrl
pip install -r requirements.txt
```

### 安装Torch：

```bash
# CPU
conda install pytorch==1.10.0 torchvision==0.11.0 torchaudio==0.10.0 cpuonly -c pytorch
# GPU
conda install pytorch==1.10.0 torchvision==0.11.0 torchaudio==0.10.0 cudatoolkit=11.3 -c pytorch -c conda-forge
# GPU镜像安装
pip install torch==1.10.0+cu113 torchvision==0.11.0+cu113 torchaudio==0.10.0 --extra-index-url https://download.pytorch.org/whl/cu113
```

## Jupter notebook里用pip安装

安装PyTorch[3]

```py
!pip install torch
```

```py
import torch
print(torch.__version__)  # torch.__version__ 返回安装的 PyTorch 的版本号
```

## 模型上线

模型上线通常包括模型的保存、加载和实际环境中的部署。

### 模型保存和加载

PyTorch提供了非常方便的API来保存和加载模型。


```py
# 保存模型
torch.save(policy_net.state_dict(), 'policy_net_model.pth')
```


```py
# 加载模型
loaded_policy_net = PolicyNetwork(input_dim, output_dim)
loaded_policy_net.load_state_dict(torch.load('policy_net_model.pth'))
```

### 部署到实际环境

模型部署的具体步骤取决于应用场景。在某些在线系统中，可能需要将PyTorch模型转换为ONNX或TensorRT格式以提高推理速度。
示例：将PyTorch模型转为ONNX格式

```py
dummy_input = torch.randn(1, input_dim)
torch.onnx.export(policy_net, dummy_input, "policy_net_model.onnx")
```


[1]: https://github.com/datawhalechina/easy-rl/tree/master/notebooks
[2]: https://developer.aliyun.com/article/1333103?spm=a2c6h.14164896.0.0.59b247c5IMNEGo
[3]: https://nb.bohrium.dp.tech/detail/6516425879
