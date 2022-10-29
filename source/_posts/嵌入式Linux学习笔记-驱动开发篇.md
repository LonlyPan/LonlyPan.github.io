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

- 字符设备驱动是占用篇幅最大的一类驱动，因为字符设备最多，从最简单的点灯到 I2C、SPI、音频等都属于字符设备驱动的类型。
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

驱动编译成模块（编译进内核这里不讲解）
1. 编写驱动函数和APP函数：
2. 模块加载（insmod或modprobe ）：调用驱动程序中的 module_init(xxx_init)，再调用和初始化函数，xxx_init()
3. 注册字符设备：一般加载时，在初始化函数中同时会调用register_chrdev() 函数完成注册
3. 创建设备节点文件，APP通过操作该文件控制设备。实际就是驱动文件的一个映射。因为用户空间无法直接操作内核空间，驱动属于内核空间
4. 通过给APP文件传输参数，从而控制设备
5. 卸载设备

## 静态分配设备号

册字符设备的时候需要给设备指定一个设备号，这个设备号可以是驱动开发者静态的指定一个设备号，比如选择 200 这个主设备号。有一些常用的设备号已经被 Linux 内核开发者给分配掉了，具体分配的内容可以查看文档 Documentation/devices.txt。并不是说内核开发者已经分配掉的主设备号我们就不能用了，具体能不能用还得看我们的硬件平台运行过程中有没有使用这个主设备号，使用 
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
> 这里的头文件是我们移植到开发板的linux源码头文件，不是ubuntu中的，因为软件最终是在板子上跑的

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

编译生成.ko驱动文件

“\_\_init”、“\_\_exit”修饰。实际上是汇编指示，编译器会把所有修饰过的放在一起，用在系统初始化，一旦内核启动后，就释放这些东西。只有在kernel初始化的时候会被调用，以后一定不会被使用，kernel可能会在以后的某个时候释放掉这个段所占用的内存，给别的地方使用

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
比如，现在要从 chrdevbase 设备中读取数据，需要输入如下命令：`./chrdevbaseApp /dev/chrdevbase 1`上述命令一共有三个参数“./chrdevbaseApp”、“/dev/chrdevbase”和“1”，这三个参数分别对应 argv[0]、argv[1]和 argv[2]。
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
- 从网络启动内核和设备树
- 从网络启动根文件系统

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
> app 文件要下赋予可执行权限
> 可能是网络不好，我这里执行程序后，串口会卡顿几秒甚至一分钟，然后板子上的灯才会有变化，如果你这里也一样，就耐心等一会
> 后期使用，发现文件夹是复制过来的，没有修改权限，程序卡顿也可能和这个有关。给与777权限后，不卡顿

```./ledApp /dev/newchrled 1	//打开 LED 灯```
输入上述命令以后观察 I.MX6U-ALPHA 开发板上的红色 LED 灯是否点亮，如果点亮的话说明驱动工作正常。在输入如下命令关闭 LED 灯：
```./ledApp /dev/newchrled 0	//关闭 LED 灯```
输入上述命令以后观察 I.MX6U-ALPHA 开发板上的红色 LED 灯是否熄灭。如果要卸载驱动的话输入如下命令即可：
```rmmod newchrled.ko```

# linux设备树

## 什么是设备树？

设备树(Device Tree)，将这个词分开就是“设备”和“树”，描述设备树的文件叫做 DTS(Device Tree Source)，这个 DTS 文件采用树形结构描述板级设备，也就是开发板上的设备信息，比如CPU 数量、 内存基地址、IIC 接口上接了哪些设备、SPI 接口上接了哪些设备等等，如图 43.1.1所示：

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658628442853.png)

在图中，树的主干就是系统总线，IIC 控制器、GPIO 控制器、SPI 控制器等都是接
到系统主线上的分支。IIC 控制器有分为 IIC1 和 IIC2 两种，其中 IIC1 上接了 FT5206 和 AT24C02这两个 IIC 设备，IIC2 上只接了 MPU6050 这个设备。DTS 文件的主要功能就是按照图所示的结构来描述板子上的设备信息，DTS 文件描述设备信息是有相应的语法规则要求的，稍后我们会详细的讲解 DTS 语法规则。

在 3.x 版本(具体哪个版本笔者也无从考证)以前的 Linux 内核中 ARM 架构并没有采用设备树。在没有设备树的时候 Linux 是如何描述 ARM 架构中的板级信息呢？在 Linux 内核源码中大量的 arch/arm/mach-xxx 和 arch/arm/plat-xxx 文件夹，这些文件夹里面的文件就是对应平台下的板级信息。

随着智能手机的发展，每年新出的 ARM 架构芯片少说都在数十、数百款，Linux 内核下板级信息文件将会成指数级增长！这些板级信息文件都是.c 或.h 文件，都会被硬编码进 Linux 内核中，导致 Linux 内核“虚胖”。

之后 ARM 社区就引入了 PowerPC 等架构已经采用的设备树(Flattened Device Tree)，将这些描述板级硬件信息的内容都从 Linux 内中分离开来，用一个专属的文件格式来描述，这个专属的文件就叫做设备树，文件扩展名为.dts。一个 SOC 可以作出很多不同的板子，这些不同的板子肯定是有共同的信息，将这些共同的信息提取出来作为一个通用的文件，其他的.dts 文件直接引用这个通用文件即可，这个通用文件就是.dtsi 文件，类似于 C 语言中的头文件。
- 一般.dts 描述板级信息(也就是开发板上有哪些 IIC 设备、SPI 设备等)，
- .dtsi 描述 SOC 级信息(也就是 SOC 有几个 CPU、主频是多少、各个外设控制器信息等)。

## DTS、DTB 和 DTC

DTS 是设备树源码文件，DTB 是将 DTS 编译以后得到的二进制文件。

如果要编译 DTS 文件的话只需要进入到 Linux 源码根目录下，然后执行如下命令：
`make all` 或者：`make dtbs`
“make all”命令是编译 Linux 源码中的所有东西，包括 zImage，.ko 驱动模块以及设备
树，如果只是编译设备树的话建议使用“make dtbs”命令。

基于 ARM 架构的 SOC 有很多种，一种 SOC 又可以制作出很多款板子，每个板子都有一个对应的 DTS 文件，那么如何确定编译哪一个 DTS 文件呢？我们就以 I.MX6ULL 这款芯片对应的板子为例来看一下，打开 arch/arm/boot/dts/Makefile，有如下内容：

```
381 dtb-$(CONFIG_SOC_IMX6UL) += \
382 imx6ul-14x14-ddr3-arm2.dtb \
383 imx6ul-14x14-ddr3-arm2-emmc.dtb \
......
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
423 imx6ull-alientek-nand.dtb \
424 imx6ull-9x9-evk.dtb \
425 imx6ull-9x9-evk-btwifi.dtb \
426 imx6ull-9x9-evk-ldo.dtb
427 dtb-$(CONFIG_SOC_IMX6SLL) += \
```

可以看出，当选中 I.MX6ULL 这个 SOC 以后(CONFIG_SOC_IMX6ULL=y)，所有使用到I.MX6ULL 这个 SOC 的板子对应的.dts 文件都会被编译为.dtb。如果我们使用 I.MX6ULL 新做了一个板子，只需要新建一个此板子对应的.dts 文件，然后将对应的.dtb 文件名添加到 dtb-$(CONFIG_SOC_IMX6ULL)下，这样在编译设备树的时候就会将对应的.dts 编译为二进制的.dtb文件。

示例代码 43.2.2 中第 422 和 423 行就是我们在给正点原子的 I.MX6U-ALPHA 开发板移植Linux 系统的时候添加的设备树。

## DTS 语法

大多时候是直接在 SOC 厂商提供的.dts文件上进行修改。我们肯定需要修改.dts文件。

本节我们就以 imx6ull-alientek-emmc.dts 这个文件为例来讲解一下 DTS 语法。关于设备树详细的语法规则请参考开发板光盘中，路 径 为 ： 4 、 参 考 资 料 ->Devicetree SpecificationV0.2.pdf 、 4 、 参 考 资 料 ->Power_ePAPR_APPROVED_v1.12.pdf

### .dtsi 头文件
和 C 语言一样，设备树也支持头文件，设备树的头文件扩展名为.dtsi。在 imx6ull-alientek-emmc.dts 中有如下所示内容：
```
12 #include <dt-bindings/input/input.h>
13 #include "imx6ull.dtsi"
```
- 第 12 行，使用“#include”来引用“input.h”这个.h 头文件。
- 第 13 行，使用“#include”来引用“imx6ull.dtsi”这个.dtsi 头文件。

在.dts 设备树文件中，可以通过 “#include”来引用.h、.dtsi 和.dts 文件。只是，我们在编写设备树头文件的时候最好选择.dtsi 后缀。

一般.dtsi 文件用于描述 SOC 的内部外设信息，比如 CPU 架构、主频、外设寄存器地址范围，比如 UART、IIC 等等。比如 imx6ull.dtsi 就是描述 I.MX6ULL 这颗 SOC 内部外设情况信息的

### 设备节点

设备树是采用树形结构来描述板子上的设备信息的文件，每个设备都是一个节点，叫做设备节点，每个节点都通过一些属性信息来描述节点信息，属性就是键—值对。以下是从imx6ull.dtsi 文件中缩减出来的设备树文件内容：

```
1 / {
2 		aliases {
3 		can0 = &flexcan1;
4 		};
5 
6		cpus {
7 			#address-cells = <1>;
8 			#size-cells = <0>;
9
10 			cpu0: cpu@0 {
11 				compatible = "arm,cortex-a7";
12 				device_type = "cpu";
13 				reg = <0>;
14 			};
15 		};
16
17 		intc: interrupt-controller@00a01000 {
18 			compatible = "arm,cortex-a7-gic";
19 			#interrupt-cells = <3>;
20 			interrupt-controller;
21 			reg = <0x00a01000 0x1000>,
22 				  <0x00a02000 0x100>;
23 		};
24 }
```

