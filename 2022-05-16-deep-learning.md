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

将输出项随机置`0`来控制模型的复杂度
一般用于神经网络的**隐藏层**
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

- 当数值过大或者过小的时候，都会导致数值的问题
- 常常发生在深度模型中，因为在深度模型中会对`n`个数**累乘**

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

#### pip 安装 换源

```yaml
 阿里源：https://mirrors.aliyun.com/pypi/simple/
 清华源：https://pypi.tuna.tsinghua.edu.cn/simple/
 豆瓣：http://pypi.douban.com/simple/
 中科大： https://pypi.mirrors.ustc.edu.cn/simple/
```

示例:`pip install autogluon -i https://mirrors.aliyun.com/pypi/simple/`

## 层和块

### 层:

例如，`softmax回归`是一个单层，但是它也是一个模型，所以，单个层也可以是一个模型(较简单罢了)

### 块

块（block）可以描述单个层、由多个层组成的组件或整个模型本身。
块可以认为是层的组合体

#### 小结

- 一个`块`可以由许多`层`组成；一个`块`可以由许多`块`组成。
- `块`可以包含代码。
- `块`负责大量的内部处理，包括参数初始化和反向传播。
- `层`和`块`的顺序连接由`Sequential`块处理。

#### python 里的super().__init__()

继承父类的init的方法

### 关于Sequential

其实可以认为是个python的列表
例如：

```yaml
net = nn.Sequential(nn.Linear(4, 8), nn.ReLU(), nn.Linear(8, 1))
X = torch.rand(size=(2, 4))
print(net[2].state_dict())
```

结果：
`OrderedDict([('weight', tensor([[ 0.0425,  0.2304, -0.0954, -0.2109, -0.0345, -0.0479,  0.3346, -0.1201]])), ('bias', tensor([-0.1229]))])`
(结果不是唯一的，因为，X是个随机数)

解析：
`net(2)`是`nn.Linear(8, 1)`，看结果就可以知道，结果为一个`tensor`,而`state_dict`是一个状态字典，对于线性回归来讲,
它的状态字典就是权重(Weight)和偏移(bias)。

#### applyh函数(pandas中的灵活，但是有用的函数)

举个例子:

```yaml
# 对列进行操作(对行进行操作也是类似的)
data=np.arange(0,16).reshape(4,4)
data=pd.DataFrame(data,columns=['0','1','2','3'])
def f(x):
    return x-1
print(data)
print(data.ix[:,['1','2']].apply(f))
# 结果
    0   1   2   3
0   0   1   2   3
1   4   5   6   7
2   8   9  10  11
3  12  13  14  15
    1   2
0   0   1
1   4   5
2   8   9
3  12  13
```

### 查看层

```yaml
# 查看第一个全连接层
print(*[(name, param.shape) for name, param in net[0].named_parameters()])

# 查看所有的全连接层
print(*[(name, param.shape) for name, param in net.named_parameters()])
```

注意：
`*号`是`解包`操作
背后原理：
先用`[]`运算符将他变成`list`，再运用`*号`来进行`解包`操作

### 保存和加载向量(并不是只有向量可以保存和下载，张量或者是字典都可以)

- 保存
  torch.save()这个函数
  例子:
  
  ```yaml
  x = torch.arange(4)
  torch.save(x, 'x-file')
  ```

- 下载
  torch.load()这个函数
  例子：
  
  ```yaml
  x2 = torch.load('x-file')
  ```

### 保存模型

保存模型其实不是将模型的定义给保存下来，其实是将一个模型的参数给保存下来，
以MLP为例子，可以将参数保存下来，接着，想要使用的话，
就可以将模型先实例化，然后将参数下载下来就好。
注意:

1. 保存还是使用`torch.save()`
   但是下载就要使用`torch.load_state_dict`

### 小结

- `save`和`load`函数可用于**张量**对象的文件读写。
- 我们可以通过`参数字典`保存和`加载网络`的全部参数。
- 保存架构必须在代码中完成，而不是在参数中完成。

## 卷积神经网络

有两个原则:

1. 平移不变性

2. 局限性
   
   ### 小结
