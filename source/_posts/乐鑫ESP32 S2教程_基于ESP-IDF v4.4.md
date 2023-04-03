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

## 01-开发板

使用[源地ESP32-S3核心板](https://item.taobao.com/item.htm?spm=a230r.1.14.1.283f2d6cAjCT7l&id=669443108979&ns=1&abbucket=6#detail)
也可以购买复刻版：[ESP32 S3核心板](https://item.taobao.com/item.htm?_u=fpkffgdda1e&id=695554257902)

N16R8（16M 外扩flash/8M PSRAM）/双Type-C USB口/W2812 rgb/高速USB转串口
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v4.4/1676468214198.png)

![ESP32-S3-0702 _9_](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v4.4/ESP32-S3-0702_9_.PNG)

## 02-模组资料：模组和芯片关系

简单来说，模组包含芯片，是一个最小系统，我们只需要模组供电，并引出引脚等，就构成了开发板。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/image.jpg)

如下图所示，单独的芯片时不能直接工作的，还需要外接晶振、Flash（基本所有的ESP32都是没有内部flash的，都需要外接flash工作）等才能正常工作

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677294268773.png)

## 02-资料获取

牢记：官网是最佳的学习渠道

- [官方技术文档下载中心](https://www.espressif.com.cn/zh-hans/support/documents/technical-documents)

以下是从官网提取的关于S3芯片的几个重要文档，其它文档（如硬件设计指南等）请去上面文档中心下载
- [ESP32 S3芯片技术规格书](https://www.espressif.com/sites/default/files/documentation/esp32-s3_datasheet_cn.pdf)
- [ESP32-S3 芯片技术参考手册](https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_cn.pdf)
 - [ESP32-S3-WROOM-1 模组技术规格书](https://www.espressif.com/sites/default/files/documentation/esp32-s3-wroom-1_wroom-1u_datasheet_cn.pdf)

另外还有一个最重要的在线文档：
- [ESP-IDF 编程指南](https://docs.espressif.com/projects/esp-idf/zh_CN/latest/esp32s3/index.html)
- pdf格式文档-英文：[下载](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/esp-idf-en-v5.1-dev-3619-g57b6be22a7-esp32s3.pdf)

推荐访问在线文档，该文档随时都会更新。另外轻易英文内容为准，中文的可能有翻译错误和更新延迟。

官方开发板资料：（产品开发参考，初学者不用看）
- [ESP32-S3-DevKitC-1](https://docs.espressif.com/projects/esp-idf/zh_CN/latest/esp32s3/hw-reference/esp32s3/user-guide-devkitc-1.html)
- [ESP32-S3-BOX](https://github.com/espressif/esp-box)
- [ESP32-S3-EYE](https://github.com/espressif/esp-who/blob/master/docs/en/get-started/ESP32-S3-EYE_Getting_Started_Guide.md)
- [ESP32-S3-Korvo-1](https://github.com/espressif/esp-skainet/blob/master/docs/en/hw-reference/esp32s3/user-guide-korvo-1.md)
- [ESP32-S3-Korvo-2](https://docs.espressif.com/projects/esp-adf/zh_CN/latest/get-started/user-guide-esp32-s3-korvo-2.html)

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

ESP IDF 是乐鑫官方推出的物联网开发框架（类似标准库或HAL库）

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
	**强烈建议保持默认路径及C盘不动，否则后续在编译和调试可能会出现一系列离谱错误，所以请保持默认路径不要动。光软件安装测试就用了两周时间的吐血教训！**
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679409094136.png)
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

打开并运行软件，会提示我们选择工作空间，工作空间相当于一个存档，保存了IDE的相关设置，如字体、排版、界面设置等自定义设置。如果下一次换了一个工作空间位置，软件就会恢复到默认设置。

工作空间的目的在于协作，我们将工程文件和工作空间文件夹一同发给别人，另一个人打开软件时，在下面界面就可以选择我们发送的那个工作空间文件夹，这样别人就能保持和你一样的软件设置，放置不同人使用的软件不用，导致一些位置错误。

这里我们时第一次使用，保持默认即可。你也可以自定义位置
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679194613824.png)

首次运行会显示欢迎界面，这里有两个很重要的链接，不过如果你点击链接后，会直接在软件中打开，而不是浏览器打开，你也可以通过下方的链接访问
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
工程结构：
https://xie.infoq.cn/article/ddb67ebf28bfe7fecce6a2368
https://blog.csdn.net/qq_40500005/article/details/113840391

1. 有两种方式新建工程，如下图所示
- 通过项目浏览器中的快捷链接
- 左上角 **File** - > **New** ->  **乐鑫IDF项目**
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677074767312.png)
2. 输入项目名 `SampleProject`，并选择项目存放位置，我们建议给项目新建一个文件夹 `01-SampleProject`，另外**注意路径不要有中文、空格、括号等**
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677077335557.png)
3. 选择模板，这里我们选择官方提供的 `sample_project` 项目模板（至于为什么要使用模板，待会就会讲到），单击 **Finish** 完成工程创建
    在这里我们也可以看到，官方提供了很多模板例程，这是一个很好的学习资料，大家可以自行尝试不同模板例程。
> 使用模板创建工程，有一个问题就是，IDE会自动修改项目名称。所以你需要在这里再次手动修改项目名

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677077564989.png)
4. 以下就是我们的工程了。此时 main.c 里只有一个主函数，还没有任何功能。
   同时也可以发现，程序中会有波浪线，和其它错误警告，在下面的编译完成后，就会都消失的
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677077700612.png)

这里我们可以发现，主程序并不是常见的 `main`，而是 `app_main`，这是因为esp32默认待 Free RTOS 系统，关于该操作系统后面涉及时在讲解；对于初学，只需要把它当成 `main` 函数就行了。
![enter description here](./img/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679929730389.png)
### 编译和下载

5. 在编译前我们需要选择目标芯片。单击齿轮图标
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677078067775.png)
6. IDF目标选择芯片类型 `esp32s3`
> 注意这里一定要先选择芯片型号，IDE有时会抽风，会自动变更这里的芯片型号，所以每次编译前最好都确认下。

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
同时在工程名，右键，可以打开快捷菜单，打开 应用程序内存分析
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677158442918.png)
可以更加直观的观察到内存占用情况
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677158536155.png)
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
步骤和上面类似。只是在模板选择界面，我们保持默认，不选择任何模板，直接点击 Finish 完成工程创建
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677158681308.png)
可以看到，默认工程中，main函数中是有一个打印函数的，打印函数的功能就是将其中的字符串通过串口发送给电脑，我们可以通过串口助手查看到开发板发送的信息
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677158721461.png)
如下所示，串口波特率默认115200
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1677158824071.png)

