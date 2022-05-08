---
title: "乐鑫ESP32 S2教程_基于ESP-IDF v4.4"
index_img:  https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/乐鑫ESP32_S2教程_基于ESP-IDF_v4.4/ESP32_S2教程封面-合集.png
date: 2022-05-08 19:23
updated: 2022-05-08 19:23
hide: false
# sticky: 100 #置顶，数字越大越靠前
# banner_img: #/img/post_banner.jpg
# comment: false
categories: 00-项目
---

个人教程，第一次制作，有配套视频教程，bilibili个人空间：[合集和列表](https://space.bilibili.com/94050349/channel/series)。使用自制开发板，教程最后会使用该开发板制作实物（产品集），也算是一个实践项目，正在更新中，慢更。

<!--more-->
## 00-ESP官方开发环境 Espressif IDE 一键安装教程-无需插件和手动

## 01-ESP32_IDF4.4开发-教程介绍和资料获取

## 01-ESP32 编程基本概念

- 资料查找
	- 芯片资料
	- 模组资料：模组和芯片关系
	- 官方案例
- ESP32芯片介绍
   - 内核
   - 资源
- 新建工程
	- 示例
	- 默认
	- 文件夹介绍（基本）
	- ESP IDF介绍
	- RTOS

## 02-GPIO、LED

## 03-button

## 04 串口、LOG日志

## 串口DMA

##  外部中断

## 看门狗

## 定时器中断

## PWM输出

## 输入捕获

## 电容触摸

## LCD显示

## 触摸

## LCD DMA

## RTC

## 待机唤醒

## ADC

## 内部温度

## DAC

## PWM DAC

## IIC

## SPI

## DMA

### 串口 DMA

###  SPI DMA

### LCD DMA

##  SRAM

## 内存管理

## SD

## FATFS

## 汉字显示

## 图片显示

## 照相机

## 音乐播放

## 视频播放

## 手写识别

## 输入法

## 串口IAP

## USB、OTA

## RTOS

## WIFI网络

## 蓝牙

## 温度传感器

## MPU6050






## Cmake学习

cmake_minimum_required()： cmake最小支持版本
include()：cmake 包含
project(): cmake 工程名
idf_component_register(): idf文件注册
- 参数：SCRS 源文件
- INCLUDE_DIRS .h头文件
- REQUIRES 依赖

file()： 文件操作命令
- 参数： GLOB 通过正则表达式匹配文件名并保存到变量中
