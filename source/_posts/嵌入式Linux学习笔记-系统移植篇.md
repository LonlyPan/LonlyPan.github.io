---
layout: post
title: "嵌入式Linux学习笔记-系统移植篇"
index_img: https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记/1654315471941.png
date: 2022-07-10 22:35
updated: 2022-07-10 22:35
hide: false
## sticky: 100 #置顶，数字越大越靠前
## banner_img: #/img/post_banner.jpg
## comment: false
categories: 01-专业
---


# 系统移植篇

## LInux系统移植概念

操作系统向下管理硬件（I/O，设备接口），向上提供接口（进程管理+文件IO+网络协议+数据库等，被APP软件调用
 
bootloader（U-Boot） -> Linux内核 -> 根文件系统(rootfs) 。这三者一起构成了一个完整的Linux系统，一个可以正常使用、功能完善的Linux系统。

开发板上电后首先运行 SOC 内部 iROM 中固化的代码（BL0），这段代码先对基本的软银见环境（时钟等）初始化，然后检测拨码开关获取启动方式，再将对应存储器（SD卡）中的Uboot搬移到内存，然后跳转到uboot运行
uboot开始运行后首先对开发板软硬件环境初始化，然后将 linux内核、设备树（dtb） 、根文件系统（rootfs）从外部存储器（或网络）搬移到内存，然后跳转到linux运行
linux开始运行，先对系统环境初始化，当系统启动完成后，linux再从内存中（或网络）挂在根文件系统

![1652410941723](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记/1652410941723.png)

根据启动过程得出移植步骤：

 - uboot移植
 - linux内核移植（包含设备树）
 - 根文件系统移植

## uboot简介

嵌入式系统上电后先执行bootloader、先初始化DDR，Flash 等外设，然后将 Linux 内核从 Flash(NAND，NOR FLASH，SD，MMC等)中读取到 DDR 中，最后启动Linux内核。

bootloader和Linux内核的关系就跟PC上的BIOS和Windows的关系一样，bootloader就相当于BIOS。

现成的bootloader软件有很多，比如U-Boot、vivi、RedBoot等等，其中以U-Boot使用最为广泛，为了方便书写，本书会将U-Boot写为uboot。

一般不会直接用uboot官方的U-Boot源码的。uboot官方的uboot源码是给半导体厂商准备的，半导体厂商会下载uboot官方的uboot源码，然后将自家相应的芯片移植进去。也就是说半导体厂商会自己维护一个版本的uboot，这个版本的uboot相当于是他们定制的。既然是定制的，那么肯定对自家的芯片支持会很全，虽然uboot官网的源码中一般也会支持他们的芯片，但是绝对是没有半导体厂商自己维护的uboot全面

NXP官方uboot下载地址：https://source.codeaurora.org/external/imx/ 

正点原子教程和NXP官方教程中的 linux与uboot下载链接都失效了，上面是最新的链接。这个网站是Linux基金会维护的一个开源网站。有点类似github。
在页面最底部找到 uboot-imx，点进去。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-系统移植篇/1657549783413.png)


点开Tags栏。记住下面的链接地址。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记/1657425111997.png)

找到你想下载的版本，比如我想下载rel_imx_4.1.15_2.1.0_ga版本。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记/1657425180071.png)

通过git clone下载（电脑需预先安装git），地址就是上面记住的链接地址，用-b指定你想下载的tag或分支。注意空格
```
git clone https://source.codeaurora.org/external/imx/linux-imx -b rel_imx_4.1.15_2.1.0_ga
```
> uboot并不是越新越好，新版本只是添加了对新处理器得到支持，越新越冗余

后面我们学习uboot移植的时候就是使用 uboot-imx-rel_imx_4.1.15_2.1.0_ga.tar.bz2 版本

**参考资料：**
- [nfs下载镜像报错File lookup fail、“TTTTTTTTTTTTTTT”](https://blog.csdn.net/qq_41709234/article/details/123160029)
- [NXP IMX6ULL老版本源码下载方法](https://blog.csdn.net/huohongpeng/article/details/106472024)
## U-Boot移植

uboot 移植的一般流程：
1. 在 uboot 中找到参考的开发平台，一般是原厂的开发板。
2. 参考原厂开发板移植 uboot 到我们所使用的开发板
### alentek-uboot编译烧录测试

1. 正点原子 uboot 复制到 ubuntu的linux -> uboot -> alentek_uboot下，解压

```
tar -vxjf uboot-imx-2016.03-2.1.0-gee88051-v1.6.tar.bz2
```

2. 创建sh文件
```
vim mx6ull_alientek_emmc.sh
```
填入内容：
```
#!/bin/bash
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- distclean
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- mx6ull_14x14_ddr512_emmc_defconfig
make V=1 ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- -j16
```
编译：
```
chmod 777 mx6ull_alientek_emmc.sh //给予可执行权限
./mx6ull_alientek_emmc.sh
```
烧录：
```
chmod 777 imxdownload //给予 imxdownload 可执行权限
./imxdownload u-boot.bin /dev/sdd //烧写到 SD 卡中，不能烧写到/dev/sda 或 sda1 里面
```

### NXP-uboot编译烧录测试

首先在 Ubuntu 中安装 ncurses 库， 否则编译会报错，安装命令如下：
`sudo apt-get install libncurses5-dev`

1. 原版Uboot复制到ubuntu中，解压
```
tar -jvxf uboot-imx-rel_imx_4.1.15_2.1.0_ga.tar.bz2 
```
2. 创建sh文件
```
vim mx6ull_14x14_emmc.sh
```
填入内容：
```
#!/bin/bash
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- distclean
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- mx6ull_14x14_evk_emmc_defconfig
make V=1 ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- -j16
```
3. 编译：
```
chmod 777 mx6ull_14x14_emmc.sh //给予可执行权限
./mx6ull_14x14_emmc.sh
```
4. 烧录：
```
chmod 777 imxdownload //给予 imxdownload 可执行权限
./imxdownload u-boot.bin /dev/sdd //烧写到 SD 卡中，不能烧写到/dev/sda 或 sda1 里面
```

运行输出

```
U-Boot 2016.03 (Jun 12 2022 - 10:24:10 +0800)

CPU:   Freescale i.MX6ULL rev1.1 69 MHz (running at 396 MHz)
CPU:   Industrial temperature grade (-40C to 105C) at 46C
Reset cause: POR
Board: MX6ULL 14x14 EVK
I2C:   ready
DRAM:  512 MiB
MMC:   FSL_SDHC: 0, FSL_SDHC: 1
unsupported panel ATK-LCD-7-1024x600
In:    serial
Out:   serial
Err:   serial
switch to partitions #0, OK
mmc0 is current device
Net:   FEC1
Normal Boot
Hit any key to stop autoboot:  0
```
可以发现 CPU频率显示69MHz，如果使用 528MHz 的 I.MX6ULL，此处会显示主频为 528MHz。但是如果使用 800MHz 的 I.MX6ULL 的话此处会显示 69MHz，这个是 uboot 文件的bug，只作为输出，不影响实际运行，可以不用管。不管是528MHz还是800MHz的I.MX6ULL，此时都运行在 396MHz。

可以打开 arch\arm\cpu\armv7\mx6\soc.c 文件，修改这个小bug，修改后的如下：
- 注释代码就是原来的代码，其下面的时修改后的，共两处
  
```  
#define OCOTP_CFG3_SPEED_528MHZ 1
//#define OCOTP_CFG3_SPEED_696MHZ 2
#define OCOTP_CFG3_SPEED_792MHZ 2

u32 get_cpu_speed_grade_hz(void)
{
	struct ocotp_regs *ocotp = (struct ocotp_regs *)OCOTP_BASE_ADDR;
	struct fuse_bank *bank = &ocotp->bank[0];
	struct fuse_bank0_regs *fuse =
		(struct fuse_bank0_regs *)bank->fuse_regs;
	uint32_t val;

	val = readl(&fuse->cfg3);
	val >>= OCOTP_CFG3_SPEED_SHIFT;
	val &= 0x3;

	if (is_cpu_type(MXC_CPU_MX6UL) || is_cpu_type(MXC_CPU_MX6ULL)) {
		if (val == OCOTP_CFG3_SPEED_528MHZ)
			return 528000000;
		else if (val == OCOTP_CFG3_SPEED_792MHZ)
			//return 69600000;
			return 792000000;
		else
			return 0;
	}
```

**参考资料：**
- [修复使用 792MHz 的 I.MX6ULL，uboot显示主频为 69MHz](http://www.openedv.com/forum.php?mod=viewthread&tid=315050&highlight=uboot%2B%D6%F7%C6%B5)

### uboot修改移植

#### 1. 添加开发板默认配置文件

前置配置，不同的配置决定了编译时会调用不同的文件（包括后面要修改的），是最高一级的配置文件。

1.  创建自定义配置文件  
```
cd configs
cp mx6ull_14x14_evk_emmc_defconfig mx6ull_alientek_emmc_defconfig
```
修改内容
```
CONFIG_SYS_EXTRA_OPTIONS="IMX_CONFIG=board/freescale/mx6ull_alientek_emmc/imximage.cfg,MX6ULL_EVK_EMMC_REWORK"
CONFIG_ARM=y
CONFIG_ARCH_MX6=y
CONFIG_TARGET_MX6ULL_ALIENTEK_EMMC=y
CONFIG_CMD_GPIO=y
```

#### 2. 添加开发板对应的头文件

里面有很多宏定义，这些宏定义基本用于配置 uboot，也有一些
I.MX6ULL 的配置项目。

拷贝头文件
```
cp include/configs/mx6ullevk.h mx6ull_alientek_emmc.h
```
将
```
#ifndef __MX6ULLEVK_CONFIG_H
#define __MX6ULLEVK_CONFIG_H
```
改为：
```
#ifndef __MX6ULL_ALIENTEK_EMMC_CONFIG_H
#define __MX6ULL_ALIENTEK_EMMC_CONFIG_H
```
其他内容默认

#### 3. 添加开发板对应的板级文件夹

uboot 中每个板子都有一个对应的文件夹来存放板级文件，比如开发板上外设驱动文件等
等。

复制 mx6ullevk文件夹，将其重命名为 mx6ull_alientek_emmc，命令如下：
```
cd board/freescale/
cp mx6ullevk/ -r mx6ull_alientek_emmc
```
进 入mx6ull_alientek_emmc目 录 中 ， 将 其 中 的mx6ullevk.c文 件 重 命 名 为mx6ull_alientek_emmc.c，命令如下：
```
cd mx6ull_alientek_emmc
mv mx6ullevk.c mx6ull_alientek_emmc.c
```

1). 修改 mx6ull_alientek_emmc 目录下的 Makefile 文件
- 重点是第 6 行的 obj-y，改为 mx6ull_alientek_emmc.o，这样才会编译 mx6ull_alientek_emmc.c
这个文件。
```
# (C) Copyright 2015 Freescale Semiconductor, Inc.
#
# SPDX-License-Identifier:	GPL-2.0+
#

obj-y  := mx6ull_alientek_emmc.o

extra-$(CONFIG_USE_PLUGIN) :=  plugin.bin
$(obj)/plugin.bin: $(obj)/plugin.o
	$(OBJCOPY) -O binary --gap-fill 0xff $< $@
```
2). 修改 mx6ull_alientek_emmc 目录下的 imximage.cfg 文件
将 imximage.cfg 中的下面一句：
```
PLUGIN board/freescale/mx6ullevk/plugin.bin 0x00907000
```
改为：
```
PLUGIN board/freescale/mx6ull_alientek_emmc /plugin.bin 0x00907000
```
3). 修改 mx6ull_alientek_emmc 目录下的 Kconfig 文件
修改 Kconfig 文件，修改后的内容如下：
```
if TARGET_MX6ULL_ALIENTEK_EMMC

config SYS_BOARD
	default "mx6ull_alientek_emmc"

config SYS_VENDOR
	default "freescale"

config SYS_SOC
	default "mx6"

config SYS_CONFIG_NAME
	default "mx6ull_alientek_emmc"

endif
```
4). 修改 mx6ull_alientek_emmc 目录下的 MAINTAINERS 文件
```
MX6ULL_ALIENTEK_EMMC BOARD
M:	Peng Fan <peng.fan@nxp.com>
S:	Maintained
F:	board/freescale/mx6ull_alientek_emmc/
F:	include/configs/mx6ull_alientek_emmc.h
```

