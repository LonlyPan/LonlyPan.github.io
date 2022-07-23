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


# 旧字符设备驱动



字符设备是 Linux 驱动中最基本的一类设备驱动，字符设备就是一个一个字节，按照字节流进行读写操作的设备，读写数据是分先后顺序的。比如我们最常见的点灯、按键、IIC、SPI，LCD 等等都是字符设备，这些设备的驱动就叫做字符设备驱动。

驱动编译成模块
1. 编写驱动函数和APP函数：
2. 模块加载（insmod或modprobe ）：调用驱动程序中的 module_init(xxx_init)，再调用和初始化函数，xxx_init()
3. 注册字符设备：一般加载时，在初始化函数中同时会调用register_chrdev() 函数完成注册
3. 创建设备节点文件，APP通过操作该文件控制设备。实际就是驱动文件的一个映射。因为用户空间无法直接操作内核空间，驱动属于内核空间
4. 通过给APP文件传输参数，从而控制设备
5. 卸载设备

## 静态分配设备号

前面讲解字符设备驱动的时候说过了，注册字符设备的时候需要给设备指定一个设备号，这个设备号可以是驱动开发者静态的指定一个设备号，比如选择 200 这个主设备号。有一些常用的设备号已经被 Linux 内核开发者给分配掉了，具体分配的内容可以查看文档 Documentation/devices.txt。并不是说内核开发者已经分配掉的主设备号我们就不能用了，具体能不能用还得看我们的硬件平台运行过程中有没有使用这个主设备号，使用 
```
cat /proc/devices
```
命令即可查看当前系统中所有已经使用了的设备号。

## 程序编写

### 1、创建 VSCode 工程

在 Ubuntu 中创建一个目录用来存放 Linux 驱动程序，比如我创建了一个名为 Linux_Drivers的目录来存放所有的 Linux 驱动。在 Linux_Drivers 目录下新建一个名为 1_chrdevbase 的子目录来存放本实验所有文件

在 1_chrdevbase 目录中新建 VSCode 工程，并且新建 chrdevbase.c 文件，完成以后1_chrdevbase 目录中的文件如图 40.4.1.2 所示：

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658499335278.png)

### 2、添加头文件路径

因为是编写 Linux 驱动，因此会用到 Linux 源码中的函数。我们需要在 VSCode 中添加 Linux源码中的头文件路径。打开 VSCode，按下“Crtl+Shift+P”打开 VSCode 的控制台，然后输入“C/C++: Edit configurations(JSON) ”，打开 C/C++编辑配置文件，如图所示：

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658499365413.png)

打开以后会自动在.vscode 目录下生成一个名为 c_cpp_properties.json 的文件，第 5 行的 includePath 表示头文件路径，需要将 Linux 源码里面的头文件路径添加进来，也就是我们前面移植的 Linux 源码中的头文件路径。添加头文件路径以后的 c_cpp_properties.json的文件内容如下所示：

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

第 7~9 行就是添加好的 Linux 头文件路径。分别是开发板所使用的 Linux 源码下的 include、arch/arm/include 和 arch/arm/include/generated 这三个目录的路径，注意，这里使用了绝对路径。

### 3、 驱动编写

