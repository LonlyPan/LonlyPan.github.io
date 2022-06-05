---
layout: post
title: "国产单片机MCU"
index_img: https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/国产单片机MCU/v2-fc0803ee05ca5304f57e6f41f2e5bbbc_720w.png
date: 2020-11-05
updated: 2020-11-05
hide: false
# sticky: 100 #置顶，数字越大越靠前
# banner_img: #/img/post_banner.jpg
# comment: false
categories: 01-专业
---

<!--more-->

## 国产芯片资讯

 - [国产MCU厂商盘点](https://bbs.21ic.com/icview-2683232-1-1.html)
 - [国产芯片替代正当时，一文看完国外芯片都有哪些替代！](https://blog.csdn.net/karaxiaoyu/article/details/107650563?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161217084416780265479104%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=161217084416780265479104&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v29-6-107650563.first_rank_v2_pc_rank_v29&utm_term=%E5%9B%BD%E4%BA%A7%E8%8A%AF%E7%89%87%E6%9B%BF%E4%BB%A3)
 - [MCU国产有哪些可以替代国外？](https://blog.csdn.net/zhouxianjin123/article/details/105667193)
 - [国产超低价单片机五宗罪](https://zhuanlan.zhihu.com/p/150220645)
 - [【颁奖】赞一赞我的国：侃侃好用的国产单片机 ](http://bbs.eeworld.com.cn/thread-1095444-1-1.html)  

## 国产芯片厂家

 - [GD32](http://www.gd32mcu.com/cn/ecosystem/officialTools?cate=learning_kit)
 - [雅特力科技](http://www.arterytek.com/cn/product/AT32F415.jsp?t=1604555352437)
 [雅特力32位MCU AT32与Sxx32/Gx32替换对照表](https://www.sekorm.com/doc/2225810.html)
 - [华大](https://www.hdsc.com.cn/)
 [HDSC产品介绍](https://www.sekorm.com/doc/2266625.html)
 - [新唐](https://www.nuvoton.com/)
 - [合泰](https://www.holtek.com.cn/)
 - [灵动](http://www.mm32mcu.com/)
 - [芯圣](http://www.holychip.cn/product10.php)
 - [中颖](http://www.sinowealth.com/homes)
 - [敏矽微电子](http://www.mesilicon.com/a/contact/)
 - [芯旺](https://www.chipon-ic.com/Product/Index/705a6385-5d5c-4859-8f13-9b66e563c2fc)
 - [航顺HK32 MCU](http://www.hsxp-hk.com/product/75.html)
 - [中基国威](http://www.sinomicon.cn/)
[HK32F030MF4P6](https://item.szlcsc.com/744582.html)
 - [中科芯](http://www.cksic.com/index.html) 
[中科芯（CETC）32位通用MCU与STM32替换对照表](https://www.sekorm.com/doc/2223266.html)
[中科芯 103/030/031/051系列32位MCU，软硬件全兼容，交期2-4周](https://www.sekorm.com/news/57262194.html)
 - [恒烁](http://www.zbitsemi.com/)
 - [赛元](https://www.socmcu.com/)

## STM8s003替换

 - [\[国产单片机\] 赛元5003，兼容STM8的003](https://bbs.21ic.com/icview-1709838-1-1.html)
 - [stm8s003f3p6的国产替代选手](http://bbs.eeworld.com.cn/thread-1126783-1-1.html)
 - [CX32L003可替代STM8S003，与新唐003 华大003一样](https://blog.csdn.net/csdndianzi/article/details/106784099)
 - [求推荐STM8S003、STM8S103替代产品？要求价格低，备货充足。](https://www.sekorm.com/faq/32248.html)
 - [\[国产单片机\] 找一个代替STM8S003的单片机，请推荐](https://bbs.21ic.com/icview-2895484-1-1.html)

### 新唐N76E003、MS51FB9AE

管脚兼容，程序不兼容。基于51内核，需要移植STM8s程序。    
购买容易，淘宝上开发板、单芯片都有。  

 - [新唐官网](https://www.nuvoton.com/)
 - [产品选型](https://www.nuvoton.com/selection-guide/)
 - [Nuvoton 1T8051-based MicrocontrollerNuTiny-SDK-N76E003User Manual](https://www.nuvoton.com/export/resource-files/UM_NuTiny-SDK-N76E003_EN_Rev1.01.pdf)  
 - [N76E003官网资料](https://www.nuvoton.com/products/microcontrollers/8bit-8051-mcus/low-pin-count-8051-series/n76e003/)  
 - [NuTiny-MS51FB](https://www.nuvoton.com/board/nutiny-ms51fb/?index=0)
 - [新唐N76E003AT20PIN对PIN完美替代STM8S003F3P6](http://bbs.eeworld.com.cn/thread-598125-1-1.html)

#### 快速入门

KEIL C51/IAR EW8051 编写软件程序，Nu-Link 调试下载程序。也可使用ISP方式下载程序（得先用Nu-Link烧写BootLoader）。  

1.	请确认计算机中至少已安装一种开发环境:
 	-	[KEIL C51](https://www.keil.com/c51/)
 	-	[IAR EW8051](https://www.iar.com/iar-embedded-workbench/?architecture=8051)
2.	请依照使用的开发环境下载及安装最新版本的 Nuvoton Nu-Link Driver，安装时请勾选并安装 Nu-Link USB Driver。
 	-	使用 Keil C51 请安装 [Nu-Link_Keil_Driver](https://www.nuvoton.com/resource-download.jsp?tp_GUID=SW1120200221180521)
 	-	使用 IAR EW8051 请安装 [Nu-Link_IAR_Driver](https://www.nuvoton.com/resource-download.jsp?tp_GUID=SW1120200221180914)
3.	请依照使用的开发环境下载及解压缩开发板支持软件包 ( Board Support Package, BSP )。
 	-	使用 Keil C51 请下载 [MS51_Series_BSP_Keil](https://www.nuvoton.com/resource-download.jsp?tp_GUID=SW0120190221172237) 
 	-	使用 IAR EW8051 请下载 [MS51_Series_BSP_IAR](https://www.nuvoton.com/resource-download.jsp?tp_GUID=SW0120190529175241&__locale=en)
4. Nu-Link 调试下载程序。也可使用ISP方式下载程序（得先用Nu-Link烧写BootLoader）。

#### 参考链接

 - [N76E003的学习之路（一）](https://www.codeprj.com/blog/7f8bae1.html)
 - [新唐N76E003，N76E616烧录，调试各种问题集【坑集】](http://blog.mangolovecarrot.net/2018/10/24/51)
 - [Nuvoton ICP Programming Tool使用指南 新唐N76E003芯片烧](http://www.51hei.com/bbs/dpj-137790-1.html)
 - [\[C51\] 制作便宜好用的N76E003的ICP烧录器(新年礼物)](https://www.mydigit.cn/forum.php?mod=viewthread&tid=7356) 
 - [新唐单片机 N76E003 烧写程序方法](https://www.yuque.com/lingyao/za/inxuqt)
 - [N76E003快速上手使用和大坑提示](http://www.360doc.com/content/19/1006/07/6889381_865078908.shtml)
 - [老司机带你入门新塘N76E003单片机](https://www.sohu.com/a/441799293_100281310) 
 - [单片机三种烧录方式ICP、IAP和ISP详解](https://zhuanlan.zhihu.com/p/69237591)
 - [N76E003的环境搭建](https://www.cnblogs.com/zhugeanran/p/9554822.html)
 - [新唐(Nuvoton)开发软件资源汇总](http://ictalk.cn/xshare/nuvoton-tools.html)
 - [nuvoTon（新唐）8bit 8051 微控制器開發環境建置](https://ruten-proteus.blogspot.com/2020/08/nuvoton-8bit-8051-keil-c51.html)

### 芯圣HC89S003


购买不易，[淘宝官方店](https://shop155767245.taobao.com/index.htm?spm=a1z10.5-c-s.w5001-21931449627.2.1faf4a4a9aBeYG&scene=taobao_shop)、[亿配芯城](https://www.yibeiic.com/)、[立创商城](https://www.szlcsc.com/)、[华秋商城](https://www.hqchip.com/)有售。    

- [芯圣官网](http://www.holychip.cn/)
- [芯圣淘宝](https://shop155767245.taobao.com/?spm=a230r.7195193.1997079397.2.7a1946b6Tlg3aD)
- [\[技术支持\] 【转载】芯圣HC89S003系列之间的恩恩怨怨](https://bbs.21ic.com/icview-2643672-1-1.html)
- [芯圣のHC89S003多路ADC采样代码分享及下载器(hc-link)使用](https://blog.csdn.net/qq_39106660/article/details/105032413)
[\[应用方案\] ST或将告急减产？芯圣携全兼容STM8S系列MCU为您提供鼎力支持!](https://bbs.21ic.com/icview-2941524-1-1.html)

#### 快速入门

1.	开发环境:
 	-	[KEIL C51](https://www.keil.com/c51/)
2.  芯圣型号支持插件
    - [HC-LINK V4.0.4.0](http://www.holychip.cn/development.php?class_id=102104101)
3.	新建工程
4.	下载程序，JTAG(4线)或者SWD(2线)方式
     - 仿真下载，使用 HC-LINK， [HC-DRIVER V3.0.4.0](http://www.holychip.cn/development.php?class_id=102104101)
     - ISP下载，固化了ISP引导程序，此版本并不支持仿真，但是可以很方便的通过串口来下载程序  [HC-ISP](http://www.holychip.cn/development.php?class_id=102105101)

#### 参考链接

 - [\[资源共享\] 版主推荐——HC89S003F4单片机的手把手教程](https://bbs.21ic.com/icview-2604262-1-1.html)
 - [\[资源共享\] HC89S003F4开发板试用一--开发环境准备和点灯，串口例子](https://bbs.21ic.com/icview-2489074-1-1.html)
 - [\[资源共享\] HC89S003F4 配置向导初始化工具](https://bbs.21ic.com/icview-2891238-1-1.html) 
 - [\[技术支持\] HC89S003F4 ISP下载问题](https://bbs.21ic.com/icview-2884532-1-1.html)
 - [\[其他\] HC89S003F4开发板测试评估---支持国货！](https://bbs.21ic.com/icview-2491092-1-1.html)

###  航顺HK32F030M

管脚兼容，程序兼容（内核相同）。基于M0内核，需要移植STM8s的程序。    
购买不易，淘宝上开发板有售、单芯片仅两家且销量2。[猎芯网](https://www.ichunt.com/)、[亿配芯城](https://www.yibeiic.com/)、[华秋商城](https://www.hqchip.com/)、[立创商城](https://www.szlcsc.com/)有售。  

HK32S003就是HK32F030M，前期丝印是HK32S003，后面丝印改成了HK32F030M   
HK32F030M 是 HK32F030 的阉割版，资源变少，其余类似。   
理论上和stm32的M0系列（STM32有对应产品）软硬件都兼容。至于极个别自主开发的产品（STM32没有对应产品）不兼容。    
理论上Cortex-M系列的内核均可以使用DAP进行烧录调试，典型的芯片如STM32全系列的芯片，HK32全系列芯片，GD32全系列芯片，nRF51/52系列等。    
GCC工具暂不支持，开发中  

 - [航顺官网](http://www.hsxp-hk.com/index.html)
 - [HK32F030MF4P6](http://www.hsxp-hk.com/product/98.html)
 - [HK32F030F4P6](http://www.hsxp-hk.com/product/24.html)

不推荐使用，可能是皮包公司。从产品名字反复更改、
[技术支持] 芯圣和航顺是什么关系，为啥数据手册一模一样？


#### 快速入门

1.	开发环境:
 	-	KEIL/IAR
2.  器件扩展包  
 	-	[下载中心-HK32F030M系列](http://www.hsxp-hk.com/companyfile/27/)  。
 	-	使用 Keil请安装  `HKMicroChip.HK32F030Mxx_DFP`，使用 IAR 请安装 `HK32F030M_IAR_EWARM_pack`
3.	新建工程
4.	下载程序，JTAG(4线)或者SWD(2线)方式
     - 仿真下载， Jlink/Ulink/STlink/DAP-LINK 等在线仿真工具。
     - ISP下载，不支持

#### 参考链接
- [航顺MCU开发入门及常见问题汇总](https://bbs.21ic.com/icview-3056622-1-1.html)
- [\[技术问答\] HK32F030M下载方式](https://bbs.21ic.com/icview-3028826-1-5.html)
- [\[技术问答\] HK32F030使用什么IDE开发？](https://bbs.21ic.com/icview-3033764-1-5.html)
- [\[技术问答\] ST-LINK无法烧录至HK32F030MF4P6上](https://bbs.21ic.com/icview-3011564-1-5.html)
- [\[工具链\] 工具兼容ARM 开发工具](https://bbs.21ic.com/icview-2982374-1-4.html)
- [\[技术文档\] STM32F030参考资料合集,适用于HK32F030系列的MCU开发的详](https://bbs.21ic.com/icview-3054594-1-4.html)
- [\[技术文档\] 手把手教你搭建航顺MCU开发环境，以HK32F030MF4P6为例](https://bbs.21ic.com/icview-3056498-1-3.html)
 - [\[技术文档\] 丑陋的HK32F030MSO8N-J4M6开发板评测](https://bbs.21ic.com/icview-3009388-1-1.html)
 - [\[生态链\] HK32F030M/开发板使用视频教程](https://bbs.21ic.com/icview-2992556-1-1.html)
 - [\[技术文档\] 仿真器使用方法，以DAP-Link为例](https://bbs.21ic.com/icview-3055196-1-1.html)
 - [\[技术文档\] HK32F103C8T6A、HK32F103CBT6A基于MXCube生成的USB例程](https://bbs.21ic.com/icview-3066654-1-1.html)
 - [STM32CubeIDE配置OpenOCD跳过STLink版本检查 跳过芯片型号检查(免破解,免修改ide任何文件)](https://www.cnblogs.com/DragonStart/p/12199455.html)
 - [\[技术文档\] 【如何使用HK32F030C8T6】进行开发工作](https://bbs.21ic.com/icview-3016726-1-1.html)
 - [\[技术文档\] 使用cubemx可以开发航顺单片机吗？](https://bbs.21ic.com/icview-3034894-1-1.html)
 - [\[方案讨论\] 开发者使用HK32F030M经验之谈](https://bbs.21ic.com/icview-3025132-1-1.html)

### 灵动微 MM32F003、MM32F0010

MM32F003、MM32F0010 基本没差别
管脚兼容，程序兼容（内核相同）。基于M0内核，需要移植STM8s的程序。教程少，价格贵  
购买容易，淘宝上开发板有售、单芯片[亿配芯城](https://www.yibeiic.com/)、[猎芯网](https://www.ichunt.com/)、[云汉芯城](https://www.ickey.cn/)、[立创商城](https://www.szlcsc.com/)、[华秋商城](https://www.hqchip.com/)有售  

 - [MM32F003-开篇--001](https://www.jianshu.com/p/75282a0a2b52)
 - [\[其他\] MM32F003 32位ARM M0核心微控制处理器](https://bbs.21ic.com/icview-2986296-1-1.html)

#### 快速入门

1.	开发环境:
 	-	KEIL/IAR
2.  器件扩展包  
 	-	[下载中心-Pack文件](http://www.mindmotion.com.cn/download.aspx?cid=2546)  。
 	-	使用 Keil 请安装  `MM32系列 KEIL pack文件包`，使用 IAR请安装 `MM32系列 IAR pack文件包`
3.	新建工程
4.	下载程序，JTAG(4线)或者SWD(2线)方式。推荐使用官方 MM32-Link
     - 仿真下载， Jlink/Ulink/STlink/DAP-LINK 等在线仿真工具。
     - ISP下载，不支持
     - 支持ICP模式

#### 参考链接

 - [下载中心](http://www.mindmotion.com.cn/download1.aspx)
 - [\[MM32软件\] 请教mm32单片机是否支持stlink，不需要REST脚](https://bbs.21ic.com/icview-2931134-1-10.html)
 - [\[MM32硬件\] MM32F003 Jlink烧录](https://bbs.21ic.com/icview-2889948-1-11.html)
 - [\[其他\] 用什么仿真器下载程序呢](https://bbs.21ic.com/icview-2869194-1-11.html)
 - [\[MM32生态\] MM32-LINK在线仿真器/编程器](https://bbs.21ic.com/icview-2744368-1-12.html) 
 - [\[MM32硬件\] MM32F0010的SWD连不上芯片](https://bbs.21ic.com/icview-3060062-1-2.html)
 - [\[MM32软件\] MM32下载 JLink和STlink](https://bbs.21ic.com/icview-3006134-1-3.html) 
 - [\[MM32生态\] 灵动的MCU支持ST-LINK吗](https://bbs.21ic.com/icview-3041298-1-3.html)
 - [\[MM32软件\] MM32能使用JLink调试和下载吗？](https://bbs.21ic.com/icview-2839136-1-4.html)
 - [\[MM32硬件\] MM32F003怎么用串口下载？](https://bbs.21ic.com/icview-2976406-1-4.html)
 - [\[MM32生态\] MM32-Link驱动安装问题解决](https://bbs.21ic.com/icview-3067210-1-1.html) 

### 中基国威 SM51F003

管脚兼容，程序不兼容。基于51内核，需要移植STM8s程序。   
购买容易，淘宝无售开发板、单芯片销量0，单芯片[亿配芯城](https://www.yibeiic.com/)、[立创商城](https://www.szlcsc.com/)（有开发板）、[华秋商城](https://www.hqchip.com/)有售  

 - [SM51F003-->>给工程师一个春节加班的理由](http://www.sinomicon.cn/news_view.aspx?TypeId=4&Id=417&Fid=t2:4:2)
 - [最近接触到可以兼容STM8S003的替代单片机芯片-->>SM51F003](https://bbs.elecfans.com/jishu_1522356_1_1.html)
 - [采用SM51F003的无线充电方案-->>完美兼容STM8S003F3P6](http://www.sinomicon.cn/news_view.aspx?TypeId=4&Id=416&Fid=t2:4:2)

#### 快速入门

资料极少

#### 参考链接

### 华大HC32F002/003/005

管脚兼容，程序兼容（内核相同）。基于M0内核，需要移植STM8s的程序。Keil官方支持  
 - 002
   去除了 VCAP 引脚 低功耗、安全保护
   2K RAM、18K Flash
 - 003
   2K RAM、16K Flash
 - 005
   4K RAM、32K Flash
购买容易，淘宝上开发板有售、单芯片[亿配芯城](https://www.yibeiic.com/)、[猎芯网](https://www.ichunt.com/)、[云汉芯城](https://www.ickey.cn/)、[华秋商城](https://www.hqchip.com/)、[立创商城](https://www.szlcsc.com/)有售  

 - [华大官网](https://www.hdsc.com.cn/)
 - [智能家电MCU---发展与支撑](http://img.cheerue.com/D5C994E3-F0CF-4E81-676E-B20D75A511B0_thinkv_2018-09-14_5b9b735d11074.pdf)
 - [使用华大单片机HC32F003 HC32F005对STM8S003 20脚单片机的替换](https://bebugless.com/blogs/3768438/)
 - [华大单片机HC32F005如何新建工程（lite库版本）](https://bebugless.com/blogs/3768463/)
 - [国产超低功耗华大单片机HC32F003开发板上手入门](https://bebugless.com/blogs/3768422/)
 - [Huada HC32F003/005 replaces XXX8S003--Resource comparison table](https://www.programmersought.com/article/6737176830/) 

#### 快速入门

1.	开发环境:
 	-	KEIL/IAR
2.  器件扩展包  
 	-	KEIL官方支持 
 	-	或者官网下载 [IDE支持包](https://www.hdsc.com.cn/Category83-1432)
3.	新建工程
4.	下载程序，JTAG(4线)或者SWD(2线)方式。
     - 仿真下载， Jlink/Ulink/STlink/DAP-LINK 等在线仿真工具。
     - ISP下载，支持。无需提前下载固件。需要软件 `HDSC MCU Programmer`。在 [HC32F003C4PB-TSSOP20](https://www.hdsc.com.cn/Category83-1432) 下载 `Cortes-M在线编译器` 压缩包。

#### 参考链接

 - [\[综合信息\] 【华大测评】+调试工具测试1](https://bbs.21ic.com/icview-3034914-1-10.html)
 - [HC32F003C4PB-TSSOP20](https://www.hdsc.com.cn/Category83-1432)
 - [\[开发工具\] 【华大编程器】，已统计10种](https://bbs.21ic.com/icview-3046094-1-1.html)
 - [\[综合信息\] 华大生产工具-离线烧写器简介及使用的注意事项](https://bbs.21ic.com/icview-3065090-1-3.html)
 - [\[综合信息\] 华大HC32F002 PDF与开发资料下载](https://bbs.21ic.com/icview-3059830-1-2.html)
 - [\[综合信息\] 三分学会国产低功耗华大单片机一 （MDK中新建工程）](https://bbs.21ic.com/icview-3065554-1-2.html)
 - [\[其他\] 华大单片机DDL库与lite库的区别](https://bbs.21ic.com/icview-3065560-1-3.html)
 - [\[开发工具\] 【华大测评】+MDK环境搭建](https://bbs.21ic.com/icview-3041540-1-8.html)
 - [\[开发工具\] 【华大测评】+开箱搭建环境，测试例程](https://bbs.21ic.com/icview-2966770-1-18.html)
 - [\[综合信息\] （转载）国产超低功耗华大单片机HC32F003开发板上手入门](https://bbs.21ic.com/icview-2817442-1-29.html)

### 赛元SC92F8003

管脚兼容，程序不兼容，下载不兼容。基于51内核，需要移植STM8s程序。  
购买容易，淘宝无售开发板、单芯片销量5，其它无售。[立创商城](https://www.szlcsc.com/)有售  

- [淘宝官方店](https://socmcu.taobao.com/)
- [与STM8S003对位——赛元微电子SC92F7003评测](https://www.21ic.com/evm/evaluate/MCU/201808/809241.htm)
- [\[讨论\] 又一国产STM8替代方案——赛元SC92F7003评估及SC LINK仿真器试用](https://bbs.elecfans.com/jishu_1771574_1_1.html)

#### 快速入门

1.	开发环境:
 	-	[KEIL C51](https://www.keil.com/c51/)
2.	器件扩展包
 	-	[003系列MCU](https://www.socmcu.com/index.php?m=Software&a=index&bid=15&pid=59) 下载 `KEIL C插件（包含头文件、DEMO程序及仿真插件）`
3.	新建工程
4.	下载程序：SC LINK 调试下载程序。需安装仿真插件 SOC_KEIL。[003系列MCU](https://www.socmcu.com/index.php?m=Software&a=index&bid=15&pid=59) 下载 `KEIL C插件（包含头文件、DEMO程序及仿真插件）`

#### 参考链接

 - [汇总资料下载-003系列MCU](https://www.socmcu.com/index.php?m=Support&a=index&bid=87&pid=59)
 - [开发工具-003系列MCU](https://www.socmcu.com/index.php?m=Software&a=index&bid=15&pid=59)
 -  [使用说明-003系列MCU](https://www.socmcu.com/index.php?m=Software&a=index&bid=34&pid=59)

### 武汉新芯-恒烁 CX32L003

管脚兼容，程序兼容（内核相同）。基于M0内核，需要移植STM8s的程序。Keil官方支持  
购买不易，淘宝上开发板有售、单芯片商家少且销量3。  

通用的IDE开发环境 ，KEIL 、IAR、ECLIPSE等都有实验例程可用，可是实验项目的快速开发，同时支持JLINK ULINK ISP烧录。  
烧录方面，研发烧录用的工具JLINK STLINK ULINK还支持ISP烧录，量产用烧录工具：锝镨的starProg-MS，轩微烧录器，可连接自动烧录机台。  

 - [恒烁官网](http://www.zbitsemi.com/)
 - [CX32L003系列--32位ARM®Cortex®-M0+内核超低功耗、高性价比微控制器](http://www.zbitsemi.com/display.php?id=44)
 - [CX32L003可替代STM8S003，与新唐003 华大003一样](https://blog.csdn.net/csdndianzi/article/details/106784099)
 - [分享一款性价比超高的国产芯片CX32L003](http://www.bonate.net/bbsele/jishu_1988399_1_1.html)

#### 快速入门

1.	开发环境:
 	-	KEIL/IAR
2.  器件扩展包  
 	-	KEIL官方支持
 	-	使用 IAR 请安装 `[CX32L003 IAR 支持文件](http://www.zbitsemi.com/display.php?id=44)`
3.	新建工程
4.	下载程序，JTAG(4线)或者SWD(2线)方式。
     - 仿真下载， Jlink/Ulink/STlink/DAP-LINK 等在线仿真工具。
     - ISP下载，支持。无需提前下载固件。

#### 参考链接

[M0官方资料](http://www.zbitsemi.com/display.php?id=44)

### 敏矽 ME32S003F6P6

管脚兼容，程序兼容（内核相同）。基于M0内核，需要移植STM8s的程序。Keil官方支持  
无售  

- [ME32S003F6P6](http://www.mesilicon.com/a/lm4/71.html)

### 总结表

从官网及技术支持来看，灵动、华大、芯圣、赛元。

| 厂家  | 型号  |  内核  |  开发工具  |   价格  |   是否测试 |  理由 |
|---|---|---|---|---|---|---|
|  新唐 | N76E003/MS51FB9AE | 1T8051   | Keil 51/IAR 51 |  3.35 / 1.84 2.63管装 1000+  |  是 |
|  航顺 | HK32F030M |  M0  |  Keil/IAR  | 1.548 华秋  2.14 立创  |  是 |  |
| 灵动  | MM32F003/MM32F0010 |  M0  | Keil/IAR   |  3.45 1000+  | 是 |
| 中基国威  | SM51F003 | M0   | Keil/IAR  |  1.14 1000+ |  否 | 无开发板、资料少  |
| 华大  | HC32F002/005 |   M0 |  Keil/IAR | 1.46/2.11/2.4  1000+ |  是 |
| 恒烁  |  CX32L003 |  M0  |Keil/IAR   |  1.5 淘宝 |  是 |
|  芯圣 | HC89S003 |  1T8051  | Keil 51/IAR 51  |  1.71 1000+  |  是 |
|  赛元 | SC92F7003 |  1T8051  | Keil 51/IAR 51 |  1.1415  1000+  |  是 |  |

## STM32G031G8替代

以下公司均无可替代产品

 - 兆易创新（GD32E230G4 / GD32F130G6 有 QFN28类似，引脚不兼容）
 - 雅特力科技
 - 华大（HK32F031G6U6 有 QFN28类似，引脚不兼容）
 - 新唐
 - 合泰（HT66F3185 有 QFN28类似，引脚不兼容）
 - 灵动
 - 芯圣
 - 中颖
 - 敏矽微电子
 - 芯旺
 - 航顺（有 QFN28类似，引脚不兼容）
 - 中基国威
 - 中科芯
 - 恒烁
 - 赛元

## 芯圣HC89S003F4学习笔记


### 开发资料下载
官网资料：[开发工具 > SDK 配套资料 > 资料下载](http://www.holychip.cn/download.php?class_id=102108101)。下载 `SDK-HC89S003F4配套资料V1.3`
解压文件内容如下：
![芯圣官网资料](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/国产单片机MCU/芯圣官网资料.png)
### 硬件平台

[芯圣淘宝官方旗舰店](https://shop155767245.taobao.com/?spm=a1z10.1-c-s.0.0.ef5c2d42FjWxWU)
芯圣SDK-HC89S003F4单片机开发板
![O1CN01wOLWld1fFVoLlGQ8D__2649473977](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/国产单片机MCU/O1CN01wOLWld1fFVoLlGQ8D_!!2649473977.png)
### 软件平台

1. 提前安装 Keil C51，并和谐完成
2. 跟随官方资料中 `1-使用前必读` 中的视频安装 HC-LINK 软件 和 HC-DRIVER 驱动。安装包在官方资料 `开发工具 > 仿真器` 中。最新版本在官网 [开发工具 > 8051仿真器 > 产品详情](http://www.holychip.cn/development.php?class_id=102104101) 中下载。

### 例程编写

软件例程直接参考官方资料中 `参考例程 > HC89S003F4 Register Example V1.0.5.0`。
数据手册参考官方资料中 `数据手册 > HC89S003F4_Datasheet_Ver1.08_cn.pdf`。

#### 时钟配置

资料参考数据手册 `4 系统时钟` 章节。

- CLKCON：时钟控制寄存器。内外部时钟使能、查看晶振状态
- CLKSWR：时钟选择寄存器。时钟源选择、分频系数设置（分频后时钟为 Fosc）
- CLKDIV： 时钟分频寄存器。设置Fosc时钟源分频系数。（分频后时钟为 Fcpu，Fcpu频率不能超过20MHz。）
- XTALCFG：外部晶振配置寄存器

##### 使用内部RC振荡器

参考例程 `CLK-时钟配置`。

```
CLKSWR = 0x51;//选择内部高频RC为系统时钟，内部高频RC 2分频，Fosc=16MHz
CLKDIV = 0x01;//Fosc 1分频得到Fcpu，Fcpu=16MHz
```
系统上电默认选择内部时钟。`CLKCON`不用设置。`XTALCFG`也不用设置。此时我们只要修改分频数即可，首先把32MHz的RC时钟2分频得到
16MHz，然后16MHz再进行1分频使Fcpu等于16MHz。HC89S003F4如果使用内部RC振荡器，那么得到最高的时钟频率就16MHz。

##### 使用外部晶振
```
CLKCON |= 0x04; //使能外部晶振
XTALCFG |= 0x01; //外部晶振选择高频晶振
while((CLKCON&0x80)!=0x80); //等待外部高频晶振起振
CLKSWR = 0xF0; //选择系统时钟为外部晶振
while((CLKSWR&0xC0)!=0xC0); //等待系统时钟切换外部高频晶振完成
CLKCON &=~ 0x02; //关闭内部高频RC
```
这个过程就是先把外面的晶振使能，等待外部晶振起振正常，然后切换到外部时钟，等待切换完成后就把内部RC振荡器关闭，需要注意内部RC在切换外部晶振后才能关闭，任何时候必须保证至少有一个晶振在工作。


#### LED


### 问题

#### 延时错误，时钟错误

执行最简单的 LED 翻转程序时，调试发现延时很慢，计算得出单个时钟频率才 0.66KHz，导致延时计算极慢。可以得出系统工作不正常。

最后瞎测试发现，更改 BOR 复位电压，芯片就正常工作了。原因未知。

1. Options -> Dedug -> Settings
单击 Option
![下载配置_setting](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/国产单片机MCU/下载配置_setting.png)
2. 进入配置界面。修改 BORVS，随便更改一个值，然后重新编译程序并下载。之后再将电压值改回来（2.4V）,重新编译，再下载就好了。
![BORV_设置](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/国产单片机MCU/BORV_设置.png)
#### 仿真下载接口

HC89S003F4 只支持 JTAG 4线仿真下载，不支持 SWD 两线。另外支持 ISP（串口）下载，但需要先使用 HC-LINK 烧写固化 ISP 固件，从而支持 ISP 下载。有另外已出厂固化好 ISP 固件的芯片，到手就可以进行串口下载。