#### 4. 修改 U-Boot 图形界面配置文件

uboot 是支持图形界面配置，关于 uboot 的图形界面配置下一章会详细的讲解。修改文件
arch/arm/cpu/armv7/mx6/Kconfig(如果用的 I.MX6UL 的话，应该修改 arch/arm/Kconfig 这个文
件)，在 207 行加入如下内容：
```
config TARGET_MX6ULL_ALIENTEK_EMMC
	bool "Support mx6ull_alientek_emmc"
	select MX6ULL
	select DM
	select DM_THERMAL
```
在最后一行的 endif 的前一行添加如下内容：
```
source "board/freescale/mx6ull_alientek_emmc/Kconfig"
```

#### 5. 编译新 uboot
在 uboot 根目录下新建一个名为 mx6ull_alientek_emmc.sh 的 shell 脚本，在这个 shell 脚本
里面输入如下内容：
```
#!/bin/bash
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- distclean
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- mx6ull_alientek_emmc_defconfig
make V=1 ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- -j16
```

```
chmod 777 mx6ull_alientek_emmc.sh
//给予可执行权限，一次即可
./mx6ull_alientek_emmc.sh
//运行脚本编译 uboot
```
等待编译完成 ， 编译完成后输入如下命令 ， 查看一下添加的mx6ull_alientek_emmc.h 这个头文件有没有被引用。
如果有很多文件都引用了 mx6ull_alientek_emmc.h 这个头文件，那就说明新板子添加成功，如图所示：
```grep -nR "mx6ull_alientek_emmc.h"```
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记/1655001869905.png)

#### 6. LCD驱动修改

一般 uboot 中修改驱动基本都是在 xxx.h 和 xxx.c 这两个文件中进行的，xxx 为板子名称，
比如 mx6ull_alientek_emmc.h 和 mx6ull_alientek_emmc.c 这两个文件。
一般修改 LCD 驱动重点注意以下几点：
①、LCD 所使用的 GPIO，查看 uboot 中 LCD 的 IO 配置是否正确。
②、LCD 背光引脚 GPIO 的配置。
③、LCD 配置参数是否正确。
正点原子的 I.MX6U-ALPHA 开发板 LCD 原理图和 NXP 官方 I.MX6ULL 开发板一致，所以 IO 部分就不用修改了。需要修改的之后 LCD 参数，
打开文件 mx6ull_alientek_emmc.c，找到如下所示内容：
```
struct display_info_t const displays[] = {{
	.bus = MX6UL_LCDIF1_BASE_ADDR,
	.addr = 0,
	.pixfmt = 24,
	.detect = NULL,
	.enable	= do_enable_parallel_lcd,
	.mode	= {
		.name			= "TFT7016",
		.xres           = 1024,
		.yres           = 600,
		.pixclock       = 19531,
		.left_margin    = 140,
		.right_margin   = 160,
		.upper_margin   = 20,
		.lower_margin   = 12,
		.hsync_len      = 20,
		.vsync_len      = 3,
		.sync           = 0,
		.vmode          = FB_VMODE_NONINTERLACED
} } };
```
 
- pixfmt 是像素格式，也就是一个像素点是多少位，如果是 RGB565 的话就是 16 位，如果是 888 的话就是 24 位，一般使用 RGB888。
 - name：LCD 名字，要和环境变量中的 panel 相等。
 - xres、yres：LCD X 轴和 Y 轴像素数量。
 - pixclock：像素时钟，每个像素时钟周期的长度，单位为皮秒。
	 - 以正点原子的 7 寸 1024\*600 分辨率的屏幕(ATK7016)为例，屏幕要求的像素时钟为 51.2MHz，因此：pixclock=(1/51200000)*10^12=19531
 - left_margin：HBP，水平同步后肩。
 - right_margin：HFP，水平同步前肩。
 - upper_margin：VBP，垂直同步后肩。
 - lower_margin：VFP，垂直同步前肩。
 - hsync_len：HSPW，行同步脉宽。
 - vsync_len：VSPW，垂直同步脉宽。
 - vmode：大多数使用 FB_VMODE_NONINTERLACED，也就是不使用隔行扫描。

以上参数都可以从lCD的规格书中获取。

打开 mx6ull_alientek_emmc.h，找到所有如下语句：
`panel=TFT43AB`
将其改为：
`panel=TFT7016`

也就是设置 panel 为 TFT7016，panel 的值要与示例代码中的.name 成员变量的值一致。修改完成以后重新编译一遍 uboot 并烧写到 SD 中启动。重启以后 LCD 驱动一般就会工作正常了，LCD 上回显示 NXP 的 logo。

但是有可能会遇到LCD 并没有工作，还是黑屏，这是什么原因呢？在 uboot 命令模式输入“print”来查看环境变
量 panel 的值，会发现 panel 的值要是 TFT43AB(或其他的，反正不是 TFT7016)，这是因为之前有将环境变量保存到 EMMC 中，uboot 启动以后会先从 EMMC 中读取环境变量，如果 EMMC 中没有环境变量的话才会使用 mx6ull_alientek_emmc.h 中的默认环境变量。如果 EMMC 中的环境变量 panel 不等于 TFT7016，那么 LCD 显示肯定不正常.。
在uboot 中修改 panel 的值为 TFT7016 即可，在 uboot 的命令模式下输入如下命令：
```
setenv panel TFT7016
saveenv
```
#### 7. 网络驱动修改

1. 网络 PHY 地址修改
```
331 #define CONFIG_FEC_ENET_DEV  1
332
333 #if (CONFIG_FEC_ENET_DEV == 0)
334 #define IMX_FEC_BASE
ENET_BASE_ADDR
335 #define CONFIG_FEC_MXC_PHYADDR 0x2
336 #define CONFIG_FEC_XCV_TYPE RMII
337 #elif (CONFIG_FEC_ENET_DEV == 1)
338 #define IMX_FEC_BASE
ENET2_BASE_ADDR
339 #define CONFIG_FEC_MXC_PHYADDR 0x1
340 #define CONFIG_FEC_XCV_TYPE  RMII
341 #endif
342 #define CONFIG_ETHPRIME   "FEC"
343
344 #define CONFIG_PHYLIB
345 #define CONFIG_PHY_MICREL
346 #endif
```
第 331 行的宏 CONFIG_FEC_ENET_DEV 用于选择使用哪个网口，默认为 1，也就是选择ENET2。第 335 行为 ENET1 的 PHY 地址，默认是 0X2，第 339 行为 ENET2 的 PHY 地址，默认为 0x1。正点原子的 I.MX6U-ALPHA 开发板 ENET1 的 PHY 地址为0X0，ENET2 的 PHY 地址为 0X1，所以需要将第 335 行的宏 CONFIG_FEC_MXC_PHYADDR改为 0x0。