```
#include <linux/types.h>
#include <linux/kernel.h>
#include <linux/delay.h>
#include <linux/ide.h>
#include <linux/init.h>
#include <linux/module.h>

#define CHRDEVBASE_MAJOR	200				/* 主设备号 */
#define CHRDEVBASE_NAME		"chrdevbase" 	/* 设备名     */

static char readbuf[100];		/* 读缓冲区 */
static char writebuf[100];		/* 写缓冲区 */
static char kerneldata[] = {"kernel data!"};

/*
 * @description		: 打开设备
 * @param - inode 	: 传递给驱动的inode
 * @param - filp 	: 设备文件，file结构体有个叫做private_data的成员变量
 * 					  一般在open的时候将private_data指向设备结构体。
 * @return 			: 0 成功;其他 失败
 */
static int chrdevbase_open(struct inode *inode, struct file *filp)
{
	//printk("chrdevbase open!\r\n");
	return 0;
}

/*
 * @description		: 从设备读取数据 
 * @param - filp 	: 要打开的设备文件(文件描述符)
 * @param - buf 	: 返回给用户空间的数据缓冲区
 * @param - cnt 	: 要读取的数据长度
 * @param - offt 	: 相对于文件首地址的偏移
 * @return 			: 读取的字节数，如果为负值，表示读取失败
 */
static ssize_t chrdevbase_read(struct file *filp, char __user *buf, size_t cnt, loff_t *offt)
{
	int retvalue = 0;
	
	/* 向用户空间发送数据 */
	memcpy(readbuf, kerneldata, sizeof(kerneldata));
	retvalue = copy_to_user(buf, readbuf, cnt);
	if(retvalue == 0){
		printk("kernel senddata ok!\r\n");
	}else{
		printk("kernel senddata failed!\r\n");
	}
	
	//printk("chrdevbase read!\r\n");
	return 0;
}

/*
 * @description		: 向设备写数据 
 * @param - filp 	: 设备文件，表示打开的文件描述符
 * @param - buf 	: 要写给设备写入的数据
 * @param - cnt 	: 要写入的数据长度
 * @param - offt 	: 相对于文件首地址的偏移
 * @return 			: 写入的字节数，如果为负值，表示写入失败
 */
static ssize_t chrdevbase_write(struct file *filp, const char __user *buf, size_t cnt, loff_t *offt)
{
	int retvalue = 0;
	/* 接收用户空间传递给内核的数据并且打印出来 */
	retvalue = copy_from_user(writebuf, buf, cnt);
	if(retvalue == 0){
		printk("kernel recevdata:%s\r\n", writebuf);
	}else{
		printk("kernel recevdata failed!\r\n");
	}
	
	//printk("chrdevbase write!\r\n");
	return 0;
}

/*
 * @description		: 关闭/释放设备
 * @param - filp 	: 要关闭的设备文件(文件描述符)
 * @return 			: 0 成功;其他 失败
 */
static int chrdevbase_release(struct inode *inode, struct file *filp)
{
	//printk("chrdevbase release！\r\n");
	return 0;
}

/*
 * 设备操作函数结构体
 */
static struct file_operations chrdevbase_fops = {
	.owner = THIS_MODULE,	
	.open = chrdevbase_open,
	.read = chrdevbase_read,
	.write = chrdevbase_write,
	.release = chrdevbase_release,
};

/*
 * @description	: 驱动入口函数 
 * @param 		: 无
 * @return 		: 0 成功;其他 失败
 */
static int __init chrdevbase_init(void)
{
	int retvalue = 0;

	/* 注册字符设备驱动 */
	retvalue = register_chrdev(CHRDEVBASE_MAJOR, CHRDEVBASE_NAME, &chrdevbase_fops);
	if(retvalue < 0){
		printk("chrdevbase driver register failed\r\n");
	}
	printk("chrdevbase init!\r\n");
	return 0;
}

/*
 * @description	: 驱动出口函数
 * @param 		: 无
 * @return 		: 无
 */
static void __exit chrdevbase_exit(void)
{
	/* 注销字符设备驱动 */
	unregister_chrdev(CHRDEVBASE_MAJOR, CHRDEVBASE_NAME);
	printk("chrdevbase exit!\r\n");
}

/* 
 * 将上面两个函数指定为驱动的入口和出口函数 
 */
module_init(chrdevbase_init);
module_exit(chrdevbase_exit);

/* 
 * LICENSE和作者信息
 */
MODULE_LICENSE("GPL");
MODULE_AUTHOR("zuozhongkai");

```
### 4、 APP编写

```
#include "stdio.h"
#include "unistd.h"
#include "sys/types.h"
#include "sys/stat.h"
#include "fcntl.h"
#include "stdlib.h"
#include "string.h"

static char usrdata[] = {"usr data!"};

/*
 * @description		: main主程序
 * @param - argc 	: argv数组元素个数
 * @param - argv 	: 具体参数
 * @return 			: 0 成功;其他 失败
 */
int main(int argc, char *argv[])
{
	int fd, retvalue;
	char *filename;
	char readbuf[100], writebuf[100];

	if(argc != 3){
		printf("Error Usage!\r\n");
		return -1;
	}

	filename = argv[1];

	/* 打开驱动文件 */
	fd  = open(filename, O_RDWR);
	if(fd < 0){
		printf("Can't open file %s\r\n", filename);
		return -1;
	}

	if(atoi(argv[2]) == 1){ /* 从驱动文件读取数据 */
		retvalue = read(fd, readbuf, 50);
		if(retvalue < 0){
			printf("read file %s failed!\r\n", filename);
		}else{
			/*  读取成功，打印出读取成功的数据 */
			printf("read data:%s\r\n",readbuf);
		}
	}

	if(atoi(argv[2]) == 2){
 	/* 向设备驱动写数据 */
		memcpy(writebuf, usrdata, sizeof(usrdata));
		retvalue = write(fd, writebuf, 50);
		if(retvalue < 0){
			printf("write file %s failed!\r\n", filename);
		}
	}

	/* 关闭设备 */
	retvalue = close(fd);
	if(retvalue < 0){
		printf("Can't close file %s\r\n", filename);
		return -1;
	}

	return 0;
}
```

- 数组 usrdata 是测试 APP 要向 chrdevbase 设备写入的数据。
- `if(argc != 3)`，判断运行测试 APP 的时候输入的参数是不是为 3 个，main 函数的 argc 参数表示参数数量，argv[]保存着具体的参数，如果参数不为 3 个的话就表示测试 APP 用法错误。
比如，现在要从 chrdevbase 设备中读取数据，需要输入如下命令：./chrdevbaseApp /dev/chrdevbase 1上述命令一共有三个参数“./chrdevbaseApp”、“/dev/chrdevbase”和“1”，这三个参数分别对应 argv[0]、argv[1]和 argv[2]。
	- 第一个参数表示运行 chrdevbaseAPP 这个软件，
	- 第二个参数表示测试APP要打开/dev/chrdevbase这个设备。
	- 第三个参数就是要执行的操作，1表示从chrdevbase中读取数据，2 表示向 chrdevbase 写数据。


