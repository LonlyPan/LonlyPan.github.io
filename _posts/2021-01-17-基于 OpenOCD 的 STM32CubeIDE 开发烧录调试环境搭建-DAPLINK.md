---
layout: post
show_title: "基于 OpenOCD 的 STM32CubeIDE 开发烧录调试环境搭建-DAPLINK"
title: "2021-01-17-基于 OpenOCD 的 STM32CubeIDE 开发烧录调试环境搭建-DAPLINK"
date: 2021-01-17
last_modified_at: 2021-01-17
categories: c&c++
---


STM32cubeIDE 是ST官方推出的一款用于开发 STM32 的工具，整合了 STM32CubeMX 和 TrueSTUDIO 而成，对于 STM32 的开发这个工具应该会在未来成为主流，毕竟有官方加持又商用免费。但仿真烧录只支持 J-LINK 和 ST-LINK 或 OpenOCD。

CMSIS-DAP/DAPLink 仿真器是硬件软件均开源的仿真器，相比当前市面上流行的jlink/st-link，烧录速度快，不丢固件，无版权风险，功能丰富，价格低廉，外观简洁精致，能较好的满足电子工程师进行日常的开发调试下载需求。

然而 STM32CubeIDE并没有直接支持它，只能通过 OpenOCD 的方式间接支持。本篇就介绍如何通过 OpenOCD 使得 STM32CubeIDE 支持 DAPLINK 调试工具

<!--more-->

## 测试环境

DAPLink 仿真器购买：[淘宝-MUSE LAB](https://item.taobao.com/item.htm?spm=a1z09.2.0.0.264d2e8dPe5l03&id=586425846353&_u=hpkffgddf47)  
STM32CubeIDE：[V1.5.1](https://www.st.com/zh/development-tools/stm32cubeide.html#get-software)
OpenOCD：[Version 20201228](https://gnutoolchains.com/arm-eabi/openocd/)

## 部署 OpenOCD

1. 下载好OpenOCD，解压到任意目录，建议路径不带空格或中文  
2. 并在 bin 目录右键，新建文本文档，并重命名为 `DAP-Linkl-stm32F4.bat`（前缀名称可以随意，后缀 `.bat`不能更改）
![enter description here](https://LonlyPan.github.io/images/Posts/2021-01-17-基于_OpenOCD_的_STM32CubeIDE_开发烧录调试环境搭建-DAPLINK/创建bat.png)
3. 右键编辑或者使用 vs-code 打开
![enter description here](https://LonlyPan.github.io/images/Posts/2021-01-17-基于_OpenOCD_的_STM32CubeIDE_开发烧录调试环境搭建-DAPLINK/编辑bat.png)
4. 输入以下内容：
   ```java
   openocd -f interface/cmsis-dap.cfg -f target/stm32f4x.cfg
   ```
   意思是：使用cmsis-dap调试stm32f4，这里默认使用 SWD 接口。通过其他配置，还可以支持 J-link 接口，未做尝试，不再叙述。
   后面的 `stm32f4x.cfg` 需要适配你的芯片型号，具体支持型号可以在 `OpenOCD-20201228-0.10.0\share\openocd\scripts\target` 的目录,在里面能找到
![enter description here](https://LonlyPan.github.io/images/Posts/2021-01-17-基于_OpenOCD_的_STM32CubeIDE_开发烧录调试环境搭建-DAPLINK/target.png)
可以看到 stm32 大部分型号都支持了。F1系列 的就改为 `stm32f1x.cfg`,F7系列 的就改为 `stm32f7x.cfg`，同理类推。
5. 单击  `DAP-Linkl-stm32F4.bat` 执行，会弹出一下窗口，表示连接成功。最小化窗口，保持后台运行。
![enter description here](https://LonlyPan.github.io/images/Posts/2021-01-17-基于_OpenOCD_的_STM32CubeIDE_开发烧录调试环境搭建-DAPLINK/打开bat.png)

## STM32CubeIDE 配置

1. 新建测试工程（我的是 LED 亮灭测试）
2. 单击菜单栏 Debug图标（绿色甲壳虫）旁的下拉按钮，单击选择 `Debug Configurations`  进入配置界面
![enter description here](https://LonlyPan.github.io/images/Posts/2021-01-17-基于_OpenOCD_的_STM32CubeIDE_开发烧录调试环境搭建-DAPLINK/debug打开.png)
3. 配置如下图所示，
 **一定要：取消勾选 `Live Expressions`**，网上教程都没这一步，导致调试失败。
 取消勾选原因：[Does STM32CubeIDE not support live variable watching?](https://community.st.com/s/question/0D53W000003NWoy/does-stm32cubeide-not-support-live-variable-watching)
 ![enter description here](https://LonlyPan.github.io/images/Posts/2021-01-17-基于_OpenOCD_的_STM32CubeIDE_开发烧录调试环境搭建-DAPLINK/debug配置.png)
 4. 弹窗单击 `Switch` ，进入调试界面
 ![enter description here](https://LonlyPan.github.io/images/Posts/2021-01-17-基于_OpenOCD_的_STM32CubeIDE_开发烧录调试环境搭建-DAPLINK/switch.png)
 
## 仿真测试
进入仿真调试界面，可以看到 GPIOC（LED的 IO 为 GPIOC_13） 的 寄存器 ODR 值会随着程序运行改变。
![enter description here](https://LonlyPan.github.io/images/Posts/2021-01-17-基于_OpenOCD_的_STM32CubeIDE_开发烧录调试环境搭建-DAPLINK/调式.gif)

再看另外两个窗口：`现场表达式` 和 `Expressions`。    
可以看到 `现场表达式` 是无法使用的（由于前面取消勾选了 `Live Expressions`），但`Expressions`是可以的。而这两者很相似啊，具体区别也未查到，不知道是软件的bug还是什么情况。反正还是可以实时查看表达式值就很开心😀  

以后每次调试都需要单击 `DAP-Linkl-stm32F4.bat` 保持后台运行。
## 参考链接

 - [MuseLab-nanoDAP-资料](https://github.com/wuxx/nanoDAP/blob/master/doc/README.md)
 - [STM32开发/烧录/调试环境搭建 基于:Win10+STM32Cube+openocd+cmsis-dap(dap-link)](https://www.cnblogs.com/DragonStart/p/12004523.html)
 - [Does STM32CubeIDE not support live variable watching?](https://community.st.com/s/question/0D53W000003NWoy/does-stm32cubeide-not-support-live-variable-watching)
 - [基于OpenOCD 的 STM32CubeIDE 开发烧录调试环境搭建 DAPLINK/STLINK](http://www.elelab.net/stm32cubeide-flash-openocd-daplink-stlink.html)
 - [在STM32CubeIDE中用OpenOCD调试STM32H750](http://www.wujique.com/2020/03/22/%E5%9C%A8stm32cubeide%E4%B8%AD%E7%94%A8openocd%E8%B0%83%E8%AF%95stm32h750/)
 - [STM32CubeIDE配置OpenOCD跳过STLink版本检查 跳过芯片型号检查(免破解,免修改ide任何文件)](https://www.cnblogs.com/DragonStart/p/12199455.html)
 - [STM32CubeIDE 调试配置](https://github.com/wuxx/nanoDAP/issues/5)
 - [STM32IDE_无线DAP调试下载器不香吗](https://www.bilibili.com/video/av968640373/)
 - [DAPLink设计与应用](https://lgg001.github.io/2018/08/12/DAPLink%E8%AE%BE%E8%AE%A1%E4%B8%8E%E5%BA%94%E7%94%A8/)