第 345 行定了一个宏 CONFIG_PHY_MICREL，此宏用于使能 uboot 中 Micrel 公司的 PHY驱动，KSZ8081 这颗 PHY 芯片就是 Micrel 公司生产的，不过 Micrel 已经被 Microchip 收购了。如果要使用 LAN8720A，那么就得将CONFIG_PHY_MICREL 改为 CONFIG_PHY_SMSC，也就是使能 uboot 中的 SMSC 公司中的 PHY 驱动，因为 LAN8720A 就是 SMSC 公司生产的。所
以示例代码 33.2.7.1 有三处要修改：
①、修改 ENET1 网络 PHY 的地址。
②、修改 ENET2 网络 PHY 的地址。
③、使能 SMSC 公司的 PHY 驱动。
修改后的网络 PHY 地址参数如下所示：
```
#define CONFIG_FEC_ENET_DEV		1

#if (CONFIG_FEC_ENET_DEV == 0)
#define IMX_FEC_BASE			ENET_BASE_ADDR
#define CONFIG_FEC_MXC_PHYADDR          0x0
#define CONFIG_FEC_XCV_TYPE             RMII
#elif (CONFIG_FEC_ENET_DEV == 1)
#define IMX_FEC_BASE			ENET2_BASE_ADDR
#define CONFIG_FEC_MXC_PHYADDR		0x1
#define CONFIG_FEC_XCV_TYPE		RMII
#endif
#define CONFIG_ETHPRIME			"FEC"

#define CONFIG_PHYLIB
#define CONFIG_PHY_SMSC
#endif
```
2. 删除 uboot 中 74LV595 的驱动代码
uboot 中网络 PHY 芯片地址修改完成以后就是网络复位引脚的驱动修改了，打开mx6ull_alientek_emmc.c，找到如下代码：
```
#define IOX_SDI IMX_GPIO_NR(5, 10)
#define IOX_STCP IMX_GPIO_NR(5, 7)
#define IOX_SHCP IMX_GPIO_NR(5, 11)
#define IOX_OE  IMX_GPIO_NR(5, 8)
```
以 IOX 开头的宏定义是 74LV595 的相关 GPIO，因为 NXP 官方I.MX6ULL EVK 开发板使用 74LV595 来扩展 IO，两个网络的复位引脚就是由 74LV595 来控制的。正点原子的 I.MX6U-ALPHA 开发板并没有使用 74LV595，因此我们将代码删除掉，替换为如下所示代码：
```
#define ENET1_RESET IMX_GPIO_NR(5, 7)
#define ENET2_RESET IMX_GPIO_NR(5, 8)
```
ENET1 的复位引脚连接到 SNVS_TAMPER7 上，对应 GPIO5_IO07，ENET2 的复位引脚连接到 SNVS_TAMPER8 上，对应 GPIO5_IO08。继续在 mx6ull_alientek_emmc.c 中找到如下代码：
- 代码是 74LV595 的 IO 配置参数结构体，将其删除掉。
```
static iomux_v3_cfg_t const iox_pads[] = {
	/* IOX_SDI */
	MX6_PAD_BOOT_MODE0__GPIO5_IO10 | MUX_PAD_CTRL(NO_PAD_CTRL),
	/* IOX_SHCP */
	MX6_PAD_BOOT_MODE1__GPIO5_IO11 | MUX_PAD_CTRL(NO_PAD_CTRL),
	/* IOX_STCP */
	MX6_PAD_SNVS_TAMPER7__GPIO5_IO07 | MUX_PAD_CTRL(NO_PAD_CTRL),
	/* IOX_nOE */
	MX6_PAD_SNVS_TAMPER8__GPIO5_IO08 | MUX_PAD_CTRL(NO_PAD_CTRL),
};
```

示例继续在mx6ull_alientek_emmc.c 中找到函数 iox74lv_init 函数是 74LV595 的初始化函数，iox74lv_set 函数用于控制 74LV595 的 IO 输出电平，将这两个函数全部删除掉！
```
static void iox74lv_init(void)
{
	int i;
	gpio_direction_output(IOX_OE, 0);
	......
	gpio_direction_output(IOX_STCP, 1);
};

void iox74lv_set(int index)
{
	int i;
	......
	gpio_direction_output(IOX_STCP, 1);
};
```

在 mx6ull_alientek_emmc.c 中找到 board_init 函数，此函数是板子初始化函数，会被 board_init_r 调用，board_init 函数内容如下：
- board_init 会调用 imx_iomux_v3_setup_multiple_pads 和 iox74lv_init 这两个函数来初始化 74lv595 的 GPIO，将这两行删除掉。至此，mx6ull_alientek_emmc.c 中关于 74LV595 芯片的驱动代码都删除掉了，接下来就是添加 I.MX6U-ALPHA 开发板两个网络复位引脚了。
```
int board_init(void)
{
	......
	imx_iomux_v3_setup_multiple_pads(iox_pads, ARRAY_SIZE(iox_pads));
	iox74lv_init();
	......
	return 0;
}
```
3. 添加 I.MX6U-ALPHA 开发板网络复位引脚驱动
在 mx6ull_alientek_emmc.c 中找到如下所示代码并修改如下：
- 添加了 651和667 两行代码，分别是 ENET1 和 ENET2 的复位 IO 配置参数。
```
640 static iomux_v3_cfg_t const fec1_pads[] = {
641 	MX6_PAD_GPIO1_IO06__ENET1_MDIO | MUX_PAD_CTRL(MDIO_PAD_CTRL),
642 	MX6_PAD_GPIO1_IO07__ENET1_MDC | MUX_PAD_CTRL(ENET_PAD_CTRL),
......
649 	MX6_PAD_ENET1_RX_ER__ENET1_RX_ER | MUX_PAD_CTRL(ENET_PAD_CTRL),
650 	MX6_PAD_ENET1_RX_EN__ENET1_RX_EN | MUX_PAD_CTRL(ENET_PAD_CTRL),
651		MX6_PAD_SNVS_TAMPER7__GPIO5_IO07 | MUX_PAD_CTRL(NO_PAD_CTRL),
652 };
653
654 static iomux_v3_cfg_t const fec2_pads[] = {
655 	MX6_PAD_GPIO1_IO06__ENET2_MDIO | MUX_PAD_CTRL(MDIO_PAD_CTRL),
656 	MX6_PAD_GPIO1_IO07__ENET2_MDC | MUX_PAD_CTRL(ENET_PAD_CTRL),
......
665 	MX6_PAD_ENET2_RX_EN__ENET2_RX_EN | MUX_PAD_CTRL(ENET_PAD_CTRL),
666 	MX6_PAD_ENET2_RX_ER__ENET2_RX_ER | MUX_PAD_CTRL(ENET_PAD_CTRL),
667 	MX6_PAD_SNVS_TAMPER8__GPIO5_IO08 | MUX_PAD_CTRL(NO_PAD_CTRL),
668 };
```
继续在文件 mx6ull_alientek_emmc.c 中找到函数 setup_iomux_fec，函数 setup_iomux_fec 就是根据 fec1_pads 和 fec2_pads 这两个网络 IO 配置数组来初始化I.MX6ULL 的网络 IO。我们需要在其中添加网络复位 IO 的初始化代码，并且复位一下 PHY 芯片，修改后的 setup_iomux_fec 函数如下
- 第 676 行~679 行和第 685 行~688 行分别对应 ENET1 和 ENET2 的复位 IO 初始化，将这两个 IO 设置为输出并且硬件复位一下 LAN8720A，这个硬件复位很重要！否则可能导致 uboot 无法识别 LAN8720A。
```
668 static void setup_iomux_fec(int fec_id)
669 {
670 	if (fec_id == 0)
671 	{
672
673 		imx_iomux_v3_setup_multiple_pads(fec1_pads,
674 				ARRAY_SIZE(fec1_pads));
675
676 		gpio_direction_output(ENET1_RESET, 1);
677 		gpio_set_value(ENET1_RESET, 0);
678 		mdelay(20);
679 		gpio_set_value(ENET1_RESET, 1);
680 	}
681 	else
682 	{
683 		imx_iomux_v3_setup_multiple_pads(fec2_pads,
684 		ARRAY_SIZE(fec2_pads));
685 		gpio_direction_output(ENET2_RESET, 1);
686 		gpio_set_value(ENET2_RESET, 0);
687 		mdelay(20);
688 		gpio_set_value(ENET2_RESET, 1);
689 	}
690 }
```

4. 修改 drivers/net/phy/phy.c 文件中的函数 genphy_update_link
uboot 中的 LAN8720A 驱动有点问题，打开文件drivers/net/phy/phy.c，找到函数 genphy_update_link，这是个通用 PHY 驱动函数，此函数用于更新 PHY 的连接状态和速度。使用 LAN8720A 的时候需要在此函数中添加一些代码，修改后的
函数 genphy_update_link 如下所示：
```
221 int genphy_update_link(struct phy_device *phydev)
222 {
223 	unsigned int mii_reg;
224
225 	#ifdef CONFIG_PHY_SMSC
226 		static int lan8720_flag = 0;
227 		int bmcr_reg = 0;
228 		if (lan8720_flag == 0) {
229 			bmcr_reg = phy_read(phydev, MDIO_DEVAD_NONE, MII_BMCR);
230 			phy_write(phydev, MDIO_DEVAD_NONE, MII_BMCR, BMCR_RESET);
231 			while(phy_read(phydev, MDIO_DEVAD_NONE, MII_BMCR) & 0X8000) {
232 				udelay(100);
233 			}
234 			phy_write(phydev, MDIO_DEVAD_NONE, MII_BMCR, bmcr_reg);
235 			lan8720_flag = 1;
236 		}
237 	#endif
238
239 	/*
240 	* Wait if the link is up, and autonegotiation is in progress
241 	* (ie - we're capable and it's not done)
242 	*/
243 	mii_reg = phy_read(phydev, MDIO_DEVAD_NONE, MII_BMSR);
......
291
292 	return 0;
293 }
```

