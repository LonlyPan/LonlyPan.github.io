---
layout: post
show_title: "STM32学习笔记-基于STM32CubeIDE"
title: "2020-12-18-STM32学习笔记-基于STM32CubeIDE"
date: 2020-12-18
last_modified_at: 2020-12-18
categories: arm
---

<!--more-->

# 安装与新建工程模板

[STM32CubeIDE使用笔记（01）：基础说明与开发流程](https://blog.csdn.net/Naisu_kun/article/details/95935283)

## 简介
![Stm32CubeIDE_show](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/Stm32CubeIDE_show.png)

>产品链接
[STM32CubeIDE官网](https://www.stmicroelectronics.com.cn/en/development-tools/stm32cubeide.html)

### 1. 特性

- 集成STM32CubeMX，提供以下服务：
   - STM32微控制器选择 
   - 引脚分配，时钟，IP和中间件配置 
   - 项目创建和初始化代码的生成

- 基于Eclipse™/ CDT，支撑ECLIPSE的™插件，GNU C / C ++中ARM ®工具链和GDB调试器。

- 其他高级调试功能包括：**
  - CPU内核，IP寄存器和内存视图
  - 实时变量观看视图
  - 系统分析和实时跟踪（SWV）
  - CPU故障分析工具
  
- 支持ST-LINK（STMicroelectronics）和J-Link（SEGGER）调试探针
- 从Atollic导入项目® TrueSTUDIO ®和AC6系统工作台的STM32
- 多支持操作系统：Windows ®，Linux的®和MacOS ®

### 2. 概述

[STM32CubeIDE](https://www.stmicroelectronics.com.cn/en/development-tools/stm32cubeide.html)是一款多功能的多操作系统开发工具，集成了TrueSTUDIO和STM32CubeMX，它是[STM32Cube](https://www.st.com/content/st_com/en/stm32cube-ecosystem.html)软件生态系统的一部分。  

简单来说就是：**STM32cubeIDE = true studio for stm32 + STM32cubeMX**

![Stm32CubeIDE](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/Stm32CubeIDE.png)

[STM32CubeIDE](https://www.stmicroelectronics.com.cn/en/development-tools/stm32cubeide.html)是一个高级C / C ++开发平台，具有用于STM32微控制器和微处理器的外设配置，代码生成，代码编译和调试功能。它基于ECLIPSE™/ CDT框架和GCCtoolchain用于开发，而GDB用于调试。它允许集成数百个现有插件，这些插件完成ECLIPSE™IDE的功能

[STM32CubeIDE](https://www.stmicroelectronics.com.cn/en/development-tools/stm32cubeide.html)集成了所有[STM32CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html)功能，以提供多合一的工具体验，并节省安装和开发时间。从板上选择空的STM32 MCU、MPU或预配置的微控制器或微处理器后，将创建项目并生成初始化代码。在开发期间的任何时候，用户都可以返回外围设备或中间件的初始化和配置，并重新生成初始化代码，而不会影响用户代码。

[STM32CubeIDE](https://www.stmicroelectronics.com.cn/en/development-tools/stm32cubeide.html)包括构建和堆栈分析器，可为用户提供有关项目状态和内存要求的有用信息。

[STM32CubeIDE](https://www.stmicroelectronics.com.cn/en/development-tools/stm32cubeide.html)还包括标准和高级调试功能，包括CPU内核寄存器，存储器和外设寄存器的视图，以及实时变量监视，串行线查看器（Serial Wire Viewer）接口或故障分析器。

[STM32CubeIDE](https://www.stmicroelectronics.com.cn/en/development-tools/stm32cubeide.html)支持基于Arm®Cortex®处理器的STM32产品

### 3. STM32Cube介绍

[STM32Cube](https://www.st.com/content/st_com/en/stm32cube-ecosystem.html)是意法半导体的一项原始计划，旨在通过减少开发工作量，时间和成本来显着提高设计师的生产率。 [STM32Cube](https://www.st.com/content/st_com/en/stm32cube-ecosystem.html)涵盖了整个STM32产品组合。

**[STM32Cube](https://www.st.com/content/st_com/en/stm32cube-ecosystem.html)是软件工具和嵌入式软件库的结合**：
- 一套完整的PC软件工具，可满足一个完整项目开发周期的所有需求
= 在STM32微控制器和微处理器上运行的嵌入式软件模块，可带来各种功能（从MCU组件驱动程序到更高级的面向应用的特性)

**[STM32Cube](https://www.st.com/content/st_com/en/stm32cube-ecosystem.html)生态系统包括**
![overview-zh](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/overview-zh.jpg)

- **一套易于使用的软件开发工具，涵盖从概念到实现的项目开发，其中**

  - [STM32CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html), 面向任意STM32设备的配置工具。这款简单易用的图形用户界面为Cortex-M内核生成初始化C代码，并为Cortex-A内核生成Linux设备树源码。
  - [STM32CubeIDE](https://www.st.com/en/development-tools/stm32cubeide.html), 一种集成开发环境。该IDE基于Eclipse或GNU C/C++工具链等开源解决方案，包括编译报告功能和高级调试功能。它还集成了其他工具，如STM32CubeMX（本身包含在STM32CubeIDE中）。
  - STM32CubeProgrammer（[STM32CubeProg](https://www.st.com/en/development-tools/stm32cubeprog.html)
）, 一种编程工具。它通过多种多样可用的通信媒介（JTAG、SWD、UART、USB DFU、I2C、SPI、CAN等）为读取、写入和验证设备和外部存储器等操作提供简单易用且高效的环境。
  - STM32CubeMonitor-Power（[STM32CubeMonPwr](https://www.st.com/en/development-tools/stm32cubemonpwr.html)
）。功能强大的监控工具，可帮助开发人员实时微调其应用程序的行为和性能。

- **[M32Cube MCU和MPU软件包](https://www.st.com/en/embedded-software/stm32cube-mcu-mpu-packages.html)，特定于每个微控制器和微处理器系列的全面嵌入式软件平台（例如STM32F4系列的STM32CubeF4），其中包括**
  - STM32Cube硬件抽象层（HAL），确保在STM32产品组合上实现最大的可移植性
  - STM32Cube低层API，通过对HW的高度用户控制来确保最佳性能和占用空间
  - 一套一致的中间件组件，例如RTOS，USB，TCP / IP和图形
  - 所有嵌入式软件实用程序以及全套外围设备和应用示例

- **[STM32Cube扩展包](https://www.st.com/en/embedded-software/stm32cube-expansion-packages.html)，包含嵌入式软件组件，这些组件通过以下功能补充STM32Cube MCU和MPU包的功能：**
  - 中间件扩展和应用层
  - 在某些特定的STMicroelectronics开发板上运行的示例


## 下载安装

### 1. 下载

可以从[STM32CubeIDE](https://www.st.com/en/development-tools/stm32cubeide.html) 网站上下载最新版本的安装程序。

在页面底部找到下图，根据自己电脑操作系统下载即可，这里以Windows版为例。  右边下拉菜单还可以选择其他版本，推荐下载最新版。
软件是免费的， 但下载时需要填写信息或注册登录。  

![download](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/download.png)

### 2. 安装

前面几步较简单，这里跳过。

**选择安装位置**
- 安装路径中不要包含中文字符，不要包含中文字符，不要包含中文字符
- 建议选择短路径以避免工作区路径过长而面临Windows®限制。
  
![Installer_location_dialog](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/Installer_location_dialog.png)

**“选择组件”对话框**
选择要与STM32CubeIDE一起安装的GDB服务器组件。用于STM32CubeIDE安装调试的每种JTAG探针类型都需要一个服务

![Selection_of_components_dialog_](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/Selection_of_components_dialog_.png)

## 汉化

如下图打开安装软件界面：

![汉化](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/汉化.gif)

复制下面代码，对应填入 `Add Repository` 对话框。
``` cpp
language

https://archive.eclipse.org/technology/babel/update-site/R0.16.1/2018-12/
```
![Add_Repository_12](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/Add_Repository_12.png)

单击 `添加` 按钮。之后会弹出下面对话框，下拉找到图中红框选项，
![汉化选择_56](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/汉化选择_56.png)

单击其最左侧的多选 `>` 按钮，在下拉框中选中打勾下图红框部分，之后单击 `下一步`。
![汉化选择2_135](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/汉化选择2_135.png)

>注：   
上图红框选项指软件的界面汉化，其中后面的（85.21%）表示汉化程度，可见还并未完全汉化。
其他选项也是汉化选项，是关于库，编程等的，安装会导致程序BUG，故不推荐安装。这里只对界面进行汉化。

单击 `下一步` 

![汉化选择3_220](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/汉化选择3_220.png)

勾选接受许可协议，点击 `完成`

![汉化选择4_281](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/汉化选择4_281.png)

之后会弹出下面的对话框，单击 install anyway

![汉化选择5_336](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/汉化选择5_336.png)

单击现在重启，汉化完成

![汉化选择6_396](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/汉化选择6_396.png)


## 新建工程模板

这里以控制 GPIO 输出/输出为例说明

### 1.1 新建工程

新建工程有两种途径。如下图示：

1. 第一种：界面左上角 `file` -> `New` -> STM32 Project`
![file新建工程](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/file新建工程.png)


2. 第二种：默认启动界面，在这里直接单击开始新工程。如果看不到下图界面，单击上图中 `红色五角星` 上方的 “感叹号” 图标，就会出现了。
![enter description here](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/启动界面新建工程.png)

之后会弹出如下的 芯片选择界面。
![芯片选择](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/芯片选择.png)

这里有很多种查找芯片的方法，我们这里直接在搜索框 （**1** 处）里搜索，在右下结果框里（**2**处）选中我们要查找的芯片即可。注意其最左边的五角星 <i class="far fa-star"></i>，单击收藏，则会变成蓝色。下次我们可以直接单击左上角搜索框上方的大五角星（ **3** 处），就能够快速查看已收藏的芯片，方便快速。  
选好后，我们单击 `下一步`。
![芯片选择2](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/芯片选择2.png)

**1** 处输入工程名称，**2**处 是工程存储地址，可以自定义 ，但要注意你需要为工程文件再单独新建一个文件夹，不然所有文件都会平铺在当前文件夹中。**3** 处是编程语言，这里选择使用C++。之后单击 `下一步`
![新建工程3](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/新建工程3.png)

这里是关于库文件的选项，第一个是将库文件链接到安装目录下，这样工程目录下其实是没有库文件了，如果话 PC 的话吗，会有问题。第二个是复制库文件到工程目录下。建议选择第二个。
![新建工程4](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/新建工程4.png)

接下来就是熟悉的 CubeMX 初始化配置界面了，操作方法也没有什么难的，就是将我们之前用代码写的初始化变成了图形化操作，按照我们初始化的思路一步步勾选即可了。下面具体操作。

### 1.2 引脚与配置(Pinout  and Configuration)

1. 系统配置。这里是调试工具选择和基准时钟源选择。我们用的是ST-Link的SWD（Serial Wire Debug）调试模式，所以选择 **Serial Wire** 串行线。时钟基准源默认系统滴答时钟 **SysTick**。
![SYS](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/SYS.png)

2. 时钟源配置。一共有以下选项。
- 外部晶体/陶瓷晶振(Crystal/Ceramic Resonator)：由外部无源晶体与MCU内部时钟驱动电路共同配合形成，有一定的启动时间，精度较高。外部晶振没有时，自动切换为自带的内部晶振。
- 旁路时钟源(Crystal/Ceramic Resonator)：直接从外界导入时钟信号。犹如芯片内部的驱动组件被旁路了
- Disable：无时钟，相关功能不工作。

  这里HSE配置为外部晶振，LSE选Disable(低速时钟是给看门狗和RTC的，目前未用)。时钟输出关闭。
![RCC](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/RCC.png)

3. 引脚配置。
1）在图左图形界面单击我们用到的引脚。这里我们使用的LED引脚有 PD2 和 PC8，都是低电平点亮，具体可查看开发板原理图。两个引脚我们都配置为输出模式。
![引脚选择3](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/引脚选择3.png)

    然后我们到左边的引脚具体配置。配置如图示，两个引脚相同。
![引脚配置2](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/引脚配置2.png)

### 1.3 时钟配置(Clock Configuration)

选择外部8MHz晶振，配置系统时钟为72MHz。注意APB1总线时钟式36MHz，需二分频。
这里和后面的配置都不需要保存的，也没有保存按钮<i class="fas fa-smile"></i>
![时钟选择](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/时钟选择.png)

### 1.4 项目管理(Project Manager)

这里我们注意勾选上图中红框部分。这样生成的代码每个外设一个文件夹，就不会全堆在main.c文件里了。其他的默认。
![项目管理](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/项目管理.png)

### 1.5 工具(Tools)

保持默认即可

### 1.6 编译生成初始化文件

单击上方工具栏的  `设备配置工具代码生成` ，完成工程建立。观察左侧的项目资源管理器，会发现多出了gpio.c文件等等。现在就可以正式编写程序了。
![初始化生成](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/初始化生成.png)

我们可以单击其左边的锤子 🔨 编译 按钮，编译应该是没有错误的。
![编译](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/编译.png)


### 注意

工作空间是会和项目文件绑定的，所以如果项目工程没有从Stm32CubeIDE中删除（不是删除文件，相当于卸载工程），期间移动工程文件是会出错的。

## 自己工程中添加文件与文件夹

![enter description here](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/添加文件与文件夹.png)
[stm32CubeIDE 在自己工程中添加.c 和.h文件](https://blog.csdn.net/qq_36300069/article/details/103226568)

## 参考链接

1. [STM32CubeIDE资源](https://www.st.com/zh/development-tools/stm32cubeide.html#resource)
2. [STM32CubeIDE属于一站式工具，本文带你体验它的强大](https://blog.csdn.net/ybhuangfugui/article/details/89702356)    
3. [STM32CubeMX系列教程03\_创建并生成代码工程](https://www.strongerhuang.com/STM32Cube/STM32CubeMX/STM32CubeMX%E7%B3%BB%E5%88%97%E6%95%99%E7%A8%8B03_%E5%88%9B%E5%BB%BA%E5%B9%B6%E7%94%9F%E6%88%90%E4%BB%A3%E7%A0%81%E5%B7%A5%E7%A8%8B.html) 




# 工程模板文件解读

#### 1.1.7 文件夹结构

CMSIS

存放STM32F1xx芯片硬件与代码桥接的相关定义。

1）include

包含位于CMSIS标准的核内设备函数曾层的Cortex-M核通用的头文件，作用是为那些采用Cortex-M核设计SOC的芯片商设计的芯片外设提供一个进入内核的接口，定义了一些内核相关的寄存器。这些文件在其他公司的Cortex-M系列芯片也是相同的。写STM32F1xx的程序时，必须用到其中的四个文件：core_cm3.h、core_cmFunc.h、corecmlnstr.h、cmsis_armcc.h，其他的文件是属于其他内核的，还有几个文件时DSP函数库使用的头文件。

2）Device

时具体芯片直接相关的文件，将举办韩启动文件、芯片外设 寄存器定义、系统时钟初始化功能的一些文件，有ST公司提供
gpio：外设初始化配置函数

  - stm32f103xe.h  头文件 （Device\ST\STM32F1xx\include\stm32f103xe.h）

是stm32f103xe系列新片片底层相关的文件，包含了STM32中所有的外设寄存器地址和结构体类型定义，比如熟悉的宏定义GPIOA，结构体类型 GPIO_TyoeDef都在该文件定义。

  - systen_stm32f1xx.c文件（Device\ST\STM32F1xx\Source、Templates\systen_stm32f1xx.c）

定义了几个函数：系统初始化函数：SystemInit()、系统内核始终更新函数：SystemCoreClockUpdate()、外部SRAM启动控制函数：SystemInit_ExtMemCtl()。SystemInit函数是在芯片复位后执行的一个函数，用于预初始化内核时钟

  - startup_stm32f103xe.s 启动文件 (文件路径：\Device\ST\STM32F1xx\Source\Templates\arm\startup_stm32f103xe.s)

是一个汇编文件，不同开发环境平台该文件内容不同(虽然文件名称相同，所以使用时注意区分)。该文件内容才真正是芯片上电后运行的内容，在运行了这文件内容后程序才跳转至 main.c 文件中的 main 函数。后面我们会单独分析该文件内容。

STM32F1xx_HAL_Driver 文件夹：有 Inc 跟 Src 两个文件夹，这里的文件属于CMSIS 之外的、芯片片上外设部分。

Src 里面是每个设备外设的驱动源程序，Inc 则是相对应的外设头文件。Src 及 Inc 文件夹是 HAL 库的主要内容。在 Src
和 Inc 文件夹里的就是 HAL 库针对每个外设而编写的库函数文件，每个外设对应一个 stm32f1xx_hal_ppp.c 和 stm32f1xx_hal_ppp.h 文件，部分外设有特殊功能还有一个 stm32f1xx_hal_ppp_ex.c 文件，其中 ppp 为外设名称，比如
gpio、adc、i2c 等等，见下面表格。

![enter description here](https://LonlyPan.github.io/images/Posts/2020-09-28-STM32CubeIDE-工程模板文件解读/图像_5.png)

main.c：存放main函数和SystemClock_Config函数

stm32f1xx_assert.c：

stm32f1xx_hal_msp.c：其中的 `HAL_MspInit()` 是芯片系统级初始化，一般实现系统的中断配置，比如硬件错误、微处理器故障、总线错误等。其在 HAL_Msplnit函数中被调配，而main函数起始就调用 HAL_Init 函数。

stm32f1xx_it.c：存放中断服务函数

stm32f1xx_hal_conf.h：见上面表格

stm32f1xx_it.h：中断服务函数声明，一般很少改动

# 添加自定义文件-文件夹

由于使用 Stm32CubeIDE 会自动生成配置初始化文件。为了将配置文件和自己的工程文件区分、避免相互影响，我们需要单独建立一个文件夹，存放我们自己的代码。 

这里有两种方法，一个是使用 STM32CubeIDE 重新建立文件和文件夹。另一种是从外部导入文件和文件夹。

## 新建文件-文件夹

![enter description here](https://LonlyPan.github.io/images/Posts/2020-12-18-STM32学习笔记-基于STM32CubeIDE/新建文件夹.png)

![enter description here](https://LonlyPan.github.io/images/Posts/2020-12-18-STM32学习笔记-基于STM32CubeIDE/输入文件名-完成.png)

## 导入外部文件-文件夹

## STM32cubeIDE_HAL库学习"


### 1. GPIO输入输出

#### 1.1 新建工程

新建工程有两种途径。如下图示：

直接单击就直接进入下面的芯片选择界面。

![enter description here](./images/新建工程_15.png)

![enter description here](./images/芯片选择1_16.png)

第二种单击后实际是转到启动主界面（默认的启动界面，如果有工程则会自动转为工程界面），在这里单击开始新工程，则会弹出上面的芯片界面了。

![enter description here](./images/Home_17.png)

这里有很多种查找芯片的方法，我们这里直接在搜索框 **1** 里搜索，在右下结果框里选中我们要查找的芯片即可 **2** 。

注意其最左边的五角星，单击关注<i class="fab fa-angellist fa-fw" style="color: #F44336"></i>，则会变成蓝色。注意左上角 **3** 处（搜索框上方）的大五角星也会变成蓝色，下次新建工程式，直接单击此处的五角星，就能够快速查找了，方便省时。

选好后，我们单击 ==下一步==
![enter description here](./images/芯片选择2_18.png)

**1** 输入工程名称，**2** 是工程存储地址，可以自定义地址 ，但要注意你需要为工程文件再单独新建一个文件夹，不然所有文件都会平铺在当前文件夹中。**3** 是变成语言，后续都会使用C++编程。之后单击 ==下一步==
![enter description here](./images/新建工程3_20.png)

这里是关于库文件的选项，只有两个可以勾选，具体的区别目前还不是很清楚。看了国外的视频，勾选的第一个（默认第二个）。

第一个是将库文件链接到安装目录下，这样工程目录下就没有库文件了。第二个是复制库文件到工程目录下。建议选择第二个。    --2019/5/8
![enter description here](./images/setup2.png)

到这里就是熟悉的CubeMX初始化配置界面了，操作方法也没有什么难的，就是将我们之前用代码写的初始化变成了图形化操作，按照我们初始化的思路一步步勾选即可了。下面具体操作。

##### 1.1.2 引脚&配置(Pinout & Configuration)

1、系统配置。这里是调试工具选择和基准时钟源选择。我们用的是ST-Link的SWD（Serial Wire Debug）调试模式，所以选择 **Serial Wire** 串行线。时钟基准源默认系统滴答时钟 **SysTick**。
![enter description here](./images/SYS.png)

2、时钟源配置。一共有以下选项。
外部晶体/陶瓷晶振(Crystal/Ceramic Resonator)：由外部无源晶体与MCU内部时钟驱动电路共同配合形成，有一定的启动时间，精度较高。外部晶振没有时，自动切换为自带的内部晶振。
旁路时钟源(Crystal/Ceramic Resonator)：直接从外界导入时钟信号。犹如芯片内部的驱动组件被旁路了
Disable：无时钟，相关功能不工作。
这里HSE配置为外部晶振，LSE选Disable(低速时钟是给看门狗和RTC的，目前未用)。时钟输出关闭。

3、引脚配置。
1）在图左图形界面单击我们用到的引脚。这里我们使用的LED引脚有 PD2 和 PC8，都是低电平点亮，具体可查看开发板原理图。两个引脚我们都配置为输出模式。
![enter description here](./images/引脚选择3.png)

然后我们到左边的引脚具体配置。配置如图示，两个引脚相同。
![enter description here](./images/引脚配置2_8_1.png)

##### 1.1.3 时钟配置(Clock Configuration)

选择外部8MHz晶振，配置系统时钟为72MHz。注意APB1总线时钟式36MHz，需二分频。
这里和后面的配置都不需要保存的，也没有保存按钮<i class="fas fa-smile"></i>
![enter description here](./images/时钟选择_12_1.png)

##### 1.1.4 项目管理(Project Manager)

这里我们注意勾选上图中红框部分。这样生成的代码就不会全堆在main.c文件里了。其他的默认。
![enter description here](./images/项目管理_15.png)

##### 1.1.5 工具(Tools)

保持默认即可

##### 1.1.6 编译生成初始化文件

单击上方工具栏的 ==设备配置工具代码生成== ，完成工程建立。观察左侧的项目资源管理器，会发现多出了gpio.c文件等等。现在就可以正式编写程序了。感觉很爽啊，点几下就完成了初始化配置，完全不需要以前那样自己编写，重复繁琐还得查看引脚手册。
![enter description here](./images/初始化生成17.png)

我们可以单击其左边的锤子 ==🔨 编译== 按钮，编译应该是没有错误的。
![enter description here](./images/编译_18.png)

##### 1.1.7 文件夹结构

CMSIS

存放STM32F1xx芯片硬件与代码桥接的相关定义。

1）include

包含位于CMSIS标准的核内设备函数曾层的Cortex-M核通用的头文件，作用是为那些采用Cortex-M核设计SOC的芯片商设计的芯片外设提供一个进入内核的接口，定义了一些内核相关的寄存器。这些文件在其他公司的Cortex-M系列芯片也是相同的。写STM32F1xx的程序时，必须用到其中的四个文件：core_cm3.h、core_cmFunc.h、corecmlnstr.h、cmsis_armcc.h，其他的文件是属于其他内核的，还有几个文件时DSP函数库使用的头文件。

2）Device

时具体芯片直接相关的文件，将举办韩启动文件、芯片外设 寄存器定义、系统时钟初始化功能的一些文件，有ST公司提供
gpio：外设初始化配置函数

  - stm32f103xe.h  头文件 （Device\ST\STM32F1xx\include\stm32f103xe.h）

是stm32f103xe系列新片片底层相关的文件，包含了STM32中所有的外设寄存器地址和结构体类型定义，比如熟悉的宏定义GPIOA，结构体类型 GPIO_TyoeDef都在该文件定义。

  - systen_stm32f1xx.c文件（Device\ST\STM32F1xx\Source、Templates\systen_stm32f1xx.c）

定义了几个函数：系统初始化函数：SystemInit()、系统内核始终更新函数：SystemCoreClockUpdate()、外部SRAM启动控制函数：SystemInit_ExtMemCtl()。SystemInit函数是在芯片复位后执行的一个函数，用于预初始化内核时钟

  - startup_stm32f103xe.s 启动文件 (文件路径：\Device\ST\STM32F1xx\Source\Templates\arm\startup_stm32f103xe.s)

是一个汇编文件，不同开发环境平台该文件内容不同(虽然文件名称相同，所以使用时注意区分)。该文件内容才真正是芯片上电后运行的内容，在运行了这文件内容后程序才跳转至 main.c 文件中的 main 函数。后面我们会单独分析该文件内容。

STM32F1xx_HAL_Driver 文件夹：有 Inc 跟 Src 两个文件夹，这里的文件属于CMSIS 之外的、芯片片上外设部分。

Src 里面是每个设备外设的驱动源程序，Inc 则是相对应的外设头文件。Src 及 Inc 文件夹是 HAL 库的主要内容。在 Src
和 Inc 文件夹里的就是 HAL 库针对每个外设而编写的库函数文件，每个外设对应一个 stm32f1xx_hal_ppp.c 和 stm32f1xx_hal_ppp.h 文件，部分外设有特殊功能还有一个 stm32f1xx_hal_ppp_ex.c 文件，其中 ppp 为外设名称，比如
gpio、adc、i2c 等等，见下面表格。

![表格](./attachments/1557371927718.table.html)

main.c：存放main函数和SystemClock_Config函数

stm32f1xx_assert.c：

stm32f1xx_hal_msp.c：其中的 `HAL_MspInit()` 是芯片系统级初始化，一般实现系统的中断配置，比如硬件错误、微处理器故障、总线错误等。其在 HAL_Msplnit函数中被调配，而main函数起始就调用 HAL_Init 函数。

stm32f1xx_it.c：存放中断服务函数

stm32f1xx_hal_conf.h：见上面表格

stm32f1xx_it.h：中断服务函数声明，一般很少改动


#### 1.2 代码编写

##### 1.2.1 GPIO相关知识

1、GPIO 初始化结构体
![enter description here](./images/GPIO_初始化结构体_10.png)

Pin： 引脚编号选择，一个 GPIO 外设有 16 个引脚可选，这里根据电路原理图选择目标引脚， 参数可选： GPIO_PIN_0、 …、 GPIO_PIN_15 和 GPIO_PIN_ALL。

Mode： 引脚工作模式选择， 
![enter description here](./images/GPIO_引脚工作模式选择_11.png)
Pull： 上拉或者下拉选择，用于输入模式， 可选： GPIO_NOPULL： 不上下拉；GPIO_PULLUP：使能上拉； GPIO_PULLDOWN： 使能下拉。

Speed： 引脚最大输出速度，可选： GPIO_SPEED_FREQ_LOW： 低速(2MHz)； 中速(10MHz)； 高速(50MHz)。

2、初始化函数

这里的引脚初始化在上面的图形配置cubeMx时，系统已经帮我们配置好了。这里只做讲解。

1) 使能 GPIO 端口时钟；
2) 初始化 GPIO 引脚， 即为 GPIO 初始化结构体成员赋值， 并调用 HAL_GPIO_Init函数完成初始化配置；
3) 根据项目要求控制引脚输出高低电平。
4) 
``` C++
void MX_GPIO_Init(void)
{
	 GPIO_InitTypeDef GPIO_InitStruct = {0};	 /* 定义 IO 硬件初始化结构体变量 */
	 
	__HAL_RCC_GPIOD_CLK_ENABLE(); 	/* 使能(开启) IO 端口时钟 */

	 HAL_GPIO_WritePin(LED0_GPIO_Port, LED0_Pin, GPIO_PIN_SET); /* 设置引脚输出为高电平，LED默认关 */

	 GPIO_InitStruct.Pin = LED0_Pin;  /* 设定引脚 IO 编号 */
	
	 GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;  /* 设定引脚 IO 为输出模式 */
	
	 GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_MEDIUM;  /* 设定引脚 IO 操作速度 */
	 /* 初始化引脚 IO */
	 HAL_GPIO_Init(LED0_GPIO_Port, &GPIO_InitStruct);
}
```
##### 1.2.2 主函数

1、HAL函数

设置引脚状态
`HAL_GPIO_WritePin(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin, GPIO_PinState PinState)`

 - GPIO_TypeDef：GPIOA、 GPIOB、 …、 GPIOG  
 - GPIO_Pin：GPIO_PIN_0…GPIO_PIN_15 
 - PinState：GPIO_PIN_SET 或者 GPIO_PIN_RESET

例：HAL_GPIO_WritePin(LED0_GPIO_Port,LED0_Pin,GPIO_PIN_SET);
这里的前两个`LED0_GPIO_Port,LED0_Pin`是宏定义，初始化时系统自动生成的，方便理解，==LED0==就是那个IO的别名，在GPIO配置时自己输入的，否则默认为IO名称。
![enter description here](./images/初始化私有定义_19.png)

翻转引脚状态，参数和上面一样。
`HAL_GPIO_TogglePin(GPIO_TypeDef \*GPIOx, uint16_t GPIO_Pin)`

延时函数（ms）
HAL_Delay(uint32_t Delay)

2、主程序，间隔500ms翻转引脚电平。

``` cpp
int main(void)
{
  HAL_Init();
  /* Configure the system clock */
  SystemClock_Config();

  /* Initialize all configured peripherals */
  MX_GPIO_Init();

  while (1)
  {
	  HAL_GPIO_TogglePin(LED0_GPIO_Port,LED0_Pin);
	  HAL_Delay(500);
	  HAL_GPIO_TogglePin(LED1_GPIO_Port,LED1_Pin);
	  HAL_Delay(500);
  }

}
```

效果演示：

 ![List item](./images/LED.gif)

##### 1.2.3 调试Debug

点击工具栏的 ==甲虫按钮==，在弹出对话框，选择 ==STM32 CPU==，单击 ==确认==。
![enter description here](./images/调试_20.png)

下面选择调试工具，在 ==调试器==下选择 ==ST-Link==，单击 ==确认==。
![enter description here](./images/调试2_21.png)

在弹出的对话框选择 ==Switch==，打开调试窗口。
![enter description here](./images/调试3_22.png)

这里就是调试窗口了，红框内的都是和调试有关的工具按钮，这里不多做介绍，自行摸索。
![enter description here](./images/调试4_23.png)

#### 1.3 按键输入

##### 1.3.1 引脚配置

我们的按键用到了==PC5和PA15作为Key0和Key1==。在CubeMX界面配置为引脚输入即可。

1、HAL库
引脚读取：
HAL_GPIO_ReadPin(GPIO_TypeDef \*GPIOx, uint16_t GPIO_Pin)

- GPIOx：GPIOA、 GPIOB、 …、 GPIOG
- GPIO_Pin：GPIO_PIN_0…GPIO_PIN_15

按键扫描编程流程分析

1) 使能按键引脚端口时钟
2) 初始化按键引脚， 即为 GPIO 初始化结构体成员赋值，这里设置为输入模式。最后并调用 HAL_GPIO_Init 函数完成初始化配置；
3) 调用 HAL_GPIO_ReadPin 函数读取按键引脚电平，从而判断按键是否被按下。
4) 当读取到按键按下电平后， 进行软件消抖处理，再次读取按键引脚电平，最终确定按键状态。
5) 判断到按键被按下时，进行应用处理。

##### 1.3.2 主要程序

按键扫描，软件消抖。

``` cpp
int Key0_Scan(void){
	if(HAL_GPIO_ReadPin(Key0_GPIO_Port,Key0_Pin) == 0 || HAL_GPIO_ReadPin(Key1_GPIO_Port,Key1_Pin) == 0){
		HAL_Delay(10);
		if(HAL_GPIO_ReadPin(Key0_GPIO_Port,Key0_Pin) == 0){
			while(HAL_GPIO_ReadPin(Key0_GPIO_Port,Key0_Pin) == 1);
			return 0;
		}
		else if(HAL_GPIO_ReadPin(Key1_GPIO_Port,Key1_Pin) == 0){
			while(HAL_GPIO_ReadPin(Key1_GPIO_Port,Key1_Pin) == 1);
			return 1;
		}
	}
	return 2;
}
```

主函数：
```cpp
while (1){
switch(Key0_Scan()){  // 扫描按键，获取键值
case 0:
	HAL_GPIO_WritePin(LED0_GPIO_Port,LED0_Pin,GPIO_PIN_SET);
	HAL_GPIO_WritePin(LED1_GPIO_Port,LED1_Pin,GPIO_PIN_SET);
  break;
case 1:
    HAL_GPIO_TogglePin(LED0_GPIO_Port,LED0_Pin);
	HAL_GPIO_TogglePin(LED1_GPIO_Port,LED1_Pin);
  break;
default :
  break;
}
}
 ```
 
 运行效果，按Key0.两个灯都点亮，Key1都引脚翻转。
 
 ### 第2章 按键中断
 
 #### 2.1 引脚配置
 
配置按键引脚PC5和PA15为外部中断引脚，设置引脚模式都为下降沿触发中断模式并使能上拉

![enter description here](./images/按键中断_27.png)

配置中断，设置分组为NVIC_PRIORITYGROUP_2，分别配置每个引脚的优先级并使能。

![enter description here](./images/中断配置_28.png)
 
 #### 2.2 中断知识
 
 ##### 2.2.1 基础知识
 
 1、NVIC寄存器
 
 NVIC寄存器定义在项目文件夹 Drivers/CMSIS/include/core_cm3.h.下
 
 ```cpp
 typedef struct
{
  __IOM uint32_t ISER[8U];               /*中断使能寄存器 */
        uint32_t RESERVED0[24U];
  __IOM uint32_t ICER[8U];               /*中断清除寄存器 */
        uint32_t RSERVED1[24U];
  __IOM uint32_t ISPR[8U];               /*中断挂起使能寄存器 */
        uint32_t RESERVED2[24U];
  __IOM uint32_t ICPR[8U];               /*中断挂起清除寄存器 */
        uint32_t RESERVED3[24U];
  __IOM uint32_t IABR[8U];               /*中断标志位激活位寄存器 */
        uint32_t RESERVED4[56U];
  __IOM uint8_t  IP[240U];               /*中断优先级寄存器(8位宽) */
        uint32_t RESERVED5[644U];
  __OM  uint32_t STIR;                   /*软件触发中断寄存器 */
}  NVIC_Type;
 ```

  抢占优先级的级别高于相应优先级。数值越低优先级越高。
  
##### 2.2.2 相关函数
  
  **设置中断优先级组**
  
  ```cpp
  /* \Drivers\STM32Fxx_HAL_DRIVER\Src\stm32f1xx_hal_cortex.c下 */
  void HAL_NVIC_SetPriorityGrouping(uint32_t PriorityGroup)   // 在HAL_Init和HAL_MspInit中都有调用
{
    /* 检测输入参数合法性 */
  assert_param(IS_NVIC_PRIORITY_GROUP(PriorityGroup));
  
  /* 设置优先级组 */
  NVIC_SetPriorityGrouping(PriorityGroup);
}

/* Drivers/CMSIS/include/core_cm3.h.下 */
__STATIC_INLINE void NVIC_SetPriorityGrouping(uint32_t PriorityGroup)
{
  uint32_t reg_value;
  uint32_t PriorityGroupTmp = (PriorityGroup & (uint32_t)0x07UL);             /* 只能使用值 0..7       */

  reg_value  =  SCB->AIRCR;                                                   /* 读取原寄存器设置    */
  reg_value &= ~((uint32_t)(SCB_AIRCR_VECTKEY_Msk | SCB_AIRCR_PRIGROUP_Msk)); /*清除寄存器相关为，全设置为0             */
  reg_value  =  (reg_value                                   |
                ((uint32_t)0x5FAUL << SCB_AIRCR_VECTKEY_Pos) |
                (PriorityGroupTmp << 8U)                      );              /* 粘合写是能寄存器和优先级组参数 */
  SCB->AIRCR =  reg_value;  /* 设置AUIRCR寄存器值 */
}

```

HAL_NVIC_SetPriorityGrouping 函数用于设置 NVIC 优先级组，有一个形参，可选参数为：
1) NVIC_PRIORITYGROUP_0：0 位抢占式优先级，4 位响应优先级；
2) NVIC_PRIORITYGROUP_1：1 位抢占式优先级，3 位响应优先级；
3) NVIC_PRIORITYGROUP_2：2 位抢占式优先级，2 位响应优先级；
4) NVIC_PRIORITYGROUP_3：3 位抢占式优先级，1 位响应优先级；
5) NVIC_PRIORITYGROUP_4：4 位抢占式优先级，0 位响应优先级；
HAL_NVIC_SetPriorityGrouping 实际是通过调用 NVIC_SetPriorityGrouping 函数实现功能的，最后通过给 SCB_AIRCR 寄存器赋值而实现优先级组配置。
 
**设置中段优先级**

```cpp
/* \Drivers\STM32Fxx_HAL_DRIVER\Src\stm32f1xx_hal_cortex.c下 */
void HAL_NVIC_SetPriority(IRQn_Type IRQn, uint32_t PreemptPriority, uint32_t SubPriority)
{ 
  uint32_t prioritygroup = 0x00U;
  
  /* 检测参数 */
  assert_param(IS_NVIC_SUB_PRIORITY(SubPriority));
  assert_param(IS_NVIC_PREEMPTION_PRIORITY(PreemptPriority));
  /* 获取中断优先级组 */
  prioritygroup = NVIC_GetPriorityGrouping();
  /* 设置优先级 */
  NVIC_SetPriority(IRQn, NVIC_EncodePriority(prioritygroup, PreemptPriority, SubPriority));
}

/* Drivers/CMSIS/include/core_cm3.h.下 */
__STATIC_INLINE void NVIC_SetPriority(IRQn_Type IRQn, uint32_t priority)
{
  if ((int32_t)(IRQn) < 0){
    SCB->SHP[(((uint32_t)(int32_t)IRQn) & 0xFUL)-4UL] = (uint8_t)((priority << (8U - __NVIC_PRIO_BITS)) & (uint32_t)0xFFUL);
  }
  else{
    NVIC->IP[((uint32_t)(int32_t)IRQn)]               = (uint8_t)((priority << (8U - __NVIC_PRIO_BITS)) & (uint32_t)0xFFUL);
  }
}
```

HAL_NVIC_SetPriority 函数用于设置一个中断的优先级，它有三个形参，第一个为 IRQn_Type 类型参数，指定中断源，它是定义在 stm32f103xe.h 文件中，见代码

```cpp
typedef enum
{
/******  Cortex-M3 Processor Exceptions Numbers ***************************************************/
  NonMaskableInt_IRQn         = -14,    /*!< 2 Non Maskable Interrupt                             */
  HardFault_IRQn              = -13,    /*!< 3 Cortex-M3 Hard Fault Interrupt                     */
  MemoryManagement_IRQn       = -12,    /*!< 4 Cortex-M3 Memory Management Interrupt              */
  BusFault_IRQn               = -11,    /*!< 5 Cortex-M3 Bus Fault Interrupt                      */
  UsageFault_IRQn             = -10,    /*!< 6 Cortex-M3 Usage Fault Interrupt                    */
  SVCall_IRQn                 = -5,     /*!< 11 Cortex-M3 SV Call Interrupt                       */
  DebugMonitor_IRQn           = -4,     /*!< 12 Cortex-M3 Debug Monitor Interrupt                 */
  PendSV_IRQn                 = -2,     /*!< 14 Cortex-M3 Pend SV Interrupt                       */
  SysTick_IRQn                = -1,     /*!< 15 Cortex-M3 System Tick Interrupt                   */

/******  STM32 specific Interrupt Numbers *********************************************************/
  WWDG_IRQn                   = 0,      /*!< Window WatchDog Interrupt                            */
  PVD_IRQn                    = 1,      /*!< PVD through EXTI Line detection Interrupt            */
  TAMPER_IRQn                 = 2,      /*!< Tamper Interrupt                                     */
  /* 省略部分内容 */
  DMA2_Channel2_IRQn          = 57,     /*!< DMA2 Channel 2 global Interrupt                      */
  DMA2_Channel3_IRQn          = 58,     /*!< DMA2 Channel 3 global Interrupt                      */
  DMA2_Channel4_5_IRQn        = 59,     /*!< DMA2 Channel 4 and Channel 5 global Interrupt        */
} IRQn_Type;
```
第二个和第三个形参分别设定中断的抢占式优先级和响应优先级，这个的设置要与中断组配合使用。
HAL_NVIC_SetPriority 函数实际 是调用 NVIC_SetPriority 函数实现功能的，该函数通过设置 SCB_SHP 寄存器或者NVIC_IPRx 寄存器实现功能。

**使能中断**

```cpp
void HAL_NVIC_EnableIRQ(IRQn_Type IRQn)
{
  /* Check the parameters */
  assert_param(IS_NVIC_DEVICE_IRQ(IRQn));

  /* 使能中断 */
  NVIC_EnableIRQ(IRQn);
}

__STATIC_INLINE void NVIC_EnableIRQ(IRQn_Type IRQn)
{
  NVIC->ISER[(((uint32_t)(int32_t)IRQn) >> 5UL)] = (uint32_t)(1UL << (((uint32_t)(int32_t)IRQn) & 0x1FUL));
}
```
HAL_NVIC_EnableIRQ 函数用于在 NVIC 控制器中使能指定中断，它有一个形参，是 IRQn_Type 类型参数，参考上面介绍。
HAL_NVIC_EnableIRQ 函数实际是通过调用定义在 core_cm3.h 文件中的NVIC_EnableIRQ 函数实现功能，NVIC_EnableIRQ 函数设置了 NVIC_ISER 寄存器内容。

**禁用中断**

```cpp
void HAL_NVIC_DisableIRQ(IRQn_Type IRQn)
{
  /* Check the parameters */
  assert_param(IS_NVIC_DEVICE_IRQ(IRQn));

  /* Disable interrupt */
  NVIC_DisableIRQ(IRQn);
}

__STATIC_INLINE void NVIC_DisableIRQ(IRQn_Type IRQn)
{
  NVIC->ICER[(((uint32_t)(int32_t)IRQn) >> 5UL)] = (uint32_t)(1UL << (((uint32_t)(int32_t)IRQn) & 0x1FUL));
}

```
HAL_NVIC_DisableIRQ 函数是在 NVIC 控制器中禁用指定中断，用法与HAL_NVIC_EnableIRQ 函数相同。最终通过设置 NVIC_ICER 寄存器值实现功能。

**中断系统复位**

```cpp

void HAL_NVIC_SystemReset(void)
{
  /* 系统复位 */
  NVIC_SystemReset();
}

__STATIC_INLINE void NVIC_SystemReset(void)
{
  __DSB();                                                          /*复位之前确保所有未完成的内存访问包括缓冲区写入完整 */
  SCB->AIRCR  = (uint32_t)((0x5FAUL << SCB_AIRCR_VECTKEY_Pos)    |
                           (SCB->AIRCR & SCB_AIRCR_PRIGROUP_Msk) |
                            SCB_AIRCR_SYSRESETREQ_Msk    );         /* 保持优先级组不被改变 */
  __DSB();                                                          /* 确保完成内存访问 */

  for(;;)                                                           /* 等待复位 */
  {
    __NOP();
  }
}

```
HAL_NVIC_SystemReset 函数用于初始化一个 MCU 复位要求，它设计调用NVIC_SystemReset 函数实现功能。

 #### 2.3 函数编写
 
按键中断编程流程分析
1) 使能 AFIO 时钟，设置 NVIC 优先级组；
2) 使能按键引脚端口 GPIOA 和 GPIOC 时钟，初始化配置Key0和Key1引脚
3) 配置按键引脚中断优先级并使能中断；
4) 编写外部中断回调函数
 
  ##### 2.3.1 中断初始化配置
 
  设置中断分组
  
  HAL_MspIni是被main主函数中的HAL_Init()函数调用的。这是系统默认生成的。
  
 ```cpp
 void HAL_MspInit(void)
{
  __HAL_RCC_AFIO_CLK_ENABLE();
  __HAL_RCC_PWR_CLK_ENABLE();

  /* 中断分组 */
  HAL_NVIC_SetPriorityGrouping(NVIC_PRIORITYGROUP_2);  

  /** NOJTAG: 禁用 JTAG-DP and 使能 SW-DP */
  /* 因为我们的Key1（PA15）用到了JTAG的一个引脚 */
  __HAL_AFIO_REMAP_SWJ_NOJTAG();
}
```

设置中断优先级和使能中断,系统默认生成在gpio.c引脚初始化中。
 
```cpp
void MX_GPIO_Init(void)
{
  /* 省略部分代码 */

  /* 中断优先级分组 */
  HAL_NVIC_SetPriority(EXTI9_5_IRQn, 2, 2); 
  /* 使能中断 */
  HAL_NVIC_EnableIRQ(EXTI9_5_IRQn);

  HAL_NVIC_SetPriority(EXTI15_10_IRQn, 2, 3);
  HAL_NVIC_EnableIRQ(EXTI15_10_IRQn);
}

中断的系统复位在HAL_NVIC_EnableIRQ(）中被调用，这里不再详述。
 
 ```
##### 2.3.2 中断服务函数

 stm32f1xx_it.c文件内容
 
 其用存放中断服务函数，配置时系统自动生成了两个中断函数。但这里并不做实际函数处理。
 
 ```cpp
/**
  * @brief This function handles EXTI line[9:5] interrupts.
  */
 void EXTI9_5_IRQHandler(void){
 	HAL_GPIO_EXTI_IRQHandler(GPIO_PIN_5);
}

/**
  * @brief This function handles EXTI line[15:10] interrupts.
  */
void EXTI15_10_IRQHandler(void){
	HAL_GPIO_EXTI_IRQHandler(GPIO_PIN_15);
}
 ```
 ==EXTI9_5_IRQHandler== 和 ==EXTI15_10_IRQHandler== 分别对应Key0和Key1的中断服务函数。不过函数内容都是调用 ==HAL_GPIO_EXTI_IRQHandler== 函数。其时HAL库内部函数，经过一系列处理后后会调用中断回调函数，我们一般在回调函数实现具体应用内容。
这里的函数处理在stm32f1xx_hal_gpio.c中。这里我们不用管如何实现的，只要直到最后会调用 ==HAL_GPIO_EXTI_Callback== 函数，我们的程序就在这里面编写。

```cpp
void HAL_GPIO_EXTI_IRQHandler(uint16_t GPIO_Pin)
{
  /* EXTI line interrupt detected */
  if (__HAL_GPIO_EXTI_GET_IT(GPIO_Pin) != RESET)
  {
    __HAL_GPIO_EXTI_CLEAR_IT(GPIO_Pin);
    HAL_GPIO_EXTI_Callback(GPIO_Pin);
  }
}
```
HAL_GPIO_EXTI_Callback函数系统时没有自动生成的，是需要我们自己编写的。如下：

``` cpp
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin){

	if(GPIO_Pin & Key0_Pin){
		HAL_GPIO_TogglePin(LED0_GPIO_Port,LED0_Pin); //电平反转
	}
	if(GPIO_Pin & Key1_Pin){
		HAL_GPIO_TogglePin(LED1_GPIO_Port,LED1_Pin); //电平反转
	}
}
```
这里你会发现，Key0和Key1的中断最终都调用了这一个函数，靠输入的参数  ==GPIO_Pin== 来识别时触发了哪个中断。这样写的好处不言而喻，坏处吗自然就是执行耗时变长了（中断回调函数里还要单独写逻辑判断，耗时）。
注意：这个回调函数是针对外部中断的（EXTI），也就是定时中断和其他中断，每类中断都还有自己的回调函数。HAL的思想大概就是每类中断集中在一个函数里，统一处理。

### 第3章 串口通信

#### 3.1 基础知识

TTL 电平标准与 RS232 电平标准 

![表格](./attachments/1557384915976.table.html)

串口数据包组成

![enter description here](./images/串口数据包组成29.png)

波特率：就是通讯的速率。在异步通讯中由于没有时钟信号，所以两个通讯设备之间需要预定好波特率，只有在波特率一致的情况下，才能保证接收方和发送方获取同样的数据。 

- 通讯的起始和停止信号。数据包的起始信号由一个逻辑 0 的数据位表示，而数据包的停止位可由 0.5、1、1.5、2 个逻辑 1 的数据位表示，通讯双方需约定一致。
- 有效数据。处于起始位和校验位之间。传输的主体数据，即为有效数据，其长度常被约定为 5、6、7 或 8 位长。
- 数据校验。在有效数据之后，有一个可选的数据校验位。作用是校验通讯过程中，是否出错。
奇校验要求有效数据和校验位中“1”的个数为奇数，比如一个 8 位长的有效数据为：10101001，此时共有 4 个“1”，为达到奇校验效果，校验位为“1”，此时传输的数据有 9 位；偶检验则正好相反，在上述例子中补校验位为“0”，即可达到偶校验的效果；0 校验是不管数据中的内容是什么，校验位总为“0”；1 校验是总为“1”；无校验情况下，数据包中不含校验位。

#### 3.2 引脚配置

如下图操作就可以了
1）设置引脚功能
2）设置USART模式，并开启串口中断
![enter description here](./images/串口引脚_31.png)

![enter description here](./images/USART_32.png)

#### 3.3 串口结构体介绍

##### 3.3.1 UART 操作结构体定义

```cpp
/* \Drivers\STM32Fxx_HAL_DRIVER\Src\stm32f1xx_hal_uart.h下 */
typedef struct
{
  USART_TypeDef                 *Instance;        /*UART 寄存器基地址         */
  UART_InitTypeDef              Init;             /*UART 通信参数     */
  uint8_t                       *pTxBuffPtr;      /* 指向 UART 发送缓冲区 */
  uint16_t                      TxXferSize;       /* UART 发送数据大小   */
  __IO uint16_t                 TxXferCount;      /* UART 发送计数器           */
  uint8_t                       *pRxBuffPtr;      /* 向 UART 接收缓冲区 */
  uint16_t                      RxXferSize;       /* UART 接收数据大小    */
  __IO uint16_t                 RxXferCount;      /* UART 接收计数器    */
  DMA_HandleTypeDef             *hdmatx;          /* UART 发送参数设置（DMA 模式）  */
  DMA_HandleTypeDef             *hdmarx;          /* UART 接收参数设置（DMA 模式） */
  HAL_LockTypeDef               Lock;             /* 锁定对象   */
  __IO HAL_UART_StateTypeDef    gState;           /*UART 通信状态 */
  __IO HAL_UART_StateTypeDef    RxState;          /*UART 通信状态 */
  __IO uint32_t                 ErrorCode;        /* UART 错误代码   */
}UART_HandleTypeDef;
```

##### 3.3.2  USART 初始化结构体

```cpp
/* \Drivers\STM32Fxx_HAL_DRIVER\Src\stm32f1xx_hal_uart.h下 */
typedef struct
{
  uint32_t BaudRate;                  /*波特率 */
  uint32_t WordLength;                /*字长 */
  uint32_t StopBits;                  /*停止位 */
  uint32_t Parity;                    /*校验位 */
  uint32_t Mode;                      /*USART 模式 */
  uint32_t HwFlowCtl;                 /*硬件流控制 */
  uint32_t OverSampling;       /* 过采样 */
}UART_InitTypeDef;

```

##### 3.3.3 相关HAl库函数

**1）数据接受**

```cpp
// 读取串口状态
HAL_UART_StateTypeDef HAL_UART_GetState(UART_HandleTypeDef *huart);

// 从串口中接收字符（阻塞，具有毫秒级的超时管理机制）
// 如果超时没接收完成，则不再接收数据到指定缓冲区，返回超时标志(HAL_TIMEOUT)
// 轮询模式。CPU不断查询IO设备，如设备有请求则加以处理。例如CPU不断查询串口是否传输完成，如传输超过则返回超时错误。轮询方式会占用CPU处理时间，效率较低
HAL_StatusTypeDef HAL_UART_Receive(UART_HandleTypeDef *huart, uint8_t *pData, uint16_t Size, uint32_t Timeout);

// 开启串口接收中断
// 把 接收缓冲区指针 指向 要存放接收数据的数组，设置 接收长度，接收计数器初值，然后使能串口接收中断。接收到数据时，会触发串口中断。
// 中断控制方式。当I/O操作完成时，输入输出设备控制器通过中断请求线向处理器发出中断信号，处理器收到中断信号之后，转到中断处理程序，对数据传送工作进行相应的处理。
HAL_StatusTypeDef HAL_UART_Receive_IT(UART_HandleTypeDef *huart, uint8_t *pData, uint16_t Size);
// 再然后，串口中断函数处理，直到接收到指定长度数据，而后关闭中断，不再触发接收中断，调用串口接收完成回调函数 
HAL_UART_RxCpltCallback() 。
```

在cube中配置完了之后并没有使能串口中断（有一个串口初始化函数，但是在这个函数中并未使能串口中断）需要用户手动使能。使能代码如下：
  HAL_UART_Receive_IT(&huart1,&RxBuffer,10);
什么意思呢？

HAL库的串口接收思路是这样的：用户你可以随便定义一个缓存区，大小随意，然后通过上边这个函数把这个缓存区对应到串口的接收，上面函数的意思就是把 RxBuffer（这是一个数组）作为缓存区，指定大小为10。然后usart2接收数据的时候就放到RxBuffer这个数组中，只有当接收到10个数据之后才调用一次callback函数（回调函数）。当然不要忘了该函数的使能串口接收中断功能， 另外串口接收完数据后会关闭使能，所以，在回调函数中一定要再写一次HAL_UART_Receive_IT(&huart2, (uint8_t \*)RxBuffer, 10)，使能接收中断。


根据Cube库函数给的函数以及示例，中断处理函数是： 

```
void USARTx_IRQHandler(void) { 
	HAL_UART_IRQHandler(&UartHandle); 
} 
```

其中void USARTx_IRQHandler(void)对应的是不同的中断处理函数，但是串口中断中调用的都是HAL_UART_IRQHandler(&UartHandle); 但因为HAL_UART_IRQHandler(&UartHandle);函数的参数不同，所以不会产生异常。这里和上面的外部中断是一个道理。

HAL_UART_IRQHandler(&UartHandle)函数在判断数据寄存器后，最终调用的是UART_Receive_IT(huart);

而UART_Receive_IT(huart);最终调用的是HAL_UART_RxCpltCallback(huart);回调函数。

所以串口中断接收的流程：

USARTx_IRQHandler(void)    ->    HAL_UART_IRQHandler(UART_HandleTypeDef \*huart)    -> UART_Receive_IT(UART_HandleTypeDef \*huart)    ->    HAL_UART_RxCpltCallback(huart);

我们最终编写中断处理函数时，只需编写 HAL_UART_RxCpltCallback(huart);就可以了，其他的在配置时系统都已经默认生成好了。

  
 
**2）数据发送**

```cpp
// 串口发送数据（阻塞，具有毫秒级的超时管理机制）
// 如果超时没发送完成，则不再发送，返回超时标志（HAL_TIMEOUT）
// 轮询模式
HAL_StatusTypeDef HAL_UART_Transmit(UART_HandleTypeDef *huart, uint8_t *pData, uint16_t Size, uint32_t Timeout); 

// 进入发送中断
// 把 发送缓冲区指针 指向 要发送的数据，设置 发送长度，发送计数器初值，然后使能串口发送中断，触发串口中断。
HAL_StatusTypeDef HAL_UART_Transmit_IT(UART_HandleTypeDef *huart, uint8_t *pData, uint16_t Size); 
// 再然后，串口中断函数处理，直到数据发送完成，而后关闭中断，不再发送数据，串口发送完成触发回调函数：
HAL_UART_TxCpltCallback()。

// 进入DMA发送中断 
HAL_StatusTypeDef HAL_UART_Transmit_DMA(UART_HandleTypeDef *huart, uint8_t *pData, uint16_t Size);
```
HAL_UART_Transmit_IT(UART_HandleTypeDef \*huart, uint8_t \*pData, uint16_t Size)
 非常类似于中断接收使能的函数。接收中断使能函数的作用是绑定接收缓存区并使能接收中断，但是对于发送，该函数的作用是发送指定长度的指定数据并使能发送中断。

比如有一个unsigned char 数组a[10]，HAL_UART_Transmit_IT(&huart2, a, 10)，这一句的意思是用usart2（串口2）发送a数组中的10个数据，然后使能发送中断。

当发送完成之后（或者发送一半，发送一半也有个中断）就会执行回调函数。

总结一下发送中断：

使用HAL_UART_Transmit_IT函数发送指定长度的数据，并使能发送中断，发送到一半和发送结束会触发中断（相关的回调函数是HAL_UART_TxHalfCpltCallback()和HAL_UART_TxCpltCallback()）中断触发后发送中断使能会被清除，然后调用回调函数，回调函数执行完成之后结束本次发送。

**3） 中断处理**

```cpp
HAL_UART_TxHalfCpltCallback(); 　　一半数据(half transfer)发送完成后，通过中断处理函数调用。
HAL_UART_TxCpltCallback();　　　　　发送完成后，通过中断处理函数调用。
HAL_UART_RxHalfCpltCallback();　　　一半数据(half transfer)接收完成后，通过中断处理函数调用。
HAL_UART_RxCpltCallback();　　　　　接收完成后，通过中断处理函数调用。
HAL_UART_ErrorCallback();　　　　　　传输过程中出现错误时，通过中断处理函数调用。
```
这里的函数都是需要用户编写的，HAl提供好了API触发机制。

#### 3.4 程序编写

1) 配置串口及中断优先级
2) 调用 HAL 库函数实现发送
3) 使能接收，进入中断
4) 编写中断回调函数

##### 3.4.1 串口初始化

此处系统已经默认配置好了。全部在 usart.c 文件中。

```cpp
/* 传口初始化 */
void MX_USART1_UART_Init(void)
{
  huart1.Instance = USART1;
  huart1.Init.BaudRate = 115200;
  huart1.Init.WordLength = UART_WORDLENGTH_8B;  // 8位
  huart1.Init.StopBits = UART_STOPBITS_1;  // 1停止位
  huart1.Init.Parity = UART_PARITY_NONE;  // 无校验位
  huart1.Init.Mode = UART_MODE_TX_RX;  // 全双工收发模式
  huart1.Init.HwFlowCtl = UART_HWCONTROL_NONE;  // 
  huart1.Init.OverSampling = UART_OVERSAMPLING_16;
  if (HAL_UART_Init(&huart1) != HAL_OK)
  {
    Error_Handler();
  }

}

/* 引脚初始化，使能中断 */
void HAL_UART_MspInit(UART_HandleTypeDef* uartHandle)
{

  GPIO_InitTypeDef GPIO_InitStruct = {0};
  if(uartHandle->Instance==USART1)
  {
    /* 时钟使能 */
    __HAL_RCC_USART1_CLK_ENABLE();
  
    __HAL_RCC_GPIOA_CLK_ENABLE();
    /**USART1 GPIO Configuration    
    PA9     ------> USART1_TX
    PA10     ------> USART1_RX 
    */
    GPIO_InitStruct.Pin = GPIO_PIN_9;
    GPIO_InitStruct.Mode = GPIO_MODE_AF_PP;
    GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_HIGH;
    HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);

    GPIO_InitStruct.Pin = GPIO_PIN_10;
    GPIO_InitStruct.Mode = GPIO_MODE_INPUT;
    GPIO_InitStruct.Pull = GPIO_NOPULL;
    HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);

    /* USART1 interrupt Init */
    HAL_NVIC_SetPriority(USART1_IRQn, 0, 0);  //优先级设置
    HAL_NVIC_EnableIRQ(USART1_IRQn);  // 中断使能
  }
}
```

##### 3.4.2 主程序

根据网上资料显示，HAL_UART_Receive_IT() 这个函数只能对串口中断接收进行一次接收，而且接收的字节大小是固定的uint16_t Size，
但是在实际使用中，不可能完全满足每次接收到的字节数都是一样的，而且是确定的。
所以大家采用的方法都是令 uint16_t Size = 1；这样的话，每接收到一个字节就中断一次。
　　那么中断处理函数处理的规则应该是 
　　1、关闭此接收中断 
　　2、将接收到的数据转移至缓存器 
　　3、再次打开中断
  
```cpp
uint8_t RxBuffer;  // 定义一个接受数组

int main(void)
{
  HAL_Init();
  SystemClock_Config();
  MX_GPIO_Init();
  
  MX_USART1_UART_Init();  // 串口初始化

  HAL_UART_Receive_IT(&huart1,&RxBuffer,1);  //开启中断
  /* Infinite loop */
  while (1)
  {
  }
}
```

3.4.2 接受中断函数

```cpp
void HAL_UART_RxCpltCallback(UART_HandleTypeDef *UartHandle)
{
  if(UartHandle->Instance==USART1){   /判断时那种中断
    HAL_UART_Transmit(&huart1,&RxBuffer,1,10);  // 发送10个数据
  }
  HAL_UART_Receive_IT(&huart1,&RxBuffer,1); // 再次开启中断
}
```

#### 3.5 printf函数支持

用户能定义自己的 C 语言库函数，连接器在连接时自动使用这些新的功能函数。这个过程叫做重定向 C 语言库函数。举例子来说，我们的 USART 口，本来库函数 fqutc()是把字符输出到调试器控制窗口去的，但由于我们把输出设备改成了UART 端口，这样一来，所有基于 fputc()系列函数输出都被重定向到 UART 端口上去了。下面代码中 int fputc() 作用是重定向 C 库函数 printf 到DEBUG_USARTx ；int fgetc() 作用是重定向 C 库函数到 getchar、scanf 到DEBUG_USARTx。

```cpp
/* uart.c */
#ifdef __GNUC__
#define PUTCHAR_PROTOTYPE int __io_putchar(int ch)  /* 防止重定义，具体为什么会用到GNUC我以为不知道*/
#else
#define PUTCHAR_PROTOTYPE int fputc(int ch, FILE *f)
#endif
PUTCHAR_PROTOTYPE{

	 HAL_UART_Transmit(&huart1,(uint8_t *)&ch,1, 1000);
     return ch;
	 
 }
 
int main(void)
{
 /* 省略部分代码 */
  while (1)
  {
    printf("233\n");  /* 串口打印数据 */
    HAL_Delay(500);
  }
}

 ```
 
### 第4章 串口DMA模式

#### 4.1 基础知识

DMA 用来提供在外设和存储器之间或者存储器和存储器之间的高速数据传输。传输过程中，CPU 是闲置的，数据的高速传输不需要用到 CPU，节省了 CPU 的资源来做其他的操作，比如在例程中的点亮 LED 灯。

**DMA功能图**

![enter description here](./images/DMA_6.png)

**DMA 处理**
在发送一个事件后，外设向 DMA 发送一个请求信号，如图 箭头。DMA 控制器根据通道的优先权处理请求。当 DMA 需要开始处理发送请求的外设时，DMA 控制器立即发送给它一个应答信号。外设得到应答信
号后，立即释放它的请求，同时 DMA 控制器撤销应答信号。

**仲裁器**
一个 DMA 控制器对应 8 个数据流，数据流包含要传输数据的源地址、目标地址、数据等信息。如果我们需要同时使用同一个 DMA 控制器多个外设请求时，那必然需要同时使用多个数据流，其中哪个数据流优先，此时由仲裁器来选定。
仲裁器管理数据流方法分为两个阶段。第一阶段属于软件阶段，我们在配置数据流时可以通过寄存器设定它的优先级别，可以在 DMA_CCRx 寄存器中设置，有最高优先级、高优先级、中等优先级和低优先级四个等级。第二阶段是硬件，如果两个请求有相同的软件优先级，则较低编号的通道比高编号的通道有较高的优先权。例如：通道 2 优先于通道 4。

**DMA 通道，**
DMA 通道，每个通道都可以在由固定地址的外设寄存器和存储器之间执行DMA 传输。DMA 的传输数据量是可编程，可以通过 DMA_CCRx 寄存器中的PSIZE 和 MSIZE 位来进行编程，数据量最大可以达到 65535。DMA 的外设繁多，例如 DMA1 控制器，从外设产生 7 个请求，通过逻辑或（例如通道 1 的三个 DMA 请求，这几个是通过逻辑或到通道 1 的，这样我们在同一时间，就只能使用其中的一个）输入到 DMA1 控制器，此时只有一个请求有效。如表 ，外设请求通过设置相应外设寄存器中的控制位，可以被独立地开启或关闭。

![enter description here](./images/DMA请求_3.png)

**DMA 通道配置：**

* 在 DMA_CPARx 寄存器中设置外设寄存器的地址，作为外设数据传输的源或目标。
* 在 DMA_CMARx 寄存器中设置数据存储器的地址，外设数据传输时，此地址用来存放存储器地址。
* 是 DMA_CNDTRx 寄存器中设置要传输的数据流。在每个数据传输后，这个数值递减
* 在 DMA_CCRx 寄存器中的 PL[1:0]位设置通道的优先级。
* 在 DMA_CCRx 寄存器中设置数据传输的方向、循环模式、外设和存储器的增量模式、外设和存储器的数据款的、传输一半产生中断或传输完成产生中断。
* 设置 DMA_CCRx 寄存器的 ENABLE 位，启动通道。传输一半数据和传输完成数据后，相应的中断标志位被置 1。于此可以设置中断。
 
**循环模式**
用于处理循环缓冲区和连续的数据传输（如 ADC 的扫描模式）。开启循环模式，在传输完一次后没会自动的按照相同配置重新传输，直到控制的停止或发生传输错误。在 DMA_CCRx 寄存器中的 CIRC 位开启这一功能。

**存储器到存储器模式**
这个模式是没有外设的情况下进行。当设置了DMA_CCRx 寄存器中的 MEN2MEN 位之后，DMA_CCRx 寄存器中的 EN 位启动，DMA 传输马上就会开启。当 DMA_CNDTRx 寄存器变为 0 时，传输结束。此模式不能喝循环模式同时使用。

**错误管理**
当 DMA 读写错误发生时，DMA_IFR 寄存器中对应通道的传输中断标志位（TEIF）将被置 1，如果在 DMA_CCRx 寄存器中设置了传输错误中断允许位，则产生中断。

**中断**
每个 DMA 都可以在下表中的中断事件产生中断。

![表格](./attachments/1557474335589.table.html)

#### 4.2 生成工程

串口配置照旧。其他按图示操作

![enter description here](./images/DMA配置_4.png)

#### 4.3 程序编写

##### 4.3.1 编程流程分析

1) 配置 DMA 传输是内存到外设
2) 中断的配置
3) 填充数据，发送数据

##### 4.3.2 程序

**串口 DMA 传输初始化** 

```cpp
void HAL_UART_MspInit(UART_HandleTypeDef* uartHandle)
{

  GPIO_InitTypeDef GPIO_InitStruct = {0};
  if(uartHandle->Instance==USART1)
  {
    /* USART1 clock enable */
    __HAL_RCC_USART1_CLK_ENABLE();
  
    __HAL_RCC_GPIOA_CLK_ENABLE();
    /**USART1 GPIO Configuration    
    PA9     ------> USART1_TX
    PA10     ------> USART1_RX 
    */
    GPIO_InitStruct.Pin = GPIO_PIN_9;
    GPIO_InitStruct.Mode = GPIO_MODE_AF_PP;
    GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_HIGH;
    HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);

    GPIO_InitStruct.Pin = GPIO_PIN_10;
    GPIO_InitStruct.Mode = GPIO_MODE_INPUT;
    GPIO_InitStruct.Pull = GPIO_NOPULL;
    HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);

    /* USART1 DMA Init */
    /* USART1_TX Init */
    hdma_usart1_tx.Instance = DMA1_Channel4;
    hdma_usart1_tx.Init.Direction = DMA_MEMORY_TO_PERIPH;
    hdma_usart1_tx.Init.PeriphInc = DMA_PINC_DISABLE;
    hdma_usart1_tx.Init.MemInc = DMA_MINC_ENABLE;
    hdma_usart1_tx.Init.PeriphDataAlignment = DMA_PDATAALIGN_BYTE;
    hdma_usart1_tx.Init.MemDataAlignment = DMA_MDATAALIGN_BYTE;
    hdma_usart1_tx.Init.Mode = DMA_CIRCULAR;
    hdma_usart1_tx.Init.Priority = DMA_PRIORITY_HIGH;
    if (HAL_DMA_Init(&hdma_usart1_tx) != HAL_OK)
    {
      Error_Handler();
    }

    __HAL_LINKDMA(uartHandle,hdmatx,hdma_usart1_tx);

    /* USART1 interrupt Init */
    HAL_NVIC_SetPriority(USART1_IRQn, 0, 0);
    HAL_NVIC_EnableIRQ(USART1_IRQn);
  }
}
```

**DMA 中断配置**

```cpp
void MX_DMA_Init(void) 
{
  __HAL_RCC_DMA1_CLK_ENABLE();

  /* DMA interrupt init */
  /* DMA1_Channel4_IRQn interrupt configuration */
  HAL_NVIC_SetPriority(DMA1_Channel4_IRQn, 0, 0);
  HAL_NVIC_EnableIRQ(DMA1_Channel4_IRQn);

}
```

**main.c 文件内容** 

```cpp
int main(void)
{
  MX_DMA_Init();

  /*  使能接收，进入中断回调函数 */
  HAL_UART_Receive_IT(&huart1,&RxBuffer,1);
  /*  使用 DMA 传输数据到电脑端 */
  HAL_UART_Transmit_DMA(&huart1,TxBuffer,5);
  while (1)
  {
    /* 串口使用 DMA 传输数据不占用 CPU，可以正常运行其他函数  */
	HAL_GPIO_WritePin(LED1_GPIO_Port, LED1_Pin, GPIO_PIN_SET);
	HAL_Delay(500);
	HAL_GPIO_WritePin(LED1_GPIO_Port, LED1_Pin, GPIO_PIN_RESET);
	HAL_Delay(500);
  }
}

```

### 第5章 TIM-基本定时器

#### 5.1 基础知识

STM32F1xx 系列有总共有 11 个定时器，其中 2 个高级控制定时器、4 个通用定时器、2 个基本定时器、2 个看门狗定时器和上一章节已经讲的系统滴答定时器。看门狗定时器以后章节会讲到，这一章节，主要研究的是剩下的 8 个定时器。其中 TIM1 和 TIM8 能够产生 3 对 PWM 互补输出，常用于三相电机的驱动，时钟由 APB2 的输出产生。TIM2~TIM5 是通用定时器，TIM6 和 TIM7 是基本定时器器，时钟由 APB1 输出产生。基本定时器的功能较为简单，包含两个定时器 TIM6和 TIM7，这两个定时器的功能主要有两个，第一就是基本定时功能，当累加的时钟脉冲数超过预定值时，能触发中断或者触发 DMA 请求。第二是专门用于驱动数模转换器（DAC）。TIM6 和 TIM7 两者间是完全独立的，当然，可以同时使用。

**定时器通道引脚分布**
![enter description here](./images/定时器通道引脚_5.png)

**各定时器的特性**
![enter description here](./images/定时器特性_7.png)

#### 5.2 生成工程 

Prescaler：定时器预分频设置，时钟源经过该分频器才是定时器时钟，它设定 TIMx_PSC 寄存器的值，可设置值范围为 0~65535，实现 1 至 65536 分频，我们的工程设置是 71，这样分频后的时钟是 1MHz。

CouterMode：定时器计数方式，基本定时器只能向上计数，即 TIMx_CNT 只能从 0 开始递增，无须初始化。

Period：定时器周期，可设置值为 0~65535。在定时器预分频我们已经得到分频后的时钟为 1MHz。Period 的值我们设置为 1000，这样，定时器产生中断的频率为：1MHz/1000=1KHz，即为 1ms 的定时周期。

![enter description here](./images/TIM6_8.png)

#### 5.3 程序

##### 5.3.1 TIM6 编程流程分析

1) 初始化LED 灯；
2) 设置定时器周期和预分频器；
3) 开启定时器中断；
4) 开启定时器，进入一次中断加一次数值
5) 无限循环中达到固定值循环点亮 LED

##### 5.3.2 代码实现

**TIM6初始化，系统已经生成好了。**

```cpp
/* tim.c文件中 */
void MX_TIM6_Init(void)
{
  TIM_MasterConfigTypeDef sMasterConfig = {0};

  htim6.Instance = TIM6;
  htim6.Init.Prescaler = 71;
  htim6.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim6.Init.Period = 1000;
  htim6.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim6) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim6, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
}
```

其中的HAL_TIM_Base_Init(&htim6)最终会调用同文件下的

```cpp
void HAL_TIM_Base_MspInit(TIM_HandleTypeDef* tim_baseHandle)
{
  if(tim_baseHandle->Instance==TIM6){
    /* TIM6 clock enable */
    __HAL_RCC_TIM6_CLK_ENABLE();

    /* TIM6 interrupt Init */
    HAL_NVIC_SetPriority(TIM6_IRQn, 0, 0);
    HAL_NVIC_EnableIRQ(TIM6_IRQn);
  }
}
```
完成中断配置和使能

**main.c文件内容**

```cpp
/* 中断回调函数 */
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
	timer_count++;
}

/* 主函数 */
int main(void)
{
  /* 在中断模式下启动定时器 */
  HAL_TIM_Base_Start_IT(&htim6); 
  while (1)
  {
    /* USER CODE END WHILE */
	  if(timer_count == 1000){  // 没1s反转LED
		  timer_count = 0; // 计数清零
		  HAL_GPIO_TogglePin(LED0_GPIO_Port, LED0_Pin);

	  }
    /* USER CODE BEGIN 3 */
  }
}
```

### 第6章 TIM-高级定时器


### 单脉冲模式






 
















 










