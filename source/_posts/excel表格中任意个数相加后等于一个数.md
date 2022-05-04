---
layout: post
title: "excel表格求任意个数相加后等于一个数"
index_img: https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/excel表格中任意个数相加后等于一个数/unnamed.png
date: 2020-05-13
categories: 其它
---

本文主要介绍通过 Microsoft Office 的 excel 表格的`规划求解`和`sumproduct`函数，来计算表格中任意几个数相加后等于一个数。  
方法同样适用于  LIbreOffice 

**缺点**
但是规划求解只能取得一个解，即使有多个解。

<!--more-->

**用例：**
以下多个数中（见下图），哪几个数相加的和是38481

### 1. 填入数字

首先我们在表格的列中填入所有数字，**注意第一行不要填写**

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/excel表格中任意个数相加后等于一个数/1添加数据_3.png)

### 2. 加载宏

点击菜单栏的 `工具` 选项，选择单击 `加载宏`, 在弹窗中勾选上 `规划求解`，单击 `确定` 退出。

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/excel表格中任意个数相加后等于一个数/2加载宏.png)

### 3. 添加公式

单击选择 C1 单元格，输入以下函数：  
`= sumproduct(A2:A19*B2:B19)`  
回车确认  
意思就是A列和B列的每行对应相乘结果再求和  

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/excel表格中任意个数相加后等于一个数/4sum公式_167.png)

输入完成后，依旧选择 C1 单元格，单击菜单栏的 `工具` 选项，选择单击 `规划求解`，进入下一步

### 4. 规划求解 

1. 设置目标单元格 ：默认设置好了，就是 C1 单元格，如果不是，单击输入框，再单击单元格 C1 即可  
2. 等于：选择 `值` 并填入要求和的值 `38481`
![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/excel表格中任意个数相加后等于一个数/6规划求解值参数_292.png)

3. 设置可变单元格：单击`可变单元格`下方的输入框，然后拖选 `B2 至 B9` 单元格，如下动图所示
![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/excel表格中任意个数相加后等于一个数/7可变单元格.gif)

 4. 添加约束：单击`约束`右侧 添加 按钮，再弹出窗口中的 `单元格引用位置` 拖选  `B2 至 B9` 单元格，`约束值`设置为 `bin  二进制`，单击 `确认` 退出。操作流程见下面动图。
![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/excel表格中任意个数相加后等于一个数/添加约束.png)

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/excel表格中任意个数相加后等于一个数/8约束.gif)

### 5. 求解

以上设置完成后，`规划求解参数界面`应该如下左图所示。没有问题后，单击 `求解` 按钮，开始自动计算。  
计算完成（有解），在弹窗中选择 `保存` ，单击 `确认` 结束。  
可以看到在 `列 B` 中多出了 `0 和 1`。其中有 1 的那一行对应 A  列中的数字就是要找的那几个数。  
这里我们找到了三个数，验证结果也是对的。  

 ![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/excel表格中任意个数相加后等于一个数/7求解_106.png)
 
 后面再算别的值，只需要更改 `规划求解参数界面` 的值，其它不用再设置。

## 参考链接

1. [excel 中任意几个数相加后等于一个数](https://blog.csdn.net/psp0001060/article/details/50537574)


