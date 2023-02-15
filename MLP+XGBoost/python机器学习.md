# 2023-2-8

## MLP

### 参数

- **activation**：激活函数
- **hidden_layer_sizes**：隐藏层的size
- **max_iter**：最大迭代次数
- **solver**：求解器

### 单变量调参

#### 隐藏层

- 隐藏层的size的不同

![size](./picture/MLp(size).png)

- 代码截图

  ![code](./picture/MLP(size)(code).png)

#### 最大迭代次数

- 迭代次数的不同

  ![item](./picture/MLP(item).png)

- 代码截图

![code](./picture/MLP(item)(code).png)

### 自动调参

#### 最优参数

![final](./picture/MLP(final).png)



#### 代码截图

![code](./picture/MLP(final)(code).png)

## XGBoost

### 参数

- **n_estimators**：树的数量
- **eta**：学习率
- **max_depth**：树的深度
- **colsample_bytree**：每棵树随机抽样出的特征占所有特征的比例
- **min_child_weight**：叶子节点上所需要的最小样本权重

### 单变量调参

#### 最大迭代次数

- 最大迭代次数

![n_e](./picture/xgboost(n_e).png)

- 代码截图

![code](./picture/xgboost(n_e)(code).png)

#### 学习率

- 学习率的不同

![eta](./picture/xgboost(eta).png)

- 代码截图

![code](./picture/xgboost(eta)(code).png)

#### 最大深度

- 最大深度的不同

![dep](./picture/xgboost(dep).png)

- 代码截图

![code](./picture/xgboost(dep)(code).png)

#### 叶子节点最小权重

- 最小权重的不同

![mcw](./picture/xgboost(mcw).png)

- 代码截图

![code](./picture/xgboost(mcw)(code).png)

### 自动调参

#### 最优参数

![final](./picture/xgboost(final).png)

#### 代码截图

![code](./picture/xgboost(final)(code).png)

 