# 2022-9-27

## 字符串（与C的差别）
1. 字符串是一串byte，一串Unicode字符串
2. C中是静态数组，C#是动态数组，非固定的地址
例子：
char[] a = "中华人民共和国"

## 字符串比较
- == 和 Equals()
  - == 用来比较数值类型是否相等
  - Equals（）是比较对象是否相等
（其实差别不大，但是equals()应用更加广泛）

## Format格式化
- 将字符串进行格式化

## XAML
- 用于声明UI和UI元素

## XAML的属性设置
- 属性语法
- 属性元素语法
- 内容元素语法
- 集合语法

## WPF（布局）
- Grid:网格
- StackPanel:栈式面板
- Canvas:画布

## Grid
- 绝对值
- 比例值
- 自动调整(auto)

## StackPanel
- 属性
- 内部元素的可见性

## canvas
- 画布，在这里定位元素，需要提供相对于左上角的水平坐标和垂直坐标
- 一旦设定就不会轻易改变（相对固定）
- 所以不适于相对于艺术性的布局

## dockpanel
两个附加属性(dockpanel.Dock和content)

## 基础控件
- textbox
- button
- passwordbox
- checkbox
- radiobutton
- border
- image控件(做图片)
- mediaelement

## Image控件
- 图片

## mediaelement
- 多媒体

