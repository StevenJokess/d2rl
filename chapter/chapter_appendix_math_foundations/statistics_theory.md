

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-02 16:51:54
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-11 20:41:42
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 统计学基础

## 抽样（Sampling）

抽样（Sampling）是一种推论统计方法，是指从目标总体（Population，或称为母体）中抽取一部分个体作为样本（Sample），通过观察样本的某一或某些属性，依据所获得的数据对总体的数量特征得出具有一定可靠性的估计判断，从而达到对总体的认识。

## 简单随机抽样（simple random sampling）

简单随机抽样（simple random sampling），也叫纯随机抽样。从总体N个单位中随机地抽取n个单位作为样本，使得每一个容量为样本都有相同的概率被抽中。**特点是：每个样本单位被抽中的概率相等，样本的每个单位是完全独立，彼此间无一定的关联性和排斥性。**

简单随机抽样是其它各种抽样形式的基础。通常只是在总体单位之间差异程度较小和数目较少时，才采用这种方法。[3]

### 例子

 对于简单随机抽样，我举个例子，箱子里面有10个球2个红色，5个绿色，3个蓝色，我现在把箱子摇一摇，把手伸进箱子里，闭着眼睛摸出来一个球，这个球是什么颜色的呢？

三种球都有可能被我摸到，摸到红色概率为0.2，摸到绿色的为0.5，摸到蓝色为0.3，在我摸之前，摸到的颜色就是随机变量X，现在我摸出来一个球，我睁开眼睛看到球是红色的，红色就是观测值x,这个过程就称为随机抽样。

我从箱子里面摸出来一个球，并且观测到它的颜色，这样一次随机抽样就完成了。

现在我换一种问法，箱子里面有很多个球，但我也不知道多少个，我现在做随机抽样，抽到红色球的概率为0.2，绿色球的概率为0.5，蓝色为0.3，我现在把手伸进箱子里摸一个球，摸到球是什么颜色的呢？这个问题其实和刚才的问题是一样的，所以应该和刚才有一样的答案，三种球都有可能被摸到，如果我只摸一次，什么球都可能被摸到。

假如我现在摸出来一个球，记录它的颜色，然后放回去，然后把箱子使劲摇，把球打散，然后重新摸一次，我重复这个过程100次，那我记录下来的颜色有什么特点呢，由于记录了100次具有统计意义了，大约20次是红色，50次是绿色，30次是蓝色，用0.2,0.3,0.5的概率来抽一个彩球，就是Random Sampling-随机抽样。[1]

### 代码

 如下，用python语言当中的numpy.random包里面的choice函数就能做这种抽样。[2]

```py
from numpy.random import choice
samples choice(['R','G','B'】,size=100,p=[0.2,0.5,0.3])
print(samples)
```

推荐书目：https://seeing-theory.brown.edu/cn.html

## Bootstrap

Boostrap是一种抽样方法，在ensemble learning中很常见。是指对样本每次有放回的抽样，抽样K个，一共抽N次[4]

[1]: https://www.guyuehome.com/38340
[2]: https://jepsonwong.github.io/2018/12/15/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%9A%84%E7%8B%AC%E7%AB%8B%E5%90%8C%E5%88%86%E5%B8%83_%E9%87%87%E6%A0%B7/
[3]: https://blog.csdn.net/SecondLieutenant/article/details/79375166
[4]: https://blog.csdn.net/qq_45832958/article/details/123188899?spm=1001.2014.3001.5501
