---
layout: post
title: "STM32CubeIDE学习笔记"
index_img: https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/stm32-cube-ide.png
date: 2020-12-18
updated: 2020-12-18
hide: false
# sticky: 100 #置顶，数字越大越靠前
# banner_img: #/img/post_banner.jpg
# comment: false
categories: 01-专业
---

<!--more-->

**开发环境：**
- win10
- IDE：STM32CubeIDE

# STM32CubeIDE简介
>产品链接。建议多看看官网的资料，很多问题都能在这些文档里找到答案。
[STM32CubeIDE官网](https://www.stmicroelectronics.com.cn/en/development-tools/stm32cubeide.html)
[STM32CubeIDE user guide](https://www.st.com/content/ccc/resource/technical/document/user_manual/group1/f8/a2/48/77/68/e6/4b/74/DM00629856/files/DM00629856.pdf/jcr:content/translations/en.DM00629856.pdf)

![Stm32CubeIDE_show](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/Stm32CubeIDE_show.png)

## 1. 特性

- 集成STM32CubeMX，提供以下服务：
   - STM32微控制器选择 
   - 引脚分配，时钟，IP和中间件配置 
   - 项目创建和初始化代码的生成

- 基于Eclipse?/ CDT，支撑ECLIPSE的?插件，GNU C / C ++中ARM ?工具链和GDB调试器。

- 其他高级调试功能包括：**
  - CPU内核，IP寄存器和内存视图
  - 实时变量观看视图
  - 系统分析和实时跟踪（SWV）
  - CPU故障分析工具
  
- 支持ST-LINK（STMicroelectronics）和J-Link（SEGGER）调试探针
- 从Atollic导入项目? TrueSTUDIO ?和AC6系统工作台的STM32
- 多支持操作系统：Windows ?，Linux的?和MacOS ?

## 2. 概述

[STM32CubeIDE](https://www.stmicroelectronics.com.cn/en/development-tools/stm32cubeide.html)是一款多功能的多操作系统开发工具，集成了TrueSTUDIO和STM32CubeMX，它是[STM32Cube](https://www.st.com/content/st_com/en/stm32cube-ecosystem.html)软件生态系统的一部分。  

简单来说就是：**STM32cubeIDE = true studio for stm32 + STM32cubeMX**

![Stm32CubeIDE](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/STM32CubeIDE.png)

[STM32CubeIDE](https://www.stmicroelectronics.com.cn/en/development-tools/stm32cubeide.html)是一个高级C / C ++开发平台，具有用于STM32微控制器和微处理器的外设配置，代码生成，代码编译和调试功能。它基于ECLIPSE?/ CDT框架和GCCtoolchain用于开发，而GDB用于调试。它允许集成数百个现有插件，这些插件完成ECLIPSE?IDE的功能

[STM32CubeIDE](https://www.stmicroelectronics.com.cn/en/development-tools/stm32cubeide.html)集成了所有[STM32CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html)功能，以提供多合一的工具体验，并节省安装和开发时间。从板上选择空的STM32 MCU、MPU或预配置的微控制器或微处理器后，将创建项目并生成初始化代码。在开发期间的任何时候，用户都可以返回外围设备或中间件的初始化和配置，并重新生成初始化代码，而不会影响用户代码。

[STM32CubeIDE](https://www.stmicroelectronics.com.cn/en/development-tools/stm32cubeide.html)包括构建和堆栈分析器，可为用户提供有关项目状态和内存要求的有用信息。

[STM32CubeIDE](https://www.stmicroelectronics.com.cn/en/development-tools/stm32cubeide.html)还包括标准和高级调试功能，包括CPU内核寄存器，存储器和外设寄存器的视图，以及实时变量监视，串行线查看器（Serial Wire Viewer）接口或故障分析器。

[STM32CubeIDE](https://www.stmicroelectronics.com.cn/en/development-tools/stm32cubeide.html)支持基于Arm?Cortex?处理器的STM32产品

## 3. STM32Cube介绍

[STM32Cube](https://www.st.com/content/st_com/en/stm32cube-ecosystem.html)是意法半导体的一项原始计划，旨在通过减少开发工作量，时间和成本来显着提高设计师的生产率。 [STM32Cube](https://www.st.com/content/st_com/en/stm32cube-ecosystem.html)涵盖了整个STM32产品组合。

**[STM32Cube](https://www.st.com/content/st_com/en/stm32cube-ecosystem.html)是软件工具和嵌入式软件库的结合**：
- 一套完整的PC软件工具，可满足一个完整项目开发周期的所有需求
= 在STM32微控制器和微处理器上运行的嵌入式软件模块，可带来各种功能（从MCU组件驱动程序到更高级的面向应用的特性)

**[STM32Cube](https://www.st.com/content/st_com/en/stm32cube-ecosystem.html)生态系统包括**
![overview-zh](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/overview-zh.jpg)

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

# 下载安装

## 1. 下载

可以从[STM32CubeIDE](https://www.st.com/en/development-tools/stm32cubeide.html) 网站上下载最新版本的安装程序。

在页面底部找到下图，根据自己电脑操作系统下载即可，这里以Windows版为例。  右边下拉菜单还可以选择其他版本，推荐下载最新版。
软件是免费的， 但下载时需要填写信息或注册登录。  

![download](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/download.png)

## 2. 安装

前面几步较简单，这里跳过。

**选择安装位置**
- 安装路径中不要包含中文字符，不要包含中文字符，不要包含中文字符
- 建议选择短路径以避免工作区路径过长而面临Windows?限制。
  
![Installer_location_dialog](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/Installer_location_dialog.png)

**“选择组件”对话框**
选择要与STM32CubeIDE一起安装的GDB服务器组件。用于STM32CubeIDE安装调试的每种JTAG探针类型都需要一个服务

![Selection_of_components_dialog_](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/Selection_of_components_dialog_.png)

## 3. 可选配置

### 1. 汉化

如下图打开安装软件界面：
![汉化](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/汉化.gif)

进入以下网址：
http://mirrors.ustc.edu.cn/eclipse/technology/babel/update-site/

选择 `R0.18.2`，并单击进入
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/eclipse汉化下载1.png)
选择如选择:`2020-12`，并单击进入
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/eclipse汉化下载2.png)
复制此时网址链接
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/eclipse汉化下载3.png)

将以下内容填入 `Add Repository` 对话框。其中的位置就是上面得到的链接。

``` cpp
language

http://mirrors.ustc.edu.cn/eclipse/technology/babel/update-site/R0.18.2/2020-12/
```
![Add_Repository_12](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/Add_Repository_12.png)

单击 `添加` 按钮。之后会弹出下面对话框，下拉找到图中红框选项，
![汉化选择_56](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/汉化选择_56.png)

单击其最左侧的多选 `>` 按钮，在下拉框中选中打勾下图红框部分，之后单击 `下一步`。
![汉化选择2_135](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/汉化选择2_135.png)

>注：   
上图红框选项指软件的界面汉化，其中后面的（85.21%）表示汉化程度，可见还并未完全汉化。
其他选项也是汉化选项，是关于库，编程等的，安装会导致程序BUG，故不推荐安装。这里只对界面进行汉化。

单击 `下一步` 

![汉化选择3_220](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/汉化选择3_220.png)

勾选接受许可协议，点击 `完成`

![汉化选择4_281](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/汉化选择4_281.png)

之后会弹出下面的对话框，单击 install anyway

![汉化选择5_336](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/汉化选择5_336.png)

单击现在重启，汉化完成

![汉化选择6_396](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/汉化选择6_396.png)

### 2. 更换主题

软件自带了几个主题，具体可以点击菜单栏 `Windows`-> `preference`，搜索 `theme`并尝试。但官方主题不太好用，所以下面讲解如何使用外部主题。  
菜单栏 `Help`，在下拉栏中双击 `Eclipse Marketplace`打开。  
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/install_theme.png)
之后按照软件提示，一路默认安装即可。最后重启软件，选择主题。  
如果后面还想更改，点击菜单栏 `Windows`-> `preference`，搜索 `theme`并尝试。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/change_theme.png)

### 3. 中文注释字体显示问题解决

当我们尝试中文注释和英文注释混编的时候，会出现中文注释突然变小的问题
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/1616038037498.png)

**解决办法：** 菜单栏 `Windows`-> `preference`
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/test-font.png)

**参考链接**
- [STM32CubeIDE注释字体问题](http://www.openedv.com/thread-300647-1-1.html)
- [STM32的开发环境cubeIDE注释混乱问题解决方法](https://blog.csdn.net/weixin_39754256/article/details/104304634)

# 新建工程模板

这里以控制 GPIO 输出/输出为例说明

## 1.1 新建工程

新建工程有两种途径。如下图示：


1. 第一种：界面左上角 `file` -> `New` -> STM32 Project`
![file新建工程](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/file新建工程.png)


2. 第二种：默认启动界面，在这里直接单击开始新工程。如果看不到下图界面，单击上图中 `红色五角星` 上方的 “感叹号” 图标，就会出现了。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/启动界面新建工程.png)

之后会弹出如下的 芯片选择界面。  
这里有很多种查找芯片的方法，我们这里直接在搜索框 （**1** 处）里搜索，在右下结果框里（**2**处）选中我们要查找的芯片即可。注意其最左边的五角星 <i class="far fa-star"></i>，单击收藏，则会变成蓝色。下次我们可以直接单击左上角搜索框上方的大五角星（ **3** 处），就能够快速查看已收藏的芯片，方便快速。  
选好后，我们单击 `下一步`。
![芯片选择2](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/芯片选择2.png)
	
**1** 处输入工程名称  
**2**处 是工程存储地址，可以自定义 ，但要注意你需要为工程文件再单独新建一个文件夹，不然所有文件都会平铺在当前文件夹中。  
**3** 处是编程语言，这里选择使用C++。之后单击 `下一步`
![新建工程3](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/新建工程3.png)

这里是关于库文件的选项，第一个是将库文件链接到安装目录下，这样工程目录下其实是没有库文件了，如果换电脑目录改变，则需要重新链接，不推荐。第二个是复制库文件到工程目录下。建议选择第二个。
![新建工程4](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/新建工程4.png)

接下来就是熟悉的 CubeMX 初始化配置界面了，操作方法也没有什么难的，就是将我们之前用代码写的初始化变成了图形化操作，按照我们初始化的思路一步步勾选即可了。下面具体操作。

## 1.2 引脚与配置(Pinout  and Configuration)

1. 系统配置。这里是调试工具选择和基准时钟源选择。我们用的是ST-Link的SWD（Serial Wire Debug）调试模式，所以选择 **Serial Wire** 串行线。时钟基准源默认系统滴答时钟 **SysTick**。
![SYS](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/SYS.png)

2. 时钟源配置。一共有以下选项。
- 外部晶体/陶瓷晶振(Crystal/Ceramic Resonator)：由外部无源晶体与MCU内部时钟驱动电路共同配合形成，有一定的启动时间，精度较高。外部晶振没有时，自动切换为自带的内部晶振。
- 旁路时钟源(Crystal/Ceramic Resonator)：直接从外界导入时钟信号。犹如芯片内部的驱动组件被旁路了
- Disable：无时钟，相关功能不工作。

  这里HSE配置为外部晶振，LSE选Disable(低速时钟是给看门狗和RTC的，目前未用)。时钟输出关闭。
![RCC](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/RCC.png)

3. 引脚配置。
在图左图形界面单击我们用到的引脚。这里我们使用的LED引脚有 PD2 和 PC8，都是低电平点亮，具体可查看开发板原理图。两个引脚我们都配置为输出模式。
![引脚选择3](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/引脚选择3.png)

    然后我们到左边的引脚具体配置。配置如图示，两个引脚相同。
![引脚配置2](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/引脚配置2.png)

## 1.3 时钟配置(Clock Configuration)

选择外部8MHz晶振，配置系统时钟为72MHz。注意APB1总线时钟式36MHz，需二分频。
这里和后面的配置都不需要保存的，也没有保存按钮<i class="fas fa-smile"></i>
![时钟选择](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/时钟选择.png)

## 1.4 项目管理(Project Manager)

这里我们注意勾选上图中红框部分。这样生成的代码每个外设一个文件夹，就不会全堆在main.c文件里了。其他的默认。
![项目管理](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/项目管理.png)

## 1.5 工具(Tools)

保持默认即可

## 1.6 编译生成初始化文件

单击上方工具栏的  `设备配置工具代码生成` ，完成工程建立。观察左侧的项目资源管理器，会发现多出了gpio.c文件等等。现在就可以正式编写程序了。
![初始化生成](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/初始化生成.png)

我们可以单击其左边的锤子 ? 编译 按钮，编译应该是没有错误的。
![编译](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/编译.png)

## 1.7 工程复制

开发过程中有时候需要复制一个已有的项目，在该项目之上进行开发，STM32CubeIDE不允许同一个工作空间中出现同名项目，这时候就需要修改项目名称了，方法如下：

### 方案一
直接在 IDE 左侧的文件浏览框中，右键项目名复制。

1. 右键项目名，选择 `Copy` 复制
2. 右键空白处，选择 `Paste` 粘贴工程，重命名项目名，并选择存放位置
3. 右键新项目的 .ioc 文件，重命名，和新项目名一样。
4. 删除新项目下原来项目命名的 .launch和.cfg 文件。
5. **在资源管理器中，删除新工程下的 .mxproject 文件!删除新工程下的 .mxproject 文件!删除新工程下的 .mxproject 文件!**
6. 现在可以自由编辑新工程了

### 方案二

在资源管理器中复制粘贴项目

1. 复制已有工程，并重命名
2. **在资源管理器中，删除新工程下的 .mxproject 文件!删除新工程下的 .mxproject 文件!删除新工程下的 .mxproject 文件!**
3. 双击新项目名并重新打开，在IDE中，右键项目名，选择 `Rename` 重命名
4. 删除新项目下原来项目命名的 .launch和.cfg 文件。
5. 现在可以自由编辑新工程了


## 1.8 新建文件夹和文件

由于使用 Stm32CubeIDE 会自动生成配置初始化文件。为了将配置文件和自己的工程文件区分、避免相互影响，我们需要单独建立一个文件夹，存放我们自己的代码。 
>之后所有的参考程序都会存放在我们新建的 `BSP` 文件夹下。

这里有两种方法，一个是使用 STM32CubeIDE 重新建立文件和文件夹（适用从零开始的工程）。另一种是从外部导入文件和文件夹（适用工程迁移）。

### 新建文件和文件夹

右键工程名，鼠标移至 `NEW`上，在右侧弹窗中单击 `Sorce Folder`新建文件夹。在这上面还有类似的 `Folder`选项，至于两者不同点，还请自行尝试。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/新建文件夹.png)
随后新建源文件或头文件，和上图类似，不过这次是在刚刚新建的`文件夹名`上右键，选择`NEW`，并在右侧弹窗中单击 `Sorce Files`或者`Header File` 新建源文件和头文件。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/新建文件.png)
头文件包含，新建文件夹后，需要在编译器中另外包含文件夹地址，否则编译会提示找不到文件。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/头文件包含1.png)


### 导入外部文件-文件夹

- [stm32CubeIDE 在自己工程中添加.c 和.h文件](https://blog.csdn.net/qq_36300069/article/details/103226568)

# 使用 C++ 编程

1. 新建 C++ 工程
如下图所示，只需要改动一处即可。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/新建C++工程.png)
2. 添加个人文件夹（参考上文的`工程中添加文件`）
为了避免软件生成的配置文件和我们自定义文件混淆，建议将自己的文件单独放在一个文件夹中。
另外还需要在G++中包含头文件夹，`Properties`->`C/C++ Build`->`Tool Settings`：
* -> `MCU G++ Compiler` -> `include paths` 和 
* -> `MCU GCC Compiler` -> `include paths`
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
4. 之后就可以自由使用 C++ 了。

**参考链接**

1. [STM32CubeIDE资源](https://www.st.com/zh/development-tools/stm32cubeide.html#resource)
2. [STM32CubeIDE属于一站式工具，本文带你体验它的强大](https://blog.csdn.net/ybhuangfugui/article/details/89702356)    
3. [STM32CubeMX系列教程03\_创建并生成代码工程](https://www.strongerhuang.com/STM32Cube/STM32CubeMX/STM32CubeMX%E7%B3%BB%E5%88%97%E6%95%99%E7%A8%8B03_%E5%88%9B%E5%BB%BA%E5%B9%B6%E7%94%9F%E6%88%90%E4%BB%A3%E7%A0%81%E5%B7%A5%E7%A8%8B.html) 
4. [STM32CubeIDE使用笔记（01）：基础说明与开发流程](https://blog.csdn.net/Naisu_kun/article/details/95935283)

# 工程模板文件解读

## 1.1.7 文件夹结构

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

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/图像_5.png)

main.c：存放main函数和SystemClock_Config函数

stm32f1xx_assert.c：

stm32f1xx_hal_msp.c：其中的 `HAL_MspInit()` 是芯片系统级初始化，一般实现系统的中断配置，比如硬件错误、微处理器故障、总线错误等。其在 HAL_Msplnit函数中被调配，而main函数起始就调用 HAL_Init 函数。

stm32f1xx_it.c：存放中断服务函数

stm32f1xx_hal_conf.h：见上面表格

stm32f1xx_it.h：中断服务函数声明，一般很少改动


# 使用问题合集

## 连接多个st-link 调试Multiple debuggers

当一台电脑同时连接多个st-link是，cubeide会提示仿真下载错误，这是需要在仿真设置里额外配置
> 一个stm32cubeide软件只能一个工程在debug模式
> 如果要同时调试两台设备，就需要打开两个cubeide软件且不在同一工作空间（Workspace）

进入Debug Configurations
1. 使能 Shared ST-Link 选项，否则后面的无法使用 `寻找` 功能即找不到st-link
2. 勾选 使用特定的ST-Link序列号，意味着我们当前工程会仅只用我们待会指定的仿真器（序列号唯一）
3. 点击 寻找，软件会自动列出当前已连接的st-link。建议在寻找之前，断点重连所有的st-link。
4. 选择想要的st-link序列号
5. 端口号码默认61234，但是当我们同时多个仿真器运行时，则必须保证每一个这里的端口号码不一样，随便填。
 ![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/1667203070396.png)
 
## STM32CubeIDE 编译报错“no such file or directory”

修改了项目名称（右键项目名 -> Rename）后，编译报错 “no such file or directory”，但实际报错的文件是存在的，且修改名称之前编译是正确的。  
原因：修改项目名后，软件会自动修改所有相关依赖项和地址路径，但不会修改用户自己添加的文件夹及文件的路径，所以还需要自己再手动修改。  
解决：修改路径包含中的项目名即可，将下图中的项目名改为`detector_A1.1`（原名称为 `detector_A1.0` ）   ，保持和工程的项目名一致。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/修改路径包含.png)

## STM32CubeIDE 文件夹及文件灰色且有一条斜杠

如下图所示，DELAY 文件夹灰色并且文件及文件夹上面都有个斜杠。另外编译时，也提示未定义 `delay_ms` 函数（实际已经在delay.c 文件种 ）
检查了 Inlcude paths  ，DELAY 文件夹已经包含，说明是其它问题导致
![图 2](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/STM32cubeide%20%E6%96%87%E4%BB%B6%E5%A4%B9%E6%96%9C%E6%9D%A0.png)  

**解决方法：**
右键项目名，进入 Properties 配置界面    
依次选择 `c/c++ general` -> `paths and symbols` -> `source location`；找到并选中 DELAY 文件夹的上一级文件夹 BSP。单价 `edit filter`
![图 3](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/STM32cubeide%20%E6%96%87%E4%BB%B6%E5%A4%B9%E6%96%9C%E6%9D%A02.png)  
在弹窗种选中 DELAY 文件夹名，单击 `Remove` 从目录中删除。`OK` 确认返回刚才界面，单击 `Apply and Close` 保存退出。
![图 4](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/STM32cubeide%20%E6%96%87%E4%BB%B6%E5%A4%B9%E6%96%9C%E6%9D%A03.png)  

这里还可以看到 spi1 文件夹也被屏蔽了，这是之前遇到一样的情况，我干脆把这个文件夹删了，并未深究。这里算是搞明白了。

从弹窗的标题 `Source Folder Exclusion Patterns` （源文件夹排除格式）。这里面添加的就是从每个文件夹种被排除的文件夹或文件，其实就是屏蔽文件来加入到工程，这也就是为什么是灰色且有斜杆且编译提示未定义了。

**参考资料**
- [eclipse cdt中.c文件而且灰色有个斜杠是什么意思](https://bbs.csdn.net/topics/390858204)

## STLINK 下载仿真问题-Failure starting GDB server: TCP port 61234 not available.

下载或Debug时，报如下错误
Failed to bind to port 61235, error code -1: No error
Failure starting SWV server on TCP port: 61235
Failed to bind to port 61234, error code -1: No error
Failure starting GDB server: TCP port 61234 not available.
Exit.
使用 STlink Utililty软件下载，可正常连接并下载程序。
查询资料：
https://community.st.com/s/question/0D50X0000C8d0oXSQQ/hii-am-using-stm32ide-if-i-entering-into-debug-mode-getting-this-error-please-help-me-to-crack-thisfailed-to-bind-to-port-61234-error-code-1-no-errorfailure-starting-gdb-server-tcp-port-61234-not-available
猜测是端口占用问题。有两种解决方案：
1. 可通过重启电脑解决问题，但后续仍会失败
2. （推荐）在调试配置中将 GDB 服务器的端口号从 61234 更改为 41234（其它任意值均可），并且无需重新启动计算机即可正常工作。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32CubeIDE学习笔记/1652411465211.png)
参考资料：
• [i am using stm32IDE if i entering into debug mode getting this error please help me to crack this.](https://community.st.com/s/question/0D50X0000C8d0oXSQQ/hii-am-using-stm32ide-if-i-entering-into-debug-mode-getting-this-error-please-help-me-to-crack-thisfailed-to-bind-to-port-61234-error-code-1-no-errorfailure-starting-gdb-server-tcp-port-61234-not-available)
• [Fix Failure starting GDB server - ST LINK V2 Failed to bind to port 61234](https://www.youtube.com/watch?v=SVIacozy9j8)
• [Failure starting GDB server](https://www.youtube.com/watch?v=VVQr6nx7LBc)
• [RT-Thread Studio调试错误 Failed to bind to port 61234, error code -1: No error](https://blog.csdn.net/qq_27508477/article/details/103705143)