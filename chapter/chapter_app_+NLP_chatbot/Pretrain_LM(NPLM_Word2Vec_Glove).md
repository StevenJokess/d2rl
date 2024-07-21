# 预训练语言模型


## 模型架构

NPLM的架构的三个主要部分：输入层、隐藏层和输出层。

1. 输入层将单词映射到连续的词向量空间；
1. 隐藏层通过非线性激活函数学习单词间的复杂关系；
1. 输出层通过Softmax层产生下一个单词的概率分布。

### 三、代码实现

3.1 创建数据集

# 构建一个简单的数据集
sentences = ["我 喜欢 玩具", "我 爱 爸爸", "我 讨厌 挨打"]
# 将所有句子连接在一起，用空格分隔成词汇，再将重复的词去除，构建词汇表
word_list = list(set(" ".join(sentences).split()))
# 创建一个字典，将每个词汇映射到一个唯一的索引
word_to_idx = {word: idx for idx, word in enumerate(word_list)}
# 创建一个字典，将每个索引映射到对应的词汇
idx_to_word = {idx: word for idx, word in enumerate(word_list)}
voc_size = len(word_list)  # 计算词汇表的大小
print('字典：', word_to_idx)  # 打印词汇到索引的映射字典
print('字典大小：', voc_size)  # 打印词汇表大小

3.2 构建模型数据
# 构建批处理数据
import torch # 导入PyTorch库
import random # 导入random库
batch_size = 2 # 每批的数据大小
def make_batch():
    input_batch = []  # 定义输入批处理列表
    target_batch = []  # 定义目标批处理列表
    selected_sentences = random.sample(sentences, batch_size) # 随机选择句子
    for sen in selected_sentences:  # 遍历每个句子
        word = sen.split()  # 用空格将句子分隔成词汇
        # 将除最后一个词以外的所有词的索引作为输入
        input = [word_to_idx[n] for n in word[:-1]]  # 创建输入数据
        # 将最后一个词的索引作为目标
        target = word_to_idx[word[-1]]  # 创建目标数据
        input_batch.append(input)  # 将输入添加到输入批处理列表
        target_batch.append(target)  # 将目标添加到目标批处理列表
    input_batch = torch.LongTensor(input_batch) # 将输入数据转换为张量
    target_batch = torch.LongTensor(target_batch) # 将目标数据转换为张量
    return input_batch, target_batch  # 返回输入批处理和目标批处理数据

input_batch, target_batch = make_batch()
print("输入批处理:",input_batch)
input_words = []
for input_idx in input_batch:
    input_words.append([idx_to_word[idx.item()] for idx in input_idx])
print("输入批处理对应的原始词:",input_words)
print("目标批处理:",target_batch)
target_words = [idx_to_word[idx.item()] for idx in target_batch]
print("目标批处理对应的原始词:",target_words)

3.3 搭建网络

import torch.nn as nn # 导入神经网络模块
# 定义神经概率语言模型 (NPLM)
class NPLM(nn.Module):
    def __init__(self):
        super(NPLM, self).__init__() # 调用父类的构造函数
        self.C = nn.Embedding(voc_size, embedding_size) # 定义一个词嵌入层
        # 第一个线性层，其输入大小为n_step * embedding_size，输出大小为n_hidden
        self.linear1 = nn.Linear(n_step * embedding_size, n_hidden)
        # 第二个线性层，其输入大小为n_hidden，输出大小为词汇表大小
        self.linear2 = nn.Linear(n_hidden, voc_size)

    def forward(self, X):  # 定义前向传播过程
        # 将输入数据通过嵌入层，生成词嵌入向量
        X = self.C(X) # X : [batch_size, n_step] -> [batch_size, n_step, embed_size]
        # 重新调整张量的形状，使其从 [batch_size, n_step, embed_size]
        X = X.view(-1, n_step * embedding_size) # 变为[batch_size, n_step * embed_size]
        hidden = torch.relu(self.linear1(X)) # 通过第一个线性层并应用ReLU激活函数
        output = self.linear2(hidden) # 通过第二个线性层得到输出
        return output # 返回输出结果

3.4 训练

n_step = 2  # 时间步数，表示每个输入序列的长度，也就是上下文长度
n_hidden = 2 # 隐藏层维度大小
embedding_size = 3 # 词嵌入维度大小
model = NPLM() # 创建神经概率语言模型实例
print(' NPLM模型结构：', model)  # 打印模型的结构


import torch.optim as optim # 导入优化器模块
criterion = nn.CrossEntropyLoss() # 定义损失函数为交叉熵损失
optimizer = optim.Adam(model.parameters(), lr=0.1) # 定义优化器为Adam，学习率为0.1
# 训练模型
for epoch in range(5000): # 设置训练迭代次数
    optimizer.zero_grad() # 清除优化器的梯度
    input_batch, target_batch = make_batch() # 创建输入和目标批处理数据
    output = model(input_batch) # 将输入数据传入模型，得到输出结果
    # output的形状为 [batch_size, n_class]，target_batch的形状为 [batch_size]
    loss = criterion(output, target_batch) #计算损失值
    if (epoch + 1) % 1000 == 0: # 每1000次迭代，打印损失值
        print(f"Epoch: {epoch+1} cost = {loss:.6f}")
    loss.backward() # 反向传播计算梯度
    optimizer.step() # 更新模型参数

3.4 预测

# 进行预测
input_strs = [['我', '讨厌'], ['我', '喜欢']]  # 需要预测的输入序列
# 将输入序列转换为对应的索引
input_indices = [[word_to_idx[word] for word in seq] for seq in input_strs]
input_tensor = torch.LongTensor(input_indices)  # 将输入序列的索引转换为张量
# 对输入序列进行预测，取输出中概率最大的类别
predict = model(input_tensor).data.max(1)[1]
# 将预测结果的索引转换为对应的词汇
predict_strs = [idx_to_word[n.item()] for n in predict.squeeze()]
for input_seq, pred in zip(input_strs, predict_strs):
    print(input_seq, '->', pred)  # 打印输入序列和预测结果