- 图像的`平移不变性`使我们以相同的方式处理局部图像，而不在乎它的位置。
- `局部性`意味着计算相应的隐藏表示只需一小部分局部图像像素。
- 在图像处理中，卷积层通常比全连接层需要**更少**的参数，但依旧获得高效用的模型。
- `卷积神经网络（CNN）`是一类特殊的神经网络，它可以包含多个卷积层。
- 多个`输入`和`输出`通道使模型在`每个空间位置`可以获取图像的`多方面特征`。
1. 卷积层将输入和核矩阵进行交叉相关，加上偏移后得到输出
2. 核矩阵和偏移是可学习的参数
3. 核矩阵的大小是超参数

## 填充和步幅

1. `填充`和`步幅`是卷积层的超参数
2. 填充:
   在输入的周围添加额外的行/列,来控制输出形状的减少量
3. 步幅:
   每次滑动`核矩阵窗口`的行/列的`步长`,可以成倍的减少输出形状

## 多输入和多输出通道

1. `输出通道数`是卷积层的`超参数`
2. 每个`输入通道`有独立的`二维`卷积核，所有通道结果相加得到`一个输出通道结果`
3. 每个输出通道有独立的三维卷积核

## 池化层

- 池化层返回窗口中`最大`或者`平均值`
- 缓解卷积层对位置的敏感性
- 有`窗口大小、填充、和步幅`作为超参数

## LeNet

- 是早期成功的神经网络
- 先使用卷积层来学习图片空间信息
- 然后使用`全连接层`来转换类别空间

## AlexNet

- AlexNet是更大更深的LeNet，10x参数个数，260x计算复杂度
- 新加入了`丢弃法`，`ReLU`,`最大池化层`和`数据增强`

## VGG

- `VGG`使用**可重复使用**的卷积块来构建深度卷积深度神经网络
- 不同的卷积块个数和超参数可以得到不同复杂度的变种
- 不同的`VGG`模型可通过每个块中卷积层数量和输出通道数量的差异来定义
- 块的使用导致网络定义的非常简洁。使用块可以有效地设计复杂的网络。
- 在VGG论文中，他们发现深层且窄的卷积(即3x3)比较浅层且宽的卷积更加有效
  所以，**深且窄**的才是`yyds`

## NiN块

- `NiN`块使用卷积层加两个1x1卷积层，后者对每个像素增加了非线性性
- `NiN`使用`全局平均池化层`来替代全连接层，这样的好处在于拥有更加少的参数，
  (`类别数`就是`全局平均池化层`的`输出通道数`)
  在之前的`VGG`和`AlexNet`中，不管如何调参，都或多或少的有过拟合的问题，
  现在运用`NiN块`的话，就会不容易过拟合，而且还会有**更加少**的参数个数。

## GoogLeNet

1. `Inception`块用`4`条不同超参数的卷积层和池化层的路来抽取不同的信息
   - 它的一个主要优点是模型参数小，计算复杂度低
2. `GoogLeNet`使用了`9`个`Inception`块，是**第一个**达到上百层的网络

## 批量归一化

- 在模型训练过程中，批量规范化利用小批量的均值和标准差，不断调整神经网络的中间输出，使得整个神经网络各层的中间输出值更
  加稳定
- 批量规范化在全连接层和卷积层的使用略有不同。
- 批量规范化层和`dropout层`一样，在训练模式和预测模式下计算不同。
- 批量规范化有许多有益的副作用，主要是`正则化`。另一方面，“减小内部协变量偏移”的原始动机似乎不是一个有效的解释。

## ResNet

- 残差块使得很深的网络更加容易训练，甚至可以训练1000层的网络
- 残差网络对随后的深层神经网络设计产生了深远影响，无论是卷积类网络还是全连接类网络。

## CPU和GPU
- CPU：
  可以处理通用计算。性能优化考虑数据读写效率和多线程
- GPU：
  使用更多的小核和更多的内存带宽，适合能大规模并行的计算任务

## 数据增强
1. 数据增广通过变形数据来罗德多样性从而使得模型泛化性能更好
2. 常见图片增广包括翻转、切割、变色
   
## 微调
- 微调通过使用在大数据上得到的预训练好的模型来初始化模型权重来完成提升精度
- 预训练模型质量很重要
- 微调通常熟读更快、精度更高

## #Pytorch报错# PytorchStreamReader failed reading zip archive failed finding central directory 的解决办法
- 删除`C:\Users\用户名/.cache\torch\hub\checkpoints.pth权重文件`