第 1 行，“/”是根节点，每个设备树文件只有一个根节点。细心的同学应该会发现， imx6ull.dtsi 和 imx6ull-alientek-emmc.dts 这两个文件都有一个“/”根节点，这样不会出错吗？不会的，因为这两个“/”根节点的内容会合并成一个根节点。
第 2、 6 和 17 行， aliases、 cpus 和 intc 是三个子节点，在设备树中节点命名格式如下：
```node-name@unit-address```
其中“node-name”是节点名字，为 ASCII 字符串，节点名字应该能够清晰的描述出节点的功能，比如“uart1”就表示这个节点是 UART1 外设。“unit-address”一般表示设备的地址或寄存器首地址，如果某个节点没有地址或者寄存器的话“unit-address”可以不要，比如“cpu@0”、“interrupt-controller@00a01000”。

但是我们在示例代码 43.3.2.1 中我们看到的节点命名却如下所示：
```cpu0:cpu@0```
用“：”隔开成了两部分，“：”前面的是节点标签(label)，“：”后面的才是节点名字，格式如下所示：
```label: node-name@unit-address```
引入 label 的目的就是为了方便访问节点，可以直接通过&label 来访问这个节点，比如通过&cpu0 就可以访问“cpu@0”这个节点，而不需要输入完整的节点名字。再比如节点 “intc:interrupt-controller@00a01000”，节点 label 是 intc，而节点名字就很长了，为“ interruptcontroller@00a01000”。很明显通过&intc 来访问“interrupt-controller@00a01000”这个节点要方便很多！
第 10 行， cpu0 也是一个节点，只是 cpu0 是 cpus 的子节点。
每个节点都有不同属性，不同的属性又有不同的内容，属性都是键值对，值可以为空或任意的字节流。设备树源码中常用的几种数据形式如下所示：
- ①、字符串
compatible = "arm,cortex-a7";
上述代码设置 compatible 属性的值为字符串“arm,cortex-a7”。
- ②、 32 位无符号整数
reg = <0>;
上述代码设置 reg 属性的值为 0， reg 的值也可以设置为一组值，比如：
reg = <0 0x123456 100>;
- ③、字符串列表
属性值也可以为字符串列表，字符串和字符串之间采用“,”隔开，如下所示：
compatible = "fsl,imx6ull-gpmi-nand", "fsl, imx6ul-gpmi-nand";
上述代码设置属性 compatible 的值为“fsl,imx6ull-gpmi-nand”和“fsl, imx6ul-gpmi-nand”。

### 标准属性

节点是由一堆的属性组成，节点都是具体的设备，不同的设备需要的属性不同，用户可以自定义属性。除了用户自定义属性，有很多属性是标准属性， Linux 下的很多外设驱动都会使用这些标准属性，本节我们就来学习一下几个常用的标准属性。

#### 1、 compatible 属性

compatible 属性也叫做“兼容性”属性，这是非常重要的一个属性！ compatible 属性的值是一个字符串列表， compatible 属性用于将设备和驱动绑定起来。字符串列表用于选择设备所要使用的驱动程序， compatible 属性的值格式如下所示：
```"manufacturer,model"```
其中 manufacturer 表示厂商， model 一般是模块对应的驱动名字。比如 imx6ull-alientekemmc.dts 中 sound 节点是 I.MX6U-ALPHA 开发板的音频设备节点， I.MX6U-ALPHA 开发板上的音频芯片采用的欧胜(WOLFSON)出品的 WM8960， sound 节点的 compatible 属性值如下：
```compatible = "fsl,imx6ul-evk-wm8960","fsl,imx-audio-wm8960";```
属性值有两个，分别为“fsl,imx6ul-evk-wm8960”和“fsl,imx-audio-wm8960”，其中“fsl”表示厂商是飞思卡尔，“imx6ul-evk-wm8960”和“imx-audio-wm8960”表示驱动模块名字。 sound这个设备首先使用第一个兼容值在 Linux 内核里面查找，看看能不能找到与之匹配的驱动文件，如果没有找到的话就使用第二个兼容值查。

驱动程序文件都会有一个 OF 匹配表，此 OF 匹配表保存着一些 compatible 值，如果设备节点的 compatible 属性值和 OF 匹配表中的任何一个值相等，那么就表示设备可以使用这个驱动。比如在驱动文件 imx-wm8960.c 中有如下内容：
```
632 static const struct of_device_id imx_wm8960_dt_ids[] = {
633 	{ .compatible = "fsl,imx-audio-wm8960", },
634 	{ /* sentinel */ }
635 };
636 MODULE_DEVICE_TABLE(of, imx_wm8960_dt_ids);
637
638 static struct platform_driver imx_wm8960_driver = {
639 	.driver = {
640 		.name = "imx-wm8960",
641 		.pm = &snd_soc_pm_ops,
642			 .of_match_table = imx_wm8960_dt_ids,
643 	},
644	 	.probe = imx_wm8960_probe,
645 	.remove = imx_wm8960_remove,
646 };
```
第 632~635 行的数组 imx_wm8960_dt_ids 就是 imx-wm8960.c 这个驱动文件的匹配表，此匹配表只有一个匹配值“fsl,imx-audio-wm8960”。如果在设备树中有哪个节点的 compatible 属性值与此相等，那么这个节点就会使用此驱动文件。
第 642 行， wm8960 采用了 platform_driver 驱动模式，关于 platform_driver 驱动后面会讲解。此行设置.of_match_table 为 imx_wm8960_dt_ids，也就是设置这个 platform_driver 所使用的OF 匹配表。

#### 2、 model 属性
model 属性值也是一个字符串，一般 model 属性描述设备模块信息，比如名字什么的，比如：
```model = "wm8960-audio";```

#### 3、 status 属性
status 属性看名字就知道是和设备状态有关的， status 属性值也是字符串，字符串是设备的状态信息，可选的状态如表 43.3.3.1 所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658631410840.png)

#### 4、 #address-cells 和#size-cells 属性
这两个属性的值都是无符号 32 位整形， #address-cells 和#size-cells 这两个属性可以用在任何拥有子节点的设备中，用于描述子节点的地址信息。 #address-cells 属性值决定了子节点 reg 属性中地址信息所占用的字长(32 位)， #size-cells 属性值决定了子节点 reg 属性中长度信息所占的字长(32 位)。 #address-cells 和#size-cells 表明了子节点应该如何编写 reg 属性值，一般 reg 属性
都是和地址有关的内容，和地址相关的信息有两种：起始地址和地址长度， reg 属性的格式一为：
```reg = <address1 length1 address2 length2 address3 length3……>```
每个“address length”组合表示一个地址范围，其中 address 是起始地址， length 是地址长度， #address-cells 表明 address 这个数据所占用的字长， #size-cells 表明 length 这个数据所占用的字长，比如:
```
1 spi4 {
2 		compatible = "spi-gpio";
3 		#address-cells = <1>;
4 		#size-cells = <0>;
5 
6		gpio_spi: gpio_spi@0 {
7 			compatible = "fairchild,74hc595";
8 			reg = <0>;
9 		};
10 };
11
12 aips3: aips-bus@02200000 {
13 		compatible = "fsl,aips-bus", "simple-bus";
14 		#address-cells = <1>;
15 		#size-cells = <1>;
16
17 		dcp: dcp@02280000 {
18 			compatible = "fsl,imx6sl-dcp";
19 			reg = <0x02280000 0x4000>;
20 		};
21 };
```
第 3， 4 行，节点 spi4 的#address-cells = <1>， #size-cells = <0>，说明 spi4 的子节点 reg 属性中起始地址所占用的字长为 1，地址长度所占用的字长为 0。
第 8 行，子节点 gpio_spi: gpio_spi@0 的 reg 属性值为 <0>，因为父节点设置了#addresscells = <1>， #size-cells = <0>，因此 addres=0，没有 length 的值，相当于设置了起始地址，而没有设置地址长度。
第 14， 15 行，设置 aips3: aips-bus@02200000 节点#address-cells = <1>， #size-cells = <1>，说明 aips3: aips-bus@02200000 节点起始地址长度所占用的字长为 1，地址长度所占用的字长也为 1。
第 19 行，子节点 dcp: dcp@02280000 的 reg 属性值为<0x02280000 0x4000>，因为父节点设置了#address-cells = <1>， #size-cells = <1>， address= 0x02280000， length= 0x4000，相当于设置了起始地址为 0x02280000，地址长度为 0x40000。

#### 5、 reg 属性
reg 属性前面已经提到过了， reg 属性的值一般是(address， length)对。 reg 属性一般用于描述设备地址空间资源信息，一般都是某个外设的寄存器地址范围信息，比如在 imx6ull.dtsi 中有如下内容：
```
323 uart1: serial@02020000 {
324 	compatible = "fsl,imx6ul-uart",
325 		"fsl,imx6q-uart", "fsl,imx21-uart";
326 	reg = <0x02020000 0x4000>;
327 	interrupts = <GIC_SPI 26 IRQ_TYPE_LEVEL_HIGH>;
328 	clocks = <&clks IMX6UL_CLK_UART1_IPG>,
329 		<&clks IMX6UL_CLK_UART1_SERIAL>;
330 	clock-names = "ipg", "per";
331 	status = "disabled";
332 };
```

上述代码是节点 uart1， uart1 节点描述了 I.MX6ULL 的 UART1 相关信息，重点是第 326 行的 reg 属性。其中 uart1 的父节点 aips1: aips-bus@02000000 设置了#address-cells = <1>、 #sizecells = <1>，因此 reg 属性中 address=0x02020000， length=0x4000。查阅《I.MX6ULL 参考手册》可知， I.MX6ULL 的 UART1 寄存器首地址为 0x02020000，但是 UART1 的地址长度(范围)并没有 0x4000 这么多，这里我们重点是获取 UART1 寄存器首地址。