- `if(atoi(argv[2]) == 1)`判断 argv[2]参数的值是 1 还是 2，因为输入命令的时候其参数都是字符串格式的，因此需要借助 atoi 函数将字符串格式的数字转换为真实的数字。
	- 当 argv[2]为 1 的时候表示要从 chrdevbase 设备中读取数据，一共读取 50 字节的
数据，读取到的数据保存在 readbuf 中，读取成功以后就在终端上打印出读取到的数据。
	- 当 argv[2]为 2 的时候表示要向 chrdevbase 设备写数据。

## 运行测试

### 1、加载驱动模块
为了方便测试，Linux 系统选择通过 TFTP 从网络启动，并且使用 NFS 挂载网络根文件系统，确保 uboot 中环境变量的值为：
```
setenv bootcmd 'tftp 80800000 zImage; tftp 83000000 imx6ull-alientek-emmc.dtb; bootz 80800000 - 83000000'
setenv bootargs 'console=ttymxc0,115200 root=/dev/nfs nfsroot=192.168.0.254:/home/lonly/linux/nfs/rootfs,proto=tcp rw ip=192.168.0.121:192.168.0.254:192.168.0.1:255.255.255.0::eth0:off'

saveenv
```

设置好以后启动 Linux 系统，检查开发板根文件系统中有没有“/lib/modules/4.1.15”这个目录，如果没有的话自行创建。注意，“/lib/modules/4.1.15”这个目录用来存放驱动模块，使用modprobe 命令加载驱动模块的时候，驱动模块要存放在此目录下。“/lib/modules”是通用的，不管你用的什么板子、什么内核，这部分是一样的。不一样的是后面的“4.1.15”，这里要根据你所使用的 Linux 内核版本来设置，比如 ALPHA 开发板现在用的是 4.1.15 版本的 Linux 内核，因此就是“/lib/modules/4.1.15”。

因为是通过 NFS 将 Ubuntu 中的 rootfs(第三十八章制作好的根文件系统)目录挂载为根文件系统，所以可以很方便的将 chrdevbase.ko 和 chrdevbaseAPP 复制到 rootfs/lib/modules/4.1.15 目录中，命令如下：
```
sudo cp chrdevbase.ko chrdevbaseApp /home/zuozhongkai/linux/nfs/rootfs/lib/modules/4.1.15/ -f
```
拷 贝 完 成 以 后 就 会 在 开 发 板 的 /lib/modules/4.1.15 目 录 下 存 在 chrdevbase.ko 和chrdevbaseAPP 这两个文件，如图所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658499732376.png)

输入如下命令加载 chrdevbase.ko 驱动文件：
```
insmod chrdevbase.ko
```
或
```
modprobe chrdevbase.ko
```
如果使用 modprobe 加载驱动的话，可能会出现如图所示的提示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658499758940.png)

直接输入 depmod 命令即可自动生成modules.dep，有些根文件系统可能没有 depmod 这个命令，如果没有这个命令就只能重新配置busybox，使能此命令，然后重新编译 busybox。输入“depmod”命令以后会自动生成 modules.alias、modules.symbols 和 modules.dep 这三个文件，如图 40.4.4.3 所示

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658499803604.png)

重新使用 modprobe 加载 chrdevbase.ko，结果如图 40.4.4.4 所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658499833966.png)

可以看到“chrdevbase init！”这一行，这一行正是 chrdevbase.c 中模块入口函数 chrdevbase_init 输出的信息，说明模块加载成功！输入“lsmod”命令即可查看当前系统中存在的模块，结果如图所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658499857642.png)

从图 40.4.4.5 可以看出，当前系统只有“chrdevbase”这一个模块。输入如下命令查看当前系统中有没有 chrdevbase 这个设备：
```
cat /proc/devices
```

### 2、创建设备节点文件

驱动加载成功需要在/dev 目录下创建一个与之对应的设备节点文件，应用程序就是通过操作这个设备节点文件来完成对具体设备的操作。输入如下命令创建
`/dev/chrdevbase`这个设备节点文件：
```
mknod /dev/chrdevbase c 200 0
```

其中
- “mknod”是创建节点命令，
- “/dev/chrdevbase”是要创建的节点文件，
- “c”表示这是个字符设备，
- “200”是设备的主设备号，
- “0”是设备的次设备号。

创建完成以后就会存在/dev/chrdevbase 这个文件：

### 3、chrdevbase 设备操作测试
一切准备就绪，使用 chrdevbaseApp 软件操作 chrdevbase 这个设备，看看读写是否正常，首先进行读操作，输入如下命令：
```
./chrdevbaseApp /dev/chrdevbase 1
```

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658500242854.png)