## 导入工程

之所叫导入教程，是因为IDE的bug，无法正常打开工程
1. 按照一般如Eclipse和STM32CubeIDE使用方式，我们直接点击 **从文件系统打开工程**
  ![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679217079133.jpg)
2. 选择我们的工程文件夹，不要勾选main文件
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679217142118.png)
否则你的mian文件夹也会出现 .project 文件。如果你的已经有了，直接删除即可
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679217195936.png)
3. 你会发现，无论怎么编译，你的工程都会有这个波浪线，提示错误和警告。所以该方法导入项目并不可行
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679217408699.png)   
 
我们选择另一种可行方案，导入工程
1. 按以下来个那种方式，都可以，单击导入 **import**
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679217587342.png)
2. 选择 **乐鑫 -> 现有IDF项目**
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679217642183.png)
3. 单击**浏览**选择项目文件夹。单击**Finish**完成导入
 ![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679217712322.png)
4. 此时编译就不会有任何错误了
 ![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679217792777.png)
 
## 调试与下载

要启用调试功能，需要先配置ESP-IDF工具

如果后续编译时遇到如下报错“
```
ninja: error: loading 'build.ninja': esp32
```
按下面教程重新安装ESP-IDF工具即可。一般都是因为windows系统自动更新导致的。

> 至于为什么这样做，我也不知道，只知道按下面操作才能启动调试功能。参考视频 [【乐鑫开发者大会-13】在 Espressif-IDE 中使用 ESP-IDF 开发应用】](https://www.bilibili.com/video/BV1D8411Y7Vz/?share_source=copy_web&vd_source=8ebff88ae6397c9ea01f30029bc60928)，3分35秒开始

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679412350559.png)
然后选择使用文件系统现有ESP-IDF，选择软件安装目录下的文件夹，如果你的软件默认在C盘，路径应该和下图一致
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679412423265.png)
Yess
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679412428834.png)
保持默认，点击 **安装工具**
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679412440813.png)
等待系统下载安装完成，这个过程可能很慢或者失败，一般半小时左右。如果半小时还没动静，建议关闭软件重新安装。右下角是进度条
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679413054380.png)
显示如下，标志安装完成
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679413036536.png)
安装完成，**重启电脑。一定要重启电脑。**
重新打开软件，可能会弹出如下提示，点击 **是** 即可。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679413448639.png)
在工程名下拉，选择**New Launch Configuration**
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679414163305.png)
单击 **Debug**，选择  **ESP-IDF GDB OpenOCD Debugging** 这里是配置调试工具，**Next**
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679414244524.png)
Main 标签页保持默认，注意这里要有 .elf文件
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679414393337.png)
Debugger 标签页，目标选择问哦们的芯片 S3，开发板应该翻译为调试工具，选择芯片内部 USB-JTAG，点击 **Apply** 保存设置，单击 **Finish** 退出设置
![enter description here](./img/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679824494192.png)
>根据官方描述，ESP32 C3和S3都内置JTAG调试器，即我们只需要通过USB线连接到ESP32的USB引脚，就能通过IDE直接调试，不需要额外的如ST-Link或Jtag调试硬件工具，非常方便。