#### 6、 ranges 属性

ranges属性值可以为空或者按照(child-bus-address,parent-bus-address,length)格式编写的数字矩阵， ranges 是一个地址映射/转换表， ranges 属性每个项目由子地址、父地址和地址空间长度这三部分组成：
- child-bus-address：子总线地址空间的物理地址，由父节点的#address-cells 确定此物理地址所占用的字长。
- parent-bus-address： 父总线地址空间的物理地址，同样由父节点的#address-cells 确定此物理地址所占用的字长。
- length： 子地址空间的长度，由父节点的#size-cells 确定此地址长度所占用的字长。如果 ranges 属性值为空值，说明子地址空间和父地址空间完全相同，不需要进行地址转换，对于我们所使用的 I.MX6ULL 来说，子地址空间和父地址空间完全相同，因此会在 imx6ull.dtsi中找到大量的值为空的 ranges 属性，如下所示：
- 
```
137 soc {
138 	#address-cells = <1>;
139 	#size-cells = <1>;
140 	compatible = "simple-bus";
141 	interrupt-parent = <&gpc>;
142 	ranges;
......
1177 }
```

第 142 行定义了 ranges 属性，但是 ranges 属性值为空。
ranges 属性不为空的示例代码如下所示：

```
1 soc {
2 	compatible = "simple-bus";
3 	#address-cells = <1>;
4 	#size-cells = <1>;
5 	ranges = <0x0 0xe0000000 0x00100000>;
6 
7	serial {
8 		device_type = "serial";
9 		compatible = "ns16550";
10 		reg = <0x4600 0x100>;
11 		clock-frequency = <0>;
12 		interrupts = <0xA 0x8>;
13 		interrupt-parent = <&ipic>;
14 	};
15 };
```
第 5 行，节点 soc 定义的 ranges 属性，值为<0x0 0xe0000000 0x00100000>，此属性值指定了一个 1024KB(0x00100000)的地址范围，子地址空间的物理起始地址为 0x0，父地址空间的物理起始地址为 0xe0000000。
第 10 行， serial 是串口设备节点， reg 属性定义了 serial 设备寄存器的起始地址为 0x4600，寄存器长度为 0x100。经过地址转换， serial 设备可以从 0xe0004600 开始进行读写操作，0xe0004600=0x4600+0xe0000000。

#### 7、 name 属性
name 属性值为字符串， name 属性用于记录节点名字， name 属性已经被弃用，不推荐使用name 属性，一些老的设备树文件可能会使用此属性。

#### 8、 device_type 属性
device_type 属性值为字符串， IEEE 1275 会用到此属性，用于描述设备的 FCode，但是设备树没有 FCode，所以此属性也被抛弃了。此属性只能用于 cpu 节点或者 memory 节点。imx6ull.dtsi 的 cpu0 节点用到了此属性，内容如下所示：

```
54 cpu0: cpu@0 {
55 	compatible = "arm,cortex-a7";
56 	device_type = "cpu";
57 	reg = <0>;
......
89 };
```

### 根节点 compatible 属性

每个节点都有 compatible 属性，根节点“/”也不例外， imx6ull-alientek-emmc.dts 文件中根节点的 compatible 属性内容如下所示：

```
14 / {
15 	model = "Freescale i.MX6 ULL 14x14 EVK Board";
16 	compatible = "fsl,imx6ull-14x14-evk", "fsl,imx6ull";
......
148 }
```

可以看出， compatible 有两个值：“fsl,imx6ull-14x14-evk”和“fsl,imx6ull”。前面我们说了，设备节点的 compatible 属性值是为了匹配 Linux 内核中的驱动程序，那么根节点中的 compatible属性是为了做什么工作的？ 通过根节点的 compatible 属性可以知道我们所使用的设备，一般第一个值描述了所使用的硬件设备名字，比如这里使用的是“imx6ull-14x14-evk”这个设备，第个值描述了设备所使用的 SOC，比如这里使用的是“imx6ull”这颗 SOC。 Linux 内核会通过根节点的 compoatible 属性查看是否支持此设备，如果支持的话设备就会启动 Linux 内核。

接下来我们就来学习一下 Linux 内核在使用设备树前后是如何判断是否支持某款设备的。

#### 1、使用设备树之前设备匹配方法

在没有使用设备树以前， uboot 会向 Linux 内核传递一个叫做 machine id 的值， machine id 也就是设备 ID，告诉 Linux 内核自己是个什么设备，看看 Linux 内核是否支持。 Linux 内核是支持很多设备的，针对每一个设备(板子)， Linux内核都用MACHINE_START和MACHINE_END来定义一个 machine_desc 结构体来描述这个设备，比如在文件 arch/arm/mach-imx/machmx35_3ds.c 中有如下定义：

```
613 MACHINE_START(MX35_3DS, "Freescale MX35PDK")
614 	/* Maintainer: Freescale Semiconductor, Inc */
615 	.atag_offset = 0x100,
616 	.map_io = mx35_map_io,
617 	.init_early = imx35_init_early,
618 	.init_irq = mx35_init_irq,
619 	.init_time = mx35pdk_timer_init,
620 	.init_machine = mx35_3ds_init,
621 	.reserve = mx35_3ds_reserve,
622 	.restart = mxc_restart,
623 MACHINE_END
```

上述代码就是定义了“ Freescale MX35PDK”这个设备，其中 MACHINE_START 和MACHINE_END 定义在文件 arch/arm/include/asm/mach/arch.h 中，内容如下：

```
#define MACHINE_START(_type,_name) \
static const struct machine_desc __mach_desc_##_type \
__used \
__attribute__((__section__(".arch.info.init"))) = { \
	.nr = MACH_TYPE_##_type, \
	.name = _name,
#define MACHINE_END \
};
```

根据 MACHINE_START 和 MACHINE_END 的宏定义，将示例代码 43.3.4.2 展开后如下所示：

```
1 static const struct machine_desc __mach_desc_MX35_3DS \
2 	__used \
3 	__attribute__((__section__(".arch.info.init"))) = {
4 	.nr = MACH_TYPE_MX35_3DS,
5 	.name = "Freescale MX35PDK",
6 	/* Maintainer: Freescale Semiconductor, Inc */
7 	.atag_offset = 0x100,
8 	.map_io = mx35_map_io,
9 	.init_early = imx35_init_early,
10 	.init_irq = mx35_init_irq,
11 	.init_time = mx35pdk_timer_init,
12 	.init_machine = mx35_3ds_init,
13 	.reserve = mx35_3ds_reserve,
14 	.restart = mxc_restart,
15 };
```

从示例代码 43.3.4.3 中可以看出，这里定义了一个 machine_desc 类型的结构体变量__mach_desc_MX35_3DS ， 这 个 变 量 存 储 在 “ .arch.info.init ” 段 中 。 第 4 行 的MACH_TYPE_MX35_3DS 就 是 “ Freescale MX35PDK ” 这 个 板 子 的 machine id 。MACH_TYPE_MX35_3DS 定义在文件 include/generated/mach-types.h 中，此文件定义了大量的
machine id，内容如下所示：
```
15 #define MACH_TYPE_EBSA110 0
16 #define MACH_TYPE_RISCPC 1
17 #define MACH_TYPE_EBSA285 4
18 #define MACH_TYPE_NETWINDER 5
19 #define MACH_TYPE_CATS 6
20 #define MACH_TYPE_SHARK 15
21 #define MACH_TYPE_BRUTUS 16
22 #define MACH_TYPE_PERSONAL_SERVER 17
......
287 #define MACH_TYPE_MX35_3DS 1645
......
1000 #define MACH_TYPE_PFLA03 4575
```
第 287 行就是 MACH_TYPE_MX35_3DS 的值，为 1645。前面说了， uboot 会给 Linux 内核传递 machine id 这个参数， Linux 内核会检查这个 machineid，其实就是将 machine id 与示例代码 43.3.4.3 中的这些 MACH_TYPE_XXX 宏进行对比，看看有没有相等的，如果相等的话就表示 Linux 内核支持这个设备，如果不支持的话那么这个设
备就没法启动 Linux 内核。

#### 2、使用设备树以后的设备匹配方法

当 Linux 内 核 引 入 设 备 树 以 后 就 不 再 使 用 MACHINE_START 了 ， 而 是 换 为 了DT_MACHINE_START。 DT_MACHINE_START 也定义在文件 arch/arm/include/asm/mach/arch.h里面，定义如下：

```
#define DT_MACHINE_START(_name, _namestr) \
static const struct machine_desc __mach_desc_##_name \
__used \
__attribute__((__section__(".arch.info.init"))) = { \
	.nr = ~0, \
	.name = _namestr,
```

可以看出， DT_MACHINE_START 和 MACHINE_START 基本相同，只是.nr 的设置不同，在 DT_MACHINE_START 里面直接将.nr 设置为~0。说明引入设备树以后不会再根据 machine id 来检查 Linux 内核是否支持某个设备了。
打开文件 arch/arm/mach-imx/mach-imx6ul.c，有如下所示内容：

```
208 static const char *imx6ul_dt_compat[] __initconst = {
209 	"fsl,imx6ul",
210 	"fsl,imx6ull",
211 	NULL,
212 };
213
214 DT_MACHINE_START(IMX6UL, "Freescale i.MX6 Ultralite (Device Tree)")
215 	.map_io = imx6ul_map_io,
216 	.init_irq = imx6ul_init_irq,
217 	.init_machine = imx6ul_init_machine,
218 	.init_late = imx6ul_init_late,
219 	.dt_compat = imx6ul_dt_compat,
220 MACHINE_END
```