首先输出“kernel senddata ok!”这一行信息，这是驱动程序中 chrdevbase_read 函数输出的信息，因为 chrdevbaseAPP 使用 read 函数从 chrdevbase 设备读取数据，因此 chrdevbase_read 函数就会执行。chrdevbase_read 函数向 chrdevbaseAPP 发送“kerneldata!”数据，chrdevbaseAPP 接收到以后就打印出来，“read data:kernel data!”就是 chrdevbaseAPP打印出来的接收到的数据。说明对 chrdevbase 的读操作正常，接下来测试对 chrdevbase 设备的写操作，输入如下命令：
```
./chrdevbaseApp /dev/chrdevbase 2
```
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658500296784.png)
只有一行“kernel recevdata:usr data!”，这个是驱动程序中的 chrdevbase_write 函数输出的。chrdevbaseAPP 使用 write 函数向 chrdevbase 设备写入数据“usr data!”。chrdevbase_write 函数接收到以后将其打印出来。说明对 chrdevbase 的写操作正常，既然读写都没问题，说明我们编写的 chrdevbase 驱动是没有问题的。

### 4、卸载驱动模块
如果不再使用某个设备的话可以将其驱动卸载掉，比如输入如下命令卸载掉 chrdevbase 这个设备：
```
rmmod chrdevbase.ko
```


# 新字符设备驱动实验

字符设备驱动开发重点是使用 register_chrdev 函数注册字符设备，当不再使用设备的时候就使用unregister_chrdev 函数注销字符设备，驱动模块加载成功以后还需要手动使用 mknod 命令创建设备节点。register_chrdev 和 unregister_chrdev 这两个函数是老版本驱动使用的函数，现在新的字符设备驱动已经不再使用这两个函数，而是使用Linux内核推荐的新字符设备驱动API函数。

本节我们就来学习一下如何编写新字符设备驱动，并且在驱动模块加载的时候自动创建设备节点文件。

## 动态分配设备号

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

```
1 int major;	/* 主设备号*/
2 int minor;	/* 次设备号*/
3 dev_t devid;	/* 设备号*/
4
5 if (major) {	/* 定义了主设备号*/
6	devid = MKDEV(major, 0);	/* 大部分驱动次设备号都选择 0 */
7	register_chrdev_region(devid, 1, "test");
8 } else {	/* 没有定义设备号*/
9	alloc_chrdev_region(&devid, 0, 1, "test"); /* 申请设备号 */
10	major = MAJOR(devid);	/* 获取分配号的主设备号*/
11	minor = MINOR(devid);	/* 获取分配号的次设备号*/
12 }
```

第 1~3 行，定义了主/次设备号变量 major 和 minor，以及设备号变量 devid。
第 5 行，判断主设备号 major 是否有效，在 Linux 驱动中一般给出主设备号的话就表示这个设备的设备号已经确定了，因为次设备号基本上都选择 0，这算个 Linux 驱动开发中约定俗成的一种规定了。
第 6 行，如果 major 有效的话就使用 MKDEV 来构建设备号，次设备号选择 0。
第 7 行，使用 register_chrdev_region 函数来注册设备号。
第 9~11 行，如果 major 无效，那就表示没有给定设备号。此时就要使用 alloc_chrdev_region

函数来申请设备号。设备号申请成功以后使用 MAJOR 和 MINOR 来提取出主设备号和次设备号，当然了，第 10 和 11 行提取主设备号和次设备号的代码可以不要。

如果要注销设备号的话，使用如下代码即可：

``1 unregister_chrdev_region(devid, 1);	/* 注销设备号 */``

## 新的字符设备注册方法

不再使用下属两种语句实现，而是使用 cdev 结构体
```
* 注册字符设备驱动 */
retvalue = register_chrdev(CHRDEVBASE_MAJOR, CHRDEVBASE_NAME, &chrdevbase_fops);

/* 注销字符设备驱动 */
unregister_chrdev(CHRDEVBASE_MAJOR, CHRDEVBASE_NAME);
```

### 1、字符设备结构
在 Linux 中使用 cdev 结构体表示一个字符设备，cdev 结构体在 include/linux/cdev.h 文件中的定义如下：

```
1 struct cdev {
2	struct kobject	kobj;
3	struct module	*owner;
4	const struct file_operations *ops;
5	struct list_head list;
6	dev_t	dev;
7	unsigned int	count;
8 };```