> USB接口同时也支持虚拟串口，所以在接USB后，我们就不要再接串口RX和TX调试了，反而还省了两个口。否则像其它ESP32都需要使用串口下载程序的，因为S3和C3的USB自带虚拟串口，所以就不需要串口 引脚了。所以建议用S3和C3时，使用USB口调试、调试、虚拟串口打印。我们在使用USB连接ESP32时，电脑会自动生成一个串口，就跟普通的串口一样，使用即可。

>同时注意，使用调试功能，USB引脚就不能用于其它功能了。除非后续不再使用调试功能，usb两个引脚才可以作为他用。

> 后面教程都会只用USB接口和虚拟串口，虚拟串口默认和串口0相连

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679414453939.png)
此时我们的目标文件名，已经自动变成 **test Configuration**，同时还需要将左侧的**Run**改为**Debug**
这里可以发现出现波浪线提示错误，单击最左侧 build 重新编译即可。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679414891654.png)
检查目标芯片和串口号没有错误
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679414982158.png)
单击左侧的虫子图标，开始调试
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679415017729.png)
等待软件编译完成，出现如下弹窗，选择 **Switch**，切换到调试界面
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679415114430.png)
软件会自动停留在程序开始位置，可以使用工具栏的调试工具进行调试运行。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679415345863.png)
值得注意的是，当我们停止调试并返回到Run模式，是需要我们手动操作的，如下，都需要我们手动选择。
>所以这里建议，以后的项目都默认直接使用Debug模式。在Debug模式下编写程序和调试。避免来回切换。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679415475910.png)
在调试模式下，如果不想调试，只想下载，可以单击**运行**图标，选择**工程文件名**，即可下载。
注意不要选错了，不要选第二个Configurantion（用于调试的）。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679415991099.png)

## 自带终端，串口助手使用
打开终端，选择串口号，其它保持默认（串口监控）
![enter description here](./img/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679578641429.png)
界面底部栏，终端标签页会显示板子发送过来的串口信息
![enter description here](./img/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679578830578.png)

> 建议还是使用专门的串口调试软件，如正点原子的 XCOM 调试软件
> 默认波特率115200

![enter description here](./img/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679925520384.png)

## FreeRTOS操作系统

建议自行参考视频资料学习：
- [什么是RTOS? - 孤独的二进制 - ESP32上的FREERTOS](https://www.bilibili.com/video/BV1q54y1Z7ca/?vd_source=03b483801bb82304e4324482b60bb93f)
- [ESP32_freeRTOS教程一： 入门介绍](https://www.bilibili.com/video/BV1Nb4y1q7xz/?vd_source=03b483801bb82304e4324482b60bb93f)
# 实战篇

## 1、printf
新建样例工程（最简工程）。
添加如下代码：
```
#include <stdio.h>

void app_main(void)
{
	printf("Hello\n");
}

```
编译下载程序，打开终端监视器
可以发现串口成功输出
> USB虚拟串口和串口0都可以输出，应该时映射的，所以此时接串口0和USB虚拟串口都会有输出

![enter description here](./img/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1679826810385.png)
如果将后面换行符 `\n`去除，再下载
```
#include <stdio.h>

void app_main(void)
{
	printf("Hello");
}
```
会发现并没有输出。这是因为串口发送会有一个缓存机制，芯片再检测到缓存满后，才会发送一次数据。而我们现在发送的数据并不能填充满缓存区，所以就没有输出。
有两种解决办法
1. 和上面一样，再输出内容最后加上换行符，可以强制打印输出
2. 加 `fflush(stdout)` ，也是强制刷新缓冲区，冲洗掉缓存区内容
该方法仅适用于传统串口，不适用于USB虚拟串口
```
#include <stdio.h>

void app_main(void)
{
	printf("Hello");
	fflush(stdout);
}
```

### sdkconfig项目配置

串口默认串口0，即GPIO43、44。默认波特率115200
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1680532028556.png)

我们也可以修改默认串口。按下图打开sdkconfig项目配置工具，找到ESP System Setttings，找到console输出配置，这里有两个关于串口的配置，我们可以点击右侧问好查看帮助信息
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1680532051942.png)
我们可以修改默认串口为用户自定义，这是后我们可以定义串口引脚（串口引脚需要查看芯片手册，按照引脚功能选择）和波特率。这里不推荐修改默认串口配置。