machine_desc 结构体中有个.dt_compat 成员变量，此成员变量保存着本设备兼容属性，示例代码 43.3.4.5 中设置.dt_compat = imx6ul_dt_compat， imx6ul_dt_compat 表里面有"fsl,imx6ul"和"fsl,imx6ull"这两个兼容值。只要某个设备(板子)根节点“ /”的 compatible 属性值与imx6ul_dt_compat 表中的任何一个值相等，那么就表示 Linux 内核支持此设备。 imx6ull-alientekemmc.dts 中根节点的 compatible 属性值如下：
```compatible = "fsl,imx6ull-14x14-evk", "fsl,imx6ull";```
其中“fsl,imx6ull”与 imx6ul_dt_compat 中的“fsl,imx6ull”匹配，因此 I.MX6U-ALPHA 开
发板可以正常启动 Linux 内核。如果将 imx6ull-alientek-emmc.dts 根节点的 compatible 属性改为其他的值，比如：
```compatible = "fsl,imx6ull-14x14-evk", "fsl,imx6ullll"```
重新编译 DTS，并用新的 DTS 启动 Linux 内核，结果如图 43.3.4.1 所示的错误提示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658632385165.png)

当我们修改了根节点 compatible 属性内容以后，因为 Linux 内核找不到对应的设备，因此Linux 内核无法启动。在 uboot 输出 Starting kernel…以后就再也没有其他信息输出了。
接下来我们简单看一下 Linux 内核是如何根据设备树根节点的 compatible 属性来匹配出对应的 machine_desc， Linux 内核调用 start_kernel 函数来启动内核， start_kernel 函数会调用setup_arch 函数来匹配 machine_desc， setup_arch 函数定义在文件 arch/arm/kernel/setup.c 中，函数内容如下(有缩减)：
```
913 void __init setup_arch(char **cmdline_p)
914 {
915 	const struct machine_desc *mdesc;
916
917 	setup_processor();
918 	mdesc = setup_machine_fdt(__atags_pointer);
919	 	if (!mdesc)
920 		mdesc = setup_machine_tags(__atags_pointer,
										__machine_arch_type);
921 	machine_desc = mdesc;
922 	machine_name = mdesc->name;
......
986 }
```

第 918 行，调用 setup_machine_fdt 函数来获取匹配的 machine_desc，参数就是 atags 的首地址，也就是 uboot 传递给 Linux 内核的 dtb 文件首地址， setup_machine_fdt 函数的返回值就是找到的最匹配的 machine_desc。函数 setup_machine_fdt 定义在文件 arch/arm/kernel/devtree.c 中，内容如下(有缩减)：
```
204 const struct machine_desc * __init setup_machine_fdt(unsigned int dt_phys)
205 {
206 	const struct machine_desc *mdesc, *mdesc_best = NULL;
......
214
215 	if (!dt_phys || !early_init_dt_verify(phys_to_virt(dt_phys)))
216 		return NULL;
217
218 	mdesc = of_flat_dt_match_machine(mdesc_best, arch_get_next_mach);
219
......
247 	__machine_arch_type = mdesc->nr;
248
249 	return mdesc;
250 }
```

第 218 行，调用函数 of_flat_dt_match_machine 来获取匹配的 machine_desc，参数 mdesc_best是 默 认 的 machine_desc ， 参 数 arch_get_next_mach 是 个 函 数 ， 此 函 数定 义 在arch/arm/kernel/devtree.c 文件中。找到匹配的 machine_desc 的过程就是用设备树根节点的compatible 属性值和 Linux 内核中 machine_desc 下.dt_compat 的值比较，看看那个相等，如果相等的话就表示找到匹配的 machine_desc， arch_get_next_mach 函数的工作就是获取 Linux 内核中下一个 machine_desc 结构体。

最后再来看一下 of_flat_dt_match_machine 函数，此函数定义在文件 drivers/of/fdt.c 中，内容如下(有缩减)：

```
705 const void * __init of_flat_dt_match_machine(const void *default_match, const void * (*get_next_compat)(const char * const**))
707 {
708 	const void *data = NULL;
709 	const void *best_data = default_match;
710 	const char *const *compat;
711 	unsigned long dt_root;
712 	unsigned int best_score = ~1, score = 0;
713
714 	dt_root = of_get_flat_dt_root();
715 	while ((data = get_next_compat(&compat))) {
716 		score = of_flat_dt_match(dt_root, compat);
717 		if (score > 0 && score < best_score) {
718 			best_data = data;
719 			best_score = score;
720 		}
721 }
......
739
740 pr_info("Machine model: %s\n", of_flat_dt_get_machine_name());
741
742 return best_data;
743 }
```

第 714 行，通过函数 of_get_flat_dt_root 获取设备树根节点。
第 715~720 行，此循环就是查找匹配的 machine_desc 过程，第 716 行的 of_flat_dt_match 函数会将根节点 compatible 属性的值和每个 machine_desc 结构体中. dt_compat 的值进行比较，直至找到匹配的那个 machine_desc。
总结一下， Linux 内核通过根节点 compatible 属性找到对应的设备的函数调用过程，如图43.3.4.2 所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658632543641.png)

### 向节点追加或修改内容

产品开发过程中可能面临着频繁的需求更改，比如第一版硬件上有一个 IIC 接口的六轴芯片 MPU6050，第二版硬件又要把这个 MPU6050 更换为 MPU9250 等。一旦硬件修改了，我们就要同步的修改设备树文件，毕竟设备树是描述板子硬件信息的文件。假设现在有个六轴芯片fxls8471， fxls8471 要接到 I.MX6U-ALPHA 开发板的 I2C1 接口上，那么相当于需要在 i2c1 这个节点上添加一个 fxls8471 子节点。先看一下 I2C1 接口对应的节点，打开文件 imx6ull.dtsi 文件，找到如下所示内容：
```
937 i2c1: i2c@021a0000 {
938 	#address-cells = <1>;
939 	#size-cells = <0>;
940 	compatible = "fsl,imx6ul-i2c", "fsl,imx21-i2c";
941 	reg = <0x021a0000 0x4000>;
942 	interrupts = <GIC_SPI 36 IRQ_TYPE_LEVEL_HIGH>;
943 	clocks = <&clks IMX6UL_CLK_I2C1>;
944 	status = "disabled";
945 };
```
示例代码 43.3.5.1 就是 I.MX6ULL 的 I2C1 节点，现在要在 i2c1 节点下创建一个子节点，这个子节点就是 fxls8471，最简单的方法就是在 i2c1 下直接添加一个名为 fxls8471 的子节点，如下所示：

```
937 i2c1: i2c@021a0000 {
938 	#address-cells = <1>;
939 	#size-cells = <0>;
940 	compatible = "fsl,imx6ul-i2c", "fsl,imx21-i2c";
941 	reg = <0x021a0000 0x4000>;
942 	interrupts = <GIC_SPI 36 IRQ_TYPE_LEVEL_HIGH>;
943 	clocks = <&clks IMX6UL_CLK_I2C1>;
944 	status = "disabled";
945
946 	//fxls8471 子节点
947 	fxls8471@1e {
948 		compatible = "fsl,fxls8471";
949 		reg = <0x1e>;
950 	};
951 };
```

第 947~950 行就是添加的 fxls8471 这个芯片对应的子节点。但是这样会有个问题！ i2c1 节点是定义在 imx6ull.dtsi 文件中的，而 imx6ull.dtsi 是设备树头文件，其他所有使用到 I.MX6ULL这颗 SOC 的板子都会引用 imx6ull.dtsi 这个文件。直接在 i2c1 节点中添加 fxls8471 就相当于在其他的所有板子上都添加了 fxls8471 这个设备，但是其他的板子并没有这个设备啊！因此，按照示例代码 43.3.5.2 这样写肯定是不行的。

这里就要引入另外一个内容，那就是如何向节点追加数据，我们现在要解决的就是如何向i2c1 节点追加一个名为 fxls8471 的子节点，而且不能影响到其他使用到 I.MX6ULL 的板子。I.MX6U-ALPHA 开发板使用的设备树文件为 imx6ull-alientek-emmc.dts，因此我们需要在imx6ull-alientek-emmc.dts 文件中完成数据追加的内容，方式如下：

```
1 &i2c1 {
2 /* 要追加或修改的内容 */
3 };
```

第 1 行， &i2c1 表示要访问 i2c1 这个 label 所对应的节点，也就是 imx6ull.dtsi 中的“i2c1:i2c@021a0000”。
第 2 行，花括号内就是要向 i2c1 这个节点添加的内容，包括修改某些属性的值。
打开 imx6ull-alientek-emmc.dts，找到如下所示内容：
```
224 &i2c1 {
225 	clock-frequency = <100000>;
226 	pinctrl-names = "default";
227 	pinctrl-0 = <&pinctrl_i2c1>;
228 	status = "okay";
229
230 	mag3110@0e {
231 		compatible = "fsl,mag3110";
232 		reg = <0x0e>;
233 		position = <2>;
234 	};
235
236 	fxls8471@1e {
237 		compatible = "fsl,fxls8471";
238 		reg = <0x1e>;
239 		position = <0>;
240 		interrupt-parent = <&gpio5>;
241 		interrupts = <0 8>;
242 	};
243 };
```

示例代码 43.3.5.4 就是向 i2c1 节点添加/修改数据，比如第 225 行的属性“clock-frequency”就表示 i2c1 时钟为 100KHz。“clock-frequency”就是新添加的属性。
第 228 行，将 status 属性的值由原来的 disabled 改为 okay。
第 230~234 行， i2c1 子节点 mag3110，因为 NXP 官方开发板在 I2C1 上接了一个磁力计芯片 mag3110，正点原子的 I.MX6U-ALPHA 开发板并没有使用 mag3110。
第 236~242 行， i2c1 子节点 fxls8471，同样是因为 NXP 官方开发板在 I2C1 上接了 fxls8471这颗六轴芯片。
因为示例代码 43.3.5.4 中的内容是 imx6ull-alientek-emmc.dts 这个文件内的，所以不会对使用 I.MX6ULL 这颗 SOC 的其他板子造成任何影响。这个就是向节点追加或修改内容，重点就是通过&label 来访问节点，然后直接在里面编写要追加或者修改的内容。