```
在 cdev 中有两个重要的成员变量：ops 和 dev，这两个就是字符设备文件操作函数集合file_operations 以及设备号 dev_t。编写字符设备驱动之前需要定义一个 cdev 结构体变量，这个变量就表示一个字符设备，如下所示：
```struct cdev test_cdev;```

### 2、cdev_init 函数

定义好 cdev 变量以后就要使用 cdev_init 函数对其进行初始化，cdev_init 函数原型如下：
```
void cdev_init(struct cdev *cdev, const struct file_operations *fops)
```
参数 cdev 就是要初始化的 cdev 结构体变量，参数 fops 就是字符设备文件操作函数集合。
使用 cdev_init 函数初始化 cdev 变量的示例代码如下：
```
1 struct cdev testcdev;
2
3 /* 设备操作函数 */
4 static struct file_operations test_fops = {
5 	owner = THIS_MODULE,
6	/* 其他具体的初始项 */
7 };
8
9 testcdev.owner = THIS_MODULE;
10 cdev_init(&testcdev, &test_fops); /* 初始化 cdev 结构体变量 */
```

### 3、cdev_add 函数

cdev_add 函数用于向 Linux 系统添加字符设备(cdev 结构体变量)，首先使用 cdev_init 函数完成对 cdev 结构体变量的初始化，然后使用 cdev_add 函数向 Linux 系统添加这个字符设备。cdev_add 函数原型如下：
```int cdev_add(struct cdev *p, dev_t dev, unsigned count)```
- 参数 p 指向要添加的字符设备(cdev 结构体变量)
- 参数 dev 就是设备所使用的设备号
- 参数 count 是要添加的设备数量。
 
完善上述示例代码，加入 cdev_add 函数，内容如下所示：
```
1 struct cdev testcdev;
2
3 /* 设备操作函数 */
4 static struct file_operations test_fops = {
	1. owner = THIS_MODULE,
6	/* 其他具体的初始项 */
7 };
8
9 testcdev.owner = THIS_MODULE;
10 cdev_init(&testcdev, &test_fops);	/* 初始化 cdev 结构体变量*/
11 cdev_add(&testcdev, devid, 1);	/* 添加字符设备*/
```

示例代码就是新的注册字符设备代码段，Linux 内核中大量的字符设备驱动都是采用这种方法向 Linux 内核添加字符设备。如果在加上分配设备号的程序，那么就它们一起实现的就是函数 register_chrdev 的功能。


### 3、cdev_del 函数

卸载驱动的时候一定要使用 cdev_del 函数从 Linux 内核中删除相应的字符设备，cdev_del函数原型如下：
```
void cdev_del(struct cdev *p)
```

参数 p 就是要删除的字符设备。如果要删除字符设备，参考如下代码：
```
1 cdev_del(&testcdev); /* 删除 cdev */
```


cdev_del 和 unregister_chrdev_region 这两个函数合起来的功能相当于unregister_chrdev 函数。

## 自动创建设备节点

在驱动中实现自动创建设备节点的功能以后，使用 modprobe 加载驱动模块成功的话就会自动在/dev 目录下创建对应的设备文件。

### mdev 机制

udev 是一个用户程序，在 Linux 下通过 udev 来实现设备文件的创建与删除，udev 可以检测系统中硬件设备状态，可以根据系统中硬件设备状态来创建或者删除设备文件。比如使用modprobe 命令成功加载驱动模块以后就自动在/dev 目录下创建对应的设备节点文件,使用rmmod 命令卸载驱动模块以后就删除掉/dev 目录下的设备节点文件。使用 busybox 构建根文件系统的时候，busybox 会创建一个 udev 的简化版本—mdev，所以在嵌入式 Linux 中我们使用mdev 来实现设备节点文件的自动创建与删除，Linux 系统中的热插拔事件也由 mdev 管理，在/etc/init.d/rcS 文件中如下语句：

```echo /sbin/mdev > /proc/sys/kernel/hotplug```

上述命令设置热插拔事件由 mdev 来管理，关于 udev 或 mdev 更加详细的工作原理这里就不详细探讨了，我们重点来学习一下如何通过 mdev 来实现设备文件节点的自动创建与删除。

### 42.2.1 创建和删除类

自动创建设备节点的工作是在驱动程序的入口函数中完成的，一般在 cdev_add 函数后面添加自动创建设备节点相关代码。首先要创建一个 class 类，class 是个结构体，定义在文件include/linux/device.h 里面。class_create 是类创建函数，class_create 是个宏定义，内容如下：
```
1 #define class_create(owner, name) \
2 ({ \
3 	static struct lock_class_key __key; \
4 	__class_create(owner, name, &__key); \
5 })
6
7 struct class *__class_create(struct module *owner, const char *name,
8 struct lock_class_key *key)
```
根据上述代码，将宏 class_create 展开以后内容如下：
```
struct class *class_create (struct module *owner, const char *name)
```
class_create 一共有两个参数，参数 owner 一般为 THIS_MODULE，参数 name 是类名字。返回值是个指向结构体 class 的指针，也就是创建的类。
卸载驱动程序的时候需要删除掉类，类删除函数为 class_destroy，函数原型如下：
```void class_destroy(struct class *cls);```
参数 cls 就是要删除的类。

### 创建设备
我们还需要在这个类下创建一个设备。使用 device_create 函数在类下面创建设备，device_create 函数原型如下：
```
struct device *device_create(struct class	*class,
	struct device *parent,
	dev_t	devt,
	void	*drvdata,
	const char	*fmt, ...)
