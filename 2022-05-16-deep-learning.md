# 2022-5-16

## 模型的一生
在`pr-pytorch`的`jupyterlab`的`快速入门`

## batch_size = 64
(一次可以读取的系列的大小)

## 损失函数
crossEntropy
交叉熵

## 优化器
SGD

## epoch
epoch = train + test
纪元或期次
batch < epoch
(类似C语言中的双层for循环)

## 安装深度学习的框架
安装`pytorch`
[pytorch](https://pytorch.org/get-started/locally/#windows-python)

## 下载所需要的jupyterlab
`mkdir d2l-zh && cd d2l-zh`
(创建一个文件夹，并且进入)
`curl https://zh-v2.d2l.ai/d2l-zh-2.0.0.zip -o d2l-zh.zip`
(下载这个压缩包，并且重命名)
`unzip d2l-zh.zip && rm d2l-zh.zip`
(解压并且删除)
在本目录下，打开jupyterlab

## 有关问题
安装PyTorch后jupyter lab中仍出现“No module named torch“
`conda install jupyter`或`conda install jupyterlab`
`conda install nb_conda`(关键)
`jupyter lab`

[google](https://aitechtogether.com/ai-question/9379.html)

### 关闭jupyterlab
快捷键`ctrl+c`

## 安装d2l的软件包
1. 在`pypi`中找到这个软件包，下载`.whl`文件
[pypi](https://pypi.org/project/d2l/#files)
2. 打开到那个文件所在的目录
[csdn](https://blog.csdn.net/qq_15969343/article/details/79055603)
3. `pip install 文件`
(如速度慢，课加个镜像)
`pip install d2l-0.17.5-py3-none-any.whl -i http://pypi.douban.com/simple --trusted-host pypi.douban.com`
[csdn](https://blog.csdn.net/mario12315/article/details/108118369?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-108118369-blog-79055603.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-108118369-blog-79055603.pc_relevant_default&utm_relevant_index=1)

## 是否装驱动
命令行`driverquery`

## 如何判断有无NVIDIA的显卡
1. 打开`任务管理器`
2. 点击`性能`
- 有`NVIDIA`的名字  有
- 无`NVIDIA`的名字  无

## 安装GPU版本的pytorch
1. 确定自己的`算力`
2. 确定自己可选择的`CUDA Runtime Versiob`
3. 确保`CUDA Driver`的版本 >=  `CUDA Runtime`版本
4. 去`pytorch`的官网查找安装口令
### 
`CUDA Driver`一般在显卡驱动里面

### 如何确定自己的`CUDA Driver`的版本
1. 打开终端
2. 输入`nvidia-smi`

## 测试是否有`GPU`版本的`pytorch`
1. `conda list`
(查找是否有`pytorch`)
2. `python`
3. `import torch`
4. torch.cuda.is_available()
- 如果有，结果为`True`
- 如果没有，结果为`False`

## 从`github`下载包(以`pandas-datareader`为例)
`git clone https://github.com/pydata/pandas-datareader.git`
`cd pandas-datareader`
`python setup.py install`
先从`github`拉下来，再下载

###
`pip insatll 包`
和
下载包到本地
`pip install 本地文件`
相通

## pip 临时配网络
`pip install numpy --proxy socks5://127.0.0.1:7890`
(proxy参数（前面是两个减号）)
(7890是代理的端口号)
[jpg](C:\Users\sunw\Desktop\软件\数据科学\实验室)


## 线性回归

损失函数：
平方误差损失函数

## softmax回归

实现`softmax回归`的三个步骤

1. 对每个项求幂（使用exp）；
2. 对每一行求和（小批量中每个样本是一行），得到每个样本的规范化常数；
3. 将每一行除以其规范化常数，确保结果的和为1。

分类精度 
- 分类精度即正确预测数量与总预测数量之比

## 损失函数

1. `L2loss`
   (均分损失)

2. `L1loss`
   (绝对值损失)

3. `Huber's Robust loss`
   (L1loss和L2loss的结合体)

## 感知机

是一个`二分类`的模型，最早的`AI模型`之一
求解算法：等价于使用批量大小为`1`的梯度下降
缺点：不能拟合`XOR`函数，导致了第一次`AI寒冬`

## 多层感知机(MLP)

1. 使用隐藏层和激活函数来得到非线性模型
2. 使用`softmax`来处理多类分类
3. 超参数是隐藏层数和各隐藏层大小

### 常用的激活函数

- `Sigmoid`
- `Tanh`
- `ReLu`

### SGD

随机梯度下降

## 误差

1. 训练误差
- 模型在训练数据集上计算得到的误差
2. 泛化误差
- 模型应用在同样从原始样本的分布中抽取的无限多数据样本时，模型误差的期望
  (训练误差就是在训练的时候出现的误差，泛化误差就是在模型在新的数据上的误差)
  (关心的其实是泛化误差)

### 影响泛化误差的几个因素

1. 可调整参数的数量。当可调整参数的数量（有时称为自由度）很大时，模型往往更容易过拟合。

2. 参数采用的值。当权重的取值范围较大时，模型可能更容易过拟合。

3. 训练样本的数量。即使你的模型很简单，也很容易过拟合只包含一两个样本的数据集。
   而过拟合一个有数百万个样本的数据集则需要一个极其灵活的模型。
   (训练样本的数量太少)

## 数据集

- 验证数据集
  用来评估模型好坏的数据集
  (不是训练数据集)
- 测试数据集
  只用一次的数据集

## 解决数据数量不够的问题

### K折交叉验证

1. 将数据集分成K块
2. `for`循环
   例如:
   K = 3

当i = 1时
第一块为验证数据集
第二三块为训练数据集

当i = 2时
第二块为验证数据集
第一三块为训练数据集

当i = 3时
第三块为验证数据集
第一二块为训练数据集

误差就为三个验证数据集的误差的平均

### 训练数据集

训练模型参数

### 验证数据集

选择模型超参数

## 模型容量(难以在不同种类的模型中比较)

2个主要因素

1. 参数的个数
2. 参数值的选择范围

## 数据复杂度

1. 样本的个数
2. 每个样本的元素个数
3. 时间、空间结构
4. 多样性

#### 

模型容量要匹配数据复杂度，否则会导致欠拟合或者过拟合

## 权重衰退

数学方法：拉格朗日乘子法

### 做法

添加一个L2范数惩罚项

### 目的

将参数的范围变小，从而降低模型容量，来应对过拟合

## 丢弃法

将输出项随机置0来控制模型的复杂度
一般用于神经网络的隐藏层
丢弃的概率是模型的一个超参数

## 数值稳定性
1. 梯度爆炸
2. 梯度消失

#### diag
对角矩阵

### 梯度爆炸的问题
- 值会超出值域(infinity)
  在16位浮点数时尤为明显(6e-5~6e4)(区间不大)
- 对学习率敏感
  1. 学习率太大 -> 参数值大 -> 更加大的梯度 -> 梯度爆炸
  2. 学习率太低 -> 训练毫无进展
   (所以， 我们需要在训练过程中不断调整学习率，但是学习率也会变得很难调)

### 梯度消失的问题
- 梯度值变成零
  对于16位浮点数尤为严重(又要迫害16位浮点数了)
- 训练没有进展
  学习率在这里就没有用了
- 对于底部层尤为严重
  顶部层的训练蛮好
  无法使神经网络更加深

#### 总结
当数值过大或者过小的时候，都会导致数值的问题
常常发生在深度模型中，因为在深度模型中会对`n`个数**累乘**

## 训练稳定
目标：让梯度在一个合理的范围内
措施：
1. 将乘法变成加法
   - ResNet, LSTN
2. 归一化
   - 梯度归一化，将梯度裁剪
3. 合理的权重初始和激活函数

### 合理的权重初始和激活函数
- 将每层的输出和梯度都看成随机变量
- 让它们的均值和方差都保持一致

### 权重初始化
- 在合理值区间里随机初始参数
- 在训练一开始的时候很容易会有数值不稳定的情况
  - 远离最优解的地方损失函数表面可能很复杂
  - 而最优解附近表面会比较平

#### 激活函数
首选`Relu`
其次`tanh`
如果要选`sigmoid`,可将其进行线性变换，例如：运用`4*sigmoid(x)-2`这个激活函数
(`Relu`函数就是为了解决梯度消失而出现的激活函数)

### 总结
合理的权重初始值和激活函数的选取可以提升数值稳定性，可以有效的防止梯度消失和梯度爆炸

