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