## 创建小型模板设备树

上一节已经对 DTS 的语法做了比较详细的讲解，本节我们就根据前面讲解的语法，从头到尾编写一个小型的设备树文件。当然了，这个小型设备树没有实际的意义，做这个的目的是为了掌握设备树的语法。在实际产品开发中，我们是不需要完完全全的重写一个.dts 设备树文件，一般都是使用 SOC 厂商提供好的.dts 文件，我们只需要在上面根据自己的实际情况做相应的修改即可。在编写设备树之前要先定义一个设备，我们就以 I.MX6ULL 这个 SOC 为例，我们需要
在设备树里面描述的内容如下：
- ①、 I.MX6ULL 这个 Cortex-A7 架构的 32 位 CPU。
- ②、 I.MX6ULL 内部 ocram，起始地址 0x00900000，大小为 128KB(0x20000)。
- ③、 I.MX6ULL 内部 aips1 域下的 ecspi1 外设控制器，寄存器起始地址为 0x02008000，大小为 0x4000。
- ④、 I.MX6ULL 内部 aips2 域下的 usbotg1 外设控制器，寄存器起始地址为 0x02184000，大小为 0x4000。
- ⑤、 I.MX6ULL 内部 aips3 域下的 rngb 外设控制器，寄存器起始地址为 0x02284000，大小为 0x4000。

为了简单起见，我们就在设备树里面就实现这些内容即可，首先，搭建一个仅含有根节点“/”的基础的框架，新建一个名为 myfirst.dts 文件，在里面输入如下所示内容：
```
1 / {
2 	compatible = "fsl,imx6ull-alientek-evk", "fsl,imx6ull";
3 }
```

设备树框架很简单，就一个根节点“/”，根节点里面只有一个 compatible 属性。我们就在这个基础框架上面将上面列出的内容一点点添加进来。

### 1、添加 cpus 节点

首先添加 CPU 节点， I.MX6ULL 采用 Cortex-A7 架构，而且只有一个 CPU，因此只有一个cpu0 节点，完成以后如下所示：
```
1 / {
2 	compatible = "fsl,imx6ull-alientek-evk", "fsl,imx6ull";
3
4 	cpus {
5 		#address-cells = <1>;
6 		#size-cells = <0>;
7 
8		//CPU0 节点
9 		cpu0: cpu@0 {
10 			compatible = "arm,cortex-a7";
11 			device_type = "cpu";
12 			reg = <0>;
13 		};
14 	};
15 }
```

第 4~14 行， cpus 节点，此节点用于描述 SOC 内部的所有 CPU，因为 I.MX6ULL 只有一个CPU，因此只有一个 cpu0 子节点。

### 2、添加 soc 节点

像 uart， iic 控制器等等这些都属于 SOC 内部外设，因此一般会创建一个叫做 soc 的父节点来管理这些 SOC 内部外设的子节点，添加 soc 节点以后的 myfirst.dts 文件内容如下所示：
```
1 / {
2 	compatible = "fsl,imx6ull-alientek-evk", "fsl,imx6ull";
3 
4	cpus {
5 		#address-cells = <1>;
6 		#size-cells = <0>;
7 
8		//CPU0 节点
9 		cpu0: cpu@0 {
10 			compatible = "arm,cortex-a7";
11 			device_type = "cpu";
12 			reg = <0>;
13 		};
14 	};
15
16 	//soc 节点
17 	soc {
18 		#address-cells = <1>;
19 		#size-cells = <1>;
20 		compatible = "simple-bus";
21 		ranges;
22 	}
23 }
```
第 17~22 行， soc 节点， soc 节点设置#address-cells = <1>， #size-cells = <1>，这样 soc 子节点的 reg 属性中起始地占用一个字长，地址空间长度也占用一个字长。
第 21 行， ranges 属性， rangeWs 属性为空，说明子空间和父空间地址范围相同。

### 3、添加 ocram 节点
根据第②点的要求，添加 ocram 节点， ocram 是 I.MX6ULL 内部 RAM，因此 ocram 节点应该是 soc 节点的子节点。 ocram 起始地址为 0x00900000，大小为 128KB(0x20000)，添加 ocram节点以后 myfirst.dts 文件内容如下所示：
```
1 / {
2 		compatible = "fsl,imx6ull-alientek-evk", "fsl,imx6ull";
3 
4		cpus {
5 			#address-cells = <1>;
6 			#size-cells = <0>;
7 
8			//CPU0 节点
9 			cpu0: cpu@0 {
10 				compatible = "arm,cortex-a7";
11 				device_type = "cpu";
12 				reg = <0>;
13 			};
14 		};
15
16 		//soc 节点
17 		soc {
18 			#address-cells = <1>;
19 			#size-cells = <1>;
20 			compatible = "simple-bus";
21 			ranges;
22
23 			//ocram 节点
24 			ocram: sram@00900000 {
25 				compatible = "fsl,lpm-sram";
26 				reg = <0x00900000 0x20000>;
27 			};
28 		}
29 }
```

第 24~27 行， ocram 节点，第 24 行节点名字@后面的 0x00900000 就是 ocram 的起始地址。
第 26 行的 reg 属性也指明了 ocram 内存的起始地址为 0x00900000，大小为 0x20000。

### 4、添加 aips1、 aips2 和 aips3 这三个子节点

I.MX6ULL 内部分为三个域： aips1~3，这三个域分管不同的外设控制器， aips1~3 这三个域对应的内存范围如表 43.4.1 所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658632984260.png)

我们先在设备树中添加这三个域对应的子节点。aips1~3 这三个域都属于 soc 节点的子节点，完成以后的 myfirst.dts 文件内容如下所示：
```
1 / {
2 compatible = "fsl,imx6ull-alientek-evk", "fsl,imx6ull";
3 4
cpus {
5 #address-cells = <1>;
6 #size-cells = <0>;
7 8
//CPU0 节点
9 cpu0: cpu@0 {
10 compatible = "arm,cortex-a7";
11 device_type = "cpu";
12 reg = <0>;
13 };
14 };
15
16 //soc 节点
17 soc {
18 #address-cells = <1>;
19 #size-cells = <1>;
20 compatible = "simple-bus";
21 ranges;
22
23 //ocram 节点
24 ocram: sram@00900000 {
25 compatible = "fsl,lpm-sram";
26 reg = <0x00900000 0x20000>;
27 };
28
29 //aips1 节点
30 aips1: aips-bus@02000000 {
31 compatible = "fsl,aips-bus", "simple-bus";
32 #address-cells = <1>;
33 #size-cells = <1>;
34 reg = <0x02000000 0x100000>;
35 ranges;
36 }
37
38 //aips2 节点
39 aips2: aips-bus@02100000 {
40 compatible = "fsl,aips-bus", "simple-bus";
41 #address-cells = <1>;
42 #size-cells = <1>;
43 reg = <0x02100000 0x100000>;
44 ranges;
45 }
46
47 //aips3 节点
48 aips3: aips-bus@02200000 {
49 compatible = "fsl,aips-bus", "simple-bus";
50 #address-cells = <1>;
51 #size-cells = <1>;
52 reg = <0x02200000 0x100000>;
53 ranges;
54 }
55 }
56 }
```

第 30~36 行， aips1 节点。
第 39~45 行， aips2 节点。
第 48~54 行， aips3 节点。
### 5、添加 ecspi1、 usbotg1 和 rngb 这三个外设控制器节点

最后我们在 myfirst.dts 文件中加入 ecspi1， usbotg1 和 rngb 这三个外设控制器对应的节点，其中 ecspi1 属于 aips1 的子节点， usbotg1 属于 aips2 的子节点， rngb 属于 aips3 的子节点。最终的 myfirst.dts 文件内容如下：

```
1 / {
2 compatible = "fsl,imx6ull-alientek-evk", "fsl,imx6ull";
3 4
cpus {
5 #address-cells = <1>;
6 #size-cells = <0>;
7 8
//CPU0 节点
9 cpu0: cpu@0 {
10 compatible = "arm,cortex-a7";
11 device_type = "cpu";
12 reg = <0>;
13 };
14 };
15
16 //soc 节点
17 soc {
18 #address-cells = <1>;
19 #size-cells = <1>;
20 compatible = "simple-bus";
21 ranges;
22
23 //ocram 节点
24 ocram: sram@00900000 {
25 compatible = "fsl,lpm-sram";
26 reg = <0x00900000 0x20000>;
27 };
28
29 //aips1 节点
30 aips1: aips-bus@02000000 {
31 compatible = "fsl,aips-bus", "simple-bus";
32 #address-cells = <1>;
33 #size-cells = <1>;
34 reg = <0x02000000 0x100000>;
35 ranges;
36
37 //ecspi1 节点
38 ecspi1: ecspi@02008000 {
39 #address-cells = <1>;
40 #size-cells = <0>;
41 compatible = "fsl,imx6ul-ecspi", "fsl,imx51-ecspi";
42 reg = <0x02008000 0x4000>;
43 status = "disabled";
44 };
45 }
46
47 //aips2 节点
48 aips2: aips-bus@02100000 {
49 compatible = "fsl,aips-bus", "simple-bus";
50 #address-cells = <1>;
51 #size-cells = <1>;
52 reg = <0x02100000 0x100000>;
53 ranges;
54
55 //usbotg1 节点
56 usbotg1: usb@02184000 {
57 compatible = "fsl,imx6ul-usb", "fsl,imx27-usb";
58 reg = <0x02184000 0x4000>;
59 status = "disabled";
60 };
61 }
62
63 //aips3 节点
64 aips3: aips-bus@02200000 {
65 compatible = "fsl,aips-bus", "simple-bus";
66 #address-cells = <1>;
67 #size-cells = <1>;
68 reg = <0x02200000 0x100000>;
69 ranges;
70
71 //rngb 节点
72 rngb: rngb@02284000 {
73 compatible = "fsl,imx6sl-rng", "fsl,imx-rng", "imxrng";
74 reg = <0x02284000 0x4000>;
75 };
76 }
77 }
78 }
```

