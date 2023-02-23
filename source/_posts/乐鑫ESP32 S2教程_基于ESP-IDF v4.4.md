---
title: 乐鑫ESP32 S3教程_基于ESP-IDF v5.0
index_img:  https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S2教程_基于ESP-IDF_v4.4/ESP32_S2教程封面-合集.png
date: 2022-05-08 19:23
updated: 2023-02-22 23:23
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

## 开环开镜搭建


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

### 简介

Espressif IDE 是乐鑫基于 Eclipse CDT，专为乐鑫物联网开发框架 ESP-IDF 打造的独立集成开发环境 (Integrated Development Environment, IDE)，支持用户使用 ESP-IDF 实现端到端物联网应用开发。

Espressif IDE 附带最新的 ESP-IDF Eclipse 插件、基本的 Eclipse CDT 插件、OpenOCD 插件以及其他来自 Eclipse 平台的第三方插件，以支持构建 ESP-IDF 应用程序。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1676987553649.png)

**Espressif IDE 的主要特性**
 - 在 Eclipse CDT 环境下构建，易于使用
 - 专为 ESP-IDF 应用程序开发而打造
 - 集成编译所需的 ESP 工具链配置
 - 可自动配置编译环境变量
 - 提供新建项目向导以及 ESP-IDF 快速入门模板
 - 具备领先的的编辑、编译以及语法着色功能
 - 配有预建的函数头和函数定义导航
 - 支持安装一个全新的 ESP-IDF 或是配置现有的 ESP-IDF
 - 可直接从 IDE 中安装和配置 ESP-IDF 工具
 - 配有用于项目设置的 SDK 配置编辑器
 - 集成 CMake 编辑器，用于编辑 CMake 文件，如 CMakeLists.txt
 - 支持基于 CMake 的编译系统
 - 支持通过 UART 和 JTAG 烧录
 - 支持自定义的 ESP-IDF OpenOCD 调试功能，包含预配置和设置
 - 支持 GDB 硬件调试
 - 集成 ESP-IDF 串行监视器，用于查看程序的串行输出
 - 配备预置编译环境变量的 ESP-IDF 终端
 - 配备应用程序大小分析编辑器，用于分析应用程序的静态内存使用情况
 - 支持堆分析，用于进行内存分析并发现内存泄漏
 - 支持 Panic 模式下 GDB Stub 调试
 - 提供应用层跟踪，以便使用启动和停止命令，分析程序行为
 - 支持 ESP32、ESP32-S2、ESP32-S3 和 ESP32-C3 系列芯片
 - 支持中英文
 - 具备可扩展性，适用于 Eclipse 生态体系中的其他第三方插件
 - 支持 Windows、macOS 和 Linux 操作系统

Espressif-IDE 离线安装器，集成了 OpenJDK、Python、CMake、Git、ESP-IDF、Eclipse IDE、IDF Eclipse 插件及相关构建工具，类似与Keil。

### 下载

下载：[Espressif-IDE 离线安装器](https://dl.espressif.cn/dl/esp-idf/?idf=4.4) 
如果上述链接失效，可按下述方法找到：
1. 进入[ESP-IDF 编程指南](https://docs.espressif.com/projects/esp-idf/zh_CN/latest/esp32/index.html)
2. 左侧栏，找到手动安装，单击右侧链接进入下载界面
![3.](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1676986539661.png)

这里选择 **espressif-ide-setup-2.8.1-with-esp-idf-5.0** 离线安装包，包含IDE和IDF。上面第一个选项是在线安装，会很慢，不推荐
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/image.png)

### 安装
1. 双击下载的安装包，开始安装
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677066678273.png)
2. 语言默认中文，点击**确定**
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677066547246.png)
3. 同意协议，单击**下一步**
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677070188632.png)
4. 等待软件完成系统检查（如果检查有问题则无法安装，需要根据提示解决相应问题），单击下一步
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677070216736.png)
5. 选择安装路径。**注意路径一定不要有中文、括号、空格及其它字符**
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677070275816.png)
6. 选择组件，我们选择**完全安装**，单击**下一步**
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677070528143.png)
7. 单击 **安装**，软件即开始自动安装，这个过程可能又10-30min，请耐心等待，中途不要退出安装或关机（最好什么都不要操作）
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677070627287.png)
8. 安装完成，这里默认勾选所有，单击**完成**
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677070693424.png)
9. 之后软件会自动运行类似下面的命令行，我们等待最后一行出现 `esp-idf` 字段时（此时命令行已停止自动运行），右上角❌关闭命令行即可
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677070758002.png)
10. 此时桌面会出现三个软件图标，我们只需要使用 **Espressif IDE** 就行了，另两个不用关心
 ![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677071231020.png)