225 行~237 行就是新添加的代码，为条件编译代码段，只有使用 SMSC 公司的 PHY 这段代码才会执行(目前只测试了 LAN8720A，SMSC 公司其他的芯片还未测试)。第 229 行读取LAN8720A 的 BMCR 寄存器(寄存器地址为 0)，此寄存器为 LAN8720A 的配置寄存器，这里先读取此寄存器的默认值并保存起来。230 行向寄存器 BMCR 寄存器写入 BMCR_RESET(值为0X8000)，因为 BMCR 的 bit15 是软件复位控制位，因此 230 行就是软件复位 LAN8720A，复位
完成以后此位会自动清零。第 231~233 行等待 LAN8720A 软件复位完成，也就是判断 BMCR的 bit15 位是否为 1，为 1 的话表示还没有复位完成。第 234 行重新向 BMCR 寄存器写入以前的值，也就是 229 行读出的那个值。

至此网络的复位引脚驱动修改完成，重新编译 uboot，然后将 u-boot.bin 烧写到 SD 卡中并启动，uboot 启动信息所示：
```
U-Boot 2016.03 (Jun 12 2022 - 10:24:10 +0800)

CPU:   Freescale i.MX6ULL rev1.1 69 MHz (running at 396 MHz)
CPU:   Industrial temperature grade (-40C to 105C) at 48C
Reset cause: POR
Board: MX6ULL 14x14 EVK
I2C:   ready
DRAM:  512 MiB
MMC:   FSL_SDHC: 0, FSL_SDHC: 1
unsupported panel ATK-LCD-7-1024x600
In:    serial
Out:   serial
Err:   serial
switch to partitions #0, OK
mmc0 is current device
Net:   FEC1
Normal Boot
Hit any key to stop autoboot:  0
```
从图 33.2.6.4 中可以看到“Net： FEC1”这一行，提示当前使用的 FEC1 这个网口，也就是 ENET2。在 uboot 中使用网络之前要先设置几个环境变量，命令如下：
```
setenv ipaddr 192.168.1.55          //开发板 IP 地址
setenv ethaddr b8:ae:1d:01:00:00    //开发板网卡 MAC 地址
setenv gatewayip 192.168.1.1	    //开发板默认网关
setenv netmask 255.255.255.0	    //开发板子网掩码
setenv serverip 192.168.1.250	    //服务器地址，也就是 Ubuntu 地址
saveenv	                            //保存环境变量
```
设置好环境变量以后就可以在 uboot 中使用网络了，用网线将 I.MX6U-ALPHA 上的 ENET2与电脑或者路由器连接起来，保证开发板和电脑在同一个网段内，通过 ping 命令来测试一下网络连(与ubuntu主机))，命令如下：
ping 192.168.1.250

如果主机一直ping不通，请尝试关闭ubuntu，退出虚拟机（包括后台），然后重新打开虚拟机和ubuntu，再尝试使用开发板ping主机网络。
>ps：一次使用时，重启了路由器，PC电脑无线网络重连（我的开发板和PC都连接到路由器）。然后开发板ping网络不同，重启ubuntu也不行，在ubuntu打开浏览器也没有网络，于是退出虚拟机，重开。后一切正常。

如果ping或dhcp后系统不断重启（如下所示），那是因为交叉编译链版本太高，不兼容，请参考 **开发环境搭建** 章节，重新安装旧版的编译环境。

```

=> ping 192.168.0.198
FEC1 Waiting for PHY auto negotiation to complete... done
Using FEC1 device
data abort
pc : [<9ff83ad0>]          lr : [<9ff84d98>]
reloc pc : [<8783bad0>]    lr : [<8783cd98>]
sp : 9ef45d08  ip : 00000000     fp : 9ff53520
r10: 00000002  r9 : 9ef45eb8     r8 : 00000000
r7 : 00000001  r6 : 00000000     r5 : 0000002a  r4 : 9ffed0ce
r3 : 14000045  r2 : bc00a8c0     r1 : 9ef45d10  r0 : 9ffed0ce
Flags: nZCv  IRQs off  FIQs off  Mode SVC_32
Resetting CPU ...

resetting ...


=> dhcp
FEC1 Waiting for PHY auto negotiation to complete... done
BOOTP broadcast 1
data abort
pc : [<9ff821e0>]          lr : [<9ff839e4>]
reloc pc : [<8783a1e0>]    lr : [<8783b9e4>]
sp : 9ef45cc0  ip : 00000000     fp : 9ff534c8
r10: 00000001  r9 : 9ef45eb8     r8 : 9ffecbe8
r7 : 0000000e  r6 : 9ffeef30     r5 : 00000000  r4 : 9ffed0ce
r3 : 00060101  r2 : 00000008     r1 : 9ffed0a2  r0 : 0000000e
Flags: nZCv  IRQs off  FIQs off  Mode SVC_32
Resetting CPU ...

resetting ...

```

#### 8. 开发板名称修改
在 uboot 启动信息中会有“Board: MX6ULL 14x14 EVK”这一句，也就是说板子名字为“MX6ULL 14x14 EVK”，要将其改为我们所使用的板子名字，比如“MX6ULL ALIENTEK EMMC”或者“MX6ULL ALIENTEK NAND”。打开文件 mx6ull_alientek_emmc.c，找到函数
checkboard，将其改为如下所示内容：
```
int checkboard(void)
{
if (is_mx6ull_9x9_evk())
puts("Board: MX6ULL 9x9 EVK\n");
else
puts("Board: MX6ULL ALIENTEK EMMC\n");
return 0;
}
```
至此 uboot 的驱动部分就修改完成了，uboot 移植也完成了

#### 9. uboot 启动linux测试

uboot 的最终目的就是启动 Linux 内核，所以需要通过启动 Linux 内核来判断 uboot 移植是否成功。
> uboot在linux启动后，就失效了，但会传递一些信息给linux
> 所以uboot中的设备驱动是不能在linux中使用的，所以linux中也要有上面移植时的那些设备驱动，这在后面linux驱动开发再讲解。

##### 从 EMMC 启动 Linux 系统
从 EMMC 启动也就是将编译出来的 Linux 镜像文件 zImage 和设备树文件保存在 EMMC中，uboot 从 EMMC 中读取这两个文件并启动，这个是我们产品最终的启动方式。但是我们目前还没有讲解如何移植 linux 和设备树文件，以及如何将 zImage 和设备树文件保存到 EMMC中。不过大家拿到手的 I.MX6U-ALPHA 开发板(EMMC 版本)已经将 zImage 文件和设备树文件烧写到了 EMMC 中，所以我们可以直接读取来测试。先检查一下 EMMC 的分区 1 中有没有zImage 文件和设备树文件，输入命令

```ls mmc 1:1```
结果下所示：
```
=> ls mmc 1:1
    38823   imx6ull-14x14-emmc-10.1-1280x800-c.dtb
    38823   imx6ull-14x14-emmc-4.3-480x272-c.dtb
    38823   imx6ull-14x14-emmc-4.3-800x480-c.dtb
    38823   imx6ull-14x14-emmc-7-1024x600-c.dtb
    38823   imx6ull-14x14-emmc-7-800x480-c.dtb
    39655   imx6ull-14x14-emmc-hdmi.dtb
    39563   imx6ull-14x14-emmc-vga.dtb
  6786272   zimage

8 file(s), 0 dir(s)
```

uboot启动，进入命令模式，设置如下
```
setenv bootargs 'console=ttymxc0,115200 root=/dev/mmcblk1p2 rootwait rw'
setenv bootcmd 'mmc dev 1; fatload mmc 1:1 80800000 zImage; fatload mmc 1:1 83000000 imx6ull-14x14-emmc-7-1024x600-c.dtb; bootz 80800000 - 83000000;'
saveenv
```
设置好以后直接输入 boot，或者 run bootcmd 即可启动 Linux 内核，如果 Linux 内核启动成功的话就会输出如下的启动信息：
```
=> boot
switch to partitions #0, OK
mmc1(part 0) is current device
reading zImage
6786272 bytes read in 226 ms (28.6 MiB/s)
reading imx6ull-14x14-emmc-7-1024x600-c.dtb
38823 bytes read in 20 ms (1.9 MiB/s)
Kernel image @ 0x80800000 [ 0x000000 - 0x678ce0 ]
## Flattened Device Tree blob at 83000000
   Booting using the fdt blob at 0x83000000
   Using Device Tree in place at 83000000, end 8300c7a6

Starting kernel ...

[    0.000000] Booting Linux on physical CPU 0x0
......
```

##### 从网络启动 Linux 系统

> 前提必须先设置好开发板的网络参数和服务器地址。详见上文 `网络驱动修改`
>  
从网络启动 linux 系统的唯一目的就是为了调试！不管是为了调试 linux 系统还是 linux 下的驱动。每次修改 linux 系统文件或者 linux 下的某个驱动以后都要将其烧写到 EMMC 中去测试，这样太麻烦了。我们可以设置 linux 从网络启动，也就是将 linux 镜像文件和根文件系统都放到 Ubuntu 下某个指定的文件夹中，这样每次重新编译 linux 内核或者某个 linux 驱动以后只需要使用 cp 命令将其拷贝到这个指定的文件夹中即可，这样就不用需要频繁的烧写 EMMC。我们可以通过 nfs 或者 tftp 从 Ubuntu 中下载 zImage 和设备树文件，根文件系统的话也可以通过 nfs 挂载。