第 38~44 行， ecspi1 外设控制器节点。
第 56~60 行， usbotg1 外设控制器节点。
第 72~75 行， rngb 外设控制器节点。
至此， myfirst.dts 这个小型的模板设备树就编写好了，基本和 imx6ull.dtsi 很像，可以看做
是 imx6ull.dtsi 的缩小版。在 myfirst.dts 里面我们仅仅是编写了 I.MX6ULL 的外设控制器节点，
像 IIC 接口， SPI 接口下所连接的具体设备我们并没有写，因为具体的设备其设备树属性内容不
同，这个等到具体的实验在详细讲解。

## 设备树在系统中的体现

Linux 内核启动的时候会解析设备树中各个节点的信息，并且在根文件系的/proc/devicetree 目录下根据节点名字创建不同文件夹，如图 43.5.1 所示：

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658633153328.png)

图 43.5.1 就是目录/proc/device-tree 目录下的内容， /proc/device-tree 目录下是根节点“/”的所有属性和子节点，我们依次来看一下这些属性和子节点。

### 1、根节点“/”各个属性

在上图中，根节点属性属性表现为一个个的文件(图中细字体文件)，比如图 43.5.1 中的“#address-cells”、“#size-cells”、“compatible”、“model”和“name”这 5 个文件，它们在设备树中就是根节点的 5个属性。既然是文件那么肯定可以查看其内容，输入cat 命令来查看 model和 compatible 这两个文件的内容，结果如图 43.5.2 所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658647137830.png)

从图 43.5.2 可以看出，文件 model 的内容是“Freescale i.MX6 ULL 14x14 EVK Board”，文件 compatible 的内容为“fsl,imx6ull-14x14-evkfsl,imx6ull”。打开文件 imx6ull-alientek-emmc.dts查看一下，这不正是根节点“/”的 model 和 compatible 属性值吗！

### 2、根节点“/”各子节点

图 43.5.1 中各个文件夹(图中粗字体文件夹)就是根节点“/”的各个子节点，比如“aliases”、“ backlight”、“ chosen”和“ clocks”等等。大家可以查看一下 imx6ull-alientek-emmc.dts 和imx6ull.dtsi 这两个文件，看看根节点的子节点都有哪些，看看是否和图 43.5.1 中的一致。
/proc/device-tree 目录就是设备树在根文件系统中的体现，同样是按照树形结构组织的，进入/proc/device-tree/soc 目录中就可以看到 soc 节点的所有子节点，如图 43.5.3 所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658647586626.png)

和根节点“/”一样，图 43.5.3 中的所有文件分别为 soc 节点的属性文件和子节点文件夹。
大家可以自行查看一下这些属性文件的内容是否和 imx6ull.dtsi 中 soc 节点的属性值相同，也可以进入“busfreq”这样的文件夹里面查看 soc 节点的子节点信息

## 特殊节点
在根节点“/”中有两个特殊的子节点： aliases 和 chosen，我们接下来看一下这两个特殊的子节点。
####  aliases 子节点
打开 imx6ull.dtsi 文件， aliases 节点内容如下所示
```
18 aliases {
19 can0 = &flexcan1;
20 can1 = &flexcan2;
21 ethernet0 = &fec1;
22 ethernet1 = &fec2;
23 gpio0 = &gpio1;
24 gpio1 = &gpio2;
......
42 spi0 = &ecspi1;
43 spi1 = &ecspi2;
44 spi2 = &ecspi3;
45 spi3 = &ecspi4;
46 usbphy0 = &usbphy1;
47 usbphy1 = &usbphy2;
48 };
```
单词 aliases 的意思是“别名”，因此 aliases 节点的主要功能就是定义别名，定义别名的目的就是为了方便访问节点。不过我们一般会在节点命名的时候会加上 label，然后通过&label来访问节点，这样也很方便，而且设备树里面大量的使用&label 的形式来访问节点。

#### chosen 子节点
chosen 并不是一个真实的设备， chosen 节点主要是为了 uboot 向 Linux 内核传递数据，重点是 bootargs 参数。一般.dts 文件中 chosen 节点通常为空或者内容很少， imx6ull-alientekemmc.dts 中 chosen 节点内容如下所示：
```
18 chosen {
19 stdout-path = &uart1;
20 };
```
从示例代码 43.6.2.1 中可以看出， chosen 节点仅仅设置了属性“stdout-path”，表示标准输出使用 uart1。但是当我们进入到/proc/device-tree/chosen 目录里面，会发现多了 bootargs 这个属性，如图 43.6.2.1 所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658648294643.png)
输入 cat 命令查看 bootargs 这个文件的内容，结果如图 43.6.2.2 所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658648317421.png)

从图 43.6.2.2 可以看出， bootargs 这个文件的内容为“console=ttymxc0,115200……”，这个不就是我们在 uboot 中设置的 bootargs 环境变量的值吗？现在有两个疑点：
- ①、我们并没有在设备树中设置 chosen 节点的 bootargs 属性，那么图 43.6.2.1 中 bootargs这个属性是怎么产生的？
- ②、为何 bootargs 文件的内容和 uboot 中 bootargs 环境变量的值一样？它们之间有什么关系？

前面讲解 uboot 的时候说过， uboot 在启动 Linux 内核的时候会将 bootargs 的值传递给 Linux内核， bootargs 会作为 Linux 内核的命令行参数， Linux 内核启动的时候会打印出命令行参数(也就是 uboot 传递进来的 bootargs 的值)，如图 43.6.2.3 所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658648354853.png)

既然 chosen 节点的 bootargs 属性不是我们在设备树里面设置的，那么只有一种可能，那就是 uboot 自己在 chosen 节点里面添加了 bootargs 属性！并且设置 bootargs 属性的值为 bootargs环境变量的值。因为在启动 Linux 内核之前，只有 uboot 知道 bootargs 环境变量的值，并且 uboot 也知道.dtb 设备树文件在 DRAM 中的位置，因此 uboot 的“作案”嫌疑最大。在 uboot 源码中全局搜索“ chosen”这个字符串，看看能不能找到一些蛛丝马迹。果然不出所料，在common/fdt_support.c 文件中发现了“chosen”的身影， fdt_support.c 文件中有个 fdt_chosen 函数，此函数内容如下所示：
```
275 int fdt_chosen(void *fdt)
276 {
277 int nodeoffset;
278 int err;
279 char *str; /* used to set string properties */
280
281 err = fdt_check_header(fdt);
282 if (err < 0) {
283 printf("fdt_chosen: %s\n", fdt_strerror(err));
284 return err;
285 }
286
287 /* find or create "/chosen" node. */
288 nodeoffset = fdt_find_or_add_subnode(fdt, 0, "chosen");
289 if (nodeoffset < 0)
290 return nodeoffset;
291
292 str = getenv("bootargs");
293 if (str) {
294 err = fdt_setprop(fdt, nodeoffset, "bootargs", str,
295 strlen(str) + 1);
296 if (err < 0) {
297 printf("WARNING: could not set bootargs %s.\n",
298 fdt_strerror(err));
299 return err;
300 }
301 }
302
303 return fdt_fixup_stdout(fdt, nodeoffset);
304 }
```

第 288 行，调用函数 fdt_find_or_add_subnode 从设备树(.dtb)中找到 chosen 节点，如果没有找到的话就会自己创建一个 chosen 节点。
第 292 行，读取 uboot 中 bootargs 环境变量的内容。
第 294 行，调用函数 fdt_setprop 向 chosen 节点添加 bootargs 属性，并且 bootargs 属性的值就是环境变量 bootargs 的内容。

证据“实锤”了，就是 uboot 中的 fdt_chosen 函数在设备树的 chosen 节点中加入了 bootargs属性，并且还设置了 bootargs 属性值。接下来我们顺着 fdt_chosen 函数一点点的抽丝剥茧，看看都有哪些函数调用了 fdt_chosen，一直找到最终的源头。这里我就不卖关子了，直接告诉大家
整个流程是怎么样的，见图 43.6.2.4：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658648460428.png)
图 43.6.2.4 中框起来的部分就是函数 do_bootm_linux 函数的执行流程，也就是说do_bootm_linux 函数会通过一系列复杂的调用，最终通过 fdt_chosen 函数在 chosen 节点中加入了 bootargs 属性。而我们通过 bootz 命令启动 Linux 内核的时候会运行 do_bootm_linux 函数，至此，真相大白，一切事情的源头都源于如下命令：
```bootz 80800000 – 83000000```
当我们输入上述命令并执行以后， do_bootz 函数就会执行，然后一切就按照图 43.6.2.4 中所示的流程开始运行。

## Linux 内核解析 DTB 文件

Linux 内核在启动的时候会解析 DTB 文件，然后在/proc/device-tree 目录下生成相应的设备树节点文件。接下来我们简单分析一下 Linux 内核是如何解析 DTB 文件的，流程如图 43.7.1 所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658648534547.png)
从图 43.7.1 中可以看出，在 start_kernel 函数中完成了设备树节点解析的工作，最终实际工作的函数为 unflatten_dt_node。

