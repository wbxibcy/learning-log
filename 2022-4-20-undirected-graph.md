# 2022-4-20-undirected-graph

## 运用的包
1. pandas (用于将excel文件导入)
2. matplotlib (用于画图)

### 关于这两个包
- 可从anaconda下载

## 算法
1. 将所需的包导入
`import matplotlib`
`import pandas as pd`
`import matplotlib.pyplot as plt`

2. 运用pandas的内置函数将excel文件导入并处理
内置函数`dd = pd.read_excel(path)`和`dd.iloc()`

3. 将数据处理成列表`list`

4. 运用`matplotlib.pyplot`包里面的内置函数
- `plt.scatter`(描点)
- `plt.plot`(连线)
- `plt.legend`(创建图例)
- `plt.title`(做标题)
- `plt.savefig`(保存为图片文件)
- `plt.show()`(显示)