这里我们使用 tftp 从 Ubuntu 中下载 zImage 和设备树文件，前提是要将 zImage 和设备树文件放到 Ubuntu 下的 tftp 目录中

需要在 Ubuntu 上搭建 TFTP 服务器，需要安装 tftp-hpa 和 tftpd-hpa，命令如下：
```
sudo apt-get install tftp-hpa tftpd-hpa
sudo apt-get install xinetd
```
在用户目录下新建一个目录存放文件，命令如下：
```
mkdir /home/lonly/linux/tftpboot
chmod 777 /home/lonly/linux/tftpboot
```
新建文件`sudo vi /etc/xinetd.d/tftp`，然后在里面输入如下内容：

```
server tftp
{
	socket_type	= dgram
	protocol	= udp
	wait	= yes
	user	= root
	server	= /usr/sbin/in.tftpd
	server_args	= -s /home/lonly/linux/tftpboot/
	disable	= no
	per_source	= 11
	cps	= 100 2
	flags	= IPv4
}
```
完了以后启动 tftp 服务，命令如下：
`sudo service tftpd-hpa start`
打开 `sudo vi /etc/default/tftpd-hpa` 文件，将其修改为如下所示内容：
```
# /etc/default/tftpd-hpa

TFTP_USERNAME="tftp"
TFTP_DIRECTORY="/home/lonly/linux/tftpboot"
TFTP_ADDRESS=":69"
TFTP_OPTIONS="-l -c -s"
```
重启 tftp 服务器：
`sudo service tftpd-hpa restart`

将 zImage 镜像文件 和 设备树 拷贝到 tftpboot 文件夹中，并且给予 zImage 相应的权限，命令如下：
```
cp zImage /home/lonly/linux/tftpboot/
cp imx6ull-14x14-emmc-7-1024x600-c.dtb /home/lonly/linux/tftpboot/
cd /home/lonly/linux/tftpboot/
chmod 777 zImage
chmod 777 imx6ull-14x14-emmc-7-1024x600-c.dtb
```

uboot启动，设置如下
```
setenv bootargs 'console=ttymxc0,115200 root=/dev/mmcblk1p2 rootwait rw'
setenv bootcmd 'tftp 80800000 zImage; tftp 83000000 imx6ull-14x14-emmc-7-1024x600-c.dtb; bootz 80800000 - 83000000'
saveenv
```
重启`boot`

显示如下：
```
=> boot
Using FEC1 device
TFTP from server 192.168.0.254; our IP address is 192.168.0.111
Filename 'zImage'.
Load address: 0x80800000
Loading: #################################################################
         #################################################################
         #################################################################
         #############################################################T ####
         #################################################################
         ###################################################T ##############
         #################################################################
         ########
         204.1 KiB/s
done
Bytes transferred = 6785480 (6789c8 hex)
Using FEC1 device
TFTP from server 192.168.0.254; our IP address is 192.168.0.111
Filename 'imx6ull-14x14-emmc-7-1024x600-c.dtb'.
Load address: 0x83000000
Loading: ###
         233.4 KiB/s
done
Bytes transferred = 39327 (999f hex)
Kernel image @ 0x80800000 [ 0x000000 - 0x6789c8 ]
## Flattened Device Tree blob at 83000000
   Booting using the fdt blob at 0x83000000
   Using Device Tree in place at 83000000, end 8300c99e

Starting kernel ...
```

### bootcmd 和 bootargs 环境变量

bootcmd 和 bootagrs 是采用类似 shell 脚本语言编写的，里面有很多的变量引用，这些变量其实都是环境变量 ， 有很多是NXP自 己定义的 。 文件mx6ull_alientek_emmc.h 中的宏 `CONFIG_EXTRA_ENV_SETTINGS` 保存着这些环境变量的默认值。

- bootcmd：保存着 uboot 默认命令
- bootargs：保存着 uboot 传递给 Linux 内核的参数

#### bootcmd

bootcmd 保存着 uboot 默认命令，uboot 倒计时结束以后就会执行 bootcmd 中的命令。
可以在 uboot 启动以后进入命令行设置 bootcmd 环境变量的值。板子第一次运行 uboot 的时候都会使用默认值来设置 bootcmd 环境变量

文件 include/env_default.h。指定了很多环境变量的默认值，比如 bootcmd 的默认值就是CONFIG_BOOTCOMMAND，bootargs 的默认值就是 CONFIG_BOOTARGS。我们可以在 mx6ull_alientek_emmc.h 文件中通过设置宏 CONFIG_BOOTCOMMAND 来设置 bootcmd 的默认值，

打开文 件 mx6ull_alientek_emmc.h，找到宏 `CONFIG_BOOTCOMMAND` ,

经过分析，浓缩出来的仅仅是 4 行精华：
```
mmc dev 1	//切换到 EMMC
fatload mmc 1:1 0x80800000 zImage	//读取 zImage 到 0x80800000 处
fatload mmc 1:1 0x83000000 imx6ull-14x14-evk.dtb //读取设备树到 0x83000000 处
bootz 0x80800000 - 0x83000000	//启动 Linux
```
NXP 官方将 CONFIG_BOOTCOMMAND 写的这么复杂只有一个目的：为了兼容多个板子，所以写了个很复杂的脚本。当我们明确知道我们所使用的板子的时候就可以大幅简化宏CONFIG_BOOTCOMMAND的 设 置 ， 比如我们要从EMMC启 动 ，那么宏
CONFIG_BOOTCOMMAND 就可简化为：
```
#define CONFIG_BOOTCOMMAND \
"mmc dev 1;" \
"fatload mmc 1:1 0x80800000 zImage;" \
"fatload mmc 1:1 0x83000000 imx6ull-alientek-emmc.dtb;" \
"bootz 0x80800000 - 0x83000000;"
```
或者可以直接在 uboot  启动后，使用命令设置 bootcmd 的值，这个值就是保存到 EMMC 中的，命令如下：
```
setenv bootcmd 'mmc dev 1; fatload mmc 1:1 80800000 zImage; fatload mmc 1:1 83000000 imx6ull-14x14-emmc-7-1024x600-c.dtb; bootz 80800000 - 83000000;'
```

#### bootargs
bootargs 保存着 uboot 传递给 Linux 内核的参数，bootargs 环境变量是由 mmcargs 设置的，mmcargs 环境变量如下：
```mmcargs=setenv bootargs console=${console},${baudrate} root=${mmcroot}```
其中 console=ttymxc0，baudrate=115200，mmcroot=/dev/mmcblk1p2 rootwait rw，因此将mmcargs 展开以后就是：
```mmcargs=setenv bootargs console= ttymxc0, 115200 root= /dev/mmcblk1p2 rootwait rw```

常用的参数有：
1. console
console 用来设置 linux 终端(或者叫控制台)，也就是通过什么设备来和 Linux 进行交互，是串口还是 LCD 屏幕？一般设置串口作为 Linux 终端。这里设置 console 为 ttymxc0，因为 linux启动以后 I.MX6ULL 的串口 1 在 linux 下的设备文件就是/dev/ttymxc0，ttymxc0 后面有个“,115200”，这是设置串口的波特率

2. root
root 用来设置根文件系统的位置，root=/dev/mmcblk1p2 用于指明根文件系统存放在mmcblk1 设备的分区 2 中。EMMC 版本的核心板启动 linux 以后会存在/dev/mmcblk0、/dev/mmcblk1、/dev/mmcblk0p1、/dev/mmcblk0p2、/dev/mmcblk1p1 和/dev/mmcblk1p2 这样的文件，其中/dev/mmcblkx(x=0~n)表示 mmc 设备，而/dev/mmcblkxpy(x=0~n,y=1~n)表示 mmc 设备x 的分区 y。在 I.MX6U-ALPHA 开发板中/dev/mmcblk1 表示 EMMC，而/dev/mmcblk1p2 表示EMMC 的分区 2。
root 后面有“rootwait rw”，rootwait 表示等待 mmc 设备初始化完成以后再挂载，否则的话mmc 设备还没初始化完成就挂载根文件系统会出错的。rw 表示根文件系统是可以读写的，不加rw 的话可能无法在根文件系统中进行写操作，只能进行读操作。

3. rootfstype

此选项一般配置 root 一起使用，rootfstype 用于指定根文件系统类型，如果根文件系统为ext 格式的话此选项无所谓。如果根文件系统是 yaffs、jffs 或 ubifs 的话就需要设置此选项，指定根文件系统的类型。


## linux内核移植

### Linux 内核获取
NXP 会从 https://www.kernel.org 下载某个版本的 Linux 内核，然后将其移植到自己的 CPU上，测试成功以后就会将其开放给 NXP 的 CPU 开发者。开发者下载 NXP 提供的 Linux 内核，然后将其移植到自己的产品上。

课参考上文uboot源码下载方式，从网站中下载NXP的linux内核源码
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-系统移植篇/1657549798768.png)
rel_imx_4.1.15_2.1.0_ga版本。与正点原子教程一样
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-系统移植篇/1657549835485.png)

### Linux 内核初次编译
编译内核之前需要先在 ubuntu 上安装 lzop 库，否则内核编译会失败！命令如下：
```
sudo apt-get install lzop
```

在 Ubuntu中新建名为 “ alientek_linux ” 的文件夹 ， 然后将正点原子移植好的 Linux 源码 linux-imx-4.1.15-2.1.0-g8a006db.tar.bz2 这个压缩包拷贝到前面新建的 alientek_linux 文件夹中并解压，命令如下：
```
tar -vxjf linux-imx-4.1.15-2.1.0-g8a006db.tar.bz2
```

