---
layout: post
title: FPGA学习笔记
index_img: https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_3_.jpg
date: 2023-08-25
updated: 2023-08-25
hide: false
## sticky: 100 #置顶，数字越大越靠前
## banner_img: #/img/post_banner.jpg
## comment: false
categories: 01-专业
---

<!--more-->

# FPGA 开发流程

## 正点原子
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1692973462775.png)

## 实际工作



![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693036157272.png)

# LED
## 工程管理

参考正点原子

 1. doc：开发过程中使用的辅助文档文件（如绘图软件绘制的波形图文件等）
 2. prj：新建工程及产生的文件（Vivado工程）
 3. rtl：开发过程中的 RTL 代码文件
 4. sim 仿真工程与仿真文件

工程路径除了英文、数字以及下划线等，不要出现中文或者其它特殊字符，否则 FPGA 开发工具无法识别工程路径

> 正点原子开发方式是先编写RTL代码，再modelsim中仿真，最终Vivado新建工程

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_1_.jpg)