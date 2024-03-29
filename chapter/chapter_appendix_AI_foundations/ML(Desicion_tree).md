# 机器学习（决策树）

## 决策树的基本原理

​	决策树（Decision Tree）是一种分而治之的决策过程。一个困难的预测问题，通过树的分支节点，被划分成两个或多个较为简单的子集，从结构上划分为不同的子问题。将依规则分割数据集的过程不断递归下去（Recursive Partitioning）。随着树的深度不断增加，分支节点的子集越来越小，所需要提的问题数也逐渐简化。当分支节点的深度或者问题的简单程度满足一定的停止规则（Stopping Rule）时, 该分支节点会停止分裂，此为自上而下的停止阈值（Cutoff Threshold）法；有些决策树也使用自下而上的剪枝（Pruning）法。

## 决策树的三要素？

一棵决策树的生成过程主要分为下3个部分：

1. 特征选择：从训练数据中众多的特征中选择一个特征作为当前节点的分裂标准，如何选择特征有着很多不同量化评估标准，从而衍生出不同的决策树算法。
2. 决策树生成：根据选择的特征评估标准，从上至下递归地生成子节点，直到数据集不可分则决策树停止生长。树结构来说，递归结构是最容易理解的方式。
3. 剪枝：决策树容易过拟合，一般来需要剪枝，缩小树结构规模、缓解过拟合。剪枝技术有预剪枝和后剪枝两种。

### 2.17.3 决策树学习基本算法

![决策树学习](../../img/ch2/2-5.png)

### 2.17.4 决策树算法优缺点

**决策树算法的优点**：

1. 决策树算法易理解，机理解释起来简单。
2. 决策树算法可以用于小数据集。
3. 决策树算法的时间复杂度较小，为用于训练决策树的数据点的对数。
4. 相比于其他算法智能分析一种类型变量，决策树算法可处理数字和数据的类别。
5. 能够处理多输出的问题。
6. 对缺失值不敏感。
7. 可以处理不相关特征数据。
8. 效率高，决策树只需要一次构建，反复使用，每一次预测的最大计算次数不超过决策树的深度。

**决策树算法的缺点**：

1. 对连续性的字段比较难预测。
2. 容易出现过拟合。
3. 当类别太多时，错误可能就会增加的比较快。
4. 在处理特征关联性比较强的数据时表现得不是太好。
5. 对于各类别样本数量不一致的数据，在决策树当中，信息增益的结果偏向于那些具有更多数值的特征。

### 熵的概念以及理解

​	熵：度量随机变量的不确定性。

​	定义：假设随机变量X的可能取值有$x_{1},x_{2},...,x_{n}$，对于每一个可能的取值$x_{i}$，其概率为$P(X=x_{i})=p_{i},i=1,2...,n$。随机变量的熵为：

$$
H(X)=-\sum_{i=1}^{n}p_{i}log_{2}p_{i}
$$

​       对于样本集合，假设样本有k个类别，每个类别的概率为$\frac{|C_{k}|}{|D|}$，其中 ${|C_{k}|}{|D|}$为类别为k的样本个数，$|D|​$为样本总数。样本集合D的熵为：



$$
H(D)=-\sum_{k=1}^{k}\frac{|C_{k}|}{|D|}log_{2}\frac{|C_{k}|}{|D|}
$$

### 信息增益的理解

​定义：以某特征划分数据集前后的熵的差值。

​熵可以表示样本集合的不确定性，熵越大，样本的不确定性就越大。因此可以使用划分前后集合熵的差值来衡量使用当前特征对于样本集合D划分效果的好坏。


假设划分前样本集合D的熵为H(D)。使用某个特征A划分数据集D，计算划分后的数据子集的熵为H(D|A)。

​则信息增益为：

$$
g(D,A)=H(D)-H(D|A)
$$
​
注：*在决策树构建的过程中我们总是希望集合往最快到达纯度更高的子集合方向发展，因此我们总是选择使得信息增益最大的特征来划分当前数据集D。

思想：计算所有特征划分数据集D，得到多个特征划分数据集D的信息增益，从这些信息增益中选择最大的，因而当前结点的划分特征便是使信息增益最大的划分所使用的特征。

另外这里提一下信息增益比相关知识：

$信息增益比=惩罚参数\times信息增益$

​信息增益比本质：在信息增益的基础之上乘上一个惩罚参数。特征个数较多时，惩罚参数较小；特征个数较少时，惩罚参数较大。

​惩罚参数：数据集D以特征A作为随机变量的熵的倒数。

### 剪枝处理的作用及策略

​	剪枝处理是决策树学习算法用来解决过拟合问题的一种办法。

​	在决策树算法中，为了尽可能正确分类训练样本， 节点划分过程不断重复， 有时候会造成决策树分支过多，以至于将训练样本集自身特点当作泛化特点， 而导致过拟合。 因此可以采用剪枝处理来去掉一些分支来降低过拟合的风险。