编译出对应的 Linux 镜像文件。新建名为
“mx6ull_alientek_emmc.sh”的 shell 脚本，然后在这个 shell 脚本里面输入如下所示内容：
```
#!/bin/sh
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- distclean
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- imx_v7_defconfig
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- menuconfig
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- all -j16
```
第 2 行，执行“make distclean”，清理工程，所以 mx6ull_alientek_emmc.sh 每次都会清理一下工程。如果通过图形界面配置了 Linux，但是还没保存新的配置文件，那么就要慎重使用mx6ull_alientek_emmc.sh 编译脚本了，因为它会把你的配置信息都删除掉！
第 3 行，执行“make xxx_defconfig”，配置工程。
第 4 行，执行“make menuconfig”，打开图形配置界面，对 Linux 进行配置，如果不想每次编译都打开图形配置界面的话可以将这一行删除掉。
第 5 行，执行“make”，编译 Linux 源码。

可以看出，Linux 的编译过程基本和 uboot 一样，都要先执行“make xxx_defconfig”来配置一下，然后在执行“make”进行编译。如果需要使用图形界面配置的话就执行“make menuconfig”。

```
sudo chmod 777 mx6ull_alientek_emmc.sh
//给予可执行权限，一次即可
./mx6ull_alientek_emmc.sh
//运行脚本编译 uboot
```

Linux 的图行界面配置和 uboot 是一样的，这里我们不需要做任何的配置，直接按两下 ESC键退出，退出图形界面以后会自动开始编译 Linux。等待编译完成，大概十几分钟。

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-系统移植篇/1657550586516.png)
编译完成以后就会在 arch/arm/boot 这个目录下生成一个叫做 zImage 的文件，zImage 就是我们要用的 Linux 镜像文件。另外也会在 arch/arm/boot/dts 下生成很多.dtb 文件，这些.dtb 就是设备树文件。

### Linux 工程目录分析
我们在分析 Linux 之前一定要先在 Ubuntu 中编译一下 Linux，因为编译过程会生成一些文件，而生成的这些恰恰是分析
Linux 不可或缺的文件。编译完成以后使用 tar 压缩命令对其进行压缩并使用 Filezilla 软件将压缩后的 uboot 源码拷贝到 Windows 下。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-系统移植篇/1657550674132.png)

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-系统移植篇/1657550811416.png)
#### 1、arch 目录
这个目录是和架构有关的目录，比如 arm、arm64、avr32、x86 等等架构。每种架构都对应一个目录，在这些目录中又有很多子目录，比如 boot、common、configs 等等，以 arch/arm 为例，其子目录如图 35.3.3 所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-系统移植篇/1657550899567.png)

图 35.3.3 是 arch/arm 的一部分子目录，这些子目录用于控制系统引导、系统调用、动态调频、主频设置等。arch/arm/configs 目录是不同平台的默认配置文件：xxx_defconfig，如图 35.3.4所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-系统移植篇/1657550940237.png)
在 arch/arm/configs 中就包含有 I.MX6U-ALPHA 开发板的默认配置文件：imx_v7_defconfig，执行“make imx_v7_defconfig”即可完成配置。
arch/arm/boot/dts 目录里面是对应开发平台的设备树文件，正点原子 I.MX6U-ALPHA 开发板对应的设备树文件如图 35.3.5 所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-系统移植篇/1657550960076.png)
arch/arm/boot 目录下会保存编译出来的 Image 和 zImage 镜像文件，而 zImage 就是我们要用的 linux 镜像文件。

arch/arm/mach-xxx 目录分别为相应平台的驱动和初始化文件，比如 mach-imx 目录里面就是 I.MX 系列 CPU 的驱动和初始化文件。

#### 2、block 目录
block 是 Linux 下块设备目录，像 SD 卡、EMMC、NAND、硬盘等存储设备就属于块设备，
block 目录中存放着管理块设备的相关文件。
#### 3、crypto 目录
crypto 目录里面存放着加密文件，比如常见的 crc、crc32、md4、md5、hash 等加密算法。
#### 4、Documentation 目录
此目录里面存放着 Linux 相关的文档，如果要想了解 Linux 某个功能模块或驱动架构的功
能，就可以在 Documentation 目录中查找有没有对应的文档。
#### 5、drivers 目录
驱动目录文件，此目录根据驱动类型的不同，分门别类进行整理，比如 drivers/i2c 就是 I2C
相关驱动目录，drivers/gpio 就是 GPIO 相关的驱动目录，这是我们学习的重点。
#### 6、firmware 目录
此目录用于存放固件。
#### 7、fs 目录
此目录存放文件系统，比如 fs/ext2、fs/ext4、fs/f2fs 等，分别是 ext2、ext4 和 f2fs 等文件系
统。
### 内核移植

#### 创建VSCode工程

这里我们使用 NXP 官方提供的 Linux 源码，将其移植到正点原子 I.MX6U-ALPHA 开发板上。使用 FileZilla 将其发送到 Ubuntu
中并解压，得到名为 linux-imx-rel_imx_4.1.15_2.1.0_ga 的目录，
```
tar -vxjf linux-imx-rel_imx_4.1.15_2.1.0_ga.tar.bz2
```

为了和 NXP 官方的名字区分，将其重命名为 “ linux-imx-rel_imx_4.1.15_2.1.0_ga_alientek”，命令如下：
```
mv linux-imx-rel_imx_4.1.15_2.1.0_ga linux-imx-rel_imx_4.1.15_2.1.0_ga_alientek
```
完成以后创建 VSCode 工程，
新建 .vscode/settings.json 文件屏蔽不用文件夹显示，内容如下

```
{
    "search.exclude": {
        "**/node_modules": true,
        "**/bower_components": true,
        "**/*.o":true,
        "**/*.su":true,
        "**/*.cmd":true,
        "Documentation":true,

        /* 屏蔽不用的架构相关的文件 */
        "arch/alpha":true,
        "arch/arc":true,
        "arch/arm64":true,
        "arch/avr32":true,
        "arch/[b-z]*":true,
        "arch/arm/plat*":true,
        "arch/arm/mach-[a-h]*":true,
        "arch/arm/mach-[n-z]*":true,
        "arch/arm/mach-i[n-z]*":true,
        "arch/arm/mach-m[e-v]*":true,
        "arch/arm/mach-k*":true,
        "arch/arm/mach-l*":true,

        /* 屏蔽排除不用的配置文件 */
        "arch/arm/configs/[a-h]*":true,
        "arch/arm/configs/[j-z]*":true,
        "arch/arm/configs/imo*":true,
        "arch/arm/configs/in*":true,
        "arch/arm/configs/io*":true,
        "arch/arm/configs/ix*":true,

        /* 屏蔽掉不用的 DTB 文件 */
        "arch/arm/boot/dts/[a-h]*":true,
        "arch/arm/boot/dts/[k-z]*":true,
        "arch/arm/boot/dts/in*":true,
        "arch/arm/boot/dts/imx1*":true,
        "arch/arm/boot/dts/imx7*":true,
        "arch/arm/boot/dts/imx2*":true,
        "arch/arm/boot/dts/imx3*":true,
        "arch/arm/boot/dts/imx5*":true,
        "arch/arm/boot/dts/imx6d*":true,
        "arch/arm/boot/dts/imx6q*":true,
        "arch/arm/boot/dts/imx6s*":true,
        "arch/arm/boot/dts/imx6ul-*":true,
        "arch/arm/boot/dts/imx6ull-9x9*":true,
        "arch/arm/boot/dts/imx6ull-14x14-ddr*":true,
    },
    "files.exclude": {
        "**/.git": true,
        "**/.svn": true,
        "**/.hg": true,
        "**/CVS": true,
        "**/.DS_Store": true,
        "**/*.o":true,
        "**/*.su":true,
        "**/*.cmd":true,
        "Documentation":true,

        /* 屏蔽不用的架构相关的文件 */
        "arch/alpha":true,
        "arch/arc":true,
        "arch/arm64":true,
        "arch/avr32":true,
        "arch/[b-z]*":true,
        "arch/arm/plat*":true,
        "arch/arm/mach-[a-h]*":true,
        "arch/arm/mach-[n-z]*":true,
        "arch/arm/mach-i[n-z]*":true,
        "arch/arm/mach-m[e-v]*":true,
        "arch/arm/mach-k*":true,
        "arch/arm/mach-l*":true,

        /* 屏蔽排除不用的配置文件 */
        "arch/arm/configs/[a-h]*":true,
        "arch/arm/configs/[j-z]*":true,
        "arch/arm/configs/imo*":true,
        "arch/arm/configs/in*":true,
        "arch/arm/configs/io*":true,
        "arch/arm/configs/ix*":true,

        /* 屏蔽掉不用的 DTB 文件 */
        "arch/arm/boot/dts/[a-h]*":true,
        "arch/arm/boot/dts/[k-z]*":true,
        "arch/arm/boot/dts/in*":true,
        "arch/arm/boot/dts/imx1*":true,
        "arch/arm/boot/dts/imx7*":true,
        "arch/arm/boot/dts/imx2*":true,
        "arch/arm/boot/dts/imx3*":true,
        "arch/arm/boot/dts/imx5*":true,
        "arch/arm/boot/dts/imx6d*":true,
        "arch/arm/boot/dts/imx6q*":true,
        "arch/arm/boot/dts/imx6s*":true,
        "arch/arm/boot/dts/imx6ul-*":true,
        "arch/arm/boot/dts/imx6ull-9x9*":true,
        "arch/arm/boot/dts/imx6ull-14x14-ddr*":true,
    }
 }
```

#### NXP内核编译

##### 修改顶层Makefile


