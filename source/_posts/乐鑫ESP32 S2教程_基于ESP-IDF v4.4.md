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

个人教程，第一次制作，有配套视频教程，bilibili个人空间：[合集和列表](https://space.bilibili.com/94050349/channel/series)，文章章节顺序和列表一致。

<!--more-->
# 内容简介

本手册将由浅入深，带领大家学习 ESP32-S3-WROOM-1-N16R8模组（ESP32-S3） 的各个功能，为您开启 ESP32-S3 的学习
之旅。本手册总共分为三篇： 

 1. 硬件篇：主要介绍本手册配套的硬件平台（开发板）；
 2. 软件篇：主要介绍ESP32-S3常用开发软件的使用以及一些下载调试的技巧，并详细介绍了几个常用的系统文件（程序）；
 3. 实战篇，主要通过 n 个实例（绝大部分都是使用 EDP-IDF 库完成的）带领大家一步步深入了解 ESP32-S3。

本手册为 ESP32-S3-WROOM-1-N16R8模组的配套教程，有详细原理图以及所有实例的完整代码，这些代码都有详细的注释，所有源码都经过我们严格测试，不会有任何警告和错误，另外，源码有我们生成好的 hex 文件，大家只需要通过仿真器下载到开发板即可看到实验现象，亲自体验实验过程。

本手册不仅非常适合广大学生和电子爱好者学习  ESP32-S3-WROOM-1-N16R8模组，其大量的实验以及详细的解说，也是公司产品开发的不二参考

# 前言

## 00-开发板

使用[源地ESP32-S3核心板](https://item.taobao.com/item.htm?spm=a230r.1.14.1.283f2d6cAjCT7l&id=669443108979&ns=1&abbucket=6#detail)
也可以购买复刻版：[ESP32 S3核心板](https://item.taobao.com/item.htm?_u=fpkffgdda1e&id=695554257902)

N16R8（16M 外扩flash/8M PSRAM）/双Type-C USB口/W2812 rgb/高速USB转串口
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v4.4/1676468214198.png)

![ESP32-S3-0702 _9_](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v4.4/ESP32-S3-0702_9_.PNG)

## 01-官方开发环境 Espressif IDE 一键安装

不使用 Eclipse 插件或VS Code等方式，直接使用 **Espressif-IDE 离线安装器**，集成了 OpenJDK、Python、CMake、Git、ESP-IDF、Eclipse IDE、IDF Eclipse 插件及相关构建工具，类似与Keil。

下载：[Espressif-IDE 离线安装器](https://github.com/espressif/idf-installer#espressif-ide-offline-installer) 。

找到 Espressif-IDE Offline Installer，找到最新版，点击 mirror（国内镜像）下载，点击 download是从国外github下载，会很慢，不推荐。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v4.4/1676468621567.png)

## 02-教程介绍和资料获取
模组资料：模组和芯片关系
- [ESP32 S3芯片资料](https://www.espressif.com/sites/default/files/documentation/esp32-s3_datasheet_cn.pdf)
 - [ESP32-S3-WROOM-1 模组资料](https://www.espressif.com/sites/default/files/documentation/esp32-s3-wroom-1_wroom-1u_datasheet_cn.pdf)
 - 官方开发板资料：（产品开发参考，初学者不用看）
	 - [ESP32-S3-DevKitC-1](https://docs.espressif.com/projects/esp-idf/zh_CN/latest/esp32s3/hw-reference/esp32s3/user-guide-devkitc-1.html)
	 - [ESP32-S3-BOX](https://github.com/espressif/esp-box)
	 - [ESP32-S3-EYE](https://github.com/espressif/esp-who/blob/master/docs/en/get-started/ESP32-S3-EYE_Getting_Started_Guide.md)
	 - [ESP32-S3-Korvo-1](https://github.com/espressif/esp-skainet/blob/master/docs/en/hw-reference/esp32s3/user-guide-korvo-1.md)
	 - [ESP32-S3-Korvo-2](https://docs.espressif.com/projects/esp-adf/zh_CN/latest/get-started/user-guide-esp32-s3-korvo-2.html)
- ESP IDF 是乐鑫官方推出的物联网开发框架（类似标准库或者HAL库）
  安装 Espressif-IDE 时已经自动安装，不需要在额外安装

 - ESP32芯片介绍

## 03-例程创建和项目文件结构介绍


 - 新建工程
	- 示例
	- 默认
	- 文件夹介绍（基本）
	- ESP IDF介绍
	- RTOS
# 软件篇

## Espressif-IDE简介与安装
## EDP-IDF介绍
## 新建工程与工程结构讲解
## 调试与下载
## FreeRTOS操作系统

# 实战篇
## 1、LED
## 2、按键输入
## 3、串口通信
## 4、外部中断

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
