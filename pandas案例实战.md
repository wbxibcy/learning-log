# 2022-04-27-pandas实战

## 导入包
`import pandas as pd`

## 文件操作(主要为表格文件)
使用pandas的类方法(function)
 1. `pd.read_csv`
 2. `pd.read_excel`

## 查看pandas的数据类型
`dd.describe()`

### surprise
数据类型和`numpy`是一样的
详情可见`numpy`讲义

## 查看行
使用pandas的实例方法(method)
`dd.head(n)`

## 查看列
`dd["x坐标"]`

## 索引

 - `loc[]`
loc[] 接受两个参数，并以','分隔。第一个位置表示行，第二个位置表示列。

 -  `iloc[]`
用法和`loc[]`近似

### 区别！！！
 - `loc[]` 只能使用**标签**索引，不能使用整数索引。当通过标签索引的切片方式来筛选数据时，它的取值**前闭后闭**。
 - `iloc[]` 只能使用**整数**索引，不能使用标签索引，通过整数索引切片选择数据时，**前闭后开**(不包含边界结束值)。

#### advise
建议使用`loc[]`，更加切合`python`的风格
`iloc[]`的话，更加像`C`的风格

## 列的操作
1. 可以对列进行计算(归功于numpy的向量运算)

### 小问题
如果用列除以一个标量，这个操作可不可行？

2. 增加列(临时)
`dd['新列的名称'] = 被赋值`

3. 多个列和单个列的区别
 - 多个列，返回的是DataFrame
 - 单个列，返回的是Series

### 无端联想
DNA和RNA的区别

4. 对列进行缝缝补补
 - 使用`concat`
 - 使用`merge`

5. 单个列的操作

## pandas的画图功能
 - 线条绘图
 - 柱状图：bar() 或 barh()
 - 直方图：hist()
 - 箱状箱：box()
 - 区域图：area()
 - 散点图：scatter()

## 导出表格
`dd.to_excel()`

### warning
慎用默认设置！

## 资料来源
[pandas](https://pandas.pydata.org/)