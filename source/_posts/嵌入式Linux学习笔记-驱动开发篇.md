---
layout: post
title: "嵌入式Linux学习笔记-驱动开发篇"
index_img: https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记/1654315471941.png
date: 2022-07-10 22:35
updated: 2022-07-10 22:35
hide: false
## sticky: 100 #置顶，数字越大越靠前
## banner_img: #/img/post_banner.jpg
## comment: false
categories: 01-专业
---

Linux 中的三大类驱动：字符设备驱动、块设备驱动和网络

- 设备驱动。其中字符设备驱动是占用篇幅最大的一类驱动，因为字符设备最多，从最简单的点灯到 I2C、SPI、音频等都属于字符设备驱动的类型。
- 块设备驱动就是存储器设备的驱动，比如 EMMC、NAND、SD 卡和 U 盘等存储设备，因为这些存储设备的特点是以存储块为基础，因此叫做块设备。
- 网络设备驱动就更好理解了，就是网络驱动，不管是有线的还是无线的，都属于网络设备驱动的范畴。

块设备和网络设备驱动要比字符设备驱动复杂，就是因为其复杂所以半导体厂商一般都给我们编写好了，大多数情况下都是直接可以使用的。

一个设备可以属于多种设备驱动类型，比如 USB WIFI，其使用 USB 接口，所以属于字符设备，但是其又能上网，所以也属于网络设备驱动。

本书使用的 Linux 内核版本为 4.1.15，其支持设备树(Device tree)，所以本篇所有例程均采用设备树。设备树将是本篇的重点！



Linux 应用程序对驱动程序的调用如图所示：

![Linux 应用程序对驱动程序的调用流程](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658498584694.png)

在 Linux 中一切皆为文件，驱动加载成功以后会在“/dev”目录下生成一个相应的文件，应用程序通过对这个名为“/dev/xxx”(xxx 是具体的驱动文件名字)的文件进行相应的操作即可实现对硬件的操作。

比如现在有个叫做/dev/led 的驱动文件，此文件是 led 灯的驱动文件。应用程序使用 open 函数来打开文件/dev/led，使用完成以后使用 close 函数关闭/dev/led 这个文件。open和 close 就是打开和关闭 led 驱动的函数，如果要点亮或关闭 led，那么就使用 write 函数来操作，也就是向此驱动写入数据，这个数据就是要关闭还是要打开 led 的控制参数。如果要获取led 灯的状态，就用 read 函数从驱动中读取相应的状态。

应用程序运行在用户空间，而 Linux 驱动属于内核的一部分，因此驱动运行于内核空间。当我们在用户空间想要实现对内核的操作，比如使用 open 函数打开/dev/led 这个驱动，因为用户空间不能直接对内核进行操作，因此必须使用一个叫做“系统调用”的方法来实现从用户空间“陷入”到内核空间，这样才能实现对底层驱动的操作。open、close、write 和 read 等这些函数是由 C 库提供的，在 Linux 系统中，系统调用作为 C 库的一部分。当我们调用 open 函数的时候流程如图 40.1.2 所示：

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658498683107.png)


# 字符设备驱动


驱动编译成模块
1. 模块加载（insmod或modprobe ）：调用驱动程序中的 module_init(xxx_init)，再调用和初始化函数，xxx_init()
2. 注册字符设备：一般加载时，在初始化函数中调用register_chrdev() 函数完成注册


字符设备是 Linux 驱动中最基本的一类设备驱动，字符设备就是一个一个字节，按照字节流进行读写操作的设备，读写数据是分先后顺序的。比如我们最常见的点灯、按键、IIC、SPI，LCD 等等都是字符设备，这些设备的驱动就叫做字符设备驱动。

## 程序编写

### 1、创建 VSCode 工程

在 Ubuntu 中创建一个目录用来存放 Linux 驱动程序，比如我创建了一个名为 Linux_Drivers的目录来存放所有的 Linux 驱动。在 Linux_Drivers 目录下新建一个名为 1_chrdevbase 的子目录来存放本实验所有文件

在 1_chrdevbase 目录中新建 VSCode 工程，并且新建 chrdevbase.c 文件，完成以后1_chrdevbase 目录中的文件如图 40.4.1.2 所示：

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658499335278.png)

### 2、添加头文件路径

因为是编写 Linux 驱动，因此会用到 Linux 源码中的函数。我们需要在 VSCode 中添加 Linux源码中的头文件路径。打开 VSCode，按下“Crtl+Shift+P”打开 VSCode 的控制台，然后输入“C/C++: Edit configurations(JSON) ”，打开 C/C++编辑配置文件，如图所示：

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658499365413.png)

打开以后会自动在.vscode 目录下生成一个名为 c_cpp_properties.json 的文件，此文件默认内容如下所示：

第 5 行的 includePath 表示头文件路径，需要将 Linux 源码里面的头文件路径添加进来，也就是我们前面移植的 Linux 源码中的头文件路径。添加头文件路径以后的 c_cpp_properties.json
的文件内容如下所示：
```
1 {
2 	"configurations": [
3 		{
4 			"name": "Linux",
5 			"includePath": [
6 				"${workspaceFolder}/**",
7				 "/home/zuozhongkai/linux/IMX6ULL/linux/temp/linux-imx-rel_imx_4.1.15_2.1.0_ga_alientek/include",
8 				"/home/zuozhongkai/linux/IMX6ULL/linux/temp/linux-imx-rel_imx_4.1.15_2.1.0_ga_alientek/arch/arm/include",
9 				"/home/zuozhongkai/linux/IMX6ULL/linux/temp/linux-imx-rel_imx_4.1.15_2.1.0_ga_alientek/arch/arm/include/generated/"
10 			],
11 			"defines": [],
......
16 		}
17 	],
18 	"version": 4
19 }
```

第 7~9 行就是添加好的 Linux 头文件路径。分别是开发板所使用的 Linux 源码下的 include、
arch/arm/include 和 arch/arm/include/generated 这三个目录的路径，注意，这里使用了绝对路径。

## 设备号

### 1.静态分配设备号

前面讲解字符设备驱动的时候说过了，注册字符设备的时候需要给设备指定一个设备号，这个设备号可以是驱动开发者静态的指定一个设备号，比如选择 200 这个主设备号。有一些常用的设备号已经被 Linux 内核开发者给分配掉了，具体分配的内容可以查看文档 Documentation/devices.txt。并不是说内核开发者已经分配掉的主设备号我们就不能用了，具体能不能用还得看我们的硬件平台运行过程中有没有使用这个主设备号，使用 
```
cat /proc/devices
```
命令即可查看当前系统中所有已经使用了的设备号。

### 2、动态分配设备号

Linux 社区推荐使用动态分配设备号，在注册字符设备之前先申请一个设备号，系统会自动给你一个没有被使用的设备号，这样就避免了冲突。卸载驱动的时候释放掉这个设备号即可，设备号的申请函数如下：
```
int alloc_chrdev_region(dev_t *dev, unsigned baseminor, unsigned count, const char *name)
```

- dev：保存申请到的设备号。
- baseminor：次设备号起始地址，alloc_chrdev_region 可以申请一段连续的多个设备号，这些设备号的主设备号一样，但是次设备号不同，次设备号以 baseminor 为起始地址地址开始递增。一般 baseminor 为 0，也就是说次设备号从 0 开始。
- count：要申请的设备号数量。
- name：设备名字。

注销字符设备之后要释放掉设备号，设备号释放函数如下：
```
void unregister_chrdev_region(dev_t from, unsigned count)
```
- from：要释放的设备号。
- count：表示从 from 开始，要释放的设备号数量。

<!--more-->