```

- device_create 是个可变参数函数，
- 参数 class 就是设备要创建哪个类下面；
- 参数 parent 是父设备，一般为 NULL，也就是没有父设备；
- 参数 devt 是设备号；
- 参数 drvdata 是设备可能会使用的一些数据，一般为 NULL；
- 参数 fmt 是设备名字，如果设置 fmt=xxx 的话，就会生成/dev/xxx这个设备文件。
- 返回值就是创建好的设备

同样的，卸载驱动的时候需要删除掉创建的设备，设备删除函数为 device_destroy，函数原型如下：
```void device_destroy(struct class *class, dev_t devt)```

参数 class 是要删除的设备所处的类，参数 devt 是要删除的设备号。

### 参考示例
在驱动入口函数里面创建类和设备，在驱动出口函数里面删除类和设备，参考示例如下：
```
1 struct class *class;	/* 类*/
2 struct device *device; /* 设备*/
3 dev_t devid;	/* 设备号 */
4
5 /* 驱动入口函数 */
6 static int __init led_init(void)
7 {
8		/* 创建类 */
9		class = class_create(THIS_MODULE, "xxx");
10		/* 创建设备 */
11		device = device_create(class, NULL, devid, NULL, "xxx");
12		return 0;
13 }
14
15 /* 驱动出口函数 */
16 static void __exit led_exit(void)
17 {
18		/* 删除设备 */
19		device_destroy(newchrled.class, newchrled.devid);
20		/* 删除类 */
21		class_destroy(newchrled.class);
22 }
23
24 module_init(led_init);
25 module_exit(led_exit);
```

## 设置文件私有数据

每个硬件设备都有一些属性，比如主设备号(dev_t)，类(class)、设备(device)、开关状态(state)等等，在编写驱动的时候你可以将这些属性全部写成变量的形式，如下所示：
```
dev_t devid;	/* 设备号 */
struct cdev cdev; /* cdev */
struct class *class; /* 类 */
struct device *device; /* 设备 */
int major;	/* 主设备号 */
int minor;	/* 次设备号 */
```
这样写肯定没有问题，但是这样写不专业！对于一个设备的所有属性信息我们最好将其做成一个结构体。编写驱动 open 函数的时候将设备结构体作为私有数据添加到设备文件中，如下所示：
```
/* 设备结构体 */
1 struct test_dev{
2	dev_t devid;	/* 设备号	*/
3	struct cdev cdev; /* cdev	*/
4	struct class *class; /* 类	*/
5	struct device *device; /* 设备 */
6	int major;	/* 主设备号 */
7	int minor;	/* 次设备号 */
8 };
9
10 struct test_dev testdev;
11
12 /* open 函数 */
13 static int test_open(struct inode *inode, struct file *filp)
14 {
15	filp->private_data = &testdev; /* 设置私有数据 */
16	return 0;
17 }
```

在 open 函数里面设置好私有数据以后，在 write、read、close 等函数中直接读取 private_data即可得到设备结构体。

## 程序编写

### 驱动程序

```
#include <linux/types.h>
#include <linux/kernel.h>
#include <linux/delay.h>
#include <linux/ide.h>
#include <linux/init.h>
#include <linux/module.h>
#include <linux/errno.h>
#include <linux/gpio.h>
#include <linux/cdev.h>
#include <linux/device.h>

#include <asm/mach/map.h>
#include <asm/uaccess.h>
#include <asm/io.h>

#define NEWCHRLED_CNT			1		  	/* 设备号个数 */
#define NEWCHRLED_NAME			"newchrled"	/* 名字 */
#define LEDOFF 					0			/* 关灯 */
#define LEDON 					1			/* 开灯 */
 
/* 寄存器物理地址 */
#define CCM_CCGR1_BASE				(0X020C406C)	
#define SW_MUX_GPIO1_IO03_BASE		(0X020E0068)
#define SW_PAD_GPIO1_IO03_BASE		(0X020E02F4)
#define GPIO1_DR_BASE				(0X0209C000)
#define GPIO1_GDIR_BASE				(0X0209C004)

/* 映射后的寄存器虚拟地址指针 */
static void __iomem *IMX6U_CCM_CCGR1;
static void __iomem *SW_MUX_GPIO1_IO03;
static void __iomem *SW_PAD_GPIO1_IO03;
static void __iomem *GPIO1_DR;
static void __iomem *GPIO1_GDIR;

/* newchrled设备结构体 */
struct newchrled_dev{
	dev_t devid;			/* 设备号 	 */
	struct cdev cdev;		/* cdev 	*/
	struct class *class;		/* 类 		*/
	struct device *device;	/* 设备 	 */
	int major;				/* 主设备号	  */
	int minor;				/* 次设备号   */
};

struct newchrled_dev newchrled;	/* led设备 */

/*
 * @description		: LED打开/关闭
 * @param - sta 	: LEDON(0) 打开LED，LEDOFF(1) 关闭LED
 * @return 			: 无
 */
void led_switch(u8 sta)
{
	u32 val = 0;
	if(sta == LEDON) {
		val = readl(GPIO1_DR);
		val &= ~(1 << 3);	
		writel(val, GPIO1_DR);
	}else if(sta == LEDOFF) {
		val = readl(GPIO1_DR);
		val|= (1 << 3);	
		writel(val, GPIO1_DR);
	}	
}