同时我们这里也可以看到，串口的第二通道，输出默认是USB-JTAG，这就解释了为什么我们的USB虚拟串口会和默认串口是一样可以打印输出了。如果这里我们关闭辅助输出功能，USB虚拟串口将不会打印输出，但还会保留启动输出。
>如果我们的默认串口连接了串口助手，那么USB虚拟串口的输出将会关闭。

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/1680532143489.png)

#### 帮助信息：

配置esp_CONSOLE_uart
控制台输出通道
选择控制台输出的发送位置（通过stdout和stderr）。
默认情况是在预定义的GPIO上使用UART0。
如果选择“自定义”，则可以选择UART0或UART1，并且可以选择任何引脚。
如果选择“None”（无），则除ROM引导加载程序的初始输出外，任何UART上都不会有控制台输出。此ROM输出可以通过GPIO捆扎或EFUSE抑制，详细信息请参阅芯片数据表。
在带有USB OTG外围设备的芯片上，“USB CDC”选项将输出重定向到CDC端口。此选项使用芯片ROM中的CDC驱动程序。此选项与TinyUSB堆栈不兼容。
在带有USB串行/JTAG调试控制器的芯片上，选择将输出重定向到该设备的CDC/ACM（串行端口仿真）组件的选项。
可用选项：

 - 默认值：UART0（ESP_CONSOLE_ART_Default）
 - USB CDC（ESP_CONSOLE_USB_CDC）
 - USB串行/JTAG控制器（ESP_CONSOLE_USB_Serial_JTAG）
 - 自定义UART（ESP_CONSOLE_ART_Custom）
 - 无（ESP_CONSOLE_None）

配置sp_CONSOLE_SECONDARY
控制台二次输出通道
位于：组件配置>ESP系统设置
当选择UART0端口作为主要端口但未连接时，此辅助选项支持通过其他特定端口（如USB_SERIAL_JTAG）输出。此辅助输出当前仅支持不使用REPL的非阻塞模式。如果您想用REPL以阻塞模式输出或通过该辅助端口输入，请在控制台输出菜单的通道中将主配置更改为该端口。
可用选项：

 - 无辅助控制台（ESP_console_CONDARY_NONE）
 - USB_SERIAL_JTAG端口（ESP_CONSOLE_condary_USB_SERIAL_JTAG）

**当UART0端口未连接时，此选项支持通过USB_SERIAL_JTAG端口输出。**输出当前仅支持非阻塞模式，而不使用控制台。如果您想使用REPL以阻塞模式输出或通过USB_SERIAL_JTAG端口输入，请将主配置更改为上面的ESP_CONSOLE_USB_SERIAL_JTAG。
## 2、sleep 延时

在上一节课基础上，添加延时函数，时间间隔定时输出
需要包含头文件 `#include <unistd.h>`,完整程序如下
```
#include <stdio.h>
#include <unistd.h>

void app_main(void)
{
    while (1) {
        printf("Hello from app_man!\n");
        sleep(1);
    }
}

```
我们选中sleep单词，按F3或右键单击Open Declaration，可以其具体实现
![右键单击去定义](./img/乐鑫ESP32_S3教程_基于ESP-IDF_v5.0/右键单击去定义.gif)
可以看到，`sleep`实际调用的`usleep`，`usleep`则调用的`vTaskDelay`
而 `vTaskDelay` 实际和FreeRTOS有关，我们的EPS32 运行时离不开FreeRTOS的，相关的教程完成也有很多，这里不再讲解，大家可以参下网络教程学习，
- [ESP32 FreeRTOS-任务的创建与删除 (1)](https://blog.csdn.net/believe666/article/details/127175049?spm=1001.2014.3001.5502)
- [ESP32编程指南 —— Task API （任务）](https://blog.csdn.net/weixin_45652444/article/details/118867092?spm=1001.2014.3001.5502)
- [ESP32_IDF学习4【ESP32上的FreeRTOS】](https://www.cnblogs.com/redlightASl/p/15539672.html)
- [【深入浅出】FreeRTOS 学习笔记](https://www.cnblogs.com/liaozhelin/p/16290465.html) 
- [006-ESP32学习开发(SDK)-关于操作系统-任务,任务堆栈空间,任务的挂起,恢复,删除](https://www.cnblogs.com/yangfengwu/p/15089797.html) 
  
## 2、GPIO 输出



## 3、GPIO 输入

## FreeRTOS

参考：
https://blog.csdn.net/believe666/article/details/127205502

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
