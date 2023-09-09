# 样式指南

## 总体要求

* 要准确、清晰、引人入胜、实用和一致

## 文本

* 章节和部分
    * 在每个章节的开头提供概述
    * 在每个部分的结构中保持一致性
        * 摘要
        * 练习
* 引用
    * 使用双引号
* 符号说明
    * 时间步 t（不是 t 时间步）
* 工具、类和函数
    * Gluon、MXNet、NumPy、spaCy、NDArray、Symbol、Block、HybridBlock、ResNet-18、Fashion-MNIST、matplotlib
        * 将这些视为没有重音符号的词（``）
    * Sequential 类/实例、HybridSequential 类/实例
        * 不使用重音符号（``）
    * `backward` 函数
        * 不是 `backward()` 函数
    * "for-loop" 而不是 "for loop"
* 术语
    * 一致使用
        * function（而不是 method）
        * instance（而不是 object）
        * weight、bias、label
        * 模型训练、模型预测（模型推理）
        * 训练/测试/验证数据集
        * 更倾向于使用 "数据/训练/测试示例" 而不是 "数据实例" 或 "数据点"
    * 区分：
        * 超参数 vs. 参数
        * 小批量随机梯度下降 vs. 随机梯度下降
* 在解释或是代码或数学的一部分时使用数字。
* 可接受的缩写
    * AI、MLP、CNN、RNN、GRU、LSTM、模型名称（例如，ELMo、GPT、BERT）
    * 我们在大多数情况下拼写全名以确保清晰（例如，NLP -> 自然语言处理）

## 数学

* 在[数学符号](/chapter/chapter_book_intro/glossary%26notations.md)上保持一致
* 如有必要，在方程式内部放置标点符号，例如逗号和句号
* 赋值符号
    * \leftarrow
* 仅在它们是数学的一部分时使用数学数字：“x 要么是 1 要么是 -1”，“12 和 18 的最大公约数是 6”。
* 我们不使用 "千位分隔符"（因为不同的出版社有不同的样式）。例如，10,000 应该写为 10000 在源 Markdown 文件中。

## 图表

* 软件
    * 使用 OmniGraffle 制作图表。
      * 以 100% 导出 PDF（无限画布），然后使用 pdf2svg 转换为 SVG
        * `ls *.pdf | while read f; do pdf2svg $f ${f%.pdf}.svg; done`
      * 不要直接从 OmniGraffle 导出 SVG（字体大小可能会稍微变化）
* 样式
    * 尺寸：
        * 水平：<= 400 像素（受页面宽度限制）
        * 垂直：<= 200 像素（可以有例外情况）
    * 线条粗细：
        * StickArrow
        * 1pt
        * 箭头头部大小：50%
    * 字体：
        * Arial（用于文本）、STIXGeneral（用于数学）、9pt（下标/上标：6pt）
        * 不要在下标或上标中使用斜体的数字或括号
    * 颜色：
        * 蓝色作为背景（文本是黑色的）
            * （尽量避免）特别深蓝：3FA3FD
            * 深蓝：66BFFF
            * 浅蓝：B2D9FF
            * （尽量避免）特别浅蓝：CFF4FF
* 注意版权问题

## 代码

* 每行最多有 <= 78 个字符（受页面宽度限制）。对于 [Cambridge 样式](https://github.com/d2l-ai/d2l-en/pull/2187)，每行最多有 <= 79 个字符。
* Python
    * 遵循 PEP8
        * 例如（https://www.python.org/dev/peps/pep-0008/#should-a-line-break-before-or-after-a-binary-operator）
* 为了节省空间，将多个赋值放在同一行上
  * 例如，`num_epochs, lr = 5, 0.1`
* 变量名要保持一致
    * `num_epochs`
        * 迭代周期数
    * `num_hiddens`
        * 隐藏单元数
    * `

继续：

* `num_inputs`
  * 输入数量
* `num_outputs`
  * 输出数量
* `net`
  * 模型
* `lr`
  * 学习率
* `acc`
  * 准确度
* 在迭代中
  * 特征：`X`
  * 标签：`y`、`y_hat` 或 `Y`、`Y_hat`
  * `for X, y in data_iter`
* 数据集：
  * 特征：`features` 或 `images`
  * 标签：`labels`
  * DataLoader 实例：`train_iter`、`test_iter`、`data_iter`
* 注释
  * 注释末尾不加句号。
  * 为了清晰起见，用重音符号括起变量名，例如 `# 'X' 的形状`
* 模块导入
  * 按字母顺序导入
* 打印变量
  * 如果可能，使用 `x, y` 而不是在代码块末尾使用 `print('x:', x, 'y:', y)`
* 字符串
  * 使用单引号
  * 使用 f-strings。要将长的 f-strings 分成多行，只需每行使用一个 f-string。
* 其他事项
  * `nd.f(x)` → `x.nd`
  * `.1` → `1.0`
  * `1.` → `1.0`

## 参考资料

* 参考 [d2lbook](https://book.d2l.ai/user/markdown.html#cross-references) 以了解如何为图表、表格和方程式添加引用。

## 网址

在设置 `style = cambridge` 时，URL 将转换为 QR 码，这需要使用 [URL 编码](https://www.urlencoder.io/learn/) 替换特殊字符。例如：

`Stanford's [large movie review dataset](https://ai.stanford.edu/~amaas/data/sentiment/)`
->
`Stanford's [large movie review dataset](https://ai.stanford.edu/%7Eamaas/data/sentiment/)`

## 引用

1. 运行 `pip install git+https://github.com/d2l-ai/d2l-book`
2. 使用 bibtool 为 bibtex 条目生成一致的键。通过 `brew install bib-tool` 安装它。
3. 在根目录的 `d2l.bib` 中添加一个 bibtex 条目。假设原始条目如下：

```
@article{wood2011sequence,
  title={The sequence memoizer},
  author={Wood, Frank and Gasthaus, Jan and Archambeau, C{\'e}dric and James, Lancelot and Teh, Yee Whye},
  journal={Communications of the ACM},
  volume={54},
  number={2},
  pages={91--98},
  year={2011},
  publisher={ACM}
}
```

4. 运行 `bibtool -s -f "%3n(author).%d(year)" d2l.bib -o d2l.bib`。现在，添加的条目将具有一致的键。作为副作用，它将按字母顺序出现在文件中的所有其他论文相对于所有其他论文的文件中：

```
@Article{	  Wood.Gasthaus.Archambeau.ea.2011,
  title		= {The sequence memoizer},
  author	= {Wood, Frank and Gasthaus, Jan and Archambeau, C{\'e}dric
		  and James, Lancelot and Teh, Yee Whye},
  journal	= {Communications of the ACM},
  volume	= {54},
  number	= {2},
  pages		= {91--98},
  year		= {2011},
  publisher	= {ACM}
}
```

5. 在文本中使用以下方式引用添加的论文：

```
:cite:`Wood.Gasthaus.Archambeau.ea.2011`
```

## 在一个框架中编辑和测试代码

1. 假设我们想在 xx.md 中编辑和测试 MXNet 代码，请运行 `d2lbook2 activate default xx.md`。然后，在 xx.md 中禁用其他框架的代码。
2. 使用 Jupyter 笔记本打开 xx.md，编辑代码，并使用 "Kernel -> Restart & Run All" 测试代码。
3. 运行 `d2lbook2 activate all xx.md`，以重新启用所有框架的代码。然后进行 Git 提交。

同样，`d2lbook2 activate pytorch/tensorflow xx.md` 将仅在 xx.md 中激活 PyTorch/TensorFlow 代码。