/*
 * @description		: 打开设备
 * @param - inode 	: 传递给驱动的inode
 * @param - filp 	: 设备文件，file结构体有个叫做private_data的成员变量
 * 					  一般在open的时候将private_data指向设备结构体。
 * @return 			: 0 成功;其他 失败
 */
static int led_open(struct inode *inode, struct file *filp)
{
	filp->private_data = &newchrled; /* 设置私有数据 */
	return 0;
}

/*
 * @description		: 从设备读取数据 
 * @param - filp 	: 要打开的设备文件(文件描述符)
 * @param - buf 	: 返回给用户空间的数据缓冲区
 * @param - cnt 	: 要读取的数据长度
 * @param - offt 	: 相对于文件首地址的偏移
 * @return 			: 读取的字节数，如果为负值，表示读取失败
 */
static ssize_t led_read(struct file *filp, char __user *buf, size_t cnt, loff_t *offt)
{
	return 0;
}

/*
 * @description		: 向设备写数据 
 * @param - filp 	: 设备文件，表示打开的文件描述符
 * @param - buf 	: 要写给设备写入的数据
 * @param - cnt 	: 要写入的数据长度
 * @param - offt 	: 相对于文件首地址的偏移
 * @return 			: 写入的字节数，如果为负值，表示写入失败
 */
static ssize_t led_write(struct file *filp, const char __user *buf, size_t cnt, loff_t *offt)
{
	int retvalue;
	unsigned char databuf[1];
	unsigned char ledstat;

	retvalue = copy_from_user(databuf, buf, cnt);
	if(retvalue < 0) {
		printk("kernel write failed!\r\n");
		return -EFAULT;
	}

	ledstat = databuf[0];		/* 获取状态值 */

	if(ledstat == LEDON) {	
		led_switch(LEDON);		/* 打开LED灯 */
	} else if(ledstat == LEDOFF) {
		led_switch(LEDOFF);	/* 关闭LED灯 */
	}
	return 0;
}

/*
 * @description		: 关闭/释放设备
 * @param - filp 	: 要关闭的设备文件(文件描述符)
 * @return 			: 0 成功;其他 失败
 */
static int led_release(struct inode *inode, struct file *filp)
{
	return 0;
}

/* 设备操作函数 */
static struct file_operations newchrled_fops = {
	.owner = THIS_MODULE,
	.open = led_open,
	.read = led_read,
	.write = led_write,
	.release = 	led_release,
};

/*
 * @description	: 驱动出口函数
 * @param 		: 无
 * @return 		: 无
 */
static int __init led_init(void)
{
	u32 val = 0;

	/* 初始化LED */
	/* 1、寄存器地址映射 */
  	IMX6U_CCM_CCGR1 = ioremap(CCM_CCGR1_BASE, 4);
	SW_MUX_GPIO1_IO03 = ioremap(SW_MUX_GPIO1_IO03_BASE, 4);
  	SW_PAD_GPIO1_IO03 = ioremap(SW_PAD_GPIO1_IO03_BASE, 4);
	GPIO1_DR = ioremap(GPIO1_DR_BASE, 4);
	GPIO1_GDIR = ioremap(GPIO1_GDIR_BASE, 4);

	/* 2、使能GPIO1时钟 */
	val = readl(IMX6U_CCM_CCGR1);
	val &= ~(3 << 26);	/* 清楚以前的设置 */
	val |= (3 << 26);	/* 设置新值 */
	writel(val, IMX6U_CCM_CCGR1);

	/* 3、设置GPIO1_IO03的复用功能，将其复用为
	 *    GPIO1_IO03，最后设置IO属性。
	 */
	writel(5, SW_MUX_GPIO1_IO03);
	
	/*寄存器SW_PAD_GPIO1_IO03设置IO属性
	 *bit 16:0 HYS关闭
	 *bit [15:14]: 00 默认下拉
     *bit [13]: 0 kepper功能
     *bit [12]: 1 pull/keeper使能
     *bit [11]: 0 关闭开路输出
     *bit [7:6]: 10 速度100Mhz
     *bit [5:3]: 110 R0/6驱动能力
     *bit [0]: 0 低转换率
	 */
	writel(0x10B0, SW_PAD_GPIO1_IO03);

	/* 4、设置GPIO1_IO03为输出功能 */
	val = readl(GPIO1_GDIR);
	val &= ~(1 << 3);	/* 清除以前的设置 */
	val |= (1 << 3);	/* 设置为输出 */
	writel(val, GPIO1_GDIR);

	/* 5、默认关闭LED */
	val = readl(GPIO1_DR);
	val |= (1 << 3);	
	writel(val, GPIO1_DR);

	/* 注册字符设备驱动 */
	/* 1、创建设备号 */
	if (newchrled.major) {		/*  定义了设备号 */
		newchrled.devid = MKDEV(newchrled.major, 0);
		register_chrdev_region(newchrled.devid, NEWCHRLED_CNT, NEWCHRLED_NAME);
	} else {						/* 没有定义设备号 */
		alloc_chrdev_region(&newchrled.devid, 0, NEWCHRLED_CNT, NEWCHRLED_NAME);	/* 申请设备号 */
		newchrled.major = MAJOR(newchrled.devid);	/* 获取分配号的主设备号 */
		newchrled.minor = MINOR(newchrled.devid);	/* 获取分配号的次设备号 */
	}
	printk("newcheled major=%d,minor=%d\r\n",newchrled.major, newchrled.minor);	
	
	/* 2、初始化cdev */
	newchrled.cdev.owner = THIS_MODULE;
	cdev_init(&newchrled.cdev, &newchrled_fops);
	
	/* 3、添加一个cdev */
	cdev_add(&newchrled.cdev, newchrled.devid, NEWCHRLED_CNT);

	/* 4、创建类 */
	newchrled.class = class_create(THIS_MODULE, NEWCHRLED_NAME);
	if (IS_ERR(newchrled.class)) {
		return PTR_ERR(newchrled.class);
	}

	/* 5、创建设备 */
	newchrled.device = device_create(newchrled.class, NULL, newchrled.devid, NULL, NEWCHRLED_NAME);
	if (IS_ERR(newchrled.device)) {
		return PTR_ERR(newchrled.device);
	}
	
	return 0;
}

