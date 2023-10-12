# Anaconda和pip

## Anaconda的简介

Anaconda具有三大优势

- 优势一：下载速度快，解决不同地区网络在下载时速度相差几十倍的窘境。
- 优势二：环境配置一次，多次使用；避免多次配置环境，以及空间浪费。
- 优势三：开源完全免费，和边车辅助一样。

Anaconda可以叫它环境配置软件，每一个程序在运行时都需要一些其他的支持性程序，以及依赖库，有很多是程序依赖于同一个环境配置的，但是stable-diffusion-webui的作者们为了方便易于使用，将环境安装路径集成进了他的文件夹，这就造成在很多方面出现bug。

例如stable-diffusion-webui依赖于Pytorch等程序，其他深度学习框架也依赖于Pytorch，当Pytorch集成进s-d-w（stable-diffusion-webui简称）文件夹中，外部程序再需要Pytorch时只能再次重新下载安装Pytorch，所以使用Anaconda的好处显而易见，可以一次配置，供多种程序使用。

## 准备工作

2.1下载安装Anaconda3

官网下载地址：https://www.anaconda.com/products/distribution

进入Anaconda官网
2.1.1点击Download都会吧。双击打开都会吧。

Anaconda安装
2.1.2 选择安装选项

一直下一步
选择安装给所有用户或者just for me都可以

2.1.3更改安装路径

可以修改路径
XXXXXXXXXX替换为你喜欢的路径。一定不要有空格和奇怪的字符


然后一路默认直到完成，如果没有报错，安装成功。

2.1.4验证安装Anaconda

在安装完成Anaconda之后，Anaconda并不会在桌面创建快捷方式，所以需要我们去电脑的开始栏进行寻找Anaconda Prompt (如果会使用并且系统是专业版默认安装了Powershell也可以用Powershell)

开始菜单
打开CMD（windows命令行）


输入conda --version。如下图所示：

安装成功[1]

## pip

更新pip至19.2.2版本，直接用以下命令即可：

python -m pip install --upgrade pip

Pip版本

要求Pip版本至少为1.5.0，可执行pip install --ignore-installed pip进行pip升级。


[1]: https://mp.weixin.qq.com/s?__biz=MzU1OTkwNzk4NQ==&mid=2247484108&idx=1&sn=0c9ff7488185c6287fbe56a3fa24a286&chksm=fc115732cb66de24dab450f458cc39effea9ffe4441010d5d3e00078badcdf132a54eb5388ba&token=366879770&lang=zh_CN#rd




## 镜像源


在下载需要的包的时候难免会遇到源带来的问题[2：

```
To search for alternate channels that may provide the conda package you're
looking for, navigate to

    https://anaconda.org

and use the search bar at the top of the page.
```


这个时候不必惊慌，我们可以指定一些稳定有效的源去下载安装。以下是我提供的镜像源：

- https://pypi.doubanio.com/simple/             # 豆瓣
- https://pypi.tuna.tsinghua.edu.cn/simple     # 清华
- https://pypi.mirrors.ustc.edu.cn/simple/     # 中国科学技术大学

指定镜像源的命令：

①conda

```python
conda install -c 镜像源 包名
```

②pip

```python
pip install 包名 -i 镜像源
```



[1]: https://www.bilibili.com/read/cv21030152/?from=search&spm_id_from=333.337.0.0 出
[2]: https://codeantenna.com/a/7r0VO6QkU2
