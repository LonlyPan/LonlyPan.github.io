---
title: 乐鑫ESP32 S3教程_基于ESP-IDF v4.4
index_img:  https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S2教程_基于ESP-IDF_v4.4/ESP32_S2教程封面-合集.png
date: 2022-05-08 19:23
updated: 2022-05-08 19:23
hide: false
# sticky: 100 #置顶，数字越大越靠前
# banner_img: #/img/post_banner.jpg
# comment: false
categories: 00-项目
---

个人教程，第一次制作，有配套视频教程，bilibili个人空间：[合集和列表](https://space.bilibili.com/94050349/channel/series)，文章章节顺序和列表一致。使用自制 ESP32 S2 开发板，基于ESP-IDF v4.4库，教程最后会使用该开发板制作实物（产品级），也算是一个实战项目。教程正在更新中，慢更。

<!--more-->
## 01-官方开发环境 Espressif IDE 一键安装

## 02-教程介绍和资料获取

- 资料查找
	- 芯片资料
	- 模组资料：模组和芯片关系
	- 官方案例
- ESP32芯片介绍
   - 内核
   - 资源
   - 
## 03-例程创建和项目文件结构介绍


- 新建工程
	- 示例
	- 默认
	- 文件夹介绍（基本）
	- ESP IDF介绍
	- RTOS

## 04-GPIO、LED

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

# ESP32/ESP8266程序下载电路

## 官方一键下载电路分析
https://www.espressif.com/zh-hans/products/modules

https://zhuanlan.zhihu.com/p/145369083
https://blog.csdn.net/weixin_41975300/article/details/104834771
https://blog.csdn.net/woniulx2014/article/details/117172805
https://blog.csdn.net/wutongpro/article/details/109101063
https://www.jianshu.com/p/d7c0dbb223a0
https://www.jianshu.com/p/fe98713e40eb
https://www.cnblogs.com/cai-zi/p/13942615.html

官方下载程序的SDK代码：
```
	# issue reset-to-bootloader:
    # RTS = either CH_PD/EN or nRESET (both active low = chip in reset
    # DTR = GPIO0 (active low = boot to flasher)
    #
    # DTR & RTS are active low signals,
    # ie True = pin @ 0V, False = pin @ VCC.
    if mode != 'no_reset':
        self._setDTR(False)  # IO0=HIGH
    1)  self._setRTS(True)   # EN=LOW, chip in reset
        time.sleep(0.1)
    2)  self._setDTR(True)   # IO0=LOW
    3)  self._setRTS(False)  # EN=HIGH, chip out of reset
        time.sleep(0.05)
    4)  self._setDTR(False)  # IO0=HIGH, done

```
分析代码可知，下载程序有四步：
1. IO0=HIGH  EN=LOW, chip in reset  延时 100ms
2. IO0=LOW EN=HIGH, chip out of reset  延时 50ms
2. IO0=HIGH done
程序下载流程：
1. 芯片断电，设置 IO0 = 0  EN = 0 进入下载模式
2. 上电，

## S2 硬件设计注意事项

- IO0相当于 WAKEUP，EN相当于 RESET
- U0TXD 需串联 499R 电阻，S2 模组已经内置。
- S2模组的管脚 IO26 默认用于连接至模组上集成的 PSRAM 的 CS 端，不可用于其他功能
- IO0 和  IO46 为系统启动模式功能。为0下载模式，为1从内部flash启动
    |管脚|默认|SPI启动模式|下载启动模式|
	|---|---|---|---|
	| IO0 |  上拉 | 1| 0|
	|IO46|下拉|x|0|
	复位放开后， 管脚和普通管脚功能相同
- IO45 设置 VDD_SPI 电压。下拉为3.3V，上拉为1.8V。S2模组中 VDD_SPI为扩展的SPI、PSRAM电压。
- GPIO18 作为 U1RXD，在芯片上电时是不确定状态，可能会影响芯片正常进入下载启动模式，需要在外部增加一个上拉电阻来解决

[用Arduino玩ESP32（05）：GPIO使用](https://www.jianshu.com/p/6f2042f7064e)