修改顶层 Makefile，直接在顶层 Makefile 文件里面定义 ARCH 和 CROSS_COMPILE 这两个的变量值为 arm 和 arm-linux-gnueabihf-，结果如图所示：

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-系统移植篇/1657634667576.png)
图中第 252 和 253 行分别设置了 ARCH 和 CROSS_COMPILE 这两个变量的值，这样在编译的时候就不用输入很长的命令了。

##### 配置并编译 Linux 内核

和 uboot 一样，在编译 Linux 内核之前要先配置 Linux 内核。每个板子都有其对应的默认配 置 文 件 ， 这 些 默 认 配 置 文 件 保 存 在 arch/arm/configs 目 录 中 。 imx_v7_defconfig 和imx_v7_mfg_defconfig 都可作为 I.MX6ULL EVK 开发板所使用的默认配置文件。但是这里建议使用 imx_v7_mfg_defconfig 这个默认配置文件，首先此配置文件默认支持 I.MX6UL 这款芯片，而且重要的一点就是此文件编译出来的 zImage 可以通过 NXP 官方提供的 MfgTool 工具烧写！！imx_v7_mfg_defconfig 中的“mfg”的意思就是 MfgTool。

进入到 Ubuntu 中的 Linux 源码根目录下，执行如下命令配置 Linux 内核：
```
make clean  //第一次编译 Linux 内核之前先清理一下
make imx_v7_mfg_defconfig   //配置 Linux 内核
```
配置完成以后如图 37.2.2.1 所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-系统移植篇/1657634658847.png)

配置完成以后就可以编译了，使用如下命令编译 Linux 内核：
```
make -j16  //编译 Linux 内核
```

也可 以 创 建 一 个 编 译 脚 本 ，imx6ull_nxp_emmc.sh，脚本内容如下：
```
#!/bin/sh
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- distclean
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- imx_v7_mfg_defconfig
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- menuconfig
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- all -j16
```

第 2 行，清理工程。
第 3 行，使用默认配置文件 imx_alientek_emmc_defconfig 来配置 Linux 内核。
第 4 行，打开 Linux 的图形配置界面，如果不需要每次都打开图形配置界面可以删除此行。
第 5 行，编译 Linux。

```
chmod 777 imx6ull_alientek_emmc.sh  //给予可执行权限
./imx6ull_alientek_emmc.sh  //执行 shell 脚本编译内核
```
等待编译完成，结果如图 37.2.2.2 所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-系统移植篇/1657634708747.png)

Linux 内核编译完成以后会在 arch/arm/boot 目录下生成 zImage 镜像文件，如果使用设备树的话还会在 arch/arm/boot/dts 目录下开发板对应的.dtb(设备树)文件，比如 imx6ull-14x14-evk.dtb就是 NXP 官方的 I.MX6ULL EVK 开发板对应的设备树文件。至此我们得到两个文件：
①、Linux 内核镜像文件：zImage。
②、NXP 官方 I.MX6ULL EVK 开发板对应的设备树文件：imx6ull-14x14-evk.dtb。

##### Linux 内核启动测试

将上一小节编译出来的 zImage 和 imx6ull-14x14-evk.dtb 复制到 Ubuntu 中的 tftp 目录下，因为我们要在 uboot 中使用 tftp 命令将其下载到开发板中，拷贝命令如下：
```
cp arch/arm/boot/zImage /home/lonly/linux/tftpboot/ -f
cp arch/arm/boot/dts/imx6ull-14x14-evk.dtb /home/lonly/linux/tftpboot/ -f
```
拷贝完成以后就可以测试了，启动开发板，进入 uboot 命令行模式，环境变量 bootargs 内容如下：
```
setenv bootargs 'console=ttymxc0,115200 root=/dev/mmcblk1p2 rootwait rw'
```
然后输入如下命令将zImage 和 imx6ull-14x14-evk.dtb 下载到开发板中并启动：

```
setenv bootcmd 'tftp 80800000 zImage; tftp 83000000 imx6ull-14x14-evk.dtb; bootz 80800000 - 83000000'
saveenv
boot
```
结果图 37.2.3.1 所示：只要出现如下内容就表示成功

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-系统移植篇/1657636430554.png)

如果EMMC里没有跟文件系统的话，会显示如下错误

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-系统移植篇/1657635727173.png)

```
Kernel panic - not syncing: VFS: Unable to mount root fs on unknown-block(0,0)
```
也就是提示内核崩溃，因为 VFS(虚拟文件系统)不能挂载根文件系统，因为根文件系统目录不存在。即使根文件系统目录存在，如果根文件系统目录里面是空的依旧会提示内核崩溃。这个就是根文件系统缺失导致的内核崩溃，但是内核是启动了的，只是根文件系统不存在而已。

#### 添加自己的开发板

参考 I.MX6ULL EVK 开发板的设置，在 Linux 内核中添加正点原子的 I.MX6U-ALPHA 开发板。

##### 添加开发板默认配置文件
将arch/arm/configs目 录 下 的imx_v7_mfg_defconfig重 新 复 制 一 份 ， 命 名 为imx_alientek_emmc_defconfig，命令如下：
```
cd arch/arm/configs
cp imx_v7_mfg_defconfig imx_alientek_emmc_defconfig
```
以后 imx_alientek_emmc_defconfig 就是正点原子的 EMMC 版开发板默认配置文件了。
以后就可以使用如下命令来配置正点原子 EMMC 版开发板对应的 Linux 内核了：
```
make imx_alientek_emmc_defconfig
```

##### 添加开发板对应的设备树文件

添加适合正点原子 EMMC 版开发板的设备树文件，进入目录 arch/arm/boot/dts 中，复制一份 imx6ull-14x14-evk.dts，然后将其重命名为 imx6ull-alientek-emmc.dts，命令如下：
```
cd arch/arm/boot/dts
cp imx6ull-14x14-evk.dts imx6ull-alientek-emmc.dts
```
.dts 是设备树源码文件，编译 Linux 的时候会将其编译为.dtb 文件。imx6ull-alientek-emmc.dts创 建 好 以 后 我 们 还 需 要 修 改 文 件arch/arm/boot/dts/Makefile ， 找 到 “ dtb-$(CONFIG_SOC_IMX6ULL)”配置项，在此配置项中加入“imx6ull-alientek-emmc.dtb” ，如下所示：
```
400 dtb-$(CONFIG_SOC_IMX6ULL) += \
401 imx6ull-14x14-ddr3-arm2.dtb \
402 imx6ull-14x14-ddr3-arm2-adc.dtb \
403 imx6ull-14x14-ddr3-arm2-cs42888.dtb \
404 imx6ull-14x14-ddr3-arm2-ecspi.dtb \
405 imx6ull-14x14-ddr3-arm2-emmc.dtb \
406 imx6ull-14x14-ddr3-arm2-epdc.dtb \
407 imx6ull-14x14-ddr3-arm2-flexcan2.dtb \
408 imx6ull-14x14-ddr3-arm2-gpmi-weim.dtb \
409 imx6ull-14x14-ddr3-arm2-lcdif.dtb \
410 imx6ull-14x14-ddr3-arm2-ldo.dtb \
411 imx6ull-14x14-ddr3-arm2-qspi.dtb \
412 imx6ull-14x14-ddr3-arm2-qspi-all.dtb \
413 imx6ull-14x14-ddr3-arm2-tsc.dtb \
414 imx6ull-14x14-ddr3-arm2-uart2.dtb \
415 imx6ull-14x14-ddr3-arm2-usb.dtb \
416 imx6ull-14x14-ddr3-arm2-wm8958.dtb \
417 imx6ull-14x14-evk.dtb \
418 imx6ull-14x14-evk-btwifi.dtb \
419 imx6ull-14x14-evk-emmc.dtb \
420 imx6ull-14x14-evk-gpmi-weim.dtb \
421 imx6ull-14x14-evk-usb-certi.dtb \
422 imx6ull-alientek-emmc.dtb \
```
第 422 行为“imx6ull-alientek-emmc.dtb”，这样编译 Linux 的时候就可以从 imx6ull-alientek-emmc.dts 编译出 imx6ull-alientek-emmc.dtb 文件了。

这里可以先编译测试一下，看是否则正确，再进行下面的修改操作。

```
cp arch/arm/boot/zImage /home/lonly/linux/tftpboot/ -f
cp arch/arm/boot/dts/imx6ull-alientek-emmc.dtb /home/lonly/linux/tftpboot/ -f
```


拷贝完成以后就可以测试了，启动开发板，进入 uboot 命令行模式，环境变量 bootargs 内容如下：
```
setenv bootargs 'console=ttymxc0,115200 root=/dev/mmcblk1p2 rootwait rw'
setenv bootcmd 'tftp 80800000 zImage; tftp 83000000 imx6ull-alientek-emmc.dtb; bootz 80800000 - 83000000'
saveenv
boot
```

如果出现下述状态，是因为没有根文件系统，使用开发板的话，厂家一般都会在emmc中下载好根文件系统，所以不会出现如下错误，这里并不影响测试linux内核和设备树。

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-系统移植篇/1658042955983.png)

> 这里正点原子视频教程中还出现了emmc设备树问题，视频弹幕反馈和自己实测并不会出现
> 如果上述启动信息中有该行内容：`mmcblk1: mmc1:0001 8GTF4R 7.28 GiB` ，表示你的emmc是没问题的（不同内存卡容量可能不同）

