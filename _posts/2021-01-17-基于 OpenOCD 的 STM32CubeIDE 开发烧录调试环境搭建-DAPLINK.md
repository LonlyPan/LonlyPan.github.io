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
   意思是：使用cmsis-dap调试stm32f4。这里默认使用 SWD 接口（DAPLink），还可以支持 J-link，未做尝试，不再叙述。
   后面的 `stm32f4x.cfg` 需要适配你的芯片型号，具体支持型号可以在 `OpenOCD-20201228-0.10.0\share\openocd\scripts\target` 的目录,在里面能找到
![enter description here](https://LonlyPan.github.io/images/Posts/2021-01-17-基于_OpenOCD_的_STM32CubeIDE_开发烧录调试环境搭建-DAPLINK/target.png)
可以看到基本stm32大部分型号都支持了。F1系列 的就改为 `stm32f1x.cfg`,F7系列 的就改为 `stm32f7x.cfg`，同理类推。
5. 单击  `DAP-Linkl-stm32F4.bat` 执行，会弹出一下窗口，表示连接成功。
![enter description here](https://LonlyPan.github.io/images/Posts/2021-01-17-基于_OpenOCD_的_STM32CubeIDE_开发烧录调试环境搭建-DAPLINK/打开bat.png)

## 参考连接