/*
 * @description	: 驱动出口函数
 * @param 		: 无
 * @return 		: 无
 */
static void __exit led_exit(void)
{
	/* 取消映射 */
	iounmap(IMX6U_CCM_CCGR1);
	iounmap(SW_MUX_GPIO1_IO03);
	iounmap(SW_PAD_GPIO1_IO03);
	iounmap(GPIO1_DR);
	iounmap(GPIO1_GDIR);

	/* 注销字符设备驱动 */
	cdev_del(&newchrled.cdev);/*  删除cdev */
	unregister_chrdev_region(newchrled.devid, NEWCHRLED_CNT); /* 注销设备号 */

	device_destroy(newchrled.class, newchrled.devid);
	class_destroy(newchrled.class);
}

module_init(led_init);
module_exit(led_exit);
MODULE_LICENSE("GPL");
MODULE_AUTHOR("zuozhongkai");

```
### APP

与旧字符设备相同

## 运行测试

### 编译

#### 1、编译驱动程序
编写 Makefile 文件，本章实验的 Makefile 文件和就旧字符设备实验基本一样，只是将 obj-m 变量的值改为 newchrled.o，Makefile 内容如下所示：
```
1 KERNELDIR := /home/zuozhongkai/linux/IMX6ULL/linux/temp/linux-imx-
rel_imx_4.1.15_2.1.0_ga_alientek
......
4 obj-m := newchrled.o
......
11 clean:
12 $(MAKE) -C $(KERNELDIR) M=$(CURRENT_PATH) clean
```
第 4 行，设置 obj-m 变量的值为 newchrled.o。
输入如下命令编译出驱动模块文件：
```make -j32```
编译成功以后就会生成一个名为“newchrled.ko”的驱动模块文件。
#### 2、编译测试 APP
输入如下命令编译测试 ledApp.c 这个测试程序：
```arm-linux-gnueabihf-gcc ledApp.c -o ledApp```
编译成功以后就会生成 ledApp 这个应用程序。

### 运行测试

将上一小节编译出来的 newchrled.ko 和 ledApp 这两个文件拷贝到 rootfs/lib/modules/4.1.15
目录中，重启开发板，进入到目录 lib/modules/4.1.15 中，输入如下命令加载 newchrled.ko 驱动
模块：
```
depmod	//第一次加载驱动的时候需要运行此命令
modprobe newchrled.ko	//加载驱动
```
驱动加载成功以后会输出申请到的主设备号和次设备号，如图 42.6.2.1 所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658565295083.png)

从图 42.6.2.1 可以看出，申请到的主设备号为 249，次设备号为 0。驱动加载成功以后会自动在/dev 目录下创建设备节点文件/dev/newchrdev，输入如下命令查看/dev/newchrdev 这个设备节点文件是否存在：
```ls /dev/newchrled -l```
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658565321117.png)

从图 42.6.2.2 中可以看出，/dev/newchrled 这个设备文件存在，而且主设备号为 249，此设备号为 0，说明设备节点文件创建成功。
驱动节点创建成功以后就可以使用 ledApp 软件来测试驱动是否工作正常，输入如下命令打开 LED 灯：
```./ledApp /dev/newchrled 1	//打开 LED 灯```
输入上述命令以后观察 I.MX6U-ALPHA 开发板上的红色 LED 灯是否点亮，如果点亮的话说明驱动工作正常。在输入如下命令关闭 LED 灯：
```./ledApp /dev/newchrled 0	//关闭 LED 灯```
输入上述命令以后观察 I.MX6U-ALPHA 开发板上的红色 LED 灯是否熄灭。如果要卸载驱动的话输入如下命令即可：
```rmmod newchrled.ko```
<!--more-->