## 绑定信息文档
设备树是用来描述板子上的设备信息的，不同的设备其信息不同，反映到设备树中就是属性不同。那么我们在设备树中添加一个硬件对应的节点的时候从哪里查阅相关的说明呢？在Linux 内核源码中有详细的.txt 文档描述了如何添加节点，这些.txt 文档叫做绑定文档，路径为：Linux 源码目录/Documentation/devicetree/bindings，如图 43.8.1 所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式Linux学习笔记-驱动开发篇/1658648591216.png)
比如我们现在要想在 I.MX6ULL 这颗 SOC 的 I2C 下添加一个节点，那么就可以查看
Documentation/devicetree/bindings/i2c/i2c-imx.txt，此文档详细的描述了 I.MX 系列的 SOC 如何在设备树中添加 I2C 设备节点，文档内容如下所示：
```
* Freescale Inter IC (I2C) and High Speed Inter IC (HS-I2C) for i.MX
Required properties:
- compatible :
- "fsl,imx1-i2c" for I2C compatible with the one integrated on i.MX1
SoC
- "fsl,imx21-i2c" for I2C compatible with the one integrated on i.MX21
SoC
- "fsl,vf610-i2c" for I2C compatible with the one integrated on Vybrid
vf610 SoC
- reg : Should contain I2C/HS-I2C registers location and length
- interrupts : Should contain I2C/HS-I2C interrupt
- clocks : Should contain the I2C/HS-I2C clock specifier
Optional properties:
- clock-frequency : Constains desired I2C/HS-I2C bus clock frequency in
Hz.
The absence of the propoerty indicates the default frequency 100 kHz.
- dmas: A list of two dma specifiers, one for each entry in dma-names.
- dma-names: should contain "tx" and "rx".
Examples:
i2c@83fc4000 { /* I2C2 on i.MX51 */
compatible = "fsl,imx51-i2c", "fsl,imx21-i2c";
reg = <0x83fc4000 0x4000>;
interrupts = <63>;
};
i2c@70038000 { /* HS-I2C on i.MX51 */
compatible = "fsl,imx51-i2c", "fsl,imx21-i2c";
reg = <0x70038000 0x4000>;
interrupts = <64>;
clock-frequency = <400000>;
};
i2c0: i2c@40066000 { /* i2c0 on vf610 */
compatible = "fsl,vf610-i2c";
reg = <0x40066000 0x1000>;
interrupts =<0 71 0x04>;
dmas = <&edma0 0 50>,
<&edma0 0 51>;
dma-names = "rx","tx";
};
```
有时候使用的一些芯片在 Documentation/devicetree/bindings 目录下找不到对应的文档，这个时候就要咨询芯片的提供商，让他们给你提供参考的设备树文件。

## 设备树常用 OF 操作函数

设备树描述了设备的详细信息，这些信息包括数字类型的、字符串类型的、数组类型的，我们在编写驱动的时候需要获取到这些信息。比如设备树使用 reg 属性描述了某个外设的寄存器地址为 0X02005482，长度为 0X400，我们在编写驱动的时候需要获取到 reg 属性的0X02005482 和 0X400 这两个值，然后初始化外设。 Linux 内核给我们提供了一系列的函数来获取设备树中的节点或者属性信息，这一系列的函数都有一个统一的前缀“of_”，所以在很多资料里面也被叫做 OF 函数。这些 OF 函数原型都定义在 include/linux/of.h 文件中



### 查找节点的 OF 函数
设备都是以节点的形式“挂”到设备树上的，因此要想获取这个设备的其他属性信息，必
须先获取到这个设备的节点。 Linux 内核使用 device_node 结构体来描述一个节点，此结构体定义在文件 include/linux/of.h 中，定义如下：
```
49 struct device_node {
50 const char *name; /* 节点名字 */
51 const char *type; /* 设备类型 */
52 phandle phandle;
53 const char *full_name; /* 节点全名 */
54 struct fwnode_handle fwnode;
55
56 struct property *properties; /* 属性 */
57 struct property *deadprops; /* removed 属性 */
58 struct device_node *parent; /* 父节点 */
59 struct device_node *child; /* 子节点 */
60 struct device_node *sibling;
61 struct kobject kobj;
62 unsigned long _flags;
63 void *data;
64 #if defined(CONFIG_SPARC)
65 const char *path_component_name;
66 unsigned int unique_id;
67 struct of_irq_controller *irq_trans;
68 #endif
69 };
```

与查找节点有关的 OF 函数有 5 个，我们依次来看一下。
#### 1、 of_find_node_by_name 函数
of_find_node_by_name 函数通过节点名字查找指定的节点，函数原型如下：
```
struct device_node *of_find_node_by_name(struct device_node *from,
const char *name);
```

函数参数和返回值含义如下：
- from：开始查找的节点，如果为 NULL 表示从根节点开始查找整个设备树。
- name：要查找的节点名字。
- 返回值： 找到的节点，如果为 NULL 表示查找失败。

#### 2、 of_find_node_by_type 函数

of_find_node_by_type 函数通过 device_type 属性查找指定的节点，函数原型如下：
```struct device_node *of_find_node_by_type(struct device_node *from, const char *type)```

函数参数和返回值含义如下：
- from：开始查找的节点，如果为 NULL 表示从根节点开始查找整个设备树。
- type：要查找的节点对应的 type 字符串，也就是 device_type 属性值。
- 返回值： 找到的节点，如果为 NULL 表示查找失败。

#### 3、 of_find_compatible_node 函数
of_find_compatible_node 函数根据 device_type 和 compatible 这两个属性查找指定的节点，函数原型如下：
```struct device_node *of_find_compatible_node(struct device_node *from, const char *type,const char *compatible)```

函数参数和返回值含义如下：
- from：开始查找的节点，如果为 NULL 表示从根节点开始查找整个设备树。
- type：要查找的节点对应的 type 字符串，也就是 device_type 属性值，可以为 NULL，表示忽略掉 device_type 属性。
- compatible： 要查找的节点所对应的 compatible 属性列表。
- 返回值： 找到的节点，如果为 NULL 表示查找失败

#### 4、 of_find_matching_node_and_match 函数
of_find_matching_node_and_match 函数通过 of_device_id 匹配表来查找指定的节点，函数原型如下：
```struct device_node *of_find_matching_node_and_match(struct device_node *from, const struct of_device_id *matches, const struct of_device_id **match)
```
函数参数和返回值含义如下：
- from：开始查找的节点，如果为 NULL 表示从根节点开始查找整个设备树。
- matches： of_device_id 匹配表，也就是在此匹配表里面查找节点。
- match： 找到的匹配的 of_device_id。
- 返回值： 找到的节点，如果为 NULL 表示查找失败

#### 5、 of_find_node_by_path 函数
of_find_node_by_path 函数通过路径来查找指定的节点，函数原型如下：
```inline struct device_node *of_find_node_by_path(const char *path)```
函数参数和返回值含义如下：
- path：带有全路径的节点名，可以使用节点的别名，比如“/backlight”就是 backlight 这个节点的全路径。
- 返回值： 找到的节点，如果为 NULL 表示查找失败

### 查找父/子节点的 OF 函数
Linux 内核提供了几个查找节点对应的父节点或子节点的 OF 函数，我们依次来看一下。
#### 1、 of_get_parent 函数
of_get_parent 函数用于获取指定节点的父节点(如果有父节点的话)，函数原型如下：
```struct device_node *of_get_parent(const struct device_node *node)```
函数参数和返回值含义如下：
- node：要查找的父节点的节点。
- 返回值： 找到的父节点。
#### 2、 of_get_next_child 函数

of_get_next_child 函数用迭代的方式查找子节点，函数原型如下：
```struct device_node *of_get_next_child(const struct device_node *node,struct device_node *prev)```
函数参数和返回值含义如下：
- node：父节点。
- prev：前一个子节点，也就是从哪一个子节点开始迭代的查找下一个子节点。可以设置为
- NULL，表示从第一个子节点开始。
- 返回值： 找到的下一个子节点。

### 提取属性值的 OF 函数

节点的属性信息里面保存了驱动所需要的内容，因此对于属性值的提取非常重要， Linux 内核中使用结构体 property 表示属性，此结构体同样定义在文件 include/linux/of.h 中，内容如下：
```
35 struct property {
36 char *name; /* 属性名字 */
37 int length; /* 属性长度 */
38 void *value; /* 属性值 */
39 struct property *next; /* 下一个属性 */
40 unsigned long _flags;
41 unsigned int unique_id;
42 struct bin_attribute attr;
43 };
```

Linux 内核也提供了提取属性值的 OF 函数，我们依次来看一下。
#### 1、 of_find_property 函数
of_find_property 函数用于查找指定的属性，函数原型如下：
```
property *of_find_property(const struct device_node *np,
const char *name,
int *lenp)
```
函数参数和返回值含义如下：
- np：设备节点。
- name： 属性名字。
- lenp：属性值的字节数
- 返回值： 找到的属性。
 
#### 2、 of_property_count_elems_of_size 函数
of_property_count_elems_of_size 函数用于获取属性中元素的数量，比如 reg 属性值是一个数组，那么使用此函数可以获取到这个数组的大小，此函数原型如下：
```int of_property_count_elems_of_size(const struct device_node *np,const char *propname,int elem_size)```

函数参数和返回值含义如下：
- np：设备节点。
- proname： 需要统计元素数量的属性名字。
- elem_size：元素长度。
- 返回值： 得到的属性元素数量。

#### 3、 of_property_read_u32_index 函数
of_property_read_u32_index 函数用于从属性中获取指定标号的 u32 类型数据值(无符号 32位)，比如某个属性有多个 u32 类型的值，那么就可以使用此函数来获取指定标号的数据值，此函数原型如下：
```int of_property_read_u32_index(const struct device_node *np,const char *propname,u32 index,u32 *out_value)```

函数参数和返回值含义如下：
- np：设备节点。
- proname： 要读取的属性名字。
- index：要读取的值标号。
- out_value：读取到的值
- 返回值： 0 读取成功，负值，读取失败， -EINVAL 表示属性不存在， -ENODATA 表示没有要读取的数据， -EOVERFLOW 表示属性值列表太小。

