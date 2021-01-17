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
## 参考连接
