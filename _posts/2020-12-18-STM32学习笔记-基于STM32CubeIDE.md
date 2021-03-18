---
layout: post
show_title: "STM32学习笔记-基于STM32CubeIDE"
title: "2020-12-18-STM32学习笔记-基于STM32CubeIDE"
date: 2020-12-18
last_modified_at: 2020-12-18
categories: arm
---

<!--more-->

[HAL库学习 GPIO---1-知乎](https://zhuanlan.zhihu.com/p/231696942)
[【STM32】HAL库 STM32CubeMX系列学习教程-csdn](https://blog.csdn.net/as480133937/article/details/99935090)
[STM32CubeMX学习使用（标准库与Cube,LL,直接写寄存器的效率对比）](https://blog.csdn.net/super828/article/details/79078693?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161559969216780357212490%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=161559969216780357212490&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v1~rank_blog_v1-2-79078693.pc_v1_rank_blog_v1&utm_term=LL%E5%BA%93)
[STM32 HAL库学习（一）：点亮led](https://blog.csdn.net/qq_42913442/article/details/106938727)
[STM32 HAL库学习系列第3篇 常使用的几种延时方式](https://blog.csdn.net/super828/article/details/79600206?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161559888716780262516364%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=161559888716780262516364&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v1~rank_blog_v1-3-79600206.pc_v1_rank_blog_v1&utm_term=STM32+HAL)
[【STM32】HAL库 STM32CubeMX系列学习教程](http://openedv.com/thread-309468-1-1.html)


## 如何入门STM32


### 购买开发板

### 编程软件

### 库选择

## 资料下载

学习STM32需要提前准备几份文档资料，在接下来的学习和今后实际运用中都会经常用到。  
资料直接从 [ST官网](https://www.st.com/) 下载即可，有的手册有中文。有的中文手册官网没有，可自行网上搜索下载。但都不推荐使用中文的：版本太老、阅读英文文档是程序员必备技能
1. 进入官网 [ST官网](https://www.st.com/) ，选择进入 `微控制器界面`
![enter description here](https://LonlyPan.github.io/images/Posts/2020-12-18-STM32学习笔记-基于STM32CubeIDE/浏览微控制器.png)
2. 在左侧栏找到自己芯片型号，并进入 `Documentation` 界面，选择对应文档下载即可。
![enter description here](https://LonlyPan.github.io/images/Posts/2020-12-18-STM32学习笔记-基于STM32CubeIDE/芯片文档下载.png)
3. 下载以下文档：
   - 数据手册：`Arm® Cortex®-M4 32-bit MCU+FPU, 105 DMIPS, 256KB Flash / 64KB RAM, 11 TIMs, 1 ADC, 11 comm. interfacesV11.0` 芯片本身的手册，相当于产品说明书。
   - 参考手册：`STM32F401xB/C and STM32F401xD/E advanced Arm®-based 32-bit MCUs` ，芯片使用参考手册
   - 编程手册：`STM32 Cortex®-M4 MCUs and MPUs programming manual` ，芯片内核的编程手册（高阶）
4. HAL和LL库参考手册我们需要另外在官网搜索才能找到，`Description of STM32F4 HAL and low-layer drivers`，库函数API驱动描述手册：
![enter description here](https://LonlyPan.github.io/images/Posts/2020-12-18-STM32学习笔记-基于STM32CubeIDE/库参考手册.png)

### 
## STM32CubeIDE简介
>产品链接
[STM32CubeIDE官网](https://www.stmicroelectronics.com.cn/en/development-tools/stm32cubeide.html)

![Stm32CubeIDE_show](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/Stm32CubeIDE_show.png)

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

![Stm32CubeIDE](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/STM32CubeIDE.png)

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

### 3. 可选配置

#### 1. 汉化

如下图打开安装软件界面：
![汉化](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/汉化.gif)

进入以下网址：
http://mirrors.ustc.edu.cn/eclipse/technology/babel/update-site/

选择 `R0.18.2`，并单击进入
![enter description here](https://LonlyPan.github.io/images/Posts/2020-12-18-STM32学习笔记-基于STM32CubeIDE/1.png)
选择如选择:`2020-12`，并单击进入
![enter description here](https://LonlyPan.github.io/images/Posts/2020-12-18-STM32学习笔记-基于STM32CubeIDE/2.png)
复制此时网址链接
![enter description here](https://LonlyPan.github.io/images/Posts/2020-12-18-STM32学习笔记-基于STM32CubeIDE/3.png)

将以下内容填入 `Add Repository` 对话框。其中的位置就是上面得到的链接。

``` cpp
language

http://mirrors.ustc.edu.cn/eclipse/technology/babel/update-site/R0.18.2/2020-12/
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

#### 2. 更换主题

#### 3. 中文注释字体显示问题解决

当我们尝试中文注释和英文注释混编的时候，会出现中文注释突然变小的问题
![enter description here](https://LonlyPan.github.io/images/Posts/2020-12-18-STM32学习笔记-基于STM32CubeIDE/1616038037498.png)

**解决办法：** 菜单栏 `Windows`-> `preference`，进入以后如下图设置
![enter description here](https://LonlyPan.github.io/images/Posts/2020-12-18-STM32学习笔记-基于STM32CubeIDE/test-font.png)

**参考链接**
- [STM32CubeIDE注释字体问题](http://www.openedv.com/thread-300647-1-1.html)
- [STM32的开发环境cubeIDE注释混乱问题解决方法](https://blog.csdn.net/weixin_39754256/article/details/104304634)

## 新建工程模板

这里以控制 GPIO 输出/输出为例说明

### 1.1 新建工程

新建工程有两种途径。如下图示：
>**注意**
软件的工作空间是会和项目文件绑定的，所以如果工程没有从Stm32CubeIDE中删除（不是真的删除，相当于卸载工程），期间在磁盘中移动工程文件是会出错的。所以当你想要移动工程时，一定要先卸载工程。
1. 第一种：界面左上角 `file` -> `New` -> STM32 Project`
![file新建工程](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/file新建工程.png)


2. 第二种：默认启动界面，在这里直接单击开始新工程。如果看不到下图界面，单击上图中 `红色五角星` 上方的 “感叹号” 图标，就会出现了。
![enter description here](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/启动界面新建工程.png)

之后会弹出如下的 芯片选择界面。  
这里有很多种查找芯片的方法，我们这里直接在搜索框 （**1** 处）里搜索，在右下结果框里（**2**处）选中我们要查找的芯片即可。注意其最左边的五角星 <i class="far fa-star"></i>，单击收藏，则会变成蓝色。下次我们可以直接单击左上角搜索框上方的大五角星（ **3** 处），就能够快速查看已收藏的芯片，方便快速。  
选好后，我们单击 `下一步`。
![芯片选择2](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/芯片选择2.png)
	
**1** 处输入工程名称  
**2**处 是工程存储地址，可以自定义 ，但要注意你需要为工程文件再单独新建一个文件夹，不然所有文件都会平铺在当前文件夹中。  
**3** 处是编程语言，这里选择使用C++。之后单击 `下一步`
![新建工程3](https://LonlyPan.github.io/images/Posts/2020-05-08-STM32CubeIDE软件教程/新建工程3.png)

这里是关于库文件的选项，第一个是将库文件链接到安装目录下，这样工程目录下其实是没有库文件了，如果换电脑目录改变，则需要重新链接，不推荐。第二个是复制库文件到工程目录下。建议选择第二个。
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
在图左图形界面单击我们用到的引脚。这里我们使用的LED引脚有 PD2 和 PC8，都是低电平点亮，具体可查看开发板原理图。两个引脚我们都配置为输出模式。
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


## 新建文件-文件夹

由于使用 Stm32CubeIDE 会自动生成配置初始化文件。为了将配置文件和自己的工程文件区分、避免相互影响，我们需要单独建立一个文件夹，存放我们自己的代码。 

这里有两种方法，一个是使用 STM32CubeIDE 重新建立文件和文件夹（适用从零开始的工程）。另一种是从外部导入文件和文件夹（适用工程迁移）。

### 新建文件和文件夹

右键工程名，鼠标移至 `NEW`上，在右侧弹窗中单击 `Sorce Folder`新建文件夹。在这上面还有类似的 `Folder`选项，至于两者不同点，还请自行尝试。
![enter description here](https://LonlyPan.github.io/images/Posts/2020-12-18-STM32学习笔记-基于STM32CubeIDE/新建文件夹.png)
随后新建源文件或头文件，和上图类似，不过这次是在刚刚新建的`文件夹名`上右键，选择`NEW`，并在右侧弹窗中单击 `Sorce Files`或者`Header File` 新建源文件和头文件。

之后就可以在新文件中编写代码了。注意这种方式新建文件夹，不需要再设置头文件包含，软件自动设置好了，我们只管新建文件和编写就好了。

#### 新建文件

右键工程名，鼠标移至 `NEW`上，在右侧弹窗中单击 `Sorce Files`或者`Header File` 新建源文件和头文件。
![enter description here](https://LonlyPan.github.io/images/Posts/2020-12-18-STM32学习笔记-基于STM32CubeIDE/新建文件.png)

![enter description here](https://LonlyPan.github.io/images/Posts/2020-12-18-STM32学习笔记-基于STM32CubeIDE/头文件包含1.png)

### 导入外部文件-文件夹

- [stm32CubeIDE 在自己工程中添加.c 和.h文件](https://blog.csdn.net/qq_36300069/article/details/103226568)

### 参考链接

1. [STM32CubeIDE资源](https://www.st.com/zh/development-tools/stm32cubeide.html#resource)
2. [STM32CubeIDE属于一站式工具，本文带你体验它的强大](https://blog.csdn.net/ybhuangfugui/article/details/89702356)    
3. [STM32CubeMX系列教程03\_创建并生成代码工程](https://www.strongerhuang.com/STM32Cube/STM32CubeMX/STM32CubeMX%E7%B3%BB%E5%88%97%E6%95%99%E7%A8%8B03_%E5%88%9B%E5%BB%BA%E5%B9%B6%E7%94%9F%E6%88%90%E4%BB%A3%E7%A0%81%E5%B7%A5%E7%A8%8B.html) 
4. [STM32CubeIDE使用笔记（01）：基础说明与开发流程](https://blog.csdn.net/Naisu_kun/article/details/95935283)

## 使用 C++ 编程

1. 新建 C++ 工程
如下图所示，只需要改动一处即可。
![enter description here](https://LonlyPan.github.io/images/Posts/2020-12-18-STM32学习笔记-基于STM32CubeIDE/新建C++工程.png)
2. 添加个人文件夹（参考上文的`工程中添加文件`）
为了避免软件生成的配置文件和我们自定义文件混淆，建议将自己的文件单独放在一个文件夹中。
3. 编写程序，注意.cpp中函数有被.c中函数调用时，需要在.cpp函数的头文中添加 （源文件.cpp中不需要添加）`extern "C" `。
   ```
   #ifndef MY_MAIN_H_
   #define MY_MAIN_H_

   #ifdef __cplusplus
   extern "C" {
   #endif

   void setup();  // 被main.c调用
   void loop();   // 被main.c调用

   #ifdef __cplusplus
   }
   #endif

   #endif /* CPP_TEST_H_ */
  
    ```
4. 包含头文件。
按下图所示，在 `C/C++ build`->`Settings`->`Tool Settings`->`MCU GCC Compiler`->`include paths`包含我们自定义的文件夹路径。  
这一步是C++独有的，非C++文件只需要安装上文中的 `新建个人问价-文件夹`所述即可，无需此步。但C++却需要单独设置添加。

5. 之后就可以自由使用 C++ 了。

## 工程模板文件解读

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


## STM32F030_HAL库学习笔记


### GPIO 操作

### GPIO 寄存器操作

[高手带你解析STM32 BSRR BRR ODR寄存器](http://news.moore.ren/industry/64985.htm)
[STM32duino GPIO Registers and programming](https://gist.github.com/iwalpola/6c36c9573fd322a268ce890a118571ca)
[GPIO Output Registers on the STM32](https://electronics.stackexchange.com/questions/336021/gpio-output-registers-on-the-stm32)
[Would my solution work for 8-bit bus addressing using BSRR and BRR?](https://stackoverflow.com/questions/56822789/would-my-solution-work-for-8-bit-bus-addressing-using-bsrr-and-brr)
[STM32裸机学习笔记（三）—寄存器映射之BSRR与延时的爱恨情仇](https://www.codenong.com/cs106676846/)
[STM32 GPIO 配置之ODR, BSRR, BRR 详解](https://blog.csdn.net/GDNNNNN/article/details/87904592?spm=1001.2014.3001.5501)

### GPIO 位带操作

关于位带操作，网上有很多讲解，这里不再详述。可以参考：`参考手册` 的 GPIO 章节，`编程手册` 的 2.2.5 Bit-banding 章节，自行深入学习。下面只说明如何使用：

```c 
//位带操作,实现51类似的GPIO控制功能
//IO口操作宏定义
// 把“位带地址+位序号”转换成别名地址的宏
#define BITBAND(addr, bitnum) ((addr & 0xF0000000)+0x2000000+((addr &0xFFFFF)<<5)+(bitnum<<2))
// 把一个地址转换成一个指针
#define MEM_ADDR(addr)  *((volatile unsigned long  *)(addr))
// 把位带别名区地址转换成指针
#define BIT_ADDR(addr, bitnum)   MEM_ADDR(BITBAND(addr, bitnum))
//IO口地址映射
#define GPIOA_ODR_Addr    (GPIOA_BASE+20) //
#define GPIOB_ODR_Addr    (GPIOB_BASE+20) //
#define GPIOC_ODR_Addr    (GPIOC_BASE+20) //
#define GPIOD_ODR_Addr    (GPIOD_BASE+20) //
#define GPIOE_ODR_Addr    (GPIOE_BASE+20) //
#define GPIOF_ODR_Addr    (GPIOF_BASE+20) //
#define GPIOG_ODR_Addr    (GPIOG_BASE+20) //

#define GPIOA_IDR_Addr    (GPIOA_BASE+16) //
#define GPIOB_IDR_Addr    (GPIOB_BASE+16) //
#define GPIOC_IDR_Addr    (GPIOC_BASE+16) //
#define GPIOD_IDR_Addr    (GPIOD_BASE+16) //
#define GPIOE_IDR_Addr    (GPIOE_BASE+16) //
#define GPIOF_IDR_Addr    (GPIOF_BASE+16) //
#define GPIOG_IDR_Addr    (GPIOG_BASE+16) //

//IO口操作,只对单一的IO口!
//确保n的值小于16!
#define PAout(n)   BIT_ADDR(GPIOA_ODR_Addr,n)  //输出
#define PAin(n)    BIT_ADDR(GPIOA_IDR_Addr,n)  //输入

#define PBout(n)   BIT_ADDR(GPIOB_ODR_Addr,n)  //输出
#define PBin(n)    BIT_ADDR(GPIOB_IDR_Addr,n)  //输入

#define PCout(n)   BIT_ADDR(GPIOC_ODR_Addr,n)  //输出
#define PCin(n)    BIT_ADDR(GPIOC_IDR_Addr,n)  //输入

#define PDout(n)   BIT_ADDR(GPIOD_ODR_Addr,n)  //输出
#define PDin(n)    BIT_ADDR(GPIOD_IDR_Addr,n)  //输入

#define PEout(n)   BIT_ADDR(GPIOE_ODR_Addr,n)  //输出
#define PEin(n)    BIT_ADDR(GPIOE_IDR_Addr,n)  //输入

#define PFout(n)   BIT_ADDR(GPIOF_ODR_Addr,n)  //输出
#define PFin(n)    BIT_ADDR(GPIOF_IDR_Addr,n)  //输入

#define PGout(n)   BIT_ADDR(GPIOG_ODR_Addr,n)  //输出
#define PGin(n)    BIT_ADDR(GPIOG_IDR_Addr,n)  //输入
```

**示例**
```
/* 操作相应I/O前，必须先配置初始化，才能使用位带操作 */
PGout(10)= 1;  // PG10 输出高电平
uint8_t io_status = PGin(9); // 读取PG9的高低电平状态
```
上述代码中：
- `#define BITBAND` 后的内容，不同内核可能需要另外修改（M3和M4内核已经验证，可通用）。 
- 数字 `20`和`16`是寄存器  ODR 和 IDR 的地址偏移，不同芯片也需要做出相应修改，具体查看 `参考手册` 的 GPIO 章节的 GPIOx_IDR、GPIOx_ODR寄存器描述
![enter description here](https://LonlyPan.github.io/images/Posts/2020-12-18-STM32学习笔记-基于STM32CubeIDE/GIPIO_adress_offset.png)
图中红框部分 0x10 、0x14 转换成十进制就是 16 和 20。例如 STM32F103 系列的是  0x08、0x0C，那这里就需要改为 12 和 8。

参考链接：
- [知乎-STM32位带操作全解](https://zhuanlan.zhihu.com/p/142586194)
- [CSDN-快速理解STM32位带操作原理和用途](https://blog.csdn.net/ybhuangfugui/article/details/108067563)
- [cnblogs-第13章 GPIO-位带操作—零死角玩转STM32-F429系列](https://www.cnblogs.com/firege/p/5748713.html)


### us延时


## STM32F030_LL库学习笔记

[STM32LL库系列教程（一）—— LL库概览及资料](https://zhuanlan.zhihu.com/p/347459515)
[【stm32cubemx专题教程】ST全外设原理、配置、API使用详解](https://www.bilibili.com/video/BV1Tv411B7Uw)
[STM32 之十一 LL 库（low-layer drivers）详解 及 移植说明](https://blog.csdn.net/ZCShouCSDN/article/details/104174662)
标准库官方已经不更新了，虽然资料很多，所以不再使用。之后学习使用了HAL库，但最近做项目需要使用16和32KB的STM32F0芯片，使用HAL库新建个工程再加上串口，基本就是10KB+了，所以也是被迫重新选择了LL库。

下面是别人做的一个不同编程方式的效率对比：
原文链接：[https://blog.csdn.net/super828/article/details/79078693](https://blog.csdn.net/super828/article/details/79078693)

<!--more-->

![enter description here](https://LonlyPan.github.io/images/Posts/2019-9-12-STM32F030_LL库学习笔记/标准库与Cube,LL,直接写寄存器的效率对比.png)

总的来说：代码效率与移植性成反比的规律是明显的。与HAL相比，LL的效率优势很明显，几乎和直接写寄存器的效率相差无几。而且目前STM32cubeIDE已经支持直接生成LL工程，对于追求效率的开发应用人员来说，非常值得推荐大家使用。


### GPIO操作

配置操作基本和前面的 HAL 一样，只有一处不同，驱动库选择 LL 库，如图所示：
![enter description here](https://LonlyPan.github.io/images/Posts/2020-12-18-STM32学习笔记-基于STM32CubeIDE/advanced_settings.png)

示例：
```
LL_GPIO_SetOutputPin(GPIOB, LL_GPIO_PIN_3); // PB3输出高电平
LL_GPIO_ResetOutputPin(GPIOB, LL_GPIO_PIN_3); // PB3输出低电平
LL_GPIO_TogglePin(GPIOB, LL_GPIO_PIN_3);    /* 翻转PB3输出电平
LL_GPIO_ReadInputPort(GPIO_TypeDef \*GPIOx);  /* 读取引脚电平状态 */

LL_mDelay(500); // ms延时，延时500ms
```
API详细使用请参考官方驱动描述手册：`Description of STM32F4 HAL and low-layer drivers`
![enter description here](https://LonlyPan.github.io/images/Posts/2020-12-18-STM32学习笔记-基于STM32CubeIDE/库参考手册.png)


### 2.1 GPIO双向 I/O
[STM32 MCU GPIO双向口使用的话题](http://www.360doc.com/content/17/1208/13/8706683_711243855.shtml)
[stm32的双向io口](https://blog.csdn.net/weixin_30443813/article/details/96729719)
[STM32 IO口双向问题](https://my.oschina.net/hoolev/blog/525208)
[STM32 GPIO八种输入输出模式的功能及区别](https://blog.csdn.net/weixin_41072132/article/details/103264249)
[STM32的8种GPIO输入输出模式深入详解](https://blog.csdn.net/baidu_37366055/article/details/80060962)

## USART

不论是重定义和自定义printf函数，若想打印float类型，都需要再IDE中单独开启，否则无法打印，且额外占用内存 18 KB 左右。

http://begild.top/article/a854db16.html

### printf重定义

```c++
/* uart.c */
#ifdef __GNUC__
#define PUTCHAR_PROTOTYPE int __io_putchar(int ch) /* 防⽌重定义， 具体为什么会⽤到GNUC我以
为不知道*/
#else
#define PUTCHAR_PROTOTYPE int fputc(int ch, FILE *f)
#endif
PUTCHAR_PROTOTYPE{
    //LL_USART_TransmitData8(USART1,Usart1_TxBuff[i]);
    USART1->TDR = ch;
    //while(!LL_USART_IsActiveFlag_TXE(USART1));
    while((USART1->ISR&0X40)==0);
return ch;
}
/* main.c*/
printf("233\n"); /* 串⼝打印数据 */

### printf自己编写

/* uart.c */
#include "stdio.h"      //
#include "stdarg.h"        //
#include "string.h"     //
char Usart1_TxBuff[256];
void my_printf(char* fmt,...)
{
    unsigned int i,length;
    va_list ap;
    va_start(ap,fmt);
    vsprintf(Usart1_TxBuff,fmt,ap);
    va_end(ap);
    length=strlen((const char*)Usart1_TxBuff);
    while(!LL_USART_IsActiveFlag_TXE(USART1));
    //while((USART1->ISR&0X40)==0);
    for(i = 0; i < length; i ++)
    {
        
        LL_USART_TransmitData8(USART1,Usart1_TxBuff[i]);
        //USART1->TDR = Usart1_TxBuff[i];
        while(!LL_USART_IsActiveFlag_TXE(USART1));
        //while((USART1->ISR&0X40)==0);
    }
}
/* main.c*/
my_printf("233\n"); /* 串⼝打印数据 */
```
>备注：
1、知识点：va_list
3、自己编写 printf 函数比重定义节省 0.46 KB，但RAM增加了。

其他发送方法

1、只发送字符串数据

```c++
/* uart.c中定义 */
void USART_Print(unsigned char *Send_Text,uint32_t Size_Text)
{
  uint32_t index = 0;
 
  for (index = 0; index < Size_Text; index++)
  {
    while (!LL_USART_IsActiveFlag_TXE(USART1));
     LL_USART_TransmitData8(USART1,Send_Text[index]);
  }
  while (!LL_USART_IsActiveFlag_TC(USART1));
}
/* 使用 */
USART_Print("Ready for Tx\r\n",(uint32_t) sizeof("Ready for Tx\r\n") );
一般接受中断

/* stm32f0xx_it.c声明中断函数 */
void USART1_IRQHandler(void)
{
  /* USER CODE BEGIN USART1_IRQn 0 */
    USART_RxIdleCallback();
  /* USER CODE END USART1_IRQn 0 */
  /* USER CODE BEGIN USART1_IRQn 1 */
  /* USER CODE END USART1_IRQn 1 */
}
/* uart.c中定义 */
void USART_RxIdleCallback(void)
{
    uint8_t tmp;
    if(LL_USART_IsActiveFlag_RXNE(USART1)) //接收中断
    {
        tmp=LL_USART_ReceiveData8(USART1);   //读取出来接收到的数据
        LL_USART_TransmitData8(USART1,tmp);  //把数据再从串口发送出去
    }
}
/* uart.h声明 */
void USART_RxIdleCallback(void);
/* 使用 */
// main.c初始化使能接受中断
LL_USART_EnableIT_RXNE(USART1);
DMA接受中断


/* uart.c中定义 */
void USART_DMA_CONFIG(void)
{
    LL_DMA_SetPeriphAddress(DMA1, LL_DMA_CHANNEL_5, (uint32_t)(&USART1->RDR));// LL_USART_DMA_GetRegAddr(USART1->DR));
    LL_DMA_SetMemoryAddress(DMA1, LL_DMA_CHANNEL_5, (uint32_t)Usart1_RxBuff);
    LL_DMA_SetDataLength(DMA1, LL_DMA_CHANNEL_5, 255);
    LL_DMA_EnableIT_TC(DMA1, LL_DMA_CHANNEL_5);
    LL_DMA_EnableChannel(DMA1, LL_DMA_CHANNEL_5);
    LL_USART_EnableDMAReq_RX(USART1);
    LL_USART_EnableIT_IDLE(USART1);
}
void USART_RxIdleCallback(void)
{
    uint8_t cnt;
    if(LL_USART_IsActiveFlag_IDLE(USART1))
    {
        LL_DMA_DisableChannel(DMA1, LL_DMA_CHANNEL_5); //
        cnt = LL_DMA_GetDataLength(DMA1,LL_DMA_CHANNEL_5);
        u1_printf("data len is:%d\r\n",cnt);
        u1_printf("data rx is:%s\r\n",Usart1_RxBuff);
        LL_DMA_SetDataLength(DMA1, LL_DMA_CHANNEL_5, 255); 
        LL_DMA_EnableChannel(DMA1, LL_DMA_CHANNEL_5);
        LL_USART_ClearFlag_IDLE(USART1);
    }
}
/* uart.h声明 */
void USART_RxIdleCallback(void);
/* 使用 */
// main.c初始化DMA
USART_DMA_CONFIG();
```
>备注：
不可使用DMA3，会出现 "VDD VALUE" redefined 错误。
只要配置了DMA，延时函数LL_mDelay()失效

总结

## ADC

```c++
HAL_ADCEx_Calibration_Start(&hadc);
HAL_ADC_Start(&hadc);
uint16_t ADC_temp1=0;
HAL_ADC_PollForConversion(&hadc,10);
if(HAL_IS_BIT_SET(HAL_ADC_GetState(&hadc),HAL_ADC_STATE_REG_EOC))
    ADC_temp1=HAL_ADC_GetValue(&hadc);//0-4095
return ADC_temp1;
float updateTemperaturesFromRawValues(void)
{
static unsigned char temp_count = 0;
static unsigned long raw_temp_0_value = 0;

  temp_count++;
  raw_temp_0_value +=(ADC_Demo2()>>2);
  if(temp_count >= 16) // per 16 times 锛宑aculate once temp
  {
      temp_count = 0;
      current_temperature = analogtemp(raw_temp_0_value);
  }
  return current_temperature;
}
```


### 参考链接

https://zhuanlan.zhihu.com/p/133874308







 
















 