## 新建工程与工程结构讲解

打开并运行软件，首次运行会显示欢迎界面，这里有两个很重要的链接，不过如果你点击链接后，会直接在软件中打开，而不是浏览器打开，你也可以通过下方的链接访问
- [Espressif-IDE Guide](https://github.com/espressif/idf-eclipse-plugin/blob/master/README_CN.md)
- [ESP-IDF 编程指南-中文](https://docs.espressif.com/projects/esp-idf/zh_CN/latest/esp32/index.html)

第一个链接其实是 ESP-IDF Eclipse 插件的使用说明，但我们的Espressif-IDE就是基于Eclipse 的，所以也可以阅读参考学习IDE的使用
第二个链接就是关于ESP-IDF的教程了，类似于标准库或HAL库，这是官方教程， 是最好的学习资料，当然本教程也是基于官方资料讲解，并且更加细、易懂，欢迎大家学习参考

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/image_1_.png)
关闭欢迎界面，来到主窗口，主要分为5个区域
1. 菜单栏
2. 项目浏览器，显示项目文件
3. 编辑页面，编写代码的地方
4. 信息栏，包含有信息窗口、查找、错误信息等关于工程和代码的信息
5. 辅助栏，类似信息栏，也会显示部分信息，主要帮助理解代码结构的，必须显示一些类、函数名、全局变量等

以上窗口的详细介绍，会在后续的教程内容中逐步讲解
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677074804627.png)

大家可能也注意到，我们安装时，选择了中文，但这里的界面只有部分时中文，这个是官方还没汉化完全，所以只能这么使用了
特别提醒大家，在菜单栏是有更改语言的选项的，但请**不要点击！不要点击！不要点击！** 否则软件会直接崩溃，彻底无法打开，只能重装软件。这是IDE软件已知有的bug了，至今还未修复。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677075285286.png)

### 新建样例工程

1. 有两种方式新建工程，如下图所示
- 通过项目浏览器中的快捷链接
- 左上角 **File** - > **New** ->  **乐鑫IDF项目**
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677074767312.png)
2. 输入项目名 `SampleProject`，并选择项目存放位置，我们建议给项目新建一个文件夹 `01-SampleProject`，另外**注意路径不要有中文、空格、括号等**
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677077335557.png)
3. 选择模板，这里我们选择官方提供的 `sample_project` 项目模板（至于为什么要使用模板，待会就会讲到），单击 **Finish** 完成工程创建
    在这里我们也可以看到，官方提供了很多模板例程，这是一个很好的学习资料，大家可以自行尝试不同模板例程。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677077564989.png)
4. 以下就是我们的工程了。此时 main.c 里只有一个主函数，还没有任何功能。
   同时也可以发现，程序中会有波浪线，和其它错误警告，在下面的编译完成后，就会都消失的
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677077700612.png)

这里我们可以发现，主程序并不是常见的 `main`，而是 `app_main`，这是因为esp32默认待 Free RTOS 系统。对于初学，只需要把它当成 `main` 函数就行了。

### 编译和下载

5. 在编译前我们需要选择目标芯片。单击齿轮图标
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677078067775.png)
6. IDF目标选择芯片类型 `esp32s3`
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677078239625.png)
7. 单击左上角锤子图标，编译程序
   编译就是将我们编写的程序变成可以在芯片上运行的文件，然后可以被我们下载到芯片中，芯片才能工作。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677078273263.png)
8. 我们可以在控制台（Console）窗口看到编译过程，同时在其右下角看到编译进度条。
   
   
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677078430451.png)
9. 编译完成，我们可以看到0个错误、0个警告，说明我们的程序没有任何问题，可以下载了。如果这里有错误我们就需要根据提示修复错误，警告看情况修复。
    我们也可以在该窗口看到程序文件大小、占用内存大小等信息。
	编译完成后，程序中一开始的那些警告、波浪线也会随之消失
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677078731730.png)
10. 下载。点击芯片类型边的齿轮图标
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677078898832.png)
11. 在串口号选择开发板串口号。如果不确定是那个端口，可以插拔一下开发板，有变动的端口就是我们需要确定的端口号。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677078097687.png)
12. 单击开始图标，开始下载
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677079011588.png)
13. 在控制台窗口，可以观看到下载进度。最后一行显示 **Done** 表示下载完成
	  由于我们程序中什么都没有，因此下载后，开发板并没有任何反应。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677079075614.png)

### 新建默认工程

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
