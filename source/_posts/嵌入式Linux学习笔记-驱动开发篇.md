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

# 字符设备驱动

字符设备是 Linux 驱动中最基本的一类设备驱动，字符设备就是一个一个字节，按照字节
流进行读写操作的设备，读写数据是分先后顺序的。比如我们最常见的点灯、按键、IIC、SPI，
LCD 等等都是字符设备，这些设备的驱动就叫做字符设备驱动。
在详细的学习字符设备驱动架构之前，我们先来简单的了解一下 Linux 下的应用程序是如
何调用驱动程序的，Linux 应用程序对驱动程序的调用如图 40.1.1 所示：

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658498584694.png)

<!--more-->
