# 2022-3-31

## 变量命名
1. 少使用大写字符的标识符
- 全大写的标识符一般为常量名
- 临时变量可以用下划线`_`表示

## 变量作用域
1. 多多使用模块，少用`global`,`nonlocal`

## 异常处理问题
因为，文件的打开和关闭，文件的读写等涉及外部设备的操作，很可能会失败，所以最好做异常操作，用`try except`

## 代码格式
在代码提交前，做一次格式检查，尤其看`python`的缩进和空行(有工具插件)

## 模块的设计
1. 有许多重复的设计，可以继续抽象模块化
2. 管理员的功能

## json文件
不用写死，例如：`filename = "student-system.json"`
可以直接放在在命令行里，例如：`python student-system.py student-system.json`
使py文件可以作为一个操作系统的命令使用


## 模块化遇到的问题
1. 模块无法被调用
`module object is not callable`
- 可能原因：
文件名和函数名相同，导致无法识别
这个时候就可以运用别名
例如：
`from menu import menu as m`

2. 遗留问题：
调用一次，但是却运行两遍？
- 可能原因：函数名和文件名相同，调用时，作为模块运行一次，作为函数运行一次
- 解决方法：在主函数中运用`if __name__ == 'main':`函数



3. 模块导入问题
`ImportError: cannot import name 'choose' from partially initialized module 'choose' (most likely due to a circular import) (C:\Users\sunw\work\py-practice\choose.py)`
- 问题描述：因为循环调用的原因
- 解决办法：删删掉好了