#### 4、 of_property_read_u8_array 函数
of_property_read_u16_array 函数
of_property_read_u32_array 函数
of_property_read_u64_array 函数

这 4 个函数分别是读取属性中 u8、 u16、 u32 和 u64 类型的数组数据，比如大多数的 reg 属性都是数组数据，可以使用这 4 个函数一次读取出 reg 属性中的所有数据。这四个函数的原型如下：
```
int of_property_read_u8_array(const struct device_node *np,
const char *propname,
u8 *out_values,
size_t sz)
int of_property_read_u16_array(const struct device_node *np,
const char *propname,
u16 *out_values,
size_t sz)
int of_property_read_u32_array(const struct device_node *np,
const char *propname,
u32 *out_values,
size_t sz)
int of_property_read_u64_array(const struct device_node *np,
const char *propname,
u64 *out_values,
size_t sz)
```
函数参数和返回值含义如下：
- np：设备节点。
- proname： 要读取的属性名字。
- out_value：读取到的数组值，分别为 u8、 u16、 u32 和 u64。
- sz： 要读取的数组元素数量。
- 返回值： 0，读取成功，负值，读取失败， -EINVAL 表示属性不存在， -ENODATA 表示没有要读取的数据， -EOVERFLOW 表示属性值列表太小。

#### 5、 of_property_read_u8 函数
of_property_read_u16 函数
of_property_read_u32 函数
of_property_read_u64 函数

有些属性只有一个整形值，这四个函数就是用于读取这种只有一个整形值的属性，分别用于读取 u8、 u16、 u32 和 u64 类型属性值，函数原型如下：
```
int of_property_read_u8(const struct device_node *np,
const char *propname,
u8 *out_value)
int of_property_read_u16(const struct device_node *np,
const char *propname,
u16 *out_value)
int of_property_read_u32(const struct device_node *np,
const char *propname,
u32 *out_value)
int of_property_read_u64(const struct device_node *np,
const char *propname,
u64 *out_value)
```
函数参数和返回值含义如下：
- np：设备节点。
- proname： 要读取的属性名字。
- out_value：读取到的数组值。
- 返回值： 0，读取成功，负值，读取失败， -EINVAL 表示属性不存在， -ENODATA 表示没有要读取的数据， -EOVERFLOW 表示属性值列表太小。

#### 6、 of_property_read_string 函数
of_property_read_string 函数用于读取属性中字符串值，函数原型如下：
```
int of_property_read_string(struct device_node *np,const char *propname,const char **out_string)
```
函数参数和返回值含义如下：
- np：设备节点。
- proname： 要读取的属性名字。
- out_string：读取到的字符串值。
- 返回值： 0，读取成功，负值，读取失败。

#### 7、 of_n_addr_cells 函数
of_n_addr_cells 函数用于获取#address-cells 属性值，函数原型如下：
```int of_n_addr_cells(struct device_node *np)```
函数参数和返回值含义如下：
- np：设备节点。
- 返回值： 获取到的#address-cells 属性值。

#### 8、 of_n_size_cells 函数
of_size_cells 函数用于获取#size-cells 属性值，函数原型如下：
```int of_n_size_cells(struct device_node *np)```
函数参数和返回值含义如下：
- np：设备节点。
- 返回值： 获取到的#size-cells 属性值。

### 其他常用的 OF 函数

#### 1、 of_device_is_compatible 函数
of_device_is_compatible 函数用于查看节点的 compatible 属性是否有包含 compat 指定的字符串，也就是检查设备节点的兼容性，函数原型如下：
```int of_device_is_compatible(const struct device_node *device,const char *compat)```
函数参数和返回值含义如下：
- device：设备节点。
- compat：要查看的字符串。
- 返回值： 0，节点的 compatible 属性中不包含 compat 指定的字符串； 正数，节点的 compatible属性中包含 compat 指定的字符串。

#### 2、 of_get_address 函数
of_get_address 函数用于获取地址相关属性，主要是“reg”或者“assigned-addresses”属性值，函数原型如下：
```const __be32 *of_get_address(struct device_node *dev,int index,u64 *size,unsigned int *flags)```
函数参数和返回值含义如下：
- dev：设备节点。
- index：要读取的地址标号。
- size：地址长度。
- flags：参数，比如 IORESOURCE_IO、 IORESOURCE_MEM 等
- 返回值： 读取到的地址数据首地址，为 NULL 的话表示读取失败。
#### 3、 of_translate_address 函数
of_translate_address 函数负责将从设备树读取到的地址转换为物理地址，函数原型如下：
```u64 of_translate_address(struct device_node *dev,const __be32 *in_addr)```
函数参数和返回值含义如下：
- dev：设备节点。
- in_addr：要转换的地址。
- 返回值： 得到的物理地址，如果为 OF_BAD_ADDR 的话表示转换失败。
#### 4、 of_address_to_resource 函数
IIC、 SPI、 GPIO 等这些外设都有对应的寄存器，这些寄存器其实就是一组内存空间， Linux内核使用 resource 结构体来描述一段内存空间，“resource”翻译出来就是“资源”，因此用 resource结构体描述的都是设备资源信息， resource 结构体定义在文件 include/linux/ioport.h 中，定义如下：
```
18 struct resource {
19 resource_size_t start;
20 resource_size_t end;
21 const char *name;
22 unsigned long flags;
23 struct resource *parent, *sibling, *child;
24 };
```
对于 32 位的 SOC 来说， resource_size_t 是 u32 类型的。其中 start 表示开始地址， end 表示结束地址， name 是这个资源的名字， flags 是资源标志位，一般表示资源类型，可选的资源标志定义在文件 include/linux/ioport.h 中，如下所示：
```
1 #define IORESOURCE_BITS 0x000000ff
2 #define IORESOURCE_TYPE_BITS 0x00001f00
3 #define IORESOURCE_IO 0x00000100
4 #define IORESOURCE_MEM 0x00000200
5 #define IORESOURCE_REG 0x00000300
6 #define IORESOURCE_IRQ 0x00000400
7 #define IORESOURCE_DMA 0x00000800
8 #define IORESOURCE_BUS 0x00001000
9 #define IORESOURCE_PREFETCH 0x00002000
10 #define IORESOURCE_READONLY 0x00004000
11 #define IORESOURCE_CACHEABLE 0x00008000
12 #define IORESOURCE_RANGELENGTH 0x00010000
13 #define IORESOURCE_SHADOWABLE 0x00020000
14 #define IORESOURCE_SIZEALIGN 0x00040000
15 #define IORESOURCE_STARTALIGN 0x00080000
16 #define IORESOURCE_MEM_64 0x00100000
17 #define IORESOURCE_WINDOW 0x00200000
18 #define IORESOURCE_MUXED 0x00400000
19 #define IORESOURCE_EXCLUSIVE 0x08000000
20 #define IORESOURCE_DISABLED 0x10000000
21 #define IORESOURCE_UNSET 0x20000000
22 #define IORESOURCE_AUTO 0x40000000
23 #define IORESOURCE_BUSY 0x80000000
```
大 家 一 般 最 常 见 的 资 源 标 志 就 是 IORESOURCE_MEM 、 IORESOURCE_REG 和IORESOURCE_IRQ 等。接下来我们回到 of_address_to_resource 函数，此函数看名字像是从设备树里面提取资源值，但是本质上就是将 reg 属性值，然后将其转换为 resource 结构体类型，函数原型如下所示
```int of_address_to_resource(struct device_node *dev,int index,struct resource *r)```
函数参数和返回值含义如下：
- dev：设备节点。
- index：地址资源标号。
- r：得到的 resource 类型的资源值。
- 返回值： 0，成功；负值，失败。

#### 5、 of_iomap 函数
of_iomap 函数用于直接内存映射，以前我们会通过 ioremap 函数来完成物理地址到虚拟地址的映射，采用设备树以后就可以直接通过 of_iomap 函数来获取内存地址所对应的虚拟地址，不需要使用 ioremap 函数了。当然了，你也可以使用 ioremap 函数来完成物理地址到虚拟地址的内存映射，只是在采用设备树以后，大部分的驱动都使用 of_iomap 函数了。 of_iomap 函数本质上也是将 reg 属性中地址信息转换为虚拟地址，如果 reg 属性有多段的话，可以通过 index 参数指定要完成内存映射的是哪一段， of_iomap 函数原型如下：
```void __iomem *of_iomap(struct device_node *np,int index)```
函数参数和返回值含义如下：
- np：设备节点。
- index： reg 属性中要完成内存映射的段，如果 reg 属性只有一段的话 index 就设置为 0。
- 返回值： 经过内存映射后的虚拟内存首地址，如果为 NULL 的话表示内存映射失败。

关于设备树常用的 OF 函数就先讲解到这里， Linux 内核中关于设备树的 OF 函数不仅仅只有前面讲的这几个，还有很多 OF 函数我们并没有讲解，这些没有讲解的 OF 函数要结合具体的驱动，比如获取中断号的 OF 函数、获取 GPIO 的 OF 函数等等，这些 OF 函数我们在后面的驱动实验中再详细的讲解。

关于设备树就讲解到这里，关于设备树我们重点要了解一下几点内容：
- ①、 DTS、 DTB 和 DTC 之间的区别，如何将.dts 文件编译为.dtb 文件。
- ②、设备树语法，这个是重点，因为在实际工作中我们是需要修改设备树的。
- ③、设备树的几个特殊子节点。
- ④、关于设备树的 OF 操作函数，也是重点，因为设备树最终是被驱动文件所使用的，而驱动文件必须要读取设备树中的属性信息，比如内存信息、 GPIO 信息、中断信息等等。要想在驱动中读取设备树的属性值，那么就必须使用 Linux 内核提供的众多的 OF 函数。

# 设备树下的 LED 驱动实验

<!--more--
>
