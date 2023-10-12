

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-09-12 13:49:49
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-12 23:05:09
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 无监督学习

[无监督学习](../../img/unsupervised_learning.png)

https://chengzhaoxi.xyz/467efbf5.html

（1）无监督学习的两大任务
 利用无标签的数据学习数据的分布或数据与数据之间的关系被称作无监督学习。
 ①聚类(clustering)
 ②降维(Dimension Reduction )

（2）聚类(clustering)
 ①聚类(clustering）,就是根据数据的“相似性”将数据分为多类的过程。
 ②估算两个不同样本之间的相似性,通常使用的方法就是计算两个样本之间的“距离”
 ③最常用的就是欧式距离,此外还有马氏距离，曼哈顿距离,余弦距离等。

（3）Sklearn vs.聚类
 ①scikit-learn库提供的常用聚类算法函数包含在sklearn.cluster模块中,如:K-Means,DBSCAN等。
 ②我们在前面的讲解中通过实例具体讲解了K-Means,DBSCAN这些经典的聚类函数的在sklearn中的使用方法，也简单介绍了他们的算法思想。
 ③对大多数聚类算法来说,需要指定聚类的数目,DBSCAN是少数不需要指定聚类数目的算法之一。
 ④K-means:对簇中心的初始化比较敏感。
 ⑤DBSCAN:它可以发现使用K均值不能发现的许多簇,但不适合密度变化太大的数据,而且对于高维数据,该方法也有问题,因为密度定义比较困难。

（4）降维
 ①降维,就是在保证数据所具有的代表性特性或者分布的情况下，将高维数据转化为低维数据的过程。
 ②sklearn库提供多种降维算法,被封装在sklearn.decomposition模块中,在前面的学习中我们也展示了PCA和NMF在鸢尾花数据集和人脸数据集上的特征提取过程的相关操作。
 ③注意:降维方法通常用于高维数据集的探索与可视化,降维过的数据可以为其他任务作数据准备。

[1]: https://aitechtogether.com/article/4681.html