```
Waiting for root device /dev/mmcblk1p2...
hub 1-1:1.0: USB hub found
mmc1: MAN_BKOPS_EN bit is not set
hub 1-1:1.0: 4 ports detected
mmc1: new HS200 MMC card at address 0001
mmcblk1: mmc1:0001 8GTF4R 7.28 GiB
mmcblk1boot0: mmc1:0001 8GTF4R partition 1 4.00 MiB
mmcblk1boot1: mmc1:0001 8GTF4R partition 2 4.00 MiB
mmcblk1rpmb: mmc1:0001 8GTF4R partition 3 512 KiB
```
##### 主频修改
正点原子 I.MX6U-ALPHA 开发板所使用的 I.MX6ULL 芯片主频都是 792MHz 的。后续可能会生产 528MHz 核心板供企业级批量用户，但是开发板搭配的都是 792MHz 主频的，本节教程也就以 792MHz 的核心板为例讲解。

1、设置 I.MX6U-ALPHA 开发板工作在 792MHz

确保 EMMC 中的根文件系统可用！然后重新启动开发板，进入终端(可以输入命令)，输入如下命令查看 cpu 信息：
```
cat /proc/cpuinfo
```
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-系统移植篇/1657802714406.png)
在中有 BogoMIPS 这一条，此时 BogoMIPS 为 3.00，BogoMIPS 是 Linux 系统中衡量处理器运行速度的一个“尺子”，我们可以通过 BogoMIPS 值来大致的判断当前处理器的性能。处理器性能越强，主频越高，BogoMIPS 值就越大。BogoMIPS 只是粗略的计算 CPU 性能，并不十分准确。

进入到目录`cd /sys/devices/system/cpu/cpu0/cpufreq`中，此目录下会有很多文件，此目录中记录了 CPU 频率等信息
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-系统移植篇/1657802753901.png)

 - cpuinfo_cur_freq：当前 cpu 工作频率，从 CPU 寄存器读取到的工作频率。
 - cpuinfo_max_freq：处理器所能运行的最高工作频率(单位: KHz）。
 - cpuinfo_min_freq ：处理器所能运行的最低工作频率(单位: KHz）。
 - cpuinfo_transition_latency：处理器切换频率所需要的时间(单位:ns)。
 - scaling_available_frequencies：处理器支持的主频率列表(单位: KHz）。
 - scaling_available_governors：当前内核中支持的所有 governor(调频)类型。
 - scaling_cur_freq：保存着 cpufreq 模块缓存的当前 CPU 频率，不会对 CPU 硬件寄存器进
 - 行检查。
 - scaling_driver：该文件保存当前 CPU 所使用的调频驱动。
 - scaling_governor：governor(调频)策略，Linux 内核一共有 5 中调频策略，
	 - ①、Performance，最高性能，直接用最高频率，不考虑耗电。
	 - ②、Interactive，一开始直接用最高频率，然后根据 CPU 负载慢慢降低。
	 - ③、Powersave，省电模式，通常以最低频率运行，系统性能会受影响，一般不会用这个！
	 - ④、Userspace，可以在用户空间手动调节频率。
	 - ⑤、Ondemand，定时检查负载，然后根据负载来调节频率。负载低的时候降低 CPU 频率，这样省电，负载高的时候提高 CPU 频率，增加性能。
 - scaling_max_freq：governor(调频)可以调节的最高频率。
 - cpuinfo_min_freq：governor(调频)可以调节的最低频率。
- stats 目录下给出了 CPU 各种运行频率的统计情况，比如 CPU 在各频率下的运行时间以及变频次数。
使用如下命令查看当前 CPU 频率：
```
cat cpuinfo_cur_freq
```
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-系统移植篇/1657802844631.png)
从图可以看出，当前 CPU 频率为 198MHz，工作频率很低！其他的值如下（使用`cat`查看上述文件）：
```
cpuinfo_cur_freq = 198000
cpuinfo_max_freq = 792000
cpuinfo_min_freq = 198000
scaling_cur_freq = 198000
scaling_max_freq = 792000
scaling_min_freq = 198000
scaling_available_frequencies = 198000 396000 528000 792000
scaling_governor = ondemand
```

调频策略为 ondemand，也就是定期检查负载，然后根据负载情况调节 CPU 频率。查看 stats 目录下的 time_in_state 文件可以看到 CPU 在各频率下的工作时间，命令如下：
```
cat /sys/bus/cpu/devices/cpu0/cpufreq/stats/time_in_state
```
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-系统移植篇/1657803051168.png)
假如我们想让 CPU 一直工作在 792MHz 那该怎么办？很简单，配置 Linux 内核，将调频策略选择为 performance。或者修改 imx_alientek_emmc_defconfig 文件，此文件中有下面几行：
`cd arch/arm/configs`

```
41 CONFIG_CPU_FREQ_DEFAULT_GOV_ONDEMAND=y
42 CONFIG_CPU_FREQ_GOV_POWERSAVE=y
43 CONFIG_CPU_FREQ_GOV_USERSPACE=y
44 CONFIG_CPU_FREQ_GOV_INTERACTIVE=y
```
第 41 行，配置 ondemand 为默认调频策略。
第 42 行，使能 powersave 策略。
第 43 行，使能 userspace 策略。
第 44 行，使能 interactive 策略。 

在 41 行前面添加：
`CONFIG_CPU_FREQ_DEFAULT_GOV_PERFORMANCE=y`
结果下所示：
```
CONFIG_CPU_FREQ_GOV_POWERSAVE=y
CONFIG_CPU_FREQ_GOV_USERSPACE=y
CONFIG_CPU_FREQ_GOV_ONDEMAND=y
CONFIG_CPU_FREQ_GOV_CONSERVATIVE=y
```

> 这里正点原子是用图形界面设置的，我这是修改config文件，所以文件内容和修改的地方与教程不同
修改后，重新启动，查看运行频率和运行策略
```
root@ATK-IMX6U:~# cd /sys/devices/system/cpu/cpu0/cpufreq
root@ATK-IMX6U:/sys/devices/system/cpu/cpu0/cpufreq# cat cpuinfo_cur_freq
792000
root@ATK-IMX6U:/sys/devices/system/cpu/cpu0/cpufreq# cat scaling_governor
performance

```


> 参考正点原子教程 ：37.4.2 使能 8 线 EMMC 驱动
```
734 &usdhc2 {
735 pinctrl-names = "default", "state_100mhz", "state_200mhz";
736 pinctrl-0 = <&pinctrl_usdhc2_8bit>;
737 pinctrl-1 = <&pinctrl_usdhc2_8bit_100mhz>;
738 pinctrl-2 = <&pinctrl_usdhc2_8bit_200mhz>;
739 bus-width = <8>;
740 non-removable;
741 status = "okay";
742 };
```

单独编译设备树文件 
```
make dtbs
```



### 顶层Makefile详解
### 内核启动流程

### 内核移植

## 根文件系统构建

## 系统烧写

Linux是一个单体内核，支持真正的抢占式多任务处理（于用户态，和版本2.6系列之后的内核态[27][28]）、虚拟内存、共享库、请求分页、共享写时复制可执行体（通过内核同页合并）、内存管理、Internet协议族和线程等功能。

设备驱动程序和内核扩展运行于内核空间（在很多CPU架构中是ring 0），可以完全访问硬件，但也有运行于用户空间的一些例外，例如基于FUSE/CUSE的文件系统，和部分UIO[29][30]。多数人与Linux一起使用的图形系统不运行在内核中。与标准单体内核不同，Linux的设备驱动程序可以轻易的配置为内核模块，并在系统运行期间可直接装载或卸载。也不同于标准单体内核，设备驱动程序可以在特定条件下被抢占；增加这个特征用于正确处理硬件中断并更好的支持对称多处理[28]。出于自愿选择，Linux内核没有二进制内核接口[31]。

硬件也被集成入文件层级中。用户应用到设备驱动的接口是在/dev或/sys目录下的入口文件[32]。进程信息也通过/proc目录映射到文件系统[32]。
![1652321949009](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记/1652321949009.png)

- [Linux内核-wiki](https://zh.wikipedia.org/wiki/Linux%E5%86%85%E6%A0%B8)
- [鸟哥linux第四版-基礎學習篇目錄 - for CentOS 7](https://linux.vbird.org/linux_basic/centos7/)

shell
- [Chapter 11 Shell 和 Shell Script](https://www.cyut.edu.tw/~ywfan/1109linux/201109chapter11shell%20script.htm)
- [Linux Shell 版本问题](https://achelous.org/BI-solutions/Linux_shell_version.html)
- [这些贝壳的插图哪一个是准确的？外壳是否仅通过外壳与内核对话？](https://www.reddit.com/r/linux4noobs/comments/kufgft/which_of_these_illustrations_of_the_shell_is/)
- https://imgur.com/a/VuvJ3dy
- [一篇文章從了解到入門shell](https://kknews.cc/code/3va5oq8.html)
- [Shell脚本是什么](http://c.biancheng.net/view/932.html)
- [Linux shell脚本](https://www.cnblogs.com/gd-luojialin/p/15028076.html)
- [Linux Shell程式設計及自動化運維實現——shell概述及變數](https://www.gushiciku.cn/pl/gLvW/zh-tw)
- [Bash 参考手册](https://www.gnu.org/software/bash/manual/bash.html)
- [bash (Bourne again shell)](https://www.techtarget.com/searchdatacenter/definition/bash-Bourne-Again-Shell)
- [什么是 Linux，为什么有 100 种 Linux 发行版？](https://itsfoss.com/what-is-linux/)
- [linux系统组成及结构](https://blog.csdn.net/kai_zone/article/details/80444872)

[Linux操作系统综述](https://blog.51cto.com/u_13800449/3049118)
[Linux系统组成.md](https://github.com/sunnyandgood/BigData)

![1652327045769](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记/1652327045769.png)


<!--more-->