​	剪枝的基本策略有预剪枝（pre-pruning）和后剪枝（post-pruning）。

​	预剪枝：在决策树生成过程中，在每个节点划分前先估计其划分后的泛化性能， 如果不能提升，则停止划分，将当前节点标记为叶结点。

​	后剪枝：生成决策树以后，再自下而上对非叶结点进行考察， 若将此节点标记为叶结点可以带来泛化性能提升，则修改之。

```python```
#coding=utf-8

from math import log
import operator

def createDataSet():
    dataSet =[[1,1,'yes'],
              [1,1,'yes'],
              [1,0,'no'],
              [0,1,'no'],
              [0,1,'no']]
    labels = ['no surfacing','flippers'] #分类的属性
    return dataSet,labels

#计算给定数据的香农熵
def calcShannonEnt(dataSet):
    numEntries = len(dataSet)
    labelCounts = {}
    for featVec in dataSet:
        currentLabel = featVec[-1] #获得标签
        #构造存放标签的字典
        if currentLabel not in labelCounts.keys():
            labelCounts[currentLabel]=0
        labelCounts[currentLabel]+=1 #对应的标签数目+1
    #计算香农熵
    shannonEnt = 0.0
    for key in labelCounts:
        prob = float(labelCounts[key])/numEntries
        shannonEnt -=prob*log(prob,2)
    return shannonEnt

#划分数据集,三个参数为带划分的数据集，划分数据集的特征，特征的返回值
def splitDataSet(dataSet,axis,value):
    retDataSet = []
    for featVec in dataSet:
        if featVec[axis] ==value:
            #将相同数据集特征的抽取出来
            reducedFeatVec = featVec[:axis]
            reducedFeatVec.extend(featVec[axis+1:])
            retDataSet.append(reducedFeatVec)
    return retDataSet #返回一个列表

#选择最好的数据集划分方式
def chooseBestFeatureToSplit(dataSet):
    numFeature = len(dataSet[0])-1
    baseEntropy = calcShannonEnt(dataSet)
    bestInfoGain = 0.0
    beatFeature = -1
    for i in range(numFeature):
        featureList = [example[i] for example in dataSet] #获取第i个特征所有的可能取值
        uniqueVals = set(featureList)  #从列表中创建集合，得到不重复的所有可能取值ֵ
        newEntropy = 0.0
        for value in uniqueVals:
            subDataSet = splitDataSet(dataSet,i,value)   #以i为数据集特征，value为返回值，划分数据集
            prob = len(subDataSet)/float(len(dataSet))   #数据集特征为i的所占的比例
            newEntropy +=prob * calcShannonEnt(subDataSet)   #计算每种数据集的信息熵
        infoGain = baseEntropy- newEntropy
        #计算最好的信息增益，增益越大说明所占决策权越大
        if (infoGain > bestInfoGain):
            bestInfoGain = infoGain
            bestFeature = i
    return bestFeature

#递归构建决策树
def majorityCnt(classList):
    classCount = {}
    for vote in classList:
        if vote not in classCount.keys():
            classCount[vote]=0
        classCount[vote]+=1
    sortedClassCount = sorted(classCount.iteritems(),key =operator.itemgetter(1),reverse=True)#排序，True升序
    return sortedClassCount[0][0]  #返回出现次数最多的

 #创建树的函数代码
def createTree(dataSet,labels):
    classList = [example[-1]  for example in dataSet]
    if classList.count(classList[0])==len(classList):
    #if set(classList)==set([classList[0]]):#类别完全相同则停止划分
        return classList[0]
    if len(dataSet[0]) ==1:             #遍历完所有特征值时返回出现次数最多的
        return majorityCnt(classList)
    bestFeat = chooseBestFeatureToSplit(dataSet)   #选择最好的数据集划分方式
    bestFeatLabel = labels[bestFeat]   #得到对应的标签值
    myTree = {bestFeatLabel:{}}
    del(labels[bestFeat])      #清空labels[bestFeat],在下一次使用时清零
    featValues = [example[bestFeat] for example in dataSet]
    uniqueVals = set(featValues)
    for value in uniqueVals:
        subLabels =labels[:]
        #递归调用创建决策树函数
        myTree[bestFeatLabel][value]=createTree(splitDataSet(dataSet,bestFeat,value),subLabels)
    return myTree

if __name__=="__main__":
    dataSet,labels = createDataSet()
    print createTree(dataSet,labels)
``````

[1]: https://raw.githubusercontent.com/scutan90/DeepLearning-500-questions/master/ch02_%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E5%9F%BA%E7%A1%80/%E7%AC%AC%E4%BA%8C%E7%AB%A0_%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E5%9F%BA%E7%A1%80.md
[2]: https://www.cntofu.com/book/85/ml/decisiontree/code.md
