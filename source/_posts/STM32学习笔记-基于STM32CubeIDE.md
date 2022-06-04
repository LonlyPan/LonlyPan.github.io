---
layout: post
title: "STM32学习笔记-基于STM32CubeIDE"
index_img: https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/index.png
date: 2020-12-18
updated: 2020-12-18
hide: false
# sticky: 100 #置顶，数字越大越靠前
# banner_img: #/img/post_banner.jpg
# comment: false
categories: 01-专业
---

<!--more-->

**参考链接：**
 - [HAL库学习 GPIO---1-知乎](https://zhuanlan.zhihu.com/p/231696942)
 - [【STM32】HAL库 STM32CubeMX系列学习教程-csdn](https://blog.csdn.net/as480133937/article/details/99935090)
 - [STM32CubeMX学习使用（标准库与Cube,LL,直接写寄存器的效率对比）](https://blog.csdn.net/super828/article/details/79078693?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161559969216780357212490%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=161559969216780357212490&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v1~rank_blog_v1-2-79078693.pc_v1_rank_blog_v1&utm_term=LL%E5%BA%93)
 - [STM32 HAL库学习（一）：点亮led](https://blog.csdn.net/qq_42913442/article/details/106938727)
 - [STM32 HAL库学习系列第3篇 常使用的几种延时方式](https://blog.csdn.net/super828/article/details/79600206?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161559888716780262516364%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=161559888716780262516364&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v1~rank_blog_v1-3-79600206.pc_v1_rank_blog_v1&utm_term=STM32+HAL)
 - [【STM32】HAL库 STM32CubeMX系列学习教程](http://openedv.com/thread-309468-1-1.html)

**开发环境：**
- 系统：win10
- IDE：STM32CubeIDE
- MCU：STM32F401CCU6  
 	开发板资料: https://pan.baidu.com/s/1UKLarbMPr0GC_aHN9ybCxg 提取码: k7gb

**资料下载**

学习STM32需要提前准备几份文档资料，在接下来的学习和今后实际运用中都会经常用到。  
资料直接从 [ST官网](https://www.st.com/) 下载即可，有的手册有中文。有的中文手册官网没有，可自行网上搜索下载。但都不推荐使用中文的：版本太老、阅读英文文档是程序员必备技能
1. 进入官网 [ST官网](https://www.st.com/) ，选择进入 `微控制器界面`
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/浏览微控制器.png)
2. 在左侧栏找到自己芯片型号，并进入 `Documentation` 界面，选择对应文档下载即可。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/芯片文档下载.png)
3. 下载以下文档：
   - 数据手册：`Arm® Cortex®-M4 32-bit MCU+FPU, 105 DMIPS, 256KB Flash / 64KB RAM, 11 TIMs, 1 ADC, 11 comm. interfacesV11.0` 芯片本身的手册，相当于产品说明书。
   - 参考手册：`STM32F401xB/C and STM32F401xD/E advanced Arm®-based 32-bit MCUs` ，芯片使用参考手册
   - 编程手册：`STM32 Cortex®-M4 MCUs and MPUs programming manual` ，芯片内核的编程手册（高阶）
4. HAL和LL库参考手册我们需要另外在官网搜索才能找到，`Description of STM32F4 HAL and low-layer drivers`，库函数API驱动描述手册：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/库参考手册.png)

# 开发环境搭建

参看博文：[STM32CubeIDE学习笔记](http://lonlypan.com/2020/12/18/STM32CubeIDE%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/)

# STM32F030_HAL库学习笔记

操作系统：Win10
硬件平台：STM32F401
软件平台：STRM32CubeIDE V1.6.0
下载器：ST-Link V2

### GPIO HAL库 操作与调试

#### 初始化配置

参考上文中的工程模板建立。

#### 程序编写

**主要API：**
1. **HAL_GPIO_WritePin(GPIO_TypeDef\* GPIOx, uint16_t GPIO_Pin, GPIO_PinState PinState)**
控制某个具体引脚的状态
   - GPIO_TypeDef：IO端口编号  GPIOA、 GPIOB、 …、 GPIOG  
   - GPIO_Pin：IO引脚编号 GPIO_PIN_0…GPIO_PIN_15  
   - PinState：IO状态 GPIO_PIN_SET 或者 GPIO_PIN_RESET  
2. **HAL_GPIO_TogglePin(GPIO_TypeDef \*GPIOx, uint16_t GPIO_Pin)**
3. **HAL_GPIO_ReadPin(GPIO_TypeDef \*GPIOx, uint16_t GPIO_Pin)**
读取某个具体引脚的状态(**需要将引脚设置为输入模式 `GPIO_input`**)
4. HAL_Delay(uint32_t Delay)
ms 级延时函数。IDE内置的，但没有内置 us 级延时函数
**主函数：**

``` cpp
int main(void)
{
  HAL_Init();
  /* Configure the system clock */
  SystemClock_Config();

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  HAL_GPIO_WritePin(LED1_GPIO_Port,LED1_Pin, GPIO_PIN_SET)  /* LED1输出高电平 */ 
  while (1)
  {
	  HAL_GPIO_TogglePin(LED0_GPIO_Port,LED0_Pin); /* 间隔500ms翻转 LED0 引脚 */
	  HAL_Delay(500);
  }
}
```

这里的前两个`LED0_GPIO_Port,LED0_Pin`是引脚的宏定义，初始化时系统根据我们在配置界面设置的 IO 别名自动生成的，方便理解，否则没有。请在 main.h 文件中查看。

#### 调试Debug

点击工具栏的 `甲虫按钮`
![图 2](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/%E8%B0%83%E8%AF%95_20.png)  
在弹出对话框，选择 `STM32 CPU`，单击 `确认` 进入配置界面。在 `调试器`下选择 ST-Link 作为调试器，单击 `确认`。
![图 3](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/%E8%B0%83%E8%AF%952_21.png)  

在弹出的对话框选择 `Switch`，打开调试窗口。
![图 4](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/%E8%B0%83%E8%AF%953_22.png)  

这里就是调试窗口了，红框内的都是和调试有关的工具按钮，这里不多做介绍，自行摸索。
![图 5](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/%E8%B0%83%E8%AF%954_23.png)  

### GPIO 寄存器操作

#### 寄存器介绍

**BSRR 和 BRR 关系**

BSRR 和 BRR 都是 STM32 系列 MCU 中 GPIO 的寄存器。 BSRR 称为端口位设置/清除寄存器，BRR称为端口位清除寄存器。
- BSRR 低 16 位用于设置 GPIO 口对应位输出高电平，高 16 位用于设置 GPIO 口对应位输出低电平。
- BRR 低 16 位用于设置 GPIO 口对应位输出低电平。高 16 位为保留地址，读写无效。

所以理论上来讲，BRR 寄存器的功能和 BSRR 寄存器高 16 位的功能是一样的，都可以控制端口输出低电平。也就是说，输出低电平可以有如下两种写法。

```
#define SET_BL_LOW() GPIOA->BRR=GPIO_Pin_0
等价于
#define SET_BL_LOW() GPIOA->BSRR=GPIO_Pin_0 << 16
```
这么来看的话，其实 BRR 寄存器是比较多余的。而实际上，在最新的 STM32F4 系列 MCU 的 GPIO寄存器中，已经找不到 BRR 寄存器了，仅保留了 BSRR 寄存器用于实现端口输出高低电平。

可见，不管是输出高还是输出低，对 BSRR 寄存器的操作最为稳妥。



BSRR常见操作
```
/* 低8位操作 */
GPIOE->BSRR = (Newdata & 0xffff) | ( (~Newdata )<<16 );
/* 16位操作 */
GPIOE->BSRR = (Newdata & 0xff) | ( (~Newdata & 0xff)<<16 );
```
BSRR还有一个特点，就是如果低6位和高16位同时置1，结果以低16位为准。
就是说同一个bit在 BSRR 低16位中为1（输出高电平），但在高16位中也是1（输出低电平），结果该bit引脚输出 1（高电平）。
此时对多位同时操作可以这么写：
```
GPIOx->BSRR = 0xFFFF0000 | PATTEN;
```
不用考虑哪些需要置1，哪些需要清零

**ODR**

ODR 寄存器也是用于输出数据的寄存器，一个 ODR 寄存器控制了一组(16位)的 GPIO 输出。因此，对 ODR 进行修改也可以到达对 IO 口输出进行配置。同时通过读取该寄存器，也能够获取 IO 的当前输出状态。而 BSRR 和 BRR 只可写。

但是，由于对 ODR 寄存器的读写操作必须以 16 位的形式进行。因此，如果使用 ODR 改写数据以控制输出时，须采用“读-改-写”的形式进行。

假设需要对 GPIOA_Pin_6 输出高电平。采用改写 ODR 寄存器的方式时，使用“读-改-写”操作，代码如下：
```
GPIOB->ODR=((GPIOB->ODR | GPIO_Pin_6); 
```

而使用 BSRR 寄存器时，仅需要使用如下语句：
```
GPIOA->BSRR = GPIO_Pin_6;
```
这是因为在修改 ODR 时，为了确保对端口 6 的修改不会影响到其他端口的输出，需要对端口的原始数据进行保存，之后再对端口 6 的值进行修改，最后再写入寄存器。而对 BSRR 的操作，是写 1 有效，写 0 不改变原状态，因此可以对端口 6 置 1，其他位保持为 0。

BSRR 为 1 的话，程序运行时自动会修改相应的 ODR 位。

**BSRR、BRR、 ODR 之间的关系**

* ODR寄存器可读可写：既能控制管脚为高电平，也能控制管脚为低电平。管脚对于位写1 GPIO管脚为高电平，写 0 为低电平（有被中断打断的风险）
* BSRR 只写寄存器：既能控制管脚为高电平，也能控制管脚为低电平。对寄存器高16位 写1 对应管脚为低电平，写0无动作；对寄存器的第16位写1对应管脚为高电平，写 0 无动作。
* BRR 只写寄存器：只能改变管脚状态为低电平，对寄存器 管脚对于位写 1 相应管脚会为低电平。写 0 无动作。

ODR 能控制管脚高低电平为什么还需要BSRR和SRR寄存器的原因是：用BSRR和BRR去改变管脚状态的时候，没有被中断打断的风险。也就不需要关闭中断，关闭中断明显会延迟或丢失一事件的捕获，所以控制GPIO的状态最好还是用SBRR和BRR。

**IDR**

GPIO 端口输入数据寄存器。只用了低 16 位。该寄存器为只读寄存器，并且只能以 16 位的形式读出。  
要想知道某个 IO 口的状态， 你只要读这个寄存器，再看某个位的状态就可以了。

#### 初始化配置
沿用 GPIO HAL 库操作时的配置
#### 程序编写

寄存器的写法可以通过查看 HAL 函数底层实现，来学习官方如何使用寄存器的。
```
void led_blink()
{
#if 0 // ODR 方式
	if(LED_GPIO_Port->ODR & LED_Pin)  // 高电平
		LED_GPIO_Port->ODR=~(~LED_GPIO_Port->ODR | LED_Pin);  // 置0
	else
		LED_GPIO_Port->ODR=(LED_GPIO_Port->ODR | LED_Pin);  // 置1

    HAL_Delay(2000);

#endif
	if(LED_GPIO_Port->IDR & LED_Pin)  // 高电平
	   LED_GPIO_Port->BSRR = LED_Pin  << 16 ;   // 高位置0
	else
	   LED_GPIO_Port->BSRR = LED_Pin; // 低位置1

    HAL_Delay(2000);
}
```
**参考链接**
* [高手带你解析STM32 BSRR BRR ODR寄存器](http://news.moore.ren/industry/64985.htm)
* [STM32duino GPIO Registers and programming](https://gist.github.com/iwalpola/6c36c9573fd322a268ce890a118571ca)
* [GPIO Output Registers on the STM32](https://electronics.stackexchange.com/questions/336021/gpio-output-registers-on-the-stm32)
* [Would my solution work for 8-bit bus addressing using BSRR and BRR?](https://stackoverflow.com/questions/56822789/would-my-solution-work-for-8-bit-bus-addressing-using-bsrr-and-brr)
* [STM32裸机学习笔记（三）—寄存器映射之BSRR与延时的爱恨情仇](https://www.codenong.com/cs106676846/)
* [STM32 GPIO 配置之ODR, BSRR, BRR 详解](https://blog.csdn.net/GDNNNNN/article/details/87904592?spm=1001.2014.3001.5501)

### GPIO 位带操作

#### 位带操作设置
关于位带操作，网上有很多讲解，这里不再详述。可以参考：`参考手册` 的 GPIO 章节，`编程手册` 的 2.2.5 Bit-banding 章节，自行深入学习。只需要将下段代码加入到任意头文件中：

```c 
//位带操作,实现51类似的GPIO控制功能
//IO口操作宏定义
// 把“位带地址+位序号”转换成别名地址的宏
#define BITBAND(addr, bitnum) ((addr & 0xF0000000)+0x2000000+((addr &0xFFFFF)<<5)+(bitnum<<2))
// 把一个地址转换成一个指针
#define MEM_ADDR(addr)  *((volatile unsigned long  *)(addr))
// 把位带别名区地址转换成指针
#define BIT_ADDR(addr, bitnum)   MEM_ADDR(BITBAND(addr, bitnum))
//IO口地址映射
#define GPIOA_ODR_Addr    (GPIOA_BASE+20) //
#define GPIOB_ODR_Addr    (GPIOB_BASE+20) //
#define GPIOC_ODR_Addr    (GPIOC_BASE+20) //
#define GPIOD_ODR_Addr    (GPIOD_BASE+20) //
#define GPIOE_ODR_Addr    (GPIOE_BASE+20) //
#define GPIOF_ODR_Addr    (GPIOF_BASE+20) //
#define GPIOG_ODR_Addr    (GPIOG_BASE+20) //

#define GPIOA_IDR_Addr    (GPIOA_BASE+16) //
#define GPIOB_IDR_Addr    (GPIOB_BASE+16) //
#define GPIOC_IDR_Addr    (GPIOC_BASE+16) //
#define GPIOD_IDR_Addr    (GPIOD_BASE+16) //
#define GPIOE_IDR_Addr    (GPIOE_BASE+16) //
#define GPIOF_IDR_Addr    (GPIOF_BASE+16) //
#define GPIOG_IDR_Addr    (GPIOG_BASE+16) //

//IO口操作,只对单一的IO口!
//确保n的值小于16!
#define PAout(n)   BIT_ADDR(GPIOA_ODR_Addr,n)  //输出
#define PAin(n)    BIT_ADDR(GPIOA_IDR_Addr,n)  //输入

#define PBout(n)   BIT_ADDR(GPIOB_ODR_Addr,n)  //输出
#define PBin(n)    BIT_ADDR(GPIOB_IDR_Addr,n)  //输入

#define PCout(n)   BIT_ADDR(GPIOC_ODR_Addr,n)  //输出
#define PCin(n)    BIT_ADDR(GPIOC_IDR_Addr,n)  //输入

#define PDout(n)   BIT_ADDR(GPIOD_ODR_Addr,n)  //输出
#define PDin(n)    BIT_ADDR(GPIOD_IDR_Addr,n)  //输入

#define PEout(n)   BIT_ADDR(GPIOE_ODR_Addr,n)  //输出
#define PEin(n)    BIT_ADDR(GPIOE_IDR_Addr,n)  //输入

#define PFout(n)   BIT_ADDR(GPIOF_ODR_Addr,n)  //输出
#define PFin(n)    BIT_ADDR(GPIOF_IDR_Addr,n)  //输入

#define PGout(n)   BIT_ADDR(GPIOG_ODR_Addr,n)  //输出
#define PGin(n)    BIT_ADDR(GPIOG_IDR_Addr,n)  //输入
```

上述代码中：
- `#define BITBAND` 后的内容，不同内核可能需要另外修改（M3和M4内核已经验证，可通用）。 
- 数字 `20`和`16`是寄存器  ODR 和 IDR 的地址偏移，不同芯片也需要做出相应修改，具体查看 `参考手册` 的 GPIO 章节的 GPIOx_IDR、GPIOx_ODR寄存器描述
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/GIPIO_adress_offset.png)
图中红框部分 0x10 、0x14 转换成十进制就是 16 和 20。例如 STM32F103 系列的是  0x08、0x0C，那这里就需要改为 12 和 8。

参考链接：
- [知乎-STM32位带操作全解](https://zhuanlan.zhihu.com/p/142586194)
- [CSDN-快速理解STM32位带操作原理和用途](https://blog.csdn.net/ybhuangfugui/article/details/108067563)
- [cnblogs-第13章 GPIO-位带操作—零死角玩转STM32-F429系列](https://www.cnblogs.com/firege/p/5748713.html)

#### 初始化配置

沿用 GPIO HAL 库操作时的配置

#### 程序编写
```
/* 操作相应I/O前，必须先配置初始化，才能使用位带操作 */
PGout(10)= 1;  // PG10 输出高电平
uint8_t io_status = PGin(9); // 读取PG9的高低电平状态
```

### GPIO DMA 操作

#### 初始化配置

沿用普通给个gpio例程，添加 DMA 配置如下：
- DMA 模式Normal
- 地址增长，只勾选 源地址（也就是内容地址），目标地址不勾选（也就是IO外设地址）
- 数据宽度设为字节（也可设为其它两个选项，具体再程序编写时说明）
- DMA 中断根据需要选择开启
![图 4](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/gpio_dma1.png)  
![图 5](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/gpio_dma2.png)  

- [STM32F103的GPIO与DMA的终极（没啥用）玩法](http://www.mcublog.cn/stm32/2021_03/stm32f103-gpio-dma/)
- [Using Direct Memory Access (DMA) in STM32 projects](https://embedds.com/using-direct-memory-access-dma-in-stm23-projects/)
- [Particle Photon (STM32F205) DMA Control of GPIO pins](https://www.kasperkamperman.com/blog/particle-photon-stm32f205-dma-control-gpio-pins/)
- [TM32 DMA输出到GPIO问题](https://bbs.21ic.com/icview-368199-1-1.html)
- [STM32并口数据通过DMA传输](https://blog.csdn.net/aaaaa098/article/details/105615573)

#### 程序编写

```
static uint32_t source_buffer[10] = {0xFFFF,0x11};                      /*A buffer for 10 rows*/

/* 9 (DMA IRQ callbacks) */
void data_tramsmitted_handler(DMA_HandleTypeDef *hdma)
{

}


void setup()
{
	delay_init(84); /* 延时函数初始化 */

	GPIOA->ODR = 0x0000;
	hdma_memtomem_dma2_stream0.XferCpltCallback = data_tramsmitted_handler;  // 注册 DMA 传输完成中断
	//hdma_memtomem_dma2_stream0.XferErrorCallback = transmit_error_handler;  // 注册传输错误中断

	HAL_DMA_Start_IT(&hdma_memtomem_dma2_stream0, (uint32_t)source_buffer,(uint32_t)&GPIOA->ODR,1);   // 传输数组的值到IO引脚，按位从低到高一一对应。
	                                                                                                  // 如果DMA的数据位宽设计为8为，不论传多少位数据，只有低8位引脚（Pin0-7）改变。
	                                                                                                  // 设为16位，则只改变低16位
}

```
- `source_buffer` 存储这引脚状态，每一元素的每一位表示一个一脚，bit0对应pinx-0，依次一一对应。  
- `hdma_memtomem_dma2_stream0.XferCpltCallback` 是注册回调函数，这里是DMA的普通模式，因此回调函数需要我们自己编写并注册。
- `HAL_DMA_Start_IT` 开启DMA传输，若不使用中断也可以使用函数 HAL_DMA_Start 代替。
- DMA传输只改变目标地址位宽对应引脚的状态，其它引脚不改变。比如这里 源地址和目标地址位宽都是8位，则最后只会改变pin0-7 引脚状态，其余引脚不受影响。如果是16位则改为pin0-15.
- 如果DMA源地址位宽是8位，目标地址位宽16，则传输数据时，数组的 [0] 和 [1] 共同表示一个引脚状态。
- 不论数组元素是多少位的，传输数据时只传送DMA源地址位宽对应的低位。比如这里数组是32位的，DMA位宽8位，则数组元素只有低8位有效

### 自定义延时

#### SysTick介绍
HAL 官方是没有 us 级延时函数的。这里参考正点原子例程，改写了一点。

SysTick定时器是存在于系统内核的一个滴答定时器，只要是ARM Cortex-M0/M3/M4/M7内核的MCU都包含这个定时器，它是一个24位的递减定时器，当计数到 0 时，将从RELOAD 寄存器中自动重装载定时初值，开始新一轮计数。使用内核的SysTick定时器来实现延时，可以不占用系统定时器，由于和MCU外设无关，所以代码的移植，在不同厂家的Cortex-M内核MCU之间，可以很方便的实现。

STM32默认设置 SysTick 定时为1ms，也就是 HAL_Delay 的时钟来源。所以我们无需再初始化,如果是其它芯片，可能需要使用下面语句初始化 SysTick：
```
SysTick_Config(SystemCoreClock / 1000000);  //定时1us
// 或
SysTick_Config(SystemCoreClock / 1000);     //定时1ms
```

下面是具体实现，在新文件中添加以下代码并引用：
```
#define F_CPU SystemCoreClock  // 系统时钟
#define CYCLES_PER_MICROSECOND (F_CPU / 1000000U)   // 1us 的时钟周期
//延时nus
//nus为要延时的us数.	
//nus:0~190887435(最大值即2^32/fac_us@fac_us=22.5)	 
void delay_us(u32 nus)
{		
	u32 ticks;
	u32 told,tnow,tcnt=0;
	u32 reload=SysTick->LOAD;				//LOAD的值	    	 
	ticks=nus*CYCLES_PER_MICROSECOND; 		//nus 需要的节拍数 
	told=SysTick->VAL;        				//计数器值
	while(1)
	{
		tnow=SysTick->VAL;	
		if(tnow!=told)
		{	    
			if(tnow<told)tcnt+=told-tnow;	//这里注意一下SYSTICK是一个递减的计数器就可以了.
			else tcnt+=reload-tnow+told;	    
			told=tnow;
			if(tcnt>=ticks)break;			//时间超过/等于要延迟的时间,则退出.
		}  
	};
}

//延时nms
//nms:要延时的ms数
void delay_ms(u16 nms)
{
	u32 i;
	for(i=0;i<nms;i++) delay_us(1000);
}
```

#### 初始化配置

参考 HAL 库操作时的 SYS 和 RCC 配置，启动 Timebase Source 和 系统时钟。

#### 程序编写

直接引用即可。

### 按键输入

这里提供两种按键输入检测方法：阻塞和非阻塞。

#### 硬件原理图

![图 5](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/按键硬件图.png) 

#### 初始化配置

基本沿用GPIO HAL 库操作时的配置，只不过在 IO 功能配置时将按键引脚配置为 **输入模式**（GPIO_Input）,上下拉配置根据硬件选择，这里选择上拉，低电平触发。

#### 阻塞-程序编写

这个就直接参考正点原子的函数即可，利用延时函数消抖延时检测。

``` cpp
// 长按无效，按键必须松开才有效
int Key_Scan(void){
	if(HAL_GPIO_ReadPin(Key0_GPIO_Port,Key0_Pin) == 0){ // 检测到按键为低电平
		HAL_Delay(10);
		if(HAL_GPIO_ReadPin(Key0_GPIO_Port,Key0_Pin) == 0){
			while(HAL_GPIO_ReadPin(Key0_GPIO_Port,Key0_Pin) == 1);
			return 1;
		}
	}
	return 0;
}

// 正点原子写法
//mode:0,不支持连续按;1,支持
//0，没有任何按键按下
//1， WKUP 按下 WK_UP
#define KEY0 HAL_GPIO_ReadPin(Key0_GPIO_Port,Key0_Pin) //KEY0 按键
u8 KEY_Scan(u8 mode)
{
    static u8 key_up=1; //按键松开标志
    if(mode == 1)key_up=1; //支持连按
    if(key_up && (KEY0 == 0){
        delay_ms(10);
        key_up=0;
        if(KEY0 == 0) return KEY0_PRES;
    }
    else if(KEY0 == 1) key_up=1;
    return 0; //无按键按下
}
```

#### 非阻塞-程序编写

在编写非阻塞程序前，我们需要先了解一个函数：`HAL_GetTick()`

如果你仔细研究 `HAL_Delay()` 函数的话，会发现其实它底层是调用了 `HAL_GetTick()`。HAL库中原型如下：
```
/**
  * @brief Provides a tick value in millisecond.
  * @note This function is declared as __weak to be overwritten in case of other 
  *       implementations in user file.
  * @retval tick value
  */
__weak uint32_t HAL_GetTick(void)
{
  return uwTick;
}
```
其中的 `uwTick`又被`HAL_IncTick()`调用，该函数又被系统滴答定时器中断（1ms）调用，每次递增 1, 所以它的值代表了系统上电运行至今的时间（ms），而我们则就可以通过`HAL_GetTick()`:
1. 获取系统运行时间，最大计时 49.7 天。（`uwTick`为32位，2^32/1000/60/60/24 = 49.7）
2. 也可以利用该函数，做一个ms的计数器

非阻塞按键检测程序正是利用第2点，移植了 [OneButtonLibrary](http://www.mathertel.de/Arduino/OneButtonLibrary.aspx) 库，同时实现按键的单击、双击、长按检测。还不会阻塞正常程序执行。

```
/*************** key.h文件 ***************/

#define millis() HAL_GetTick()  /* 获取系统时间，用于按键的计时基准ms */
typedef unsigned long millis_t; /* 专门用于 millis() 型变量声明 */

/* 按键状态 */
enum KEY_STATUS{
    NO_CLICK,        /* 没有动作 */
    SINGLE_CLICK,    /* 单击 */
    DOUBLE_CLICK,    /* 双击 */
    LONGLE_CLICK     /* 长按 */
};

// 按键低电平有效
#define BUTTON_PRESSED  (HAL_GPIO_ReadPin(KEY_GPIO_Port, KEY_Pin)!=1)

/**
 * @brief  按键扫描状态机（FSM）
 * @retval enum key_status
 */
u8 button_tick(void);


/*************** key.c文件 ***************/

#define DEBOUNCETIME  50 // ms 按键去抖时间
#define CLICKTIME  100  // ms 检测到单击之前必须经过的时间（超过这个时间可能是双击或长按）
#define LONGTIME  1500 // ms 检测到长按之前必须经过的时间（超过这个时间是长按）

// 这些变量在按键检测时保存信息。
// 它们在程序启动时初始化一次，并在每次调用key_scan()函数时更新。
static u8 s_state = 0;  // 状态机标志位
static millis_t s_startTime = 0; // will be set in state 1
static millis_t s_stopTime = 0; // will be set in state 2

u8 button_tick(void)
{
    u8 key_status = 0;
    millis_t now = millis(); // 获取当前的时间 ms.

    if (s_state == 0) { // 等待按键按下.
        if (BUTTON_PRESSED) {
            s_state = 1; // 转到状态1
            s_startTime = now; // 记住按键按下的时间（当前时间）
        }
        else
            key_status = NO_CLICK;
    }
    else if (s_state == 1) { // 等待按键被释放

        if ((!BUTTON_PRESSED) && ((unsigned long)(now - s_startTime) < DEBOUNCETIME)) {
            // 按键释放太快，认为是抖动，返回状态0，认为按键没有被按下，再次返回状态0
            s_state = 0;
        }
        else if (!BUTTON_PRESSED) { // 按键释放且有效
            s_state = 2; // // 转到状态2
            s_stopTime = now; // remember stopping time
        }
        else if ((BUTTON_PRESSED) && ((unsigned long)(now - s_startTime) > LONGTIME)) {
            s_state = 6; // 按键已知每释放，且按下的时间超过了长按的时间，则认为是长按，转到状态6
            s_stopTime = now; // // 记住当前时间
        } else {
            // Button was pressed down. wait. Stay in this state.
        } // if
    }
    else if (s_state == 2) {
        // waiting for menu pin being pressed the second time or timeout.
        if ((unsigned long)(now - s_startTime) > CLICKTIME) {
            // this was only a single short click
            key_status = SINGLE_CLICK;
            s_state = 0; // restart.
        } else if ((BUTTON_PRESSED) && ((unsigned long)(now - s_stopTime) > DEBOUNCETIME)) {
            s_state = 3; // step to state 3
            s_startTime = now; // remember starting time
        } // if
    }
    else if (s_state == 3) { // waiting for menu pin being released finally.
        // Stay here for at least _debounceTicks because else we might end up in
        // state 1 if the button bounces for too long.
        if ((!BUTTON_PRESSED) && ((unsigned long)(now - s_startTime) > DEBOUNCETIME)) {
            // this was a 2 click sequence.
            key_status = DOUBLE_CLICK;
            s_state = 0; // restart.
            s_stopTime = now; // remember stopping time
        } // if
    }
    else if (s_state == 6) {  /* 长按状态处理 */
        // waiting for pin being release after long press.
        if (!BUTTON_PRESSED) {
            s_state = 0; // restart.
            s_stopTime = now; // remember stopping time
        } else {
            // button is being long pressed
            key_status =  LONGLE_CLICK;
        } // if
    } // if
    return key_status;
} // OneButton.tick()
```

之后只要在主程序中循环调用函数 `button_tick()`，读取返回值，就能知道按键的状态。

### GPIO 双向 I/O

有时需要IO既要作为输出，还要作为输入读取。如果采用初始化重新配置的话，就会很慢且繁琐。
如果希望某GPIO做双向传输，将其配制为OD输出模式，

#### F401 开漏输出模式介绍
* 开漏模式：输出寄存器中的“0”可激活 N-MOS，而输出寄存器中的“1”会使端
* 口保持高组态 (Hi-Z)（ P-MOS 始终不激活）。
* 施密特触发器输入被打开
* 根据 GPIOx_PUPDR 寄存器中的值决定是否打开弱上拉电阻和下拉电阻
* 输入数据寄存器每隔 1 个 AHB1 时钟周期对 I/O 引脚上的数据进行一次采样
* 对输入数据寄存器的读访问可获取 I/O 状态
* 对输出数据寄存器的读访问可获取最后的写入值

![图 2](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/GPIO输出内部结构图.png)  

另外其实将IO设置为推挽输出模式时，也可以随时读取 IO 引脚状态，但在该模式下，不论输出高、低电平，P-MOS和N-MOS总有一个处于导通状态，轻则影响外部输入信号，重则烧毁芯片（外部拉低或拉高，MOS都相当于短路，导致大电流）。所以并不能作为双向 IO。

#### 初始化配置

* 将该引脚配置为Output-OpenDrain，
* 在引脚上连接一个上拉电阻（从上图可以看出，**F401 能通过软件配置上下拉电阻的；但在 F103 上是没有的，则需要外部硬件上拉**）

上述具体实现：在沿用 GPIO HAL 库操作时的配置基础上，只需修改以下图示部分：
![图 2](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/test.png)  


#### 程序编写

* 输出时： 
    ```
    GPIOx->BSRR ＝ 输出值;
    ```
* 输入时： 先输出高电平(否则如果之前输出的是低电平，N-MOS则会导通，影响外部输入)，然后通过 GPIOx->IDR 读.
   ```
   LED_GPIO_Port->ODR=(LED_GPIO_Port->ODR | LED_Pin);  // 置1
   LED_Pin_status = LED_GPIO_Port->IDR & LED_Pin;
   ```

#### 参考链接
* [STM32 MCU GPIO双向口使用的话题](http://www.360doc.com/content/17/1208/13/8706683_711243855.shtml)
* [stm32的双向io口](https://blog.csdn.net/weixin_30443813/article/details/96729719)
* [STM32 IO口双向问题](https://my.oschina.net/hoolev/blog/525208)
* [STM32 GPIO八种输入输出模式的功能及区别](https://blog.csdn.net/weixin_41072132/article/details/103264249)
* [STM32的8种GPIO输入输出模式深入详解](https://blog.csdn.net/baidu_37366055/article/details/80060962)

### GPIO 模拟配置

#### 模拟配置介绍
该模式一般用于复用状态下或低功耗要求下。不作为普通输入输出控制下的模式配置。

![图 3](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/GPIO%E6%A8%A1%E6%8B%9F%E9%85%8D%E7%BD%AE.png)  

**总结**
1、模拟配置会关闭引脚的一切内部相关联设施，此时普通 I/O 操作失效（不能读也不能输出）。因此引脚功耗为0。因此可以通过将引脚配置为该模式来**降低芯片功耗**。 
2、模拟配置另外好处就是保证了这个引脚是 **“干净”** 的，如果和外部连接，那个该引脚就完全反映了外部引脚状态。因此将该引脚内联到A/D 片上外设，就可以精确测量引脚的模拟值了。实际上在我们将引脚复用为 A/D 功能时，就会默认配置为 Analog 模式。

#### 初始化配置

`Pinout` 界面，引脚单击选择即可
![图 3](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/analog.png)  

或者在 `Project Manager` 界面选中将不用的引脚都配置为模拟模式，降低功耗
![图 4](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/analog%20full.png)  

#### 程序编写

无

**参考资料**
- [1、Question about ADC versus GPIO Analog](https://community.st.com/s/question/0D50X00009XkfqtSAB/question-about-adc-versus-gpio-analog)  
- [2、What pins can I use for Analog Input/Output? STM32CubeMX allows every GPIO to be set to ''GPIO_Analog''?](https://community.st.com/s/question/0D50X00009XkWkeSAF/what-pins-can-i-use-for-analog-inputoutput-stm32cubemx-allows-every-gpio-to-be-set-to-gpioanalog)  



### 外部中断

通过外部按键，中断触发，再中断函数中翻转LED。

#### 硬件原理图

![图 5](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/按键硬件图.png) 

#### 初始化配置

1. 配置引脚为外部中断模式
2. 配置引脚：中断触发模式，上下拉。
根据按键原理图，这里设置为上拉，下降沿触发。
![图 5](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/%E6%8C%89%E9%94%AE%E4%B8%AD%E6%96%AD_%E5%BC%95%E8%84%9A%E9%85%8D%E7%BD%AE.png)  
3. 中断配置：使能中断，中断分组及优先级
![图 6](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/%E6%8C%89%E9%94%AE%E4%B8%AD%E6%96%AD_%E4%B8%AD%E6%96%AD%E9%85%8D%E7%BD%AE.png)  

#### 程序编写

```
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin){

	if(GPIO_Pin & KEY_Pin){
		 led_toggle(); //电平反转
		 HAL_Delay(1000);  // 防抖，防止频繁触发中断，导致LED翻转现象不明显
	}
}

```
`HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)`为 HAL 库的引脚外部中断回调函数，所有的引脚中断都会调用该函数。用户只需要在这里面编写中断处理函数接即可。`GPIO_Pin`传参表示触发中断的引脚编号。  
`GPIO_Pin & KEY_Pin`判断当前中断是否由按键引脚触发的，再运行处理函数。

>**注意：** 这个回调函数是只针对外部中断的（EXTI），定时中断和其他中断都都还有自己的回调函数。HAL的思想大概就是同类中断集中在一个回调函数，不同类的分开。


### 中断与事件

#### Cortex-M3 处理器内核 vs 基于Cortex-M3的MCU
Cortex-M3 处理器内核是由 ARM 公司设计的，传统意义上的 ARM7/ARM9（简称A7/A9）  也是处理器内核，也是 ARM 公司设计的。

Cortex‐M3处理器内核：故名思意就是单片机（MCU）的核心，是单片机的中央处理单元（CPU）

完整的基于CM3的MCU还需要很多其它组件。在芯片制造商得到CM3处理器内核的使用授权后，它们就可以把CM3内核用在自己的硅片设计中，添加存储器，外设， I/O以及其它功能块。不同厂家设计出的单片机会有不同的配置，包括存储器容量、类型、外设等都各具特色。

![图 4](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/内核与芯片.png)

#### 中断和异常

中断属于异常的一种。所有能打断正常执行流的事件都称为异常

CM3 的所有中断机制都由 NVIC 实现。除了支持 240 条中断之外， NVIC 还支持 16‐4‐1=11 个内
部异常源（保留了 4+1 个档位），可以实现 fault 管理机制。结果， CM3 就有了 256 个预定义的异常类型。其中编号为 1－15 的对应系统异常，大于等于 16 的则全是外部中断。

类型编号为 1－15 的系统异常如表 7.1 所示（注意： 没有编号为 0 的异常），从 16 开始的外部中断类型如表 7.2 所示
![图 1](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/CM3%E5%BC%82%E5%B8%B8%E7%B1%BB%E5%9E%8B.png) 

![在这里插入图片描述](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/CM3%E5%A4%96%E9%83%A8%E4%B8%AD%E6%96%AD%E6%B8%85%E5%8D%95.png)  

虽然 CM3 是支持 240 个外中断的，但具体使用了多少个是由芯片制造商决定。 CM3 还有一个NMI（不可屏蔽中断）输入脚。当它被置为有效（ assert）时， NMI 服务例程会无条件地执行。

#### STM32外部中断（EXTI ）
STM32F103 是基于 CM3 内核设计的，ST 公司（芯片制造商）在原有 CM3 内核基础上，添加了储如定时器、串口、DMA等外设，最终组合成一个STM32单片机。其中 CM3 内核是整个单片机的核心部分，相当于CPU（大脑）

所以 STM32 根据原有 NVIC 中断，从中选择性添加了部分中断，并重新命名与排序。下图是STM32的中断向量表：

![图 3](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/中断向量表.png)  

从表中可以看出，STM32 对上文中 CM3 内核的系统异常/外部中断表重新进行了编排和删减，把编号从-3 至 6 的中断向量定义为系统异常。从编号 7 开始将原本 CM3 所描述的外部中断又分成了若干中断类型：外部中断（EXTI）、定时器中断、DMA中断等等。


细心的朋友可能已经发现了这里有一个概念冲突：**外部中断**。释义如下:
>CM3 内核描述中的外部中断均是相对于内核而言的，比如串口中断、定时器中断等等都是（内核的）外部中断！而这里提到的STM32的外部中断（EXTI）指的是芯片的外部中断，主要是由芯片外部事件触发的中断，不是内核的外部中断！
STM32的外部中断（EXTI）属于内核的外部中断一部分。在STM32手册中外部中断（EXIT）均是指芯片的外部中断**加粗样式**，也就是上表中的 EXIT0-9。
这里的 `内外部` 就是物理空间的内外部。

所以当阅读 STM32 参考手册时，外部中断（EXTI）指的均是芯片外部（IO引脚）事件触发的中断。而当阅读网络文章时，则要注意区分。为了避免混淆，都会加 `（EXTI）` 以区分。

这里还有一个概念：`软件中断` ，下文中再详述。
另外 STM32 是没有 ~~内部中断~~  这个概念的，

![图 2](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/内核与中断.png)  

#### 中断/事件关系
MCU运行过程，其中会有许多各种各样的事件，比方：管脚电平变化、计数器溢出、DMA空、FIFO非空、AD转换结束、超时、外设使能、初始化等等。
其中有些事件本身是不会导致中断产生的，比方外设使能或部分初始化动作是不会导致中断发生的；有些事件则可能导致中断发生，比方计数器溢出，AD转换结束等，这些就是中断事件。当然这些中断事件最终能否触发后续中断，还需要对中断事件进行配置。

**先说结论**
- 中断：处理器运行的一个状态，该状态会打断处理器当前正常的进程。
- 事件：就是事件。其可能触发中断。
- 中断事件：触发中断的事件，而且软件上也有中断函数的，叫中断事件
- 中断是中断事件发生的结果，中断事件属于事件，事件可分为中断事件或非中断事件

我们可以借助 STM32 MCU的GPIO的外部事件与中断控制器的框图来理解上述结论。
>这张图的在 STM32中文手册 中是错误的，英文版的是对的。因而网上很多文章此处的配图都有误，我这里重置了。


![图 1](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/EXTI%E5%8A%9F%E8%83%BD%E6%A1%86%E5%9B%BE.png)  

我们先关注两个寄存器：`中断屏蔽寄存器`和`事件屏蔽寄存器`。这两个寄存器决定了从编号1、2、3输入进来的事件最终会输出脉冲发生器（不产生中断）还是 NVIC 中断控制器（产生中断）。从而决定了输入的事件是中断事件还是非中断事件。

MCU参考手册里在谈到事件的触发方式时引入了`事件模式`和`中断模式`两个概念。这里的不同模式就是通过控制这两个寄存器实现的。
>**例子:**
比方STM32的GPIO口的电平跳变是可能触发外部中断（EXIT）的。但在具体配置时，可以根据需要来决定启用还是禁用相关脚的中断功能，从而选择不同的事件触发方式，即：`外部事件模式`和`外部中断模式`。如果不希望电平跳变事件触发中断，就配置为事件模式，反之，配置为中断模式

**接下来详细说明 EXIT 执行过程。**
上图中信号线上划有一条斜线，旁边标志 19字样的注释，表示相同的这样的中断线路共有19条。EXTI中有一个边沿检测电路(编号②)监视着输入线（编号①），并分别与上升沿和下降沿选择寄存器对比。 如果在这两个寄存器中相应的中断线检测开启了，那么当中断线上有上升沿或者下降沿时边沿检测电路就会产生一个事件触发信号给后继的或门。

除了边沿检测电路的输出外，或门（编号 ③）还接受一个`软件中断事件寄存器`的输入。 `软件中断事件寄存器`的存在使得我们可以通过软件的形式直接触发某一个中断线上的事件。
>我们可以通过程序控制此处的`软件中断事件寄存器`，人为的通过或门（编号 ③）输入一个外部事件，从而不需要真实的外部输入，就能产生一个可能触发中断的事件，相当与模拟该中断线上的事件。

>诸如ADC、串口、定时器之类产生的中断，就叫 `名称+中断`，如：定时器中断、串口中断、ADC中断。并不属于这里的`软件中断`范畴，STM32手册中唯一提到`软件中断`这个词的就是指这个寄存器，不要混淆了。

或门的输出接到了两个与门（编号 ④、⑤）上，一方面与中断屏蔽寄存器求与编号（④）触发中断， 另一方面与事件屏蔽寄存器求与（⑤）触发事件。 中断屏蔽寄存器控制了相应的中断是否开启了，如果开启了中断将会产生一个中断触发信号，置位中断请求寄存器， 同时将中断触发信号提交给中断控制器(NVIC)。 同样的道理，事件屏蔽寄存器控制事件是否开启，如果开启则直接产生一个脉冲通知后继的功能模块处理事件，例如通知DMA读写内存等。

从这张图上我们也可以知道，从外部激励信号来看，中断和事件的产生源都可以是一样的。之所以分成2个部分，因为中断是需要CPU参与的，需要软件的中断服务函数才能完成中断后产生的结果；但是事件，是靠脉冲发生器产生一个脉冲，进而由硬件自动完成这个事件产生的结果，当然相应的��动部件需要先设置好，比如引起DMA操作，AD转换等;

>**简单举例：** 外部I/O触发AD转换,来测量外部物品的重量;
> - 如果使用传统的中断通道，需要I/O触发产生外部中断（EXIT），外部中断（EXIT）服务程序启动AD转换，AD转换完成中断服务程序提交最后结果；
> - 要是使用事件通道，I/O触发产生事件，然后联动触发AD转换，AD转换完成中断服务程序提交最后结果；

相比之下，后者不要软件参与启动AD转换，并且响应速度也更块；要是再使用事件触发DMA操作，就完全不用软件参与（AD转换后操作）就可以完成某些联动任务了。

**总结:**
- 事件触发：机制提供了一个完全由硬件自动完成的触发到产生结果的通道，不要软件的参与，降低了CPU的负荷，节省了中断资源，提高了响应速度(硬件总快于软件)，是利用硬件来提升CPU芯片处理事件能力的一个有效方法;
- 中断触发：由软件控制，CPU 参与。

**参考链接**
- STM32F10xxx参考手册（Reference manual STM32F101xx, STM32F102xx, STM32F103xx, STM32F105xx and STM32F107xx advanced ARM®-based 32-bit MCUs）
- Cortex-M3权威指南（The Definitive Guide to the ARM COrtex-M3） 
- [Interrupt-Driven Input/Outputon the STM32F407 Microcontroller](https://docplayer.net/54908905-Interrupt-driven-input-output-on-the-stm32f407-microcontroller.html)
- [Exceptions and Interrupts——Cuauhtemoc Carbajal-ITESM CEM](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwjK-vytkcbvAhUmJzQIHXtSARgQFjAAegQIAxAD&url=http://homepage.cem.itesm.mx/carbajal/Microcontrollers/SLIDES/Interrupts.pdf&usg=AOvVaw3OiAMagNDaIjozquuDcoMa)
- [新手入门之stm32中断系统](https://zhuanlan.zhihu.com/p/64921464)
- [stm32异常、中断和事件的区别](https://blog.csdn.net/jian3214/article/details/99818975)
- [STM32中断系统（NVIC和EXTI）](https://my.oschina.net/u/4383286/blog/4440596)
- [STM32的“外部中断”和“事件”区别和理解](https://blog.csdn.net/tanyjin/article/details/53359883)
- [【STM32】EXTI---外部中断/事件控制器](https://blog.csdn.net/qq_43328313/article/details/106559934)
- [STM32中断与事件](https://mp.weixin.qq.com/s?__biz=MzIxNTg1NzQwMQ==&mid=2247484547&idx=2&sn=de7a4464fe0dadadb3f9b8843dd33d0b&chksm=9790a515a0e72c036be83b413489bbc5cbf367caf11a0cb7403a05823e2938e00126248344ce&scene=178&cur_album_id=1359585244344696836#rd)
- [浅谈STM32中断模块](https://www.codenong.com/cs106293064/)
- [外部中断(EXTI)控制LED灯](https://gaoyichao.com/Xiaotu/?book=stm32&title=%E5%A4%96%E9%83%A8%E4%B8%AD%E6%96%AD%E6%8E%A7%E5%88%B6LED%E7%81%AF)


### 串口

#### 初始化配置

1. 配置引脚为串口输入输出模式
![图 1](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/%E4%B8%B2%E5%8F%A3%E5%BC%95%E8%84%9A%E9%85%8D%E7%BD%AE.png)  
2. USART配置中选择异步通信模式，并开启串口中断
![图 2](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/USART%E9%85%8D%E7%BD%AE.png)  

#### 程序编写

同外部中断类似，串口中断也有自己的中断回调函数，我们再需要的地方编写即可。

**主要API：**
1. HAL_UART_Transmit();串口轮询模式发送，使用超时管理机制，阻塞
2. HAL_UART_Receive();串口轮询模式接收，使用超时管理机制，阻塞
3. HAL_UART_Transmit_IT();串口中断模式发送，非阻塞
4. HAL_UART_Receive_IT();串口中断模式接收，非阻塞
5. HAL_UART_TxHalfCpltCallback();一半数据发送完成时调用
6. HAL_UART_TxCpltCallback();数据完全发送完成后调用
7. HAL_UART_RxHalfCpltCallback();一般数据接收完成时调用
8. HAL_UART_RxCpltCallback();数据完全接受完成后调用
8. HAL_UART_ErrorCallback();传输出现错误时调用

**主函数：**

```
/************************* uart.cpp *************************/
uint8_t RxBuffer;  // 定义一个接受数组

/**
  * @brief 串口初始化，启动接收中断
  */
void uart_init()
{
	HAL_UART_Receive_IT(&huart1,&RxBuffer,1);  //开启中断
}

/**
  * @brief 串口接收中断，每接收一个字节中断一次，并发送该数据
  */
void HAL_UART_RxCpltCallback(UART_HandleTypeDef *UartHandle)
{
	if(UartHandle->Instance == USART1){   //判断时那种中断
		HAL_UART_Transmit(&huart1,&RxBuffer,1,10);  // 发送10个数据
	}
	HAL_UART_Receive_IT(&huart1,&RxBuffer,1); // 再次开启中断
}

```

**使用步骤：**

1. 添加以上代码
2. 调用`uart_init()`初始化
3. 然后通过串口助手发送信息，单片机即返回所发送的信息。

#### printf，getchar重定义

fgetc，fputc 属于 C 标准可，因此在.ccp 文件中重定义是，需要添加 extern "C" 声明。

```
// 将以下函数放在 .cpp 文件中，需要添加 extern "C" ，否则重定义无效。
// 或者可以将其放在任意 .c 文件中
#ifdef __cplusplus
extern "C" {
#endif

/**
  * @brief 重定向c库函数getchar,scanf到USARTx
  * @retval None
  */
#ifdef __GNUC__
#define GETCHAR_PROTOTYPE int __io_getchar(int ch)  /* 防止重定义，具体为什么会用到GNUC我以为不知道*/
#else
#define GETCHAR_PROTOTYPE int fgetc(int ch, FILE *f)
#endif
GETCHAR_PROTOTYPE{
  HAL_UART_Receive(&huart1,(uint8_t *)&ch, 1, 0xffff);
  return ch;
}

/**
  * @brief 重定向c库函数printf到USARTx
  * @retval None
  */
#ifdef __GNUC__
#define PUTCHAR_PROTOTYPE int __io_putchar(int ch)  /* 防止重定义，具体为什么会用到GNUC我以为不知道*/
#else
#define PUTCHAR_PROTOTYPE int fputc(int ch, FILE *f)
#endif
PUTCHAR_PROTOTYPE{
	 HAL_UART_Transmit(&huart1,(uint8_t *)&ch,1,1000);
     return ch;
}

#ifdef __cplusplus
}
#endif
```

**使用步骤：**
1. 添加以上代码
2. 包含头文件 `#include "stdio.h"` 
3. 添加测试代码：`printf("\n===函数Printf函数发送数据===\n");` 测试

**打印浮点数**

IDE在编译使，默认不支持打印浮点数的（耗费内存和运存）。可以右键单击项目名，在 Properties 中开启该功能：
![图 3](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/float%E6%94%AF%E6%8C%81.png)  

或者自己编写浮点数打印函数：
```]
/*
 * 打印浮点类型
 * @param（data）要打印的浮点数
 * @param（precision）小数点精度（小数点后几位）
 */
void printf_float(double data,uint8_t precision){
	uint8_t num = 0;
	uint32_t data_int = (uint32_t)data;
	uint32_t data_float = 0;
	printf("%d",(int)data_int);
	printf(".");
	while(num<precision){
		data_float = (int)(data*pow(10,num+1))%10;
		printf("%d",(int)data_float);
		num++;
	}
}
```

### 串口DMA模式

**主要API：**
1. HAL_UART_Transmit_DMA(); // 使用DMA模式发送数据
2. HAL_UART_Receive_DMA();  // 使用DMA模式接收数据


#### UART以DMA方式接收和发送的函数调用顺序：

**循环模式接收**：
`HAL_UART_Receive_DMA()` -> `DMA1_Channelx_IRQHandler()` -> `HAL_DMA_IRQHandler()` -> `UART_DMAReceiveCplt()` -> `HAL_UART_RxCpltCallback()`

**正常模式发送：**
`HAL_UART_Transmit_DMA()` -> `DMA1_Channelx_IRQHandler()` -> `HAL_DMA_IRQHandler()` -> `UART_DMATransmitCplt()` -> `USART3_IRQHandler()` -> `HAL_UART_IRQHandler()` -> `UART_EndTransmit_IT()` -> `HAL_UART_TxCpltCallback()`

循环发送与正常接收模式与上述类似，不再叙述。这当中还会调用传输 Half 中断，这里也不再讨论了。  
以上整个调用过程不需要CPU参与，自动执行。我们只需要关心状态变化即可，无需关心数据怎么传输的。

**总结：**
对于上述过程，我们只需要知道：DMA 在执行过程中是会调用正常的 USART 的 API 接口函数的就行。也就意味着，如果我们使用了 DMA接收，则不能再用中断接收以及相应的接收函数，否则两者的数据会又冲突，实际测试也是如此。所有的接收过程不应再有软件的参与，我们只需要关系数据是否到来、一半、结束几个标志。同理DMA发送与USART发送函数不可同时使用。特别是在开启了循环模式时。

#### 循环模式

循环模式下，DMA的发送与接收是不断循环的，不会停止。我们只需要在初始化时开启即可。

**初始化配置**

承接上面 USART 的配置，最初以下修改：
1. 在 USART 配置界面中，选中DMA设置
   1.1 添加USART_RX/TX 两个通道
   1.2 两个通道均选择 循环模式，数据宽度为 1字节
2. 在 USART 中断配置界面中，取消串口全局中断

![图 4](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/USART_DMA%E9%85%8D%E7%BD%AE.png)  

如前文所述，我们开启了DMA的循环模式，为了避免冲突，这里需要关闭串口的全局中断。

**软件编写**

我们实现串口接收啥就返回啥。和前面的串口功能一样，但这里并不需要软件参与，全程自动执行。
> **注意：** 这里的数组只有 5 个字节，所有只能接收 5 个字节的数据，多了则只保留后5位。
由于发送是自动的，所以下面的程序在接收到数据后，就会不断重复发送该数据，除非有新的外部数据或手动清空。
```
uint8_t buffer[5] = {0};
void setup() {
    HAL_UART_Receive_DMA (&huart1,&RxBuffer,5);  // 开启DMA接收
    HAL_UART_Transmit_DMA (&huart1,&RxBuffer,5); // 开启DMA发送
}
```
经测试在双循环模式下，printf也是不能使用的，总之在循环模式下，不要调用任何有关发送和接收数据的函数。
经测试，就算没开启DMA接收，之开启DMA发送，并且数组数据为空，系统仍会不断往外发送数据，但是乱码。所以循环发送模式并不推荐。
#### 正常模式

正常模式下的发送与接收，每一次DMA传输都只会执行一次就接收，如果想要继续使用，就得再手动开启DMA传输。所以如果想要再正常模式下实现自动收发，我们就需要借助中断函数，初始化时下开启 DMA 接收，然后在接收中断中使用DMA发送接收到的数据，并再次启动DMA接收。


**初始化配置**
在双循环配置基础上，都选择 正常模式（normal）并勾选串口中断。
**软件编写**

改写之前的串口初始化和接收中断函数，实现功能与前文一样，收啥发啥。
```
/**
  * @brief 串口初始化，启动接收中断
  */
void uart_init()
{
	HAL_UART_Receive_IT(&huart1,&RxBuffer,1);  //开启中断
	HAL_UART_Receive_DMA (&huart1,&RxBuffer,5);  // 启动DMA接收
}

/**
  * @brief 串口接收中断，每接收一个字节中断一次
  */
void HAL_UART_RxCpltCallback(UART_HandleTypeDef *UartHandle)
{
	if(UartHandle->Instance == USART1){   //判断时那种中断
		HAL_UART_Transmit_DMA (&huart1,&RxBuffer,5);
	}
	HAL_UART_Receive_DMA (&huart1,&RxBuffer,5);

}
```
这和一般的串口中断很相似，唯一的区别就是在发送和接收数据时，不需要CPU的参与，仅在这个数据的流动过程是不同的。
经过实际测试，这种模式下，DMA的数据是有问题的。由于DMA发送接收不需要CPU参与，所以在接收中断中调用`HAL_UART_Transmit_DMA()`发送串口数据，之后再`HAL_UART_Receive_DMA ();`启动接收，整个过程近似无延时，所以当你的数据超过数组长度时，下一次接收是会接收到上一次发送的多余的数据。

![正常模式下返回数据](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/正常模式下返回数据.gif)

有一个解决办法是将 `HAL_UART_Receive_DMA ();` 放到初始化源码的中断函数里

![图 5](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/USART_DMA-normal.png)  


#### 混合模式

这里用接收循环模式，发送正常模式说明。

**初始化配置**

在双循环配置基础上，发送选择正常模式，接收选择循环模式。

**软件编写**

改写之前的串口初始化和接收中断函数，实现功能与前文一样，收啥发啥。
```

/**
  * @brief 串口初始化，启动接收中断
  */
void uart_init()
{
	HAL_UART_Receive_DMA (&huart1,&RxBuffer,10);
}

/**
  * @brief 串口接收中断，每接收一个字节中断一次
  */
void HAL_UART_RxCpltCallback(UART_HandleTypeDef *UartHandle)
{
	if(UartHandle->Instance == USART1){   //判断时那种中断
		//HAL_UART_DMAStop(&huart1); //
		HAL_UART_Transmit_DMA (&huart1,&RxBuffer,10);
		//HAL_DMA_PollForTransfer(huart1.hdmatx,HAL_DMA_FULL_TRANSFER,1000);
	}


}
```
这里的收发数据问题和双正常模式一样，接收数据会记住多余的数据。由于接收数据是自动执行的，随意这里并不能更改修复。
有一个解决办法的思路，就是在发送数据后，清空 DMA 接收缓存，并重置数据指针，但比较麻烦，似乎并不划算。

### TIM定时器

#### 初始化配置

这里选择不常用的 TIM10 作为定时器，未提及的沿用 GPIO HAL 库操作时的配置
1. 模式（mode）只需勾选使能即可
2. 参数配置设置成1KHz定时（系统时钟84MHz）
3. 使能定时器中断
![图 1](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/定时器配置.png)  

#### 程序编写

**主要API：**
- `HAL_TIM_PeriodElapsedCallback()`  
非阻塞模式下经过一段时间的回调
- `HAL_TIM_PeriodElapsedHalfCpltCallback()`  
在非阻塞模式下，经过了一半的时间完成了回调
- `HAL_TIM_Base_Start_IT()`  
启动中断模式下的定时器。
- `HAL_TIM_Base_Stop_IT()`  
停止中断模式下的定时器

**主要程序**

这里实现定时器中断计数，每1s led翻转 一次。
```
void timer_init()
{
	/* 启动定时器 */
	HAL_TIM_Base_Start_IT(&htim10);
}
/* 中断回调函数 */
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
	timer_count++;
	if(timer_count == 1000){
		led_toggle();
		timer_count = 0; // 计数清零
	}
}
```
**使用步骤**
1. 系统初始化时调用 `timer_init()` 启动定时器
2. 在定时器中断中编写处理程序

### PWM

#### 初始化配置

1. 配置 PA0 引脚为定时器2通道1
2. 找到定时器2配置，通道1配置为PWm输出
3. 配置定时器参数即PWM参数，周期为1Khz

同一定时器的不同通道使用的PWM频率是一样的

![图 2](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/pwm%E9%85%8D%E7%BD%AE.png)  

#### 程序编写

**主要API**
- `HAL_StatusTypeDef HAL_TIM_PWM_Start()`
启动对应通道的PWM
- `HAL_StatusTypeDef HAL_TIM_PWM_Stop()`
停止对应通道的PWM
- `__HAL_TIM_SET_COMPARE()`
配置对于通道占空比

**主程序**

/**************** pwm.c *********************/
```
void pwm_init()
{
	HAL_TIM_PWM_Start(&htim2,TIM_CHANNEL_1);
}

void pwm_test()
{
	static millis_t timer_count = 0;
	if(timer_count == 1000)
		timer_count = 0;
	timer_count++;
	__HAL_TIM_SET_COMPARE(&htim2, TIM_CHANNEL_1, timer_count);
}

/******************* timer.c ***********************/
/* 中断回调函数 */
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
	static uint16_t timer_count = 0;
	timer_count++;
	if(timer_count == 10){
		pwm_test();
		timer_count = 0; // 计数清零
	}
}

```
`pwm_init` 为初始化函数，启动PWM计数。`pwm_test` 不断改变通道占空比。定时器中断中调用该函数，实现计数变化。  
硬件上，将PA0和LED端口相连。  
实现功能：LED亮度会逐渐变暗（10S一周期）

**使用步骤：**
1. 在系统初始化函数中调用 pwm_init 初始化
2. 使用 `__HAL_TIM_SET_COMPARE` 控制指定通道的占空比输出


### 输入捕获

我们通过输入捕获计算按键按下低电平的时间

#### 初始化配置
1. 引脚配置，IO配置为上拉
2. 定时器配置
捕获频率1Mhz，计数周期最大（能够测量更多时间，防止溢出，当然程序也做了一处处理）
由于外部按键时低电平有效，所以这里选择下降沿捕获

![图 1](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/%E8%BE%93%E5%85%A5%E6%8D%95%E8%8E%B7%E9%85%8D%E7%BD%AE.png)  

#### 程序编写

**主要API**
- `HAL_TIM_IC_Start_IT();`
启动输入捕获
- `HAL_TIM_IC_CaptureCallback()`
输入捕获中断回调函数
- `__HAL_TIM_SET_CAPTUREPOLARITY();  `
在运行时设置定时器输入捕获极性。
- ` HAL_TIM_PeriodElapsedCallback()`
定时器溢出中断回调函数
- `__HAL_TIM_SET_COUNTER();`
在运行时设置TIM计数器寄存器的值。
- `HAL_TIM_ReadCapturedValue()`
从捕获比较单元读取捕获的值，其实就是捕获中断发生时的定时器计数值

**主程序**
```
/*****************capture.cpp********************/
//变量存储
typedef struct
{
    uint8_t   flg; //0为未开始，1已经开始，2为结束
    uint32_t  num; //计数值
    uint16_t  num_period;//溢出次数
}COUNT_TEMP;

COUNT_TEMP count_temp={0};

// 输入捕获初始化
void capture_init()
{
	HAL_TIM_IC_Start_IT(&htim5, TIM_CHANNEL_2);    //启动输入捕获
}

// 输入捕获测试程序
// 打印低电平时间，并重新使能输入捕获计数
void capture_test()
{
	if(count_temp.flg == 2 )
	{
	    //计数计数值，0xFFFF为最大计数
	    uint32_t ulTime = (uint32_t)count_temp .num_period * 0xFFFF + count_temp .num;
	    //输出测量的值
	    printf ( "low level time:%d us\r\n",ulTime/1000);
	    count_temp .flg = 0;
	}

}

//捕获中断发送时的回调函数
void HAL_TIM_IC_CaptureCallback(TIM_HandleTypeDef *htim)
{
    //判断定时器5
    if(TIM5 == htim->Instance){
        if (count_temp.flg == 0 )  // 下降沿触发
        {
            // 清零定时器计数
            __HAL_TIM_SET_COUNTER(htim,0);
            //设置上升沿触发
            __HAL_TIM_SET_CAPTUREPOLARITY(&htim5, TIM_CHANNEL_2, TIM_INPUTCHANNELPOLARITY_RISING);
            count_temp .flg = 1;           //标志已捕获到下降沿
            count_temp .num_period = 0;    //溢出计数清零
            count_temp .num = 0;           //计数清零
        }
        else  // 上升沿触发
        {
            // 获取定时器计数值
            count_temp .num = HAL_TIM_ReadCapturedValue(&htim5,TIM_CHANNEL_2);
            //设置下降沿触发
            __HAL_TIM_SET_CAPTUREPOLARITY(&htim5, TIM_CHANNEL_2, TIM_INPUTCHANNELPOLARITY_FALLING);
            count_temp .flg = 2;  // 标志捕获完成
        }
    }
}
/* 中断回调函数 */
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{

    if(TIM5 == htim->Instance){
		//每次溢出时间为 2^32 us
		if(count_temp.flg==1)//还未成功捕获
		{
			if(count_temp.num_period==0XFFFF){ //低 电平太长了,强制完成
				count_temp.flg=2;              //标记成功捕获了一次
				count_temp.num=0XFFFFFFFF;
			}
			else
				count_temp.num_period ++;
		}
    }

/*****************capture.cpp********************/
void setup() {
    uart_init();
    capture_init();

}

void loop()
{
	capture_test();
}

```

- 先初始化捕获中断为下降沿触发，当下降沿触发后，立即设置为上升沿触发，保存来个那次触发时的定时器计数值，在和定时器频率 1MHz 计算就能得出低电平的总时间。   
- 溢出中断则计算溢出的次数，防止低电平时间过长当时计数器溢出。
- 主程序循环判断捕获标志位，打印输出时间

### IWDG

独立看门狗（IWDG)由专用的低速时钟（LSI）驱动（40kHz），即使主时钟发生故障它仍有效。独立看门狗适合应用于需要看门狗作为一个在主程序之外 能够完全独立工作，并且对时间精度要求低的场合。

独立看门狗只适用于系统死机的情况，如果某个程序异常，但系统仍能正常喂狗，此时独立看门狗时不会起作用的。

如果需要检测某个程序段是否正常，使用窗口看门口狗，后续会单独讲解。

#### 初始化配置

1. 配置PA0为GPIO输入模式，上拉。作为后面的按键检测
2. IWDG 使能，配置时钟分频和重装载值
![图 2](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/IWDG%E9%85%8D%E7%BD%AE.png)  
IWDG的超时时间 Tout = (4*2^prv) / LSI * rlv (s) prv是预分频器寄存器的值，rlv是重装载寄存器的值
根据时钟图分析
![图 3](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/IWDG%E6%97%B6%E9%92%9F.png)  
LSI 为 25 KHz，当 prv 取 IWDG_ PRESCALER_64 ，rlv 取 500 时，Tout=64/32*500=1s。

#### 程序编写
**主要API**
- `HAL_IWDG_Refresh(&hiwdg)`
刷新看门狗（喂狗）
**主程序**
看门狗不需要额外初始化，上电即运行，所以要注意一点，如果系统的初始化时间过长，应该及时喂狗。
建议使用定时器定时喂狗，且最先初始化定时器
```
void setup() {
    uart_init();
    printf("\n\r***** IWDG Test Start *****\n\r");
}

void loop()
{
	printf("\n\r Refreshes the IWDG !!!\n\r");
	if(HAL_GPIO_ReadPin(KEY_GPIO_Port, KEY_Pin) != 0){
	    HAL_IWDG_Refresh(&hiwdg);
	}
	delay_ms(800);

}
```
主程序不断检测按键电平值，由于按键默认上拉，所以系统会每 800 ms喂狗一次（超时溢出为 1 秒），此时系统正常
当我们按下按键不放时，程序停止喂狗，会看到系统会1s重启一次（根据打印的数据查看系统状态）


### WWDG

窗口看门狗跟独立看门狗一样，也是一个递减计数器不断的往下递减计数，当减到一个固定值 0x3F 时还不喂狗的话，产生复位，这个值叫窗口的下限，是固定的值，不能改变。

窗口看门狗之所以称为窗口，就是因为其喂狗时间是在一个有上下限的范围内（窗口上限值~下限值0x3F），在这个范围内才可以喂狗，可以通过设定相关寄存器，设定其上限时间（但是下限是固定的0x3F）

![图 4](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/pic_1616832615030.png)  

图中：
- 数字 1 处为计数器的初始值（重装载值）
- 2 是我们设置的窗口上限值（只能取低7为值，也就是最大值为 127）
- 3 是下窗口值(0x3F, 那么W[]最小为64)

当窗口看门狗计数器的值只有处在 2 和3 之间(上窗口和下窗口之间)才可以喂狗，其余时间喂狗都时异常。

窗口看门狗还可以使能提前唤醒中断，如果系统出现问题，喂狗函数没有生效，那么在计数器由减到0x40  (0x3f+1) 的时候，便会先进入提前唤醒中断，之后才会复位，你也可以在该中断里面喂狗（不建议在中断里喂狗，不然效果和独立看门狗类似，无意义）

窗口看门狗的超时公式如下：
`Twwdg=(4096× 2^WDGTB× (T[5:0]+1)) /Fpclk1;`
其中：
- Twwdg： WWDG 超时时间（单位为 ms）(看门狗的计数周期)
- Fpclk1： APB1 的时钟频率（单位为 Khz）注意看门狗时钟靠在PCLK1下，一般为主时钟一半。
- WDGTB： WWDG 的预分频系数（系数范围[0-3],2^WDGTB = 分频值）
- T[5:0]：窗口看门狗的计数器低 6 位（0-64）

根据前面所述，你的 W[\]只能取 64-127，则计数器T[]的值范围为 0-63。假设 Fpclk1=42Mhz，分频值为8，W[] = 127（T[] = 63）,则看门狗计数周期：
T = 4096\*8\*[63+1]/42000 = 50ms

###  初始化配置

1. 开启看门狗中断、配置参数（超时时间 = 4096\*8\*[63+1]/42000 = 50ms）
2. 开启提前唤醒中断
3. 开启中断
![图 5](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/WWDG%E4%B8%AD%E6%96%AD%E9%85%8D%E7%BD%AE.png)  


### 程序编写

**主要API**
- `HAL_WWDG_Refresh()` 
看门狗喂狗
- `HAL_WWDG_EarlyWakeupCallback()`
看门狗提前唤醒中断

**主程序**

```
void HAL_WWDG_EarlyWakeupCallback(WWDG_HandleTypeDef* hwwdg)
{
	HAL_WWDG_Refresh(hwwdg);
	printf("\n\rWWDG well!\n\r");
}

void setup() {
    uart_init();
    printf("\n\rWWDG Test Start\n\r");
}

void loop()
{

}
```
我们在唤醒中断里不断喂狗（实际使用时不建议在中断里放喂狗函数，这里应放置整个系统故障的 “临终遗嘱”）。
通过串口助手的时间戳显示，两条信息 `WWDG well!` 之间的时间约为 50 ms。如果注释掉喂狗函数，系统就会不断重启。

### 待机唤醒

STM32 的低功耗模式有 3 种：
- 1)睡眠模式（CM3 内核停止，外设仍然运行）
- 2)停止模式（所有时钟都停止）
- 3)待机模式（1.8V 内核电源关闭）

在运行模式下，我们也可以通过降低系统时钟关闭 APB 和 AHB 总线上未被使用的外设的时钟来降低功耗。
在这三种低功耗模式中，最低功耗的是待机模式。停机模式是次低功耗的。最后就是睡眠模式了。
这里将对 STM32 的最低功耗模式-待机模式做介绍

STM32 进入及退出待机模式的条件
![图 6](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/进入及退出待机模式的条件.png)  

我们有使用WKUP 引脚上的上升沿 方式退出待机模式。从待机唤醒后，除了电源控制/状态寄存器(PWR_CSR)， 所有寄存器被复位。从待机模式唤醒后的代码执行等同于复位后的执行(采样启动模式引脚，读取复位向量等)。电源控制/状态寄存器(PWR_CSR)将会指示内核由待机状态退出。

#### 初始化配置

配置PA0（WKUP 引脚）为输入下拉。

### 程序编写

**主要API**
- `__HAL_RCC_PWR_CLK_ENABLE() `
使能 PWR 时钟
- `HAL_PWR_EnableWakeUpPin() `
设置 WKUP 用于唤醒
- `HAL_PWR_EnterSTANDBYMode()`
设置 SLEEPDEEP 位，设置 PDDS 位，执行 WFI 指令，进入待机模式。
- `__HAL_PWR_CLEAR_FLAG(PWR_FLAG_WU)`
清除Wake_UP标志

**主程序**
```
void Sys_Enter_Standby(void){
	__HAL_RCC_PWR_CLK_ENABLE();		//使能PWR时钟

	__HAL_PWR_CLEAR_FLAG(PWR_FLAG_WU);		//清除Wake_UP标志
	HAL_PWR_EnableWakeUpPin(PWR_WAKEUP_PIN1);	//设置WAKEUP用于唤醒
	HAL_PWR_EnterSTANDBYMode();		//进入待机模式
}

void setup() {
    uart_init();
    printf("\n\rWWDG Test Start\n\r");
}

void loop()
{
	printf("Time: 3\r\n");
	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_13,GPIO_PIN_RESET);
	HAL_Delay(1000);

	printf("Time: 2\r\n");
	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_13,GPIO_PIN_SET);
	HAL_Delay(1000);

	printf("Time: 1\r\n");
	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_13,GPIO_PIN_RESET);
	HAL_Delay(1000);


	printf("Entered Standby Mode...Please press KEY_UP to wakeup system!\r\n");
	Sys_Enter_Standby();
}
```
PA0 按键用来唤醒待机模式，并使用串口1打印相关调试信息
系统运行时倒计时，3秒钟后进入待机模式。当 PA0 接高电平时，待机模式被唤醒，系统重新运行，重新倒计时。

>低功耗模式下载 Debug 需要 reset 按键手动复位
## ADC

### ADC时钟

挂靠在 PCLK2（APB2时钟，最大84HHz）下。分频因子可配置2/4/6/8分频
ADC转换周期：

```
T = 采样时间(周期) + 12.5个周期，其中1周期为1/ADCCLK
```

例如，当 ADCCLK=14Mhz 的时候，并设置 1.5 个周期的采样时间，则得到： Tcovn=1.5+12.5=14 个周期=1us。

根据芯片数据手册，电气特性（Electrical characteristics）-> 操作条件（Operating conditions）所述：
![图 4](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/ADC%E6%97%B6%E9%92%9F.png)  

由 表6.3.1 可知，ADC的最大采样速率（转换速率）与VDDA有关，当VDDA低于2.4V时，转换速率最大只有1.2Msps（million samples per second）；而当VDDA高于2.4V时，可达2.4Msps，即每秒一百二十万次转换。

无论是1.2Msps还是2.4Msps，都是相对于12位分辨率来说的，即表14中给出的是最高分辨率（12bit）下的最大转换速率。STM32F4系列MCU支持12位、10位、8位和6位可编程分辨率，更低的分辨率可以缩短转换周期。因此采用降低分辨率的方法还可以进一步获得更大的转换速率。

由 表6.3.20 可知，ADC的最大时钟频率在VDDA低于2.4V时为18MHz，VDDA高于2.4V时为36MHz。

对于12位分辨率来说，转换周期为12个ADC周期，采样时间可编程的最小值为3个ADC周期，即12位分辨率的最少转换周期数为15个ADC周期。

因此，当VDDA低于2.4V时12位分辨率的最大转换速率为 18/15 Msps，即上面提到的1.2Msps。当VDDA高于2.4V时12位分辨率的最大转换速率为 36/15 Msps，即上面提到的2.4 Msps。

为了保证ADC转换结果的准确性，ADC的时钟最好不超过14M。（有的stm32单片机最高只支持1MHz转换速率）

**参考链接**
- [STM32F4系列ADC最大转换速率及操作条件（以STM32F407ZGT6为例）](https://blog.csdn.net/weixin_44567318/article/details/114449108)
### ADC的转换模式 (重要，请务必看懂)**

1 单次转换模式：ADC只执行一次转换，转换完成后，必须再手动开启

2 连续转换模式：转换结束之后马上开始新的转换，每次转换结束，ADC的值会被刷新，所以需要及时读出数据；

3 扫描模式：ADC扫描被规则通道和注入通道选中的所有通道，在每个组的每个通道上执行单次转换。在每个转换结束时，这一组的下一个通道被自动转换。

4 间断模式：触发一次，转换一个通道，在触发，在转换。在所选转换通道循环，由触发信号启动新一轮的转换，直到转换完成为止。

扫描模式简单的说是一次对所有所选中的通道进行转换，比如开了ch0，ch1，ch4，ch5。  ch0转换完以后就会自动转换通道1,4,5直到转换完这个过程不能被打断。如果开启了连续转换模式，则会在转换完ch5之后开始新一轮的转换。

间断模式，可以说是对扫描模式的一种补充。它可以把0,1,4,5这四个通道进行分组。可以分成0,1一组，4,5一组。也可以每个通道单独配置为一组。这样每一组转换之前都需要先触发一次。

### 单通道、多通道配置

**ADC单通道：**

只进行一次ADC转换：配置为“单次转换模式”，扫描模式关闭。ADC通道转换一次后，就停止转换。等待再次使能后才会重新转换

进行连续ADC转换：配置为“连续转换模式”，扫描模式关闭。ADC通道转换一次后，接着进行下一次转换，不断连续。

**ADC多通道：**

只进行一次ADC转换：配置为“单次转换模式”，扫描模式使能。ADC的多个通道，按照配置的顺序依次转换一次后，就停止转换。等待再次使能后才会重新转换

进行连续ADC转换：配置为“连续转换模式”，扫描模式使能。ADC的多个通道，按照配置的顺序依次转换一次后，接着进行下一次转换，不断连续。

也就是：多通道必须使能扫描模式

### 数据左对齐或右对齐

因为ADC得到的数据是12位精度的，但是数据存储在 16 位数据寄存器中，所以ADC的存储结果可以分为左对齐或右对齐方式（12位）

![图 1](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/ADC%E6%95%B0%E6%8D%AE%E5%B7%A6%E5%AF%B9%E9%BD%90%E5%8F%B3%E5%AF%B9%E9%BD%90.png)  

### ADC输入通道
从ADCx_INT0-ADCx_INT15 对应三个ADC的16个外部通道，进行模拟信号转换 此外，还有两个内部通道：温度检测或者内部电压检测
选择对应通道之后，便会选择对应GPIO引脚，相关的引脚定义和描述可在开发板的数据手册里找

#### 注入通道，规则通道

我们看到，在选择了ADC的相关通道引脚之后，在模拟至数字转换器中有两个通道，注入通道，规则通道，
规则通道至多16个，注入通道至多4个

**规则通道：**
规则通道相当于你正常运行的程序，看它的名字就可以知道，很规矩，就是正常执行程序
**注入通道：**
注入通道可以打断规则通道，听它的名字就知道不安分，如果在规则通道转换过程中，有注入通道进行转换，那么就要先转换完注入通道，等注入通道转换完成后，再回到规则通道的转换流程

无法连续转换注入通道。连续模式下唯一的例外情况是，注入通道配置为在规则通道之后自动转换（使用 JAUTO 位），请参见自动注入一节

![图 2](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/%20ADC%E6%B3%A8%E5%85%A5%E9%80%9A%E9%81%93%E4%B8%8E%E8%A7%84%E5%88%99%E9%80%9A%E9%81%93.png)  

### 中断
中断触发条件有三个，规则通道转换结束，注入通道转换结束，或者模拟看门狗状态位被设置时都能产生中断，

![图 3](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/ADC%E4%B8%AD%E6%96%AD.png)  

转换结束中断就是正常的ADC完成一次转换，进入中断，这个很好理解

**模拟看门狗中断**
当被ADC转换的模拟电压值低于低阈值或高于高阈值时，便会产生中断。阈值的高低值由ADC_LTR和ADC_HTR配置
模拟看门狗，听他的名字就知道，在ADC的应用中是为了防止读取到的电压值超量程或者低于量程

### DMA
同时ADC还支持DMA触发，规则和注入通道转换结束后会产生DMA请求，用于将转换好的数据传输到内存。

注意，只有部分ADC组可以产生DMA请求

因为涉及到DMA传输，所以这里我们不再详细介绍，之后几节会更新DMA,一般我们在使用ADC 的时候都会开启DMA 传输。

### 单通道单次转换
#### 初始化配置

1. 配置引脚为ADC1_IN1
2. 使能通道1
3. 匹配ADC参数
这里根据上描述设置ADC时钟为6分频，14MHz。其余配置保持默认即可，也不要修改
![图 5](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/ADC-%E5%8D%95%E9%80%9A%E9%81%93%E9%85%8D%E7%BD%AE.png)  

**参数讲解**
- ADC_Mode_Independent  这里设置为独立模式
独立模式模式下，双ADC不能同步，每个ADC接口独立工作。所以如果不需要ADC同步或者只是用了一个ADC的时候，应该设成独立模式，多个ADC同时使用时会有其他模式，如双重ADC同步模式，两个ADC同时采集一个或多个通道，可以提高采样率
- Data Alignment (数据对齐方式): 右对齐/左对齐
- Scan Conversion Mode( 扫描模式 ) ：   DISABLE
如果只是用了一个通道的话，DISABLE就可以了(也只能DISABLE)，如果使用了多个通道的话，会自动设置为ENABLE。 就是是否开启扫描模式
- Continuous Conversion Mode(连续转换模式)    DISABLE
设置为ENABLE，即连续转换。如果设置为DISABLE，则是单次转换。两者的区别在于连续转换直到所有的数据转换完成后才停止转换，而单次转换则只转换一次数据就停止，要再次触发转换才可以进行转换
- Discontinuous Conversion Mode(间断模式)    DISABLE
多通道模式下使用
- Enable Regular Conversions (启用常规转换模式)    ENABLE
使能 否则无发进行下方配置
- Number OF Conversion(转换通道数)    1
用到几个通道就设置为几，数字大于1即多个通道会自动使能上面的扫描模式
- Extenal Trigger Conversion Source (外部触发转换源)
设定ADC的触发方式，外部事件触发时使用。详见 `中断与事件` 章节
- Regular Conversion launched by software 规则的软件触发 调用函数触发即可
上述时触发ADC，这个时触发ADC的规则处理，如：启动下一次转换
- Rank          转换顺序
这个只修改通道采样时间即可 默认为1.5个周期。其余配置在多通道再讲解。


**HAL库中关于个参数的讲解：**

```
   ScanConvMode;                        / *！<配置常规组和注入组的序列。
                                            此参数可以与参数'DiscontinuousConvMode'关联，以将主序列细分为连续的部分。
                                            如果禁用：转换以单模式执行（转换了一个通道，在等级1中定义了一个通道）。
                                                    参数“ NbrOfConversion”和“ InjectedNbrOfConversion”将被丢弃（等效于设置为1）。
                                            如果启用：转换以顺序模式执行（由“ NbrOfConversion” /“ InjectedNbrOfConversion”定义的多个等级以及每个通道等级）。
                                                    扫描方向朝上：从等级1到等级'n'。
                                            该参数可以设置为ENABLE或DISABLE * /
  EOCSelection;                         / *！<指定通过轮询和中断将什么EOC（转换结束）标志用于转换：每个等级或完整序列的转换结束。
                                            此参数可以是@ref ADC_EOCSelection的值。
                                            注意：对于注入组，仅在序列末尾才引发转换结束（flag＆IT）。
                                                因此，如果将转换结束设置为每次转换结束，则不应将插入组与中断一起使用（HAL_ADCEx_InjectedStart_IT）
                                                或轮询（HAL_ADCEx_InjectedStart和HAL_ADCEx_InjectedPollForConversion）。顺便说一句，轮询仍然是可能的，因为驱动程序将使用估计的时间来完成注入转换。
                                            注意：如果要使用溢出功能，请在参数“ EOCSelection”设置为每次转换结束时，在“中断”模式（函数HAL_ADC_Start_IT（））中使用ADC，或者在“通过DMA传输”模式（函数HAL_ADC_Start_DMA（））中使用ADC。
                                                如果要绕过超限功能，则必须在参数“ EOCSelection”的“轮询”或“中断”模式下使用ADC，并且必须将其设置为序列的结尾* /
  ContinuousConvMode;                   / *！<指定对于常规组是在单模式（一次转换）还是连续模式下进行转换，
                                              在发生选定的触发器（软件启动或外部触发器）之后。
  NbrOfConversion;                      / *！<指定将在常规组音序器中转换的等级数。
                                              要使用常规组音序器并转换几个等级，必须启用参数“ ScanConvMode”。
                                              此参数必须是介于Min_Data = 1和Max_Data = 16之间的数字。* /
  DiscontinuousConvMode;                / *！<指定是否以完全序列/不连续序列（主序列细分为连续部分）执行常规组的转换序列。
                                              仅当启用了定序器（参数“ ScanConvMode”）时，才使用不连续模式。如果禁用了音序器，则此参数将被丢弃。
                                              仅当禁用连续模式时，才能启用非连续模式。如果启用了连续模式，则此参数设置将被放弃。
                                              该参数可以设置为ENABLE或DISABLE。 * /
 NbrOfDiscConversion;                   / *！<指定不连续转换的数量，在该不连续转换中将细分常规组的主要序列（参数NbrOfConversion）。
                                              如果禁用了参数“ DiscontinuousConvMode”，则该参数将被丢弃。
                                              此参数必须是Min_Data = 1和Max_Data = 8之间的数字。* /
  DMAContinuousRequests；               / *！<指定是否以单发模式执行DMA请求（当达到转换次数时DMA传输停止）
                                             或在连续模式下（无限制的DMA传输，无论转换次数如何）。
                                             注意：在连续模式下，必须在循环模式下配置DMA。否则，当达到DMA缓冲区最大指针时，将触发溢出。
                                             注意：在常规组和注入组上都没有进行任何转换时（禁用ADC，或启用ADC而没有连续模式或可能触发转换的外部触发），必须修改此参数。
                                             该参数可以设置为ENABLE或DISABLE。 * /
```
#### 程序编写

**主要API**

- `HAL_ADC_Start();`     
启动ADC转换
- `__HAL_ADC_GET_FLAG()`
查询标志位，判断ADC状态
- `HAL_ADC_GetValue()`   
获取ADC值
- `HAL_ADC_PollForConversion(&hadc1, 50)`
等待转换完成，第二个参数表示超时时间，单位ms.
- `HAL_ADC_GetState(&hadc1)`
换取ADC状态，HAL_ADC_STATE_REG_EOC表示转换完成标志位，需配合 `HAL_ADC_PollForConversion()`使用。


**主程序**
```
/****************** adc.c **********************/

// 开始ADC转换
void adc1_start()
{
	HAL_ADC_Start(&hadc1);     //启动ADC转换
	//HAL_ADC_PollForConversion(&hadc1, 50);   //等待转换完成，50为最大等待时间，单位为ms
}

// 查询ADC转换是否完成
// 返回1 完成
uint8_t adc1_ready()
{
	if(__HAL_ADC_GET_FLAG(&hadc1,ADC_FLAG_EOC))
    //if(HAL_IS_BIT_SET(HAL_ADC_GetState(&hadc1), HAL_ADC_STATE_REG_EOC))
		return 1;
	else
		return 0;
}

// 获取ADC值
uint16_t get_adc()
{
	return HAL_ADC_GetValue(&hadc1);   //获取ADC值
}

/****************** main.c **********************/

void setup() {
    uart_init();
    printf("Test Start\n\r");
    adc1_start();  // 启动转换
}

void loop()
{
	if(adc1_ready()){
		printf("ADC = %d\n\r",get_adc());
		adc1_start();
	}
	printf("idle\n\r");
	delay_ms(1000);
}

```
系统初始化组后，启动ADC转换。主程序不断查询ADC转换状态，如果就绪，就打印ADC值并重新启动下一次转换。

网上很多教程会使用函数
```
HAL_IS_BIT_SET(HAL_ADC_GetState(&hadc1), HAL_ADC_STATE_REG_EOC)
```
查询ADC状态，但必须配合函数
```
HAL_ADC_PollForConversion(&hadc1, 50);
```
使用，这种方案使用阻塞查询，也就是启动转换后，会等待转换完成，会阻塞程序，不推荐使用。详细使用参考上述程序对应注释部分。

网上教程还会有校准函数
```
HAL_ADCEx_Calibration_Start(&hadc2);    //AD校准
```
 这个依芯片而异，这里使用的 F401 就没有。
**开启ADC 3种模式 ( 轮询模式 中断模式 DMA模式 ）**

- HAL_ADC_Start(&hadcx);       //轮询模式开启ADC
- HAL_ADC_Start_IT(&hadcx);       //中断轮询模式开启ADC
- HAL_ADC_Start_DMA(&hadcx)；       //DMA模式开启ADC

**关闭ADC 3种模式 ( 轮询模式 中断模式 DMA模式 ）**

- HAL_ADC_Stop()
- HAL_ADC_Stop_IT()
- HAL_ADC_Stop_DMA()

### 单通道单次转换-中断方式

#### 初始化配置
紧跟上文的配置，只需要再使能中断即可。
![图 6](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/ADC-%E5%8D%95%E9%80%9A%E9%81%93%E4%B8%AD%E6%96%AD%E9%85%8D%E7%BD%AE.png)  

#### 程序编写

**主要API**
-  `HAL_ADC_Start_IT()  `    
中断模式下开启ADC转换
- `HAL_ADC_Stop_IT()`
停止准换
- `HAL_ADC_ConvCpltCallback()`
中断模式下转换完成后回调，DMA模式下DMA传输完成后也会调用

**主程序**
```
/****************** adc.c **********************/
void adc1_start()
{
	HAL_ADC_Start_IT(&hadc1); //定时器中断里面开启ADC中断转换，1ms开启一次采集
}

void HAL_ADC_ConvCpltCallback(ADC_HandleTypeDef* hadc)    //ADC转换完成回调
{
    //HAL_ADC_Stop_IT(&hadc1);    //关闭定时器
	printf("ADC1 Reading : %d \r\n",HAL_ADC_GetValue(&hadc1));
    HAL_ADC_Start_IT(&hadc1);
    //HAL_TIM_Base_Start_IT(&htim3);   //开启定时器
}
/****************** main.c **********************/
void setup() {
    uart_init();
    printf("Test Start\n\r");
    adc1_start();
}
```
系统初始化组后开启ADC中断转换，中断回调函数里读取当前ADC数据并打印出来。其中的开关全局中断为可选项，是为了避免其它中断打断这里的ADC数据读取，这里只有一个中断，并未开启。

### 多通道单次转换

STM32的多通道是没有多通道值存储寄存器的，也就是说在普通轮询模式下，我们只能通过函数
```
HAL_ADC_GetValue(&hadc1)
```
读取一个通道的值，并不能一次性读取多个通道，STM32每次只能转换一个通道。 
随意我们在开启多通道时，读取数值的基本方法就是转换一次，读取一次，在开启转换，在读取。

初始化配置中能帮我们解决的只有：不用手动切换通道以及切换通道的重新初始化配置。
#### 初始化配置

在**单通道单次转换**配置基础上修改：
1. 使能多个通道
2. 参数配置
![图 1](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/ADC-%E5%A4%9A%E9%80%9A%E9%81%93%E9%85%8D%E7%BD%AE.png)  


**参数详解**
- Scan Conversion Mode( 扫描模式 ) ： ENABLE
如果使用了多个通道的话，会自动设置为ENABLE。
- Discontinuous Conversion Mode(间断模式)    ENABLE
根据数据手册描述：
    ```
    在不使用 DMA 的情况下管理转换序列
    如果转换过程足够慢，则可使用软件来处理转换序列。在这种情况下，必须将 ADC_CR2 寄
    存器中的 EOCS 位置 1，才能使 EOC 状态位在每次转换结束时置 1，而不仅是在序列结束
    时置 1。当 EOCS = 1 时，会自动使能溢出检测。因此，每当转换结束时， EOC 都会置 1，
    并且可以读取 ADC_DR 寄存器。溢出管理与使用 DMA 时的管理相同。
    要在 EOCS 位置 1 时将 ADC 从 OVR 状态中恢复，请按以下步骤操作：
    1. 将 ADC_SR 寄存器中的 ADC OVR 位清零
    2. 触发 ADC 以开始转换。
    ```
大致意思就是，要是没有使用DMA，那么管理多个通道（转化序列）转换就需要使用软件处理，需开启间断模式（EOCS = 1），每次转换后，转换完成标志为（EOC = 1），和单次一样，通过判断这个标志位读取ADC值，之后再手动开启转换，系统就会自动开始转换下一个组。依次循环往复（最后一组转换完成会自动跳转到第一组）如果我们不开启间断模式，那么系统就会一次性转换所有组，那么我们在读取时也就只能读到最后一个通道的值。
- Number of Discontinuous Conversion 1
定义序列长度，距离分组模式。
比如我们一共有  1、3、4、5、6  通道
这里设为1，则每个通道为一组，那么每一个通道转换完成后都会置位 EOC。
如果设备2，这、则 1、3一组，4、5一组、6一组，那么没两个通道转换完成后，才会置位EOC，那么其实我们也就只能读取到每组最后一个通道的ADC值。
- Number OF Conversion(转换通道数)    3
用到几个通道就设置为几，数字大于1即多个通道会自动使能上面的扫描模式
- Rank          转换顺序
这里是设置通道的转换顺序，Ranx 系统是按照x的顺序开始转换的。
例如如果这里我们 Rank1设为通道3，Rank2设为通道4，依次设置，则转换顺序为 3、4、5、6、1.
如果我们的序列长度为2，则3、4一组，5、6一组。系统会先转换3、4通道，按序执行。而不是1通道先转换。

这里我们序列长度设为1，转换顺序为1、3、5.则按一般顺序转换，没完成一个通道，EOC置位1次。

#### 程序编写

```
/******************* adc.c ***************/
uint16_t adcBuf[3] = {0};

void adc1_start()
{
	HAL_ADC_Start(&hadc1); // 开启ADC转换
}

// ADC 通道轮询  扫描模式+间断模式
// 注意必须在初始化开启一次转换，否则数据会错位。应为ADC是按顺序轮询的
void adc1_scan()
{
	static uint8_t ch = 0;

	if(__HAL_ADC_GET_FLAG(&hadc1,ADC_FLAG_EOC)){  // 获取一次ADC值
		adcBuf[ch]=HAL_ADC_GetValue(&hadc1);      // 保存值
		HAL_ADC_Start(&hadc1);                    // 启动下一个通道的转换

		if(++ch>=3)
			ch = 0;
	}

}

// 获取ADC值
// ch 通道索引  注意这个值并不直接对应实际通道号，仅仅是数组的索引，对应通道存入数组的顺序
// 例如本次共开启了1、3、4通道，并在 adc1_scan() 中依次存入了 adcBuf 中
// 则 ch=1 对应通道1；ch=2 对应通道3
uint16_t adc1_get(uint8_t ch)
{
	return adcBuf[ch];
}

/********************** main.c *******************/
void setup() {
    uart_init();
    printf("Test Start\n\r");
    adc1_start();
}

void loop()
{
	adc1_scan();
	for(int i = 0;i<3;i++){
		printf("adc1_CH%d = %d\n\r",i,adc1_get(i));
	}
	delay_ms(1000);
}
```

初始化需先开启依次转换，不然我们的 `adc1_scan()` 中判断会不通过。
主程序不断调用 `adc1_scan()` 扫描每个通道，并将值存储到数组，在一次性读取打印出来。

在 `adc1_scan()` 我们无需关心通道的配置，系统会按照我们初始化配置是的顺序自动轮询转换。
不然我们就需要像下面这要，手动切换通道并初始化：
```
void adc_start_conversion(uint32_t ch)
{
	ADC_ChannelConfTypeDef sConfig ;

	sConfig.Channel = ch;
	sConfig.Rank = ADC_REGULAR_RANK_1;                // 1个序列，序列1
	sConfig.SamplingTime = ADC_SAMPLINGTIME_COMMON_1;
	HAL_ADC_ConfigChannel(&hadc1, &sConfig);          //通道配置
    HAL_ADC_Start(&hadc1);                            // 启动ADC转换
}
```

这里还要解答一个疑问点：
既然 `Number of Discontinuous Conversion`序列长度大于1时，我们就无法读取部分通道的值，那么这个设置是做什么的呢？
数据手册中也有解答：
```
ADC 在转换一个或多个通道时不是每次都读取数据的情况下，这可能会很有用（例如，存在
模拟看门狗时）。为此，必须禁止 DMA (DMA = 0) 并且仅在序列结束 (EOCS = 0) 时才将
EOC 位置 1。在此配置中，溢出检测已禁止。
```
意思就是并不是每个ADC通道的值都需要我们读取，有的可能转换完成后会用于其它目的（模拟看门狗）。比如 1、3通道是用于模拟看门狗的，那么我们的实际程序是不需要知道这两个通道的值，系统需要，我们也就不需要读取。这是序列为2后，系统会自动在每个通道转换完成后获取通道值并用于模拟看门狗比较。在这一组转换完成后，EOC置1，告诉我们这两个通过转换完成。

同样连续转换模式 `Continuous Conversion Mode` 如果再没开启DMA情况下，我们手动读取数据，也只能读取到最后一个通道的转换结果。所以一般模式下，不是能该选项。

### DMA转换-连续模式-circle

DMA转换的好处就是无需手动获取查询转换状态吗，再手动保存通道值，DMA会自动将数据放进数组中。

#### 初始化配置

在上面**多通道单次转换**基础上修改：
1. 添加一路DMA，配置参数：循环模式，字节为 World（必须为字，因为ADC变量是32位的）
2. 配置ADC参数

这里DMA中断时强制开启的，无法关闭。DMA循环模式会一致更新数据到数组中，如果改为Normal模式，则需要在 ADC 转换完成回调函数中手动启动下一次 DMA 转换。（DMA中断最终会调用ADC的所有回调函数，即使没启动ADC中断）
![图 2](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/DMA1%E9%85%8D%E7%BD%AE.png)  

#### 程序编写

**主要API**
- HAL_ADC_Start_DMA(&hadc1, ();  
开启ADC DMA转换
```
/******************* adc.c ***************/
uint32_t adcBuf[3] = {0};

void adc1_start()
{
	HAL_ADC_Start_DMA(&hadc1, (uint32_t*)&adcBuf, 3);  // 开启ADC DMA转换
}

// 获取ADC值
// ch 通道索引  注意这个值并不直接对应实际通道号，仅仅是数组的索引，对应通道存入数组的顺序
// 例如本次共开启了1、3、4通道，并在 adc1_scan() 中依次存入了 adcBuf 中
// 则 ch=1 对应通道1；ch=2 对应通道3
uint16_t adc1_get(uint8_t ch)
{
	return adcBuf[ch];
}

/********************** main.c *******************/
void setup() {
    uart_init();
    printf("Test Start\n\r");
    adc1_start();
}

void loop()
{
	for(int i = 0;i<3;i++){
		printf("adc1_CH%d = %d\n\r",i,adc1_get(i));
	}
	delay_ms(1000);
}

```

系统初始化开启 DMA 传输即可，后面不用再管了，主程序值负责读取即可。
如果你选择了DMA 的  Normal 模式，则需要再中断里手动开启下一次转换。
### DMA-不连续模式-circle
该模式和 DMA ，DMA在一轮转换完成后会停止，需要手动再次开启下一轮转换。

#### 初始化配置

只在上述配置中将 Continuous Conversion Mode 改为 Disable。不是使能连续转换。

![图 3](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/DMA2%E9%85%8D%E7%BD%AE.png)  


#### 程序编写

```
/******************* adc.c ***************/
uint32_t adcBuf[3] = {0};

void adc1_start()
{
	HAL_ADC_Start_DMA(&hadc1, (uint32_t*)&adcBuf, 3);  // 开启ADC DMA转换
}

// 获取ADC值
// ch 通道索引  注意这个值并不直接对应实际通道号，仅仅是数组的索引，对应通道存入数组的顺序
// 例如本次共开启了1、3、4通道，并在 adc1_scan() 中依次存入了 adcBuf 中
// 则 ch=1 对应通道1；ch=2 对应通道3
uint16_t adc1_get(uint8_t ch)
{
	return adcBuf[ch];
}

// 在 Readme 参数配置下，不论是否开启  ADC 中断，都会调用该回调函数
// 此时更多的是通知所有通道转换完成，并无实际操作
void HAL_ADC_ConvCpltCallback(ADC_HandleTypeDef* AdcHandle)
{
	HAL_ADC_Start_DMA(&hadc1, (uint32_t*)&adcBuf, 3);  // 开启ADC DMA转换
}


/********************** main.c *******************/
void setup() {
    uart_init();
    printf("Test Start\n\r");
    adc1_start();
}

void loop()
{
	for(int i = 0;i<3;i++){
		printf("adc1_CH%d = %d\n\r",i,adc1_get(i));
	}
	delay_ms(1000);
}

```
系统初始化开启 DMA 传输即可，中断里手动开启下一次转换。
主程序读取ADC值并打印。

### DMA-不连续模式-Normal

该模式和 DMA-不连续模式-circle 很像。
这两者差别目前并不清楚。推测时应用场景不同，连续模式下，所有通道转换完成才会触发中断，不连续模式下，每个通道转换完成触发依次中断。

#### 初始化配置

只在上述配置中将 Continuous Conversion Mode 改为 Disable。DMA 改为 Normal 模式
>注意， Continuous Conversion Mode 必须改为 Disable。否则数组中的数据会发生偏移，比如，通道1数据第一次会存储在 adcBuf[0]，下一次转换再读取，数据就会存储再adcBuf[1]，其它通道数据依次移动，然后再循环往复。

从实验结果来看，无论是不是能连续模式，每次DMA转换都会转换所有的通道，所以连续在这的意义即使是数据偏移？？？

参考链接：[STM32使用HAL库的ADC多通道数据采集（DMA+非DMA方式）+ 读取内部传感器温度](https://blog.csdn.net/qq153471503/article/details/108123019)
![图 4](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/DMA4%E9%85%8D%E7%BD%AE.png)  

#### 程序编写

```
/******************* adc.c ***************/
uint32_t adcBuf[3] = {0};

void adc1_start()
{
	HAL_ADC_Start_DMA(&hadc1, (uint32_t*)&adcBuf, 3);  // 开启ADC DMA转换
}

// 获取ADC值
// ch 通道索引  注意这个值并不直接对应实际通道号，仅仅是数组的索引，对应通道存入数组的顺序
// 例如本次共开启了1、3、4通道，并在 adc1_scan() 中依次存入了 adcBuf 中
// 则 ch=1 对应通道1；ch=2 对应通道3
uint16_t adc1_get(uint8_t ch)
{
	return adcBuf[ch];
}

// 在 Readme 参数配置下，不论是否开启  ADC 中断，都会调用该回调函数
// 此时更多的是通知所有通道转换完成，并无实际操作
void HAL_ADC_ConvCpltCallback(ADC_HandleTypeDef* AdcHandle)
{
	HAL_ADC_Start_DMA(&hadc1, (uint32_t*)&adcBuf, 3);  // 开启ADC DMA转换
}


/********************** main.c *******************/
void setup() {
    uart_init();
    printf("Test Start\n\r");
    adc1_start();
}

void loop()
{
	for(int i = 0;i<3;i++){
		printf("adc1_CH%d = %d\n\r",i,adc1_get(i));
	}
	delay_ms(1000);
}

```
系统初始化开启 DMA 传输即可，中断里手动开启下一次转换。
主程序读取ADC值并打印。

### 其它

关于 ADC在DMA模式下的 `DMA Continuous Request`作用，目前仍无结论。下面是可能有帮助的参考：
- [DMA Continuous Request functionality](https://electronics.stackexchange.com/questions/500827/dma-continuous-request-functionality)
## 内部温度传感器

温度传感器可用于测量器件的环境温度 (TA)。
- 对于 STM32F40x 和 STM32F41x 器件，温度传感器内部连接到 ADC1_IN16 通道，而
ADC1 用于将传感器输出电压转换为数字值

主要特性
- 支持的温度范围： —40 °C 到 125 °C
- 精度： ±1.5 °C

使用以下公式计算温度：
```
温度（单位为 °C） = {(VSENSE — V25) / Avg_Slope} + 25
其中：
— V25 = 25 °C 时的 VSENSE 值。
— Avg_Slope = 温度与 VSENSE 曲线的平均斜率（以 mV/°C 或 μV/°C 表示））
```
VSENSE 位电压值 v。
有关 V25 和 Avg_Slope 实际值的相关信息，请参见数据手册中的电气特性一节。
一般典型值为：V25 = 0.76； Avg_Slope = 2.5mv/℃
注意： 传感器从掉电模式中唤醒需要一个启动时间，启动时间过后其才能正确输出 VSENSE。 ADC 在
上电后同样需要一个启动时间，因此，为尽可能减少延迟间，应同时将 ADON 和 TSVREFE
位置 1。

>温度传感器的输出电压随温度线性变化。由于工艺不同，该线性函数的偏移量取决于各个芯片（芯片之间的温度变化可达 45 °C）。

>内部温度传感器更适用于对温度变量而非绝对温度进行测量的应用情况。如果需要读取精确温度，则应使用外部温度传感器。**

### 初始化配置

在 `多通道单次转换`基础上修改
1. 使能内部温度传感器
2. 配置adc参数：通道数改为4，rank4选择内部温度通道。
![图 5](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/ADC-%E5%86%85%E9%83%A8%E6%B8%A9%E5%BA%A6%E9%85%8D%E7%BD%AE.png)  

### 程序编写

程序和 `多通道单次转换` 一样，将adcBuf改为4就行了。主程序修改一下：
```
void loop()
{
	adc1_scan();
	for(int i = 0;i<4;i++){
		if(i<3){
			printf("adc1_CH%d = %d\n\r",i,adc1_get(i));
		}
		else{
			double temperate = (float)adc1_get(i)*(3.3/4096);
			temperate=(temperate - 0.76)/0.0025+25; //转换为温度值
			printf("temper = %d\n\r",(int)temperate);
		}

	}
	delay_ms(1000);
}
```
单独将通道16的数值转换位温度值并打印输出。我这初始输出温度 31，比环境温度高不少（环境15度左右），可见内部温度如手册所说，不准确。用手触摸芯片，会发现温度在升高。

## DAC

STM32F401 不支持 DAC，暂空。

## IIC（EEPROM）

## IIC DMA

## SPI（Flash）

使用SPI驱动外部 flash 芯片（W25q128）。

### 初始化配置

- 配置PA4引脚位输出，作为片选
- 开启spi，参数默认
![图 6](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/SPI%E9%85%8D%E7%BD%AE.png)  

### 程序编写

**主要API**
- `HAL_SPI_TransmitReceive();`
在阻塞模式下发送和接收大量数据。
- `HAL_SPI_Receive()`
在阻塞模式下接收大量数据。
- `HAL_SPI_Transmit()`
在阻塞模式下发送大量数据。

先编写两个 spi 底层读写接口。共flash函数使用。
```
/******************** spi1.c *******************/

//SPI速度设置函数
//SPI速度=fAPB1/分频系数
//@ref SPI_BaudRate_Prescaler:SPI_BAUDRATEPRESCALER_2~SPI_BAUDRATEPRESCALER_2 256
//fAPB1时钟  般为42Mhz
void SPI1_SetSpeed(uint8_t SPI_BaudRatePrescaler)
{
    assert_param(IS_SPI_BAUDRATE_PRESCALER(SPI_BaudRatePrescaler));//判断有效
    __HAL_SPI_DISABLE(&hspi1);            //关闭SPI
    hspi1.Instance->CR1&=0XFFC7;          //�???3-5清零，用来设置波特率
    hspi1.Instance->CR1|=SPI_BaudRatePrescaler;//设置SPI速度
    __HAL_SPI_ENABLE(&hspi1);             //使能SPI

}

//SPI1 读写个字
//TxData:要写入的字节
//返回:读取到的字节
uint8_t SPI1_ReadWriteByte(uint8_t TxData)
{
	uint8_t Rxdata;
    HAL_SPI_TransmitReceive(&hspi1,&TxData,&Rxdata,1, 100);
 	return Rxdata;          		    //返回收到的数
}
```
然后是 Flash 的驱动函数.
w25qxx.h 主要是一些宏定义和指令表，不详细列出，具体请查看源码，另外定义了片选引脚（位带操作）。
```
/****************** w25qxx.h **********************/

//W25X系列/Q系列芯片列表
#define W25Q80 	0XEF13
#define W25Q16 	0XEF14
#define W25Q32 	0XEF15
#define W25Q64 	0XEF16
#define W25Q128	0XEF17
#define W25Q256 0XEF18

extern u16 W25QXX_TYPE;					//定义W25QXX芯片型号

#define	W25QXX_CS 		PAout(4)  		//W25QXX的片选信号

//////////////////////////////////////////////////////////////////////////////////
//指令表
#define W25X_WriteEnable		0x06
#define W25X_WriteDisable		0x04
#define W25X_ReadStatusReg1		0x05
#define W25X_ReadStatusReg2		0x35
#define W25X_ReadStatusReg3		0x15
#define W25X_WriteStatusReg1    0x01
#define W25X_WriteStatusReg2    0x31
#define W25X_WriteStatusReg3    0x11
#define W25X_ReadData			0x03
...  部分代码省略
```

w25qxx.c 就是主要的 flash 操作函数了。这里也不在=列出，详细请查看源码。
我们直接看主程序：
```
//要写入到 W25Q64 的字符串数组
const u8 TEXT_Buffer[]={"MiniSTM32 SPI TEST"};
#define SIZE sizeof(TEXT_Buffer)

void setup() {

	u8 datatemp[SIZE];
	u32 FLASH_SIZE=128*1024*1024; //FLASH 大小为 128M 字节
    uart_init();
    printf("Test Start\n\r");
	W25QXX_Init();  /* W25Q256-Flash初始化 */
	while(W25QXX_ReadID()!=W25Q128)
	{
		printf("W25Q64 Failed!\n\r");  //
	}
	printf("W25Q64 OK!\n\r");
	W25QXX_Write((u8*)TEXT_Buffer,FLASH_SIZE-100,SIZE);
	W25QXX_Read(datatemp,FLASH_SIZE-100,SIZE);
	printf("The Data Readed Is: ");//提示传送完成
	printf("%s\r\n",datatemp); //显示读到的字符串
}

void loop()
{

	printf("Test Start\n\r");
	delay_ms(1000);
}
```
我们在初始化配置时先检测 flash ID型号，判断是否初始化成功。
随后写入一串字符，之后读取并打印出来。如果打印内容与字符串内容一致，说明 falsh 操作成功。

## SPI DMA

flash读写会占用比较长的时间 ，如果这个SPI上还挂载了其他SPI 器件，如SPI显示屏，就需要通过开启DMA来提升速度了。

###  初始化配置
承接上文 SPI 配置，添加以下几项
1. 开启DMA通道，可以只开一个
2. 配置DMA参数
3. 关闭SPI中断
![图 7](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/SPI_DMA%E9%85%8D%E7%BD%AE.png)  

### 程序编写

**主要API**

- `HAL_SPI_Transmit_DMA()`
使用DMA在非阻塞模式下传输大量数据。
- `HAL_SPI_Receive_DMA()`
使用DMA在非阻塞模式下接收大量数据。
- `HAL_SPI_TransmitReceive_DMA()`
使用DMA在非阻塞模式下发送和接收大量数据。
- `HAL_SPI_GetState()`
返回SPI句柄状态。
- `HAL_SPI_TxCpltCallback()`
SPI发送完成回调函数，DMA中断调��
- `HAL_SPI_RxCpltCallback()`
SPI接收完成回调函数，DMA中断调用

**主程序**
 在 **SPI** 的基础上，修改两个函数：SPI的读与写
 ```
//读取SPI FLASH
//在指定地址开始读取指定长度的数据
//pBuffer:数据存储区
//ReadAddr:开始读取的地址(24bit)
//NumByteToRead:要读取的字节数(最大65535)
void W25QXX_Read(u8* pBuffer,u32 ReadAddr,u16 NumByteToRead)
{

	W25QXX_CS=0;                            //使能器件
    SPI1_ReadWriteByte(W25X_ReadData);      //发送读取命令
    if(W25QXX_TYPE==W25Q256)                //如果是W25Q256的话地址为4字节的，要发送最高8位
    {
        SPI1_ReadWriteByte((u8)((ReadAddr)>>24));
    }
    SPI1_ReadWriteByte((u8)((ReadAddr)>>16));   //发送24bit地址
    SPI1_ReadWriteByte((u8)((ReadAddr)>>8));
    SPI1_ReadWriteByte((u8)ReadAddr);
//  u16 i = 0xFF;
//  for(i=0;i<NumByteToRead;i++)
//	{
//      pBuffer[i]=SPI1_ReadWriteByte(0XFF);    //循环读数
//  }
    // 如果使用普通SPI读写，请注释下面两行行，上面四行取消注释
    HAL_SPI_Receive_DMA(&hspi1,pBuffer,NumByteToRead);
    while (hspi1.State == HAL_SPI_STATE_BUSY_RX);  // 必须加这一行，等待spi读结束
    //u8 i = 0xFF;
    //HAL_SPI_TransmitReceive_DMA(&hspi1,&i,pBuffer,NumByteToRead);  !!! 不能使用该语句写操作，会导致读写错误 !!!
	W25QXX_CS=1;
}
//SPI在一页(0~65535)内写入少于256个字节的数据
//在指定地址开始写入最大256字节的数据
//pBuffer:数据存储区
//WriteAddr:开始写入的地址(24bit)
//NumByteToWrite:要写入的字节数(最大256),该数不应该超过该页的剩余字节数!!!
void W25QXX_Write_Page(u8* pBuffer,u32 WriteAddr,u16 NumByteToWrite)
{
 	u16 i;
    W25QXX_Write_Enable();                  //SET WEL
	W25QXX_CS=0;                            //使能器件
    SPI1_ReadWriteByte(W25X_PageProgram);   //发送写页命令
    if(W25QXX_TYPE==W25Q256)                //如果是W25Q256的话地址为4字节的，要发送最高8位
    {
        SPI1_ReadWriteByte((u8)((WriteAddr)>>24));
    }
    SPI1_ReadWriteByte((u8)((WriteAddr)>>16)); //发送24bit地址
    SPI1_ReadWriteByte((u8)((WriteAddr)>>8));
    SPI1_ReadWriteByte((u8)WriteAddr);
    //for(i=0;i<NumByteToWrite;i++)SPI1_ReadWriteByte(pBuffer[i]);//循环写数
    //HAL_SPI_TransmitReceive_DMA(&hspi1,pBuffer,&i,NumByteToWrite);
    HAL_SPI_Transmit_DMA(&hspi1,pBuffer,NumByteToWrite);
    while (hspi1.State == HAL_SPI_STATE_BUSY_TX);
    W25QXX_CS=1;                            //取消片选
	W25QXX_Wait_Busy();					   //等待写入结束
}

 ```

 这里的 SPI DMA操作实际还是阻塞模式，每次传输完成必须使用 while 检查 falsh状态，才能开启下一次传输，否则可能会导致只一次还未结束，有开始下一轮数据传输（DMA非阻塞，与主程序并行），所以主程序和DMA传输数据直接存在交叉现象，可以添加标志位知识DMA传输状态，但和此处的while效果一样，最终还是需要等待每一次数据传输完成才能开始下一次。

 这里使用DMA的唯一好处就是读写速度加快，虽然主程序会等待 DMA 完成，但 数据的传输过程不需要 CPU 参与，所以同一数据使用DMA的速度更快，也就是这里的等待时间更短。
 
 根据网上他人测试结论：DMA速度是普通（基于HAL库）的 3倍，另外使用寄存器方式也会比 HAl 库快三倍，如果使用 DMA+寄存器 方式就会比 HAL快9倍


## 内部Flash模拟EEPROM

不同型号的STM32F4xC/E，其FLASH容量也有所不同，最小的只有256K字节，最大的512K字节。STM32F401的FLASH容量为256K字节，STM32F411xC/E产品的闪存模块组织如图所示：
![图 7](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/stm.png)  

STM32F4的闪存模块由：主存储器、系统存储器、OPT区域和选项字节等4部分组成。

主存储器，该部分用来存放代码和数据常数（如const类型的数据）。分为8个扇区，前4个扇区为16KB大小，然后扇区4是64KB大小，扇区5~7是128K大小，不同容量的STM32F411拥有的扇区数不一样，比如我们的STM32F411RCT6，则拥有6个扇区，从上图可以看出主存储器的起始地址就是0X08000000， B0、B1都接GND的时候，就是从0X08000000开始运行代码的。

系统存储器，这个主要用来存放STM32F4的bootloader代码，此代码是出厂的时候就固化在STM32F4里面了，专门来给主存储器下载代码的。当B0接V3.3，B1接GND的时候，从该存储器启动（即进入串口下载模式）。

OTP区域，即一次性可编程区域，共528字节，被分成两个部分，前面512字节（32字节为1块，分成16块），可以用来存储一些用户数据（一次性的，写完一次，永远不可以擦除！！），后面16字节，用于锁定对应块。

选项字节，用于配置读保护、BOR级别、软件/硬件看门狗以及器件处于待机或停止模式下的复位。 

闪存存储器接口寄存器，该部分用于控制闪存读写等，是整个闪存模块的控制机构。

在执行闪存写操作时，任何对闪存的读操作都会锁住总线，在写操作完成后读操作才能正确地进行；既在进行写或擦除操作时，不能进行代码或数据的读取操作。

STM23F4的FLASH读取是很简单的。例如，我们要从地址addr，读取一个字（一个字为32位），可以通过如下的语句读取：  
`data=*(vu32*)addr;`  
将addr强制转换为vu32指针，然后取该指针所指向的地址的值，即得到了addr地址的值。类似的，将上面的vu32改为vu8，即可读取指定地址的一个字节。相对FLASH读取来说，STM32F4 FLASH的写就复杂一点了，下面我们介绍STM32F4闪存的编程和擦除。

**闪存的编程和擦除**
执行任何Flash编程操作（擦除或编程）时，CPU时钟频率 (HCLK)不能低于1 MHz。如果在Flash操作期间发生器件复位，无法保证Flash中的内容。

在对 STM32F4的Flash执行写入或擦除操作期间，任何读取Flash的尝试都会导致总线阻塞。只有在完成编程操作后，才能正确处理读操作。这意味着，写/擦除操作进行期间不能从Flash中执行代码或数据获取操作。

STM32F4的闪存编程由6个32位寄存器控制，他们分别是：
- FLASH访问控制寄存器(FLASH_ACR)
- FLASH秘钥寄存器(FLASH_KEYR)
- FLASH选项秘钥寄存器(FLASH_OPTKEYR)
- FLASH状态寄存器(FLASH_SR)
- FLASH控制寄存器(FLASH_CR)
- FLASH选项控制寄存器(FLASH_OPTCR) 

STM32F4复位后，FLASH编程操作是被保护的，不能写入FLASH_CR寄存器；通过写入特定的序列（0X45670123和0XCDEF89AB）到FLASH_KEYR寄存器才可解除写保护，只有在写保护被解除后，我们才能操作相关寄存器。

FLASH_CR的解锁序列为：
1. 写0X45670123到FLASH_KEYR
2. 写0XCDEF89AB到FLASH_KEYR

通过这两个步骤，即可解锁FLASH_CR，如果写入错误，那么FLASH_CR将被锁定，直到下次复位后才可以再次解锁。
STM32F4闪存的编程位数可以通过FLASH_CR的PSIZE字段配置，PSIZE的设置必须和电源电压匹配，见表：29.1.2：
![图 8](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/%E7%BC%96%E7%A8%8B%E6%93%A6%E9%99%A4%E5%B9%B6%E8%A1%8C%E4%BD%8D%E6%95%B0%E4%B8%8E%E7%94%B5%E5%8E%8B%E5%85%B3%E7%B3%BB%E8%A1%A8.png)  

由于我们开发板用的电压是3.3V，所以PSIZE必须设置为10，即32位并行位数。擦除或者编程，都必须以32位为基础进行。
STM32F4的FLASH在编程的时候，也必须要求其写入地址的FLASH是被擦除了的（也就是其值必须是0XFFFFFFFF），否则无法写入。STM32F4的标准编程步骤如下：
1. 检查FLASH_SR中的BSY位，确保当前未执行任何FLASH操作。
2. 将FLASH_CR寄存器中的PG位置1，激活FLASH编程。
3. 针对所需存储器地址（主存储器块或OTP区域内）执行数据写入操作：
    —并行位数为x8时按字节写入（PSIZE=00）
    —并行位数为x16时按半字写入（PSIZE=01）
    —并行位数为x32时按字写入（PSIZE=02）
    —并行位数为x64时按双字写入（PSIZE=03）
4. 等待BSY位清零，完成一次编程。

按以上四步操作，就可以完成一次FLASH编程。不过有几点要注意：1，编程前，要确保要写如地址的FLASH已经擦除。2，要先解锁（否则不能操作FLASH_CR）。3，编程操作对OPT区域也有效，方法一模一样。
我们在STM32F4的FLASH编程的时候，要先判断缩写地址是否被擦除了，所以，我们有必要再介绍一下STM32F4的闪存擦除，STM32F4的闪存擦除分为两种：扇区擦除和整片擦除。
扇区擦除步骤如下：
1. 检查FLASH_CR的LOCK是否解锁，如果没有则先解锁
2. 检查FLASH_SR寄存器中的BSY 位，确保当前未执行任何FLASH操作
3. 在FLASH_CR寄存器中，将SER位置1，并从主存储块的12个扇区中选择要擦除的
扇区 (SNB)
4. 将FLASH_CR寄存器中的STRT位置1，触发擦除操作
5. 等待BSY位清零
	
经过以上五步，就可以擦除某个扇区。本章，我们只用到了STM32F4的扇区擦除功能，整片擦除功能我们在这里就不介绍了.。


通过访问内部 falsh 地址及其内容，达到类似 eeprom 的读写操作。

目前测试：访问内部数据会得到数据并打印出来，但之后程序死机。原因未知

###  初始化配置

在上文 `SPI(Flash)`基础上编写，不用额外初始化。

### 程序编写

新建 stmflash.c文件，用于操作stm32的内部flash
```
/****************** stmflash.c ************************/
#include "stmflash.h"
#include "delay.h"

//读取指定地址的字(32位数据)
//faddr:读地址
//返回值:对应数据.
u32 STMFLASH_ReadWord(u32 faddr)
{
	return *(vu32*)faddr;
}

//获取某个地址所在的flash扇区
//addr:flash地址
//返回值:0~11,即addr所在的扇区
u8 STMFLASH_GetFlashSector(u32 addr)
{
	if(addr<ADDR_FLASH_SECTOR_1)return FLASH_SECTOR_0;
	else if(addr<ADDR_FLASH_SECTOR_2)return FLASH_SECTOR_1;
	else if(addr<ADDR_FLASH_SECTOR_3)return FLASH_SECTOR_2;
	else if(addr<ADDR_FLASH_SECTOR_4)return FLASH_SECTOR_3;
	else if(addr<ADDR_FLASH_SECTOR_5)return FLASH_SECTOR_4;

	return FLASH_SECTOR_5;
}

//从指定地址开始写入指定长度的数据
//特别注意:因为STM32F4的扇区实在太大,没办法本地保存扇区数据,所以本函数
//         写地址如果非0XFF,那么会先擦除整个扇区且不保存扇区数据.所以
//         写非0XFF的地址,将导致整个扇区数据丢失.建议写之前确保扇区里
//         没有重要数据,最好是整个扇区先擦除了,然后慢慢往后写.
//该函数对OTP区域也有效!可以用来写OTP区!
//OTP区域地址范围:0X1FFF7800~0X1FFF7A0F(注意：最后16字节，用于OTP数据块锁定，别乱写！！)
//WriteAddr:起始地址(此地址必须为4的倍数!!)
//pBuffer:数据指针
//NumToWrite:字(32位)数(就是要写入的32位数据的个数.)
void STMFLASH_Write(u32 WriteAddr,u32 *pBuffer,u32 NumToWrite)
{
	FLASH_EraseInitTypeDef FlashEraseInit;
	HAL_StatusTypeDef FlashStatus=HAL_OK;
	u32 SectorError=0;
	u32 addrx=0;
	u32 endaddr=0;
	if(WriteAddr<STM32_FLASH_BASE||WriteAddr%4)return;	//非法地址

	HAL_FLASH_Unlock();             //解锁
	addrx=WriteAddr;				//写入的起始地址
	endaddr=WriteAddr+NumToWrite*4;	//写入的结束地址

	if(addrx<0X1FFF0000)
	{
		while(addrx<endaddr)		//扫清一切障碍.(对非FFFFFFFF的地方,先擦除)
		{
			 if(STMFLASH_ReadWord(addrx)!=0XFFFFFFFF)//有非0XFFFFFFFF的地方,要擦除这个扇区
			{
				FlashEraseInit.TypeErase=FLASH_TYPEERASE_SECTORS;       //擦除类型，扇区擦除
				FlashEraseInit.Sector=STMFLASH_GetFlashSector(addrx);   //要擦除的扇区
				FlashEraseInit.NbSectors=1;                             //一次只擦除一个扇区
				FlashEraseInit.VoltageRange=FLASH_VOLTAGE_RANGE_3;      //电压范围，VCC=2.7~3.6V之间!!
				if(HAL_FLASHEx_Erase(&FlashEraseInit,&SectorError)!=HAL_OK)
				{
					break;//发生错误了
				}
				}else addrx+=4;
				FLASH_WaitForLastOperation(FLASH_WAITETIME);                //等待上次操作完成
		}
	}
	FlashStatus=FLASH_WaitForLastOperation(FLASH_WAITETIME);            //等待上次操作完成
	if(FlashStatus==HAL_OK)
	{
		 while(WriteAddr<endaddr)//写数据
		 {
			if(HAL_FLASH_Program(FLASH_TYPEPROGRAM_WORD,WriteAddr,*pBuffer)!=HAL_OK)//写入数据
			{
				break;	//写入异常
			}
			WriteAddr+=4;
			pBuffer++;
		}
	}
	HAL_FLASH_Lock();           //上锁
}

//从指定地址开始读出指定长度的数据
//ReadAddr:起始地址
//pBuffer:数据指针
//NumToRead:字(32位)数
void STMFLASH_Read(u32 ReadAddr,u32 *pBuffer,u32 NumToRead)
{
	u32 i;
	for(i=0;i<NumToRead;i++)
	{
		pBuffer[i]=STMFLASH_ReadWord(ReadAddr);//读取4个字节.
		ReadAddr+=4;//偏移4个字节.
	}
}

//////////////////////////////////////////测试用///////////////////////////////////////////
//WriteAddr:起始地址
//WriteData:要写入的数据
void Test_Write(u32 WriteAddr,u32 WriteData)
{
	STMFLASH_Write(WriteAddr,&WriteData,1);//写入一个字
}

/******************** stmflash.h **********************/

//FLASH起始地址
#define STM32_FLASH_BASE 0x08000000 	//STM32 FLASH的起始地址
#define FLASH_WAITETIME  50000          //FLASH等待超时时间

//FLASH 扇区的起始地址
#define ADDR_FLASH_SECTOR_0     ((u32)0x08000000) 	//扇区0起始地址, 16 Kbytes
#define ADDR_FLASH_SECTOR_1     ((u32)0x08004000) 	//扇区1起始地址, 16 Kbytes
#define ADDR_FLASH_SECTOR_2     ((u32)0x08008000) 	//扇区2起始地址, 16 Kbytes
#define ADDR_FLASH_SECTOR_3     ((u32)0x0800C000) 	//扇区3起始地址, 16 Kbytes
#define ADDR_FLASH_SECTOR_4     ((u32)0x08010000) 	//扇区4起始地址, 64 Kbytes
#define ADDR_FLASH_SECTOR_5     ((u32)0x08020000) 	//扇区5起始地址, 128 Kbytes


u32 STMFLASH_ReadWord(u32 faddr);		  	//读出字
void STMFLASH_Write(u32 WriteAddr,u32 *pBuffer,u32 NumToWrite);		//从指定地址开始写入指定长度的数据
void STMFLASH_Read(u32 ReadAddr,u32 *pBuffer,u32 NumToRead);   		//从指定地址开始读出指定长度的数据
//测试写入
void Test_Write(u32 WriteAddr,u32 WriteData);

```
该部分代码，我们重点介绍一下STMFLASH_Write函数，该函数用于在STM32F4的指定地址写入指定长度的数据，该函数的实现基本类似第24章的SPI_Flash_Write函数，不过该函数对写入地址是有要求的，必须保证以下两点：
1. 该地址必须是用户代码区以外的地址。
2. 该地址必须是4的倍数。
3. 对OTP区域编程也有效。 

第1点比较好理解，如果把用户代码给卡擦了，可想而知你运行的程序可能就被废了，从而很可能出现死机的情况。不过，因为STM32F4的扇区都比较大（最少16K，大的128K），所以本函数不缓存要擦除的扇区内容，也就是如果要擦除，那么就是整个扇区擦除，所以建议大家使用该函数的时候，写入地址定位到用户代码占用扇区以外的扇区，比较保险。

第2点则是3.3V时，设置PSIZE=2所决定的，每次必须写入32位，即4字节，所以地址必须是4的倍数。第3点，该函数对OTP区域的操作同样有效，所以大家要写OTP字节，也可以直接通过该函数写入，不过注意OTP是一次写入的，无法擦除，所以，一般不要写OTP字节。

然后打开stmflash.h，该文件代码代码非常简单，我们就不做介绍了。
最后，打开main.c文件，main函数如下：
```
//要写入到STM32 FLASH的字符串数组
const u8 TEXT_Buffer[]={"STM32 FLASH TEST"};
#define TEXT_LENTH sizeof(TEXT_Buffer)	 		  	//数组长度
#define SIZE TEXT_LENTH/4+((TEXT_LENTH%4)?1:0)

#define FLASH_SAVE_ADDR  0X0800C004 	//设置FLASH 保存地址(必须为4的倍数，且所在扇区,要大于本代码所占用到的扇区.
										//否则,写操作的时候,可能会导致擦除整个扇区,从而引起部分程序丢失.引起死机.

void setup() {

	u32 datatemp[SIZE];
    uart_init();

    delay_ms(1000);

    printf("Test Start\n\r");
    printf("\r\nStart Write FLASH....\r\n");
    STMFLASH_Write(FLASH_SAVE_ADDR,(u32*)TEXT_Buffer,SIZE);
    printf("FLASH Write Finished!\r\n");//提示传送完成

    printf("\r\nStart Read FLASH.... \r\n");
	STMFLASH_Read(FLASH_SAVE_ADDR,(u32*)datatemp,SIZE);
	printf("The Data Readed Is:  \r\n");//提示传送完成
	printf("%s\r\n",datatemp);//显示读到的字符串
}

void loop()
{

	printf("Test Start\n\r");
	delay_ms(1000);
}
```
主函数部分代码非常简单，首先先进行写操作，然后再读。至此，我们的软件设计部分就结束了。

>**这里要提醒以下：**
> 主函数的 `u32 datatemp[SIZE];` 定义在正点原子的程序是 `u8 datatemp[SIZE];`,而我们在读操作时
>```
>STMFLASH_Read(FLASH_SAVE_ADDR,(u32*)datatemp,SIZE);
>```
>用的是32位的，正点原子的例程在读取函数这里将8位强制转换为32位，这在keil上面是没问题（正点原子例程是用的keil）的。但是在 STM32CubeIDe 中这么使用则是错误的，会导致读操作时造成硬件报错 `HardFault_Handler`。所以这里在一开始定义就要定义为32位的。


**参考链接**
- [STM32F407.FLASH 读写经验](https://blog.csdn.net/huan447882949/article/details/79726380)
- [STM32F4内部Flash读写](https://blog.csdn.net/zhang062061/article/details/114275376)
- [STM32F030 Read/Write Flash Throwing HardFault](https://community.arm.com/developer/tools-software/tools/f/keil-forum/33821/stm32f030-read-write-flash-throwing-hardfault)
- [STM32 Hardfault exception when writing globally declared buffer to FLASH](https://stackoverflow.com/questions/41892267/stm32-hardfault-exception-when-writing-globally-declared-buffer-to-flash)

## 内存管理


### 使用标准库

#### void *
在进行下面话题之前，我们先回忆一下 void * 是什么？

void * 表示未确定类型的指针。C/C++规定，void * 类型可以强制转换为任何其它类型的指针。

void * 也被称之为无类型指针，void * 可以指向任意类型的数据，就是说可以用任意类型的指针对 void * 赋值，如下示例：
```
void *p1;
int *p2;
p1 = p2;
```
但一般不会反过来使用，如下示例在有些编译器上面可以编译通过，有些就不行：
```
void *p1;
int *p2;
p2 = p1;

```
可以修改一下代码，将 void * 转换为对应的指针类型再进行赋值，如下示例：
```
void *p1;
int *p2;
p2 = (char *)p1;
```
由于 GNU 和 ANSI 对 void * 类型指针参与运算的规定不一样，所以为了兼容二者并且让程序有更好的兼容性，最好还是将 void * 转换为有明确类型的指针再参与运算，如下示例。
```
void *p1;
int *p2;
p2 = (char *)p1;
```

#### malloc
void * malloc(size_t size);
malloc 向系统申请分配指定 size 个字节的内存空间，即 malloc 函数用来从堆空间中申请指定的 size 个字节的内存大小，返回类型是 void * 类型，如果成功，就会返回指向申请分配的内存，否则返回空指针，所以 malloc 不保证一定成功。

另外需要注意一个问题，使用 malloc 函数分配内存空间成功后，malloc 不会对数据进行初始化，里边数据是随机的垃圾数据，所以一般结合 memset 函数和 malloc 函数 一起使用。
```
int *arr;
arr = (int *)malloc(10 * sizeof(int));
if (NULL != arr) {
    memset(arr, 0, 10 * sizeof(int));
    printf("arr: %p\n", arr);
}
```

#### free

void free(void *ptr);

free 函数会释放指针指向的内存分配空间。

对于 free 函数我们要走出一个误区，不要以为调用了 free 函数，变量就变为 NULL 值了。本质是 free 函数只是割断了指针所指的申请的那块内存之间的关系，并没有改变所指的地址（本身保存的地址并没有改变）。如下示例：
```
char *pchar = (char *)malloc(10 * sizeof(char));
        
if (NULL != pchar) {
    strcpy(pchar, "blog");
    /* pchar所指的内存被释放，但是pchar所指的地址仍然不变 */
    free(pchar);
    
    /* 该判断没有起到防错作用，此时 pchar 并不为 NULL */
    if (NULL != pchar) {
        strcpy(pchar, "it");
        printf("pchar: %s", pchar);
    }
}
```
正确且安全的做法是对指针变量先进行 free 然后再将其值置为 NULL，如下下面示例：
```
char *pchar = (char *)malloc(10 * sizeof(char));
        
if (NULL != pchar) {
    strcpy(pchar, "blog");
    /* pchar所指的内存被释放，但是pchar所指的地址仍然不变 */
    free(pchar);
    /* 将其置为 NULL 值 */
    pchar = NULL;
    
    /* 该判断没有起到防错作用，此时 pchar 并不为 NULL */
    if (NULL != pchar) {
        strcpy(pchar, "it");
        printf("pchar: %s", pchar);
    }
}
```

#### calloc 函数

void * calloc(size_t count, size_t size);

在堆上，分配 n*size 个字节，并初始化为0，返回 void *类型，返回值情况跟 malloc 一致。

函数 malloc() 和函数 calloc() 的主要区别是前者不能初始化所分配的内存空间，而后者能。如果由 malloc() 函数分配的内存空间原来没有被使用过，则其中的每一位可能都是0；反之，如果这部分内存曾经被分配过，则其中可能遗留有各种各样的数据。也就是说，使用 malloc() 函数的程序开始时(内存空间还没有被重新分配)能正常进行，但经过一段时间(内存空间还已经被重新分配)可能会出现问题。

函数 calloc() 会将所分配的内存空间中的每一位都初始化为零，也就是说，如果你是为字符类型或整数类型的元素分配内存，那么这些元素将保证会被初始化为0；如果你是为指针类型的元素分配内存，那么这些元素通常会被初始化为空指针；如果你为实型数据分配内存，则这些元素会被初始化为浮点型的零。

#### realloc() 函数

void * realloc(void *ptr, size_t size);

realloc() 会将 ptr 所指向的内存块的大小修改为 size，并将新的内存指针返回。假设之前内存块的大小为 n，如果 size <= n，那么截取的内容不会发生变化，如果 size > n，那么新分配的内存不会被初始化。

对于上面说的新的内存指针地址可能变也可能不变，假如原来alloc的内存后面还有足够多剩余内存的话，realloc后的内存=原来的内存+剩余内存，realloc还是返回原来内存的地址即不会创建新的内存。假如原来alloc的内存后面没有足够多剩余内存的话，realloc将申请新的内存，然后把原来的内存数据拷贝到新内存里，原来的内存将被free掉，realloc返回新内存的地址。

另外要注意，如果 ptr = NULL，那么相当于调用 malloc(size)；如果 ptr != NULL且size = 0，那么相当于调用 free(ptr)。

当调用 realloc 失败的时候，返回NULL，并且原来的内存不改变，不会释放也不会移动。

#### 示例
```
// 对其分配内存，这个时候pchar值是随机的垃圾值
char *pchar = (char *)malloc(16);
// 手动初始化pchar的值，下面的方法则不需要
memset(pchar, 0, 16);

// calloc分配内存，会自动设置为0，不需要memset
char *pchar_orig = (char *)calloc(12, sizeof(char));

// 在原内存基础上，在堆内存空间中连续增加内存
// 如果原内存没有连续空间可拓展，realloc会新分配一个空间，将原有内存copy到新空间，然后释放原内存  
// 注意：realloc和malloc，只分配内存不进行赋值操作
char *pchar_dest = (char *)realloc(pchar_orig, 10);
  
// 相当于 malloc(60)
char *pchar_ini = (char *)realloc(NULL, 60);

free(pchar);
pchar = NULL;

free(pchar_orig);
pchar_orig = NULL;

free(pchar_dest);
pchar_dest = NULL;

free(pchar_ini);
pchar_ini = NULL;
```

**参考链接**
- [C语言中free、malloc 等内存管理函数](https://blog.csdn.net/veryitman/article/details/89761768)

### 自写malloc库

- **栈区（stack）**：由编译器自动分配和释放，存放函数的参数值、局部变量的值等，其操作方式类似于数据结构中的栈。
- **堆区（heap）**：一般由程序员分配和释放，若程序员不释放，程序结束时可能由操作系统回收。分配方式类似于数据结构中的链表。

stm32cubeide 默认配置
```
Stack_Size      EQU     0x400
Heap_Size       EQU     0x200
```
0x00000400 等于1024字节所以等于1K
0x00000200 等于512字节所以等于512 Byte

由于 `malloc()` 分配的动态内存在堆区域，因此调大堆空间 `Heap_Size` 为 0xC00，即 3072 字节大小。用户可以自由使用的堆空间，大约为这里分配的堆总空间的一半。超过时系统就会死机，也就是 3072/2 字节可以被用户用来自由使用。

![图 1](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/%E5%A0%86%E4%BF%AE%E6%94%B9.png)  

[STM32CUBEIDE——malloc](https://blog.csdn.net/qq_26410201/article/details/102842270)
[STM32分配堆栈空间不足问题原因及解决方法](https://blog.csdn.net/lighthear/article/details/69485942)


## USB虚拟串口

STM32向PC发送的是USB协议的数据包，跟串口自身没有关系
PC端的USB接口收到USB协议的数据包后，由驱动程序来解包并放入操作系统的串口缓冲区里，这样，串口助手类的工具就能够从缓冲区里读到数据，串口助手就认为是有 uart数据到来了。

所以虚拟串口和串口不是一个概念，本质也不同，那么其实串口的参数配置对虚拟串口来说也就没用了。在我们链接虚拟串口测试时，无论怎么更改串口助手的波特率，都不会影响数据接收和发送。

###  初始化配置

开发板已经将芯片的USB引脚接到了 micro 接口上，我们只需要用数据线连接电脑和开发板即可。这里的 `USBDP/DM` 引脚是直接和usb接口时直连的，不需要一般串口需要接一个串口芯片。

![图 5](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/USBD_CDC%20%E7%A1%AC%E4%BB%B6.png)  


1. 使能USB接口
参数保持默认，speed 参数设置通信速度，可自行再尝试修改；引脚再使能后系统自动选择配置，和我们的硬件一致，无需更改。
![图 3](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/USBD_CDC%20%E9%85%8D%E7%BD%AE1.png)  

2. 配置USB模式位虚拟串口
参数保持默认。其中的USB CDC Rx Buffer Size 是定义接收数组大小，下面的是发送数组大小，可以尝试修改。
![图 4](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/USBD_CDC%20%E9%85%8D%E7%BD%AE2.png)  


### 程序编写

USB 虚拟串口的API我们常用的对外接口都在 usbd_cdc_if.c/.h 文件中。其中主要API：

- `CDC_Receive_FS()`
接收数据。
- `CDC_Transmit_FS()`
发送的数据
- `CDC_TransmitCplt_FS()`
数据发送完成回调函数

以上三个函数都在usbd_cdc_if.c文件中，一般需要需改是 `CDC_Receive_FS()`函数，用来处理接收的数据

**主程序**
初始化配置后，我们无需任何修改，就可以直接发送数据。主函数如下：
```
u8 test_buf[] = {"Test Start\n\r"};

void loop()
{
	CDC_Transmit_FS(test_buf,sizeof(test_buf));
	delay_ms(1000);
}
```
通多usb连接 

### 数据接收

需要更改一下原来的`CDC_Receive_FS()`函数：
```
static int8_t CDC_Receive_FS(uint8_t* Buf, uint32_t *Len)
{
  /* USER CODE BEGIN 6 */
  USBD_CDC_SetRxBuffer(&hUsbDeviceFS, &Buf[0]);
  USBD_CDC_ReceivePacket(&hUsbDeviceFS);

  CDC_Transmit_FS(Buf,*Len);
  return (USBD_OK);
  /* USER CODE END 6 */
}
```
我们仅添加了
```
CDC_Transmit_FS(Buf,*Len);
```
实现数据的回传。

### printf 功能

通过自定义一个printf函数，实现与串口中的重定向printf 功能。
在 `usbd_cdc_if.c` 添加如下内容：
```
void usb_printf(const char *format, ...)
{
    va_list args;
    uint32_t length;

    va_start(args, format);
    length = vsnprintf((char *)UserTxBufferFS, APP_TX_DATA_SIZE, (char *)format, args);
    va_end(args);
    CDC_Transmit_FS(UserTxBufferFS, length);
}
```
然后在.h 中声明一下。
```
void usb_printf(const char *format, ...);
```

最后主程序中直接调用即可：
```
void loop()
{
	usb_printf("idle\n\r");
	delay_ms(1000);
}

```

**参考链接:**
- [STM32 usb虚拟串口 最大速度可以达到多少 波特率可以设置到多少？](https://www.amobbs.com/thread-4764331-1-1.html)
- [使用STM32CubeMX把USB配置成虚拟串口（virtual com port）](https://blog.csdn.net/d1w2jsw/article/details/112206357)
- [使用STM32CubeMX实现USB虚拟串口的环回测试功能](https://bbs.21ic.com/icview-1241432-1-1.html)
- [stm32Cubemx USB虚拟串口](https://blog.csdn.net/qq_36561846/article/details/109427606)
- [STM32使用虚拟串口CDC重定向printf](http://www.mcublog.cn/stm32/2020_04/stm32-cdc-printf/)
- [STM32CubeMX之USB从机](https://mp.weixin.qq.com/s?__biz=MzUxMTcxMTU5Mg==&mid=2247483844&idx=1&sn=3ad9d076b353ebca20054922812d5b84&chksm=f96ec1c3ce1948d56f22387edf21bf6ea2f4f0e08b4c95c0e533d479703bb454e237f7256433&scene=21#wechat_redirect)
- [STM32 USB虚拟串口波特率问题（含源码）](https://blog.csdn.net/zhang062061/article/details/113483845)
- [STM32CubeMX实现STM32的USB虚拟串口功能](https://www.sunev.cn/embedded/732.html)

## USB-U盘

将外部flash W25Qxx 作为U盘，通过电脑可以像访问U盘一样访问 falsh 里的数据。类似U盘
### 初始化配置

在之前的spi驱动，读写 W25Q256 基础上配置。

1. 使能USB接口
参数保持默认，speed 参数设置通信速度，可自行再尝试修改；引脚再使能后系统自动选择配置，和我们的硬件一致，无需更改。
![图 3](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/USBD_CDC%20%E9%85%8D%E7%BD%AE1.png)  

2. 配置USB模式，选择大容量存储设备。读写扇区大小改为4096
![图 6](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/USB%20u%E7%9B%98%E9%85%8D%E7%BD%AE.png)  

### 程序编写

主要修改 `usbd_storage_if.c` w文件内容，添加 SPi 的读写接口到 USb读写中。
在原有的宏定义下，重新定义，应为如果我们直接修改的话，下次初始化就又会被 IDE 该回来。
- 定义扇区数  = 1024x8 扇区，内存大小 =扇区数x扇区大小 = 1024*8*4096 = 32M。这个数字是根据 W25Q256 = 32M，倒退计算得来的。
- 定义块大小，必须和我们初始化配置时的一致。要改一起改，而且要和上面的计算配合，不然实际程序虽然也能用，但显示的内存容量就会不一样。
>这里的扇区大小和芯片的扇区是两个概念，这里的扇区指 USB协议下的 U盘设备的扇区大小，所以通常比芯片的扇区大很多。

```
/** @defgroup USBD_STORAGE_Private_Defines
  * @brief Private defines.
  * @{
  */
#define STORAGE_LUN_NBR                  1
#define STORAGE_BLK_NBR                  0x10000
#define STORAGE_BLK_SIZ                  0x200

/* USER CODE BEGIN PRIVATE_DEFINES */
#undef STORAGE_BLK_NBR
#undef STORAGE_BLK_SIZ
#define STORAGE_BLK_NBR                  1024*8  //Kb = 32M
#define STORAGE_BLK_SIZ                  4096
/* USER CODE END PRIVATE_DEFINES */
```

然后修改连个读写接口：
将SPI读写API添加进来。其余不许改动，之后直接使用即可。

```
/**
  * @brief  .
  * @param  lun: .
  * @retval USBD_OK if all operations are OK else USBD_FAIL
  */
int8_t STORAGE_Read_FS(uint8_t lun, uint8_t *buf, uint32_t blk_addr, uint16_t blk_len)
{
  /* USER CODE BEGIN 6 */
	W25QXX_Read((uint8_t*)buf,blk_addr*STORAGE_BLK_SIZ,blk_len*STORAGE_BLK_SIZ);
  return (USBD_OK);
  /* USER CODE END 6 */
}

/**
  * @brief  .
  * @param  lun: .
  * @retval USBD_OK if all operations are OK else USBD_FAIL
  */
int8_t STORAGE_Write_FS(uint8_t lun, uint8_t *buf, uint32_t blk_addr, uint16_t blk_len)
{
  /* USER CODE BEGIN 7 */
	W25QXX_Write((uint8_t*)buf,blk_addr*STORAGE_BLK_SIZ,blk_len*STORAGE_BLK_SIZ);
  return (USBD_OK);
  /* USER CODE END 7 */
}
```
**主函数：**
只需要初始化 W25Q256 接可。其实在原有程序上完全不用改动。下面的程序还是原来 SPI 读写的程序。
```
void setup() {

	u8 datatemp[SIZE];
	u32 FLASH_SIZE=128*1024*1024; //FLASH 大小为 128M 字节
    uart_init();
    printf("Test Start\n\r");
	W25QXX_Init();  /* W25Q256-Flash初始化 */
	while(W25QXX_ReadID()!=W25Q128)
	{
		printf("W25Q64 Failed!\n\r");  //
	}
	printf("W25Q64 OK!\n\r");
	W25QXX_Write((u8*)TEXT_Buffer,FLASH_SIZE-100,SIZE);
	W25QXX_Read(datatemp,FLASH_SIZE-100,SIZE);
	printf("The Data Readed Is: ");//提示传送完成
	printf("%s\r\n",datatemp); //显示读到的字符串
}

void loop()
{

	printf("Test Start\n\r");
	delay_ms(1000);
}
```

然后使用 USB 现连接开发板和电脑，随后电脑会弹窗提示格式化，点确认
![图 7](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/U%E7%9B%98%E6%A0%BC%E5%BC%8F%E5%8C%96.png)  
U盘初始化配置保持和下图一致即可，然后点击 `开始`
![图 8](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/U%E7%9B%98%E6%A0%BC%E5%BC%8F%E5%8C%962.png)  
格式化完成
![图 9](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/U%E7%9B%98%E6%A0%BC%E5%BC%8F%E5%8C%963.png)  

之后就想一般u盘操作，保存读取文件即可。（注意文件不要太大，就32M）。下图是存放的图片，重新插拔USB线，图片依旧还在，也能正常读取，删减。测试成功.
![图 10](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/U%E7%9B%98%E5%AD%98%E5%82%A8.png)  

**参考连接**
[stm32USB之模拟U盘](https://blog.csdn.net/qq_38420206/article/details/110572003)
[stm32 cubemx usb spi flash w25q128 u盘调试笔记](https://blog.csdn.net/zhaqonianzhu/article/details/108363938)
[用STM32F0系列内部Flash虚拟出U盘](https://www.amobbs.com/thread-5705198-1-1.html)
[stm32使用外部SPI FLASH模拟U盘(大容量存储设备MSC)求职学习资料](https://www.b2bchain.cn/10000.html)
[cubemx配置 USB读卡器+FATFS](https://www.pianshen.com/article/1915956443/)

## Fatfs 文件系统移植

再上述模拟u盘基础上，添加文件系统。这样就可以使用usb线实现将内存存储到flash中，再通过文件系统识别读取flash中的文件。

### 初始化配置
再上述 USB-U 盘章节基础上修改
- 勾选使能FATFS
- 配置FATFS
    - 支持长文件名并将缓存放在 STACK（栈）中
    - 最大扇区(MAX_SS)修改为4096
![图 1](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/fatfs-%E5%88%9D%E5%A7%8B%E5%8C%96%E9%85%8D%E7%BD%AE.png)  
1. 缓存工作区为什么放在栈？其实fatfs提供了三个选项：BSS，STACK , HEAP，根据个人情况选一个。
    - 在BSS上启用带有静态工作缓冲区的LFN，不能动态分配。
    - 如果选择了HEAP(堆)且自己有属于自己的malloc就去重写ff_memalloc  ff_memfree函数。如果是库的malloc就不需要。
    - 一般都选择使用STACK（栈），能动态分配。
    - 当使用堆栈作为工作缓冲区时，请注意堆栈溢出。Stack Size只要不溢出就行。
![图 2](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/%E5%A0%86%E6%A0%88%E5%A4%A7%E5%B0%8F%E8%AE%BE%E7%BD%AE.png)  

3、为什么最大扇区大小是4096Byte?一般别人都是512Byte？   其实这个是根据你自己使用的存储芯片和驱动相关的。因为我使用的W25Q128这款芯片是最小擦除单位是4096。不使用512byte是因为效率大大降低但是优点是空间利用率会大大提高。比如你文件系统最大分区是512，但是芯片最小擦除单位是4096，那么你在驱动就要实现先用缓存区把整个扇区4096byte全部读出来，然后判读其中写入512byte中有没有擦除过（即全0xFF），没有的话先擦除，在把数据写入缓存区最后写入芯片。所以步骤繁琐效率低，但是优点就是存储空间的利用率会大大提高，避免太多浪费。
另外可能需要修改堆栈大小。

### 程序编写

我们需要先配置底层函数接口，在 `user_diskio.c` 文件中

```
// user_diskio.c
#define FLASH_SECTOR_COUNT     1024*8  // 16M
#define FLASH_BLOCK_SIZE   	   1       // 单次擦除的快数量


DSTATUS USER_initialize (
	BYTE pdrv           /* Physical drive nmuber to identify the drive */
)
{
  /* USER CODE BEGIN INIT */
	W25QXX_Init();
	if(W25QXX_ReadID()== W25QXX_TYPE){
		Stat = RES_OK;
	}
    else{
    	Stat = RES_ERROR;
    }
    return Stat;
  /* USER CODE END INIT */
}

DSTATUS USER_status (
	BYTE pdrv       /* Physical drive number to identify the drive */
)
{
  /* USER CODE BEGIN STATUS */
	if(W25QXX_ReadID()== W25QXX_TYPE){
		Stat = RES_OK;
	}
    else{
    	Stat = RES_ERROR;
    }
    return Stat;
  /* USER CODE END STATUS */
}

DRESULT USER_read (
	BYTE pdrv,      /* Physical drive nmuber to identify the drive */
	BYTE *buff,     /* Data buffer to store read data */
	DWORD sector,   /* Sector address in LBA */
	UINT count      /* Number of sectors to read */
)
{
  /* USER CODE BEGIN READ */
    if (!count)return RES_PARERR;//参数�?�?
	for(;count>0;count--)
	{
		W25QXX_Read(buff,sector*_MAX_SS,_MAX_SS);
		sector++;
		buff+=_MAX_SS;
	}
    return RES_OK;
  /* USER CODE END READ */
}

DRESULT USER_write (
	BYTE pdrv,          /* Physical drive nmuber to identify the drive */
	const BYTE *buff,   /* Data to be written */
	DWORD sector,       /* Sector address in LBA */
	UINT count          /* Number of sectors to write */
)
{
  /* USER CODE BEGIN WRITE */
  /* USER CODE HERE */
    if (!count)return RES_PARERR;//参数�?�?
	for(;count>0;count--)
	{
		W25QXX_Write((u8*)buff,sector*_MAX_SS,_MAX_SS);
		sector++;
		buff+=_MAX_SS;
	}
    return RES_OK;
  /* USER CODE END WRITE */
}

DRESULT USER_ioctl (
	BYTE pdrv,      /* Physical drive nmuber (0..) */
	BYTE cmd,       /* Control code */
	void *buff      /* Buffer to send/receive control data */
)
{
  /* USER CODE BEGIN IOCTL */
    DRESULT res = RES_ERROR;
	switch(cmd)
	{
		case CTRL_SYNC:
			res = RES_OK;
			break;
		case GET_SECTOR_SIZE:
			*(WORD*)buff = _MAX_SS;
			res = RES_OK;
			break;
		case GET_BLOCK_SIZE:
			*(WORD*)buff = FLASH_BLOCK_SIZE;
			res = RES_OK;
			break;
		case GET_SECTOR_COUNT:
			*(DWORD*)buff = FLASH_SECTOR_COUNT;
			res = RES_OK;
			break;
		default:
			res = RES_PARERR;
			break;
	}
    return res;
  /* USER CODE END IOCTL */
}
```
之后编写主程序测试：
```
void setup() {

	u8 name_buf[512] = " ";  // 字符数组，用于存放从文件中读取的内容
	UINT num =1;  // 如果不初始化，该值会保存最后一次值（不是局部变量吗？实际测竟会保留值。。。）
	u16 i = 0;    // 数组元素索引
    uart_init();
    printf("Test Start\n\r");
	W25QXX_Init();  /* W25Q256-Flash初始化 */
	while(W25QXX_ReadID()!=W25Q128)
	{
		printf("W25Q64 Failed!\n\r");  //
	}
	printf("W25Q64 OK!\n\r");

    /* 挂载外部flash */
	retUSER = f_mount(&USERFatFS, "0:", 0);
	if(retUSER == FR_OK)
	{
		//* Create and Open a new text file object with read access */
		retUSER = f_open(&USERFile, "FatFs_test.txt", FA_READ);
		if(retUSER == FR_OK) // FA_WRITE  identity_true_blue.bin
		{
			 f_lseek(&USERFile,0);
			 while(num != 0){ // 读取文件内容直到结束
				 f_read(&USERFile,name_buf+i,1,&num);
				 i++;
			 }
			 f_close(&USERFile); // 读完后关闭文件
		}
		f_mount(NULL, "0:", 0);
	}
	if(i != 0)
		printf("%s\n\r",name_buf);
}
```

下载程序并运行。程序会查找flash中的文件 `name.txt`没有则会创建一个空白文件。我们使用usb连接电脑，会发现falsh中多了一个`FatFs_test.txt`文件，我们打开该文件，手动添加一些内容（不能是中文，中文支持后面再说明）保存，重启，会观察到串口输出了文件内容，表示测试成功。

## fatfs文件系统中文支持

## IAP

STM32的内部闪存（FLASH）地址起始于0x08000000，一般情况下，程序文件就从此地址开始写入。此外STM32是基于Cortex-M3内核的微控制器，其内部通过一张“中断向量表”来响应中断，程序启动后，将首先从“中断向量表”取出复位中断向量执行复位中断程序完成启动，而这张“中断向量表”的起始地址是0x08000004，当中断来临，STM32的内部硬件机制亦会自动将PC指针定位到“中断向量表”处，并根据中断源取出对应的中断向量执行中断服务程序。

**常规程序运行状态：**
STM32在复位后，先从0X08000004地址取出复位中断向量的地址，并跳转到复位中断服务程序，如图标号①所示；在复位中断服务程序执行完之后，会跳转到我们的main函数，如图标号②所示；而我们的main函数一般都是一个死循环，在main函数执行过程中，如果收到中断请求（发生重中断），此时STM32强制将PC指针指回中断向量表处，如图标号③所示；然后，根据中断源进入相应的中断服务程序，如图标号④所示；在执行完中断服务程序以后，程序再次返回main函数执行，如图标号⑤所示。
![图 1](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/tes.png)  

**IAP程序运行流程：**
STM32复位后，还是从0X08000004地址取出复位中断向量的地址，并跳转到复位中断服务程序，在运行完复位中断服务程序之后跳转到IAP的main函数，如图标号①所示，此部分同图33.1.1一样；在执行完IAP以后（即将新的APP代码写入STM32的FLASH，灰底部分。新程序的复位中断向量起始地址为0X08000004+N+M），跳转至新写入程序的复位向量表，取出新程序的复位中断向量的地址，并跳转执行新程序的复位中断服务程序，随后跳转至新程序的main函数，如图标号②和③所示，同样main函数为一个死循环，并且注意到此时STM32的FLASH，在不同位置上，共有两个中断向量表。

在main函数执行过程中，如果CPU得到一个中断请求，PC指针仍强制跳转到地址0X08000004中断向量表处，而不是新程序的中断向量表，如图标号④所示；程序再根据我们设置的中断向量表偏移量，跳转到对应中断源新的中断服务程序中，如图标号⑤所示；在执行完中断服务程序后，程序返回main函数继续运行，如图标号⑥所示。

**通过以上两个过程的分析，我们知道IAP程序必须满足两个要求：**
1. 新程序必须在IAP程序之后的某个偏移量为x的地址开始；
2. 必须将新程序的中断向量表相应的移动，移动的偏移量为x；

![图 2](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/pic_1617687693919.png)  

### 初始化配置

沿用前面的前面的 **GPIO HAL库操作**（LED闪烁）和 **串口** 两个程序，不修改初始化配置。

### 程序编写

我们有2个程序，一个为Bootloader即IAP程序，一个是app即主程序。
- Bootloader程序负责接收串口发送过来的app程序，并存储到flash中，再实现程序跳转，运行已经存储的app程序。
- app程序就是我们要运行的主程序，也是IAO升级要修改的程序，以后更新该程序时可以直接通过串口发送更新。



#### app程序

app程序直接使用**GPIO HAL库操作**（LED闪烁）程序，main 函数修改如下：
```
int main(void)
{
  SCB->VTOR = FLASH_BASE | 0x8000;  // 更新中断向量表偏移地址。

  HAL_Init();
  SystemClock_Config();
  MX_GPIO_Init();

  setup();
  while (1)
  {
	loop();
  }
}
```
注意这里修改的时 Src 文件夹下初始化生成main函数。这里只在程序开始处添加了一行`SCB->VTOR = FLASH_BASE | 0x8000; `,用于中断向量表偏移量的设置。这里的 `0x8000` 一定要大于我们的 `Bootloader程序` 占用的flash大小，这个需要我们在编写完`Bootloader程序`后，编译执行才能看到，从下图得知本次的`Bootloader程序`falsh占用 17.68KB，而 `0x8000` 约为 32KB，满足要求。
![图 3](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/bootloader%E7%A8%8B%E5%BA%8F%E5%8D%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E5%A4%A7%E5%B0%8F.png)  

还要修改一下程序flash链接地址。打开 STM32Fxxxx_FLASH.ld 文件，在开头部分找到下图所示内容
![图 4](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/link%E6%96%87%E4%BB%B6%E4%BF%AE%E6%94%B9falsh%E5%9C%B0%E5%9D%80.png)  
将falsh起始地址改为0x08008000，大小改为 224KB（256KB-32KB）。

修改好后，重新编译，会默认生成 bin 文件，在项目文件夹下的 `Debug` 文件夹中。留好备用。

#### Bootloader程序

Bootloader程序直接使用**串口**程序，首先修改串口接收回调函数

Bootloader程序部分大概思路：
1. 先将Bootloader程序通过stlink烧录到MCU
2. 运行MCU，Bootloader程序开始会循环检测串口有没有数据，如果在一段时间后仍没有数据过来，Bootloader程序将会执行跳转，运行旧的app程序
3. 如果等待期间有数据过来，Bootloader程序将通过串口接收APP文件，利用数组先保存下来存储到USART_Buffer中，然后再写入flash，最后执行跳转，运行新的app程序（刚从串口接收的app）

```
#define USART_REC_LEN  			20*1024 //定义最大接收字节数 10K

//串口1中断服务程序
//注意,读取USARTx->SR能避免莫名其妙的错误
//u8 USART_RX_BUF[USART_REC_LEN] __attribute__ ((at(0X20001000))) ={0};     该语句在GCC编译器不支持，需改为以下语句，并修改Link文件
u8 __attribute__((section(".myBufSection"))) USART_RX_BUF[USART_REC_LEN];  //接收缓冲,最大USART_REC_LEN个字节,起始地址为0X20001000.

u32 USART_RX_CNT=0;		  //接收的字节数
/**
  * @brief 串口接收中断，每接收一个字节中断一次
  */
void HAL_UART_RxCpltCallback(UART_HandleTypeDef *UartHandle)
{
	if(UartHandle->Instance == USART1){   //判断时那种中断
		if(USART_RX_CNT<USART_REC_LEN)
		{
			USART_RX_BUF[USART_RX_CNT]=RxBuffer;
			USART_RX_CNT++;
		}
	}
    HAL_UART_Receive_IT(&huart1,&RxBuffer,1); // 再次开启中断	
}
```
回调函数没啥特别的，主要讲解 `USART_RX_BUF[USART_REC_LEN]` 的定义，正点原子使用 `__attribute__ ((at(0X20001000))) ={0}`，其作用就是把变量或函数绝对定位到 Flash 或者 RAM 中，区别就是后面的地址，写成0x80000000就是定位到falsh中，写成0x20000000就是定位到RAM中。

这里我们选择定位到 RAM 中，一般用于数据量比较大的缓存，如串口的接收缓存，再就是某个位置的特定变量。用于用于串口发送过来的app程序。

但以上语句只能在keil编译器（基于MDK）中使用，而在STM32CubeIDE（基于GCC）是不支持的，所以我们需要修改为：
`__attribute__((section(".myBufSection"))) USART_RX_BUF[USART_REC_LEN]`,同时还需要再次修改link文件，打开 STM32Fxxxx_FLASH.ld 文件，将
```
  /* placing my named section at given address: */
  .myBufBlock 0X20001000 :
  {
    KEEP(*(.myBufSection)) /* keep my variable even if not referenced */
  } > RAM
```
添加到 `SECTIONS`处，如下图所示。
![图 6](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/section%E4%BF%AE%E6%94%B9.png)  

具体含义及使用参考以下链接：
- [GCC编译器对变量绝对定位怎么写？](https://www.zhihu.com/question/266452426/answer/308181759)
- [Defining Variables at Absolute Addresses with gcc](https://mcuoneclipse.com/2012/11/01/defining-variables-at-absolute-addresses-with-gcc/)
- [GNU Linker, can you NOT Initialize my Variable?](https://mcuoneclipse.com/2014/04/19/gnu-linker-can-you-not-initialize-my-variable/)
- [C语言__attribute__的使用](https://www.yuque.com/ixxw/it/__attribute__)

这里有个bug，这里使用的 STM32F401 单片的RAM空间只有64KB，而flash空间有256KB，所以如果我们的app程序大64KB，就无法一次性存储到 RAM 中了，必须采用边接收边写入的方式，这里我们的app程序只有5.76Kb，数组设置为10K，完全够用。
![图 5](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/app%E7%A8%8B%E5%BA%8F%E5%8D%A0%E7%94%A8%E7%A9%BA%E9%97%B4.png)  

然后修改主函数如下：
```
void loop()
{
	u16 oldcount=0;	//老的串口接收数据值
	u16 applenth=0;	//接收到的app代码长度

	while(1){
		for(int i = 0;i<20;i++){  // 循环检查串口是否有数据发过来
			while(USART_RX_CNT)   // 有数据，直接进入循环，接收所有数据
			{
				if(oldcount==USART_RX_CNT)//新周期内,没有收到任何数据,认为本次数据接收完成.
				{
					applenth=USART_RX_CNT;
					oldcount=0;
					USART_RX_CNT=0;
					printf("用户程序接收完成!\r\n");
					printf("代码长度:%dBytes\r\n",applenth);
					printf("开始更新固件...\r\n");
					if(((*(vu32*)(0X20001000+4))&0xFF000000)==0x08000000)//判断是否为0X08XXXXXX.此时SRAM存储的是新的app程序，检查前四个字节地址是否正确，判断是否是app程序
					{
						iap_write_appbin(FLASH_APP1_ADDR,USART_RX_BUF,applenth);//更新FLASH代码 
						printf("固件更新完成!\r\n");
					}else
					{
						printf("非FLASH应用程序!\r\n");
						applenth = 0;
					}

				}else {
					oldcount=USART_RX_CNT;
					delay_ms(10);
				}
			}
			printf("没有可以更新的固件!");  // 没有数据，等待200ms
            printf("\r\n");
			delay_ms(200);
		}
	    printf("没有可以更新的固件!\r\n");  // 没有app更新
		printf("开始执行FLASH用户代码!!\r\n");
		delay_ms(10);
		if(((*(vu32*)(FLASH_APP1_ADDR+4))&0xFF000000)==0x08000000)//判断是否为0X20XXXXXX.检查app地址的字节是否正确，判断是否是一个程序
		{
			iap_load_app(FLASH_APP1_ADDR);//执行FLASH APP代码
		}else
		{
			printf("非FLASH应用程序,无法执行!\r\n");
            printf("\r\n");
		}
	}
}
```

程序一开始会循环等待 20*200ms，查看串口是否有新的程序发送过来，有的话就接收并存储到 RAM 中，接收完成后，在写入到falsh中，然后跳转执行新的app程序。如果没有新程序发过来，在等待时间结束后，程序仍会跳转执行app程序（旧的）。

**使用：**
1. 将Bootloader程序下载至MCU，上电运行。
2. 通过串口助手发送之前编译好的APP的bin文件，等待写入flash，直至完成。
3. 等待跳转到APP运行。

**参考资料：**
- [STM32 IAP 升级设计（HAL）](https://tw511.com/a/01/12332.html)
- [stm32cubeide iap](https://www.cnblogs.com/ramlife/p/12435986.html)
- [STM32CubeMx生成的工程中使用Printf函数调试和IAP（在线下载功能）](https://blog.csdn.net/mynameislinduan/article/details/83579725)
- [STM32F4 IAP学习笔记](https://blog.csdn.net/qq_18150255/article/details/82430847)
- [STM32CUBEIDE IAP跳转失败，求助？！！](http://www.openedv.com/thread-320143-1-1.html)
- [STM32CubeIDE IAP原理讲解，及UART双APP迭代升级IAP实现](https://blog.csdn.net/sudaroot/article/details/106932736)
- [STM32 Cube IDE 下实现 IAP —— (1) 程序跳转](http://ibotx.com/?p=191)

## LCD驱动

**参考资料**
- [My Top 5 Arduino Displays](https://www.youtube.com/watch?v=0FMs0hA4Xzo)

- [ST7789_3D_Filled_Vector_Ext](https://github.com/cbm80amiga/ST7789_3D_Filled_Vector_Ext)

- [ST7735_3d_filled_vector](https://github.com/cbm80amiga/ST7735_3d_filled_vector)
- [stm32_graphics_display_drivers](https://github.com/RobertoBenjami/stm32_graphics_display_drivers)
- [Bodmer/TFT_eSPI](https://github.com/Bodmer/TFT_eSPI)
### ST7789 3.5寸 8位并口

#### 硬件配置
本次驱动采用8位并口驱动，无触摸（pin1、2、3、4不接），DB0-7可以悬空不用接地。
![图 1](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/TFT%E5%B1%8F8%E4%BD%8D%E6%8E%A5%E7%BA%BF.png)  


#### 初始化配置
在 **串口** 例程基础上修改。主要是添加以下 IO 引脚。
- PB0- 7作为数据引脚接硬件DB8-15
- PB10 LCD背光（实际未接）
- PB12 RESET
- PB14 CS
- PC13 RD
- PC14 WR
- PC14 RS
![图 2](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/TFT%E5%B1%8F8%E4%BD%8D%E5%88%9D%E5%A7%8B%E5%8C%96%E9%85%8D%E7%BD%AE.png)  

#### 程序编写

一般 TFTLCD 模块的使用流程：

![图 3](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/TFT%E5%B1%8F%E4%BD%BF%E7%94%A8%E6%B5%81%E7%A8%8B.png)  

第一个是 LCD_WR_DATA 函数，该函数在 lcd.h 里面，通过宏定义的方式申明。该函数通
过 并口向 LCD 模块写入一个 16 位的数据，使用频率是最高的，这里我们采用了宏定义的方
式，以提高速度，实际测试刷屏速度比放在 .c 文件中块 33ms 。其代码如下
```
#define DATAOUT(x) GPIOB->ODR=((GPIOB->ODR & 0xff00) | (x));  // 比下方语句增加加了15ms显示时间，但避免修改了高八位引脚状态
//#define DATAOUT(x) GPIOB->ODR=x; //数据输出 低8位
#define LCD_WR_DATA(data){\
	LCD_RS=1;\
	LCD_CS=0;\
	DATAOUT(data);\
	LCD_WR=0;\
	LCD_WR=1;\
	LCD_CS=1;\
}
```
第二个是： LCD_WR_DATAX 函数，该函数在 ILI93xx.c 里面定义，功能和 LCD_WR_DATA
一模一样。宏定义函数的好处就是速度快（直接嵌到被调用函数里面去了），坏处就是占空
间大。在 LCD_Init 函数里面，有很多地方要写数据，如果全部用宏定义的 LCD_WR_DATA 函
数，那么就会占用非常大的 flash，所以我们这里另外实现一个函数： LCD_WR_DATAX，专门
给 LCD_Init 函数调用，从而大大减少 flash 占用量。
该函数代码如下：
```
//写数据函数
//可以替代LCD_WR_DATAX宏,拿时间换空间
//主要用于初始化，这里测试大约可以节约 1.7KB rom空间
//data:寄存器值
void LCD_WR_DATAX(u8 data){
	LCD_RS=1;
	LCD_CS=0;
	DATAOUT(data);
	LCD_WR=0;
	LCD_WR=1;
	LCD_CS=1;
}
```
第三个是 LCD_WR_REG 函数，该函数是通过 8080 并口向 LCD 模块写入寄存器命令，因
为该函数使用频率不是很高，我们不采用宏定义来做（宏定义占用 FLASH 较多），通过 LCD_RS
来标记是写入命令（LCD_RS=0）还是数据（LCD_RS=1）。该函数代码如下：
```
//写寄存器函数
//regval:寄存器值，寄存器值都是8位的
void LCD_WR_REG(u8 regval)
{
	LCD_RS=0;//写地址
 	LCD_CS=0;
	DATAOUT(regval);  // 先写高8位
	LCD_WR=0;
	LCD_WR=1;
 	LCD_CS=1;
}
```
LCD_WriteReg 用于向 LCD 指定寄存器写入指定数据，
```
//写寄存器
//LCD_Reg:寄存器地址
//LCD_RegValue:要写入的数据
void LCD_WriteReg(u8 LCD_Reg, u16 LCD_RegValue)
{
	LCD_WR_REG(LCD_Reg);
	LCD_WR_DATA(LCD_RegValue);
}
```
坐标设置函数。
```
//设置光标位置
//Xpos:横坐标
//Ypos:纵坐标
void LCD_SetCursor(u16 Xpos, u16 Ypos)
{
	LCD_WR_REG(lcddev.setxcmd);
	LCD_WR_DATA(Xpos>>8);
	LCD_WR_DATA(Xpos&0XFF);

	LCD_WR_REG(lcddev.setycmd);
	LCD_WR_DATA(Ypos>>8);
	LCD_WR_DATA(Ypos&0XFF);
}
```
画点函数。先设置坐标，然后往坐标写颜色。其中 POINT_COLOR 是我们
定义的一个全局变量，用于存放画笔颜色。LCD_DrawPoint 函数虽然简单，但是至关重要，其他几乎所有上层函数，都是通过调用这个函数实现的。
```
//画点
//x,y:坐标
//POINT_COLOR:此点的颜色
void LCD_DrawPoint(u16 x,u16 y)
{
	LCD_SetCursor(x,y);		//设置光标位置
	LCD_WriteRAM_Prepare();	//开始写入GRAM
	LCD_WriteRAM(POINT_COLOR);
}

```
字符显示函数 LCD_ShowChar，字符显示函数多了一个功能，就是可以以叠加方式显示，或者以非叠加方式显示。叠加方式显示多用于在显示的图片上再显示字符。非叠加方式一般用于普通的显示。
该函数实现代码如下：
```
//在指定位置显示一个字符
//x,y:起始坐标
//num:要显示的字符:" "--->"~"
//size:字体大小 12/16
//mode:叠加方式(1)还是非叠加方式(0)
void LCD_ShowChar(u16 x,u16 y,u8 num,u8 size,u8 mode)
{
    u16 temp,t1,t;
	u16 y0=y;
	//u16 colortemp=POINT_COLOR;
	//设置窗口
	num=num-' ';//得到偏移后的值
	u8 csize=(size/8+((size%8)?1:0))*(size/2);		//得到字体一个字符对应点阵集所占的字节数

	for(t=0;t<csize;t++)
	{
		if(size==12)temp=asc2_1206[num][t];  //调用1206字体
		else if(size==16) temp=asc2_1608[num][t];		 //调用1608字体
		else if(size==24)temp=asc2_2412[num][t];
		else if(size==32) temp=asc2_3216[num][t];
		else ;
		for(t1=0;t1<8;t1++)
		{
			if(temp&0x80)LCD_Fast_DrawPoint(x,y,POINT_COLOR);
			else if(mode==0)LCD_Fast_DrawPoint(x,y,BACK_COLOR);
			temp<<=1;
			y++;
			if(y>=lcddev.height)return;		//超区域了
			if((y-y0)==size)
			{
				y=y0;
				x++;
				if(x>=lcddev.width)return;	//超区域了
				break;
			}
		}
	}
}

```
在 LCD_ShowChar 函数里面，我们采用快速画点函数 LCD_Fast_DrawPoint 来画点显示字符，该函数同 LCD_DrawPoint 一样，只是带了颜色参数，且减少了函数调用的时间，详见本例程源码。 该代码中我们用到了三个字符集点阵数据数组 asc2_2412、 asc2_1206 和 asc2_1608，都存放在 lcd_font.h  文件中，点阵数据的提取方式该文件中有详细描述。

最后是我们的主函数测试程序：
```
void setup()
{
	delay_init(84); /* 延时函数初始化 */
	LCD_Init();     /* LCD初始化 */

	LCD_DrawLine(10,10,60,100);  // 画线，LCD显示测试用，正式版请删除
    LCD_Clear(BLUE);
    LCD_ShowChar(50,50,'A',16,1);
}

void loop()
{
	
}
```
先初始化，然后画线，清屏，再显示字符。

### ST7789 SPI 四线

当改为 spi 串口驱动时，需要注意屏幕FPC后方的模式选择硬件接口（有的屏幕没有则不需要关注）。
当前使用的屏幕时需要通过调整电阻接线方式来选择屏幕的数据接口位数的。
![图 6](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/FPC接口选择.png)  
当前屏幕出厂默认接口方式如下：
![图 7](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/FPC%E5%AE%9E%E9%99%85%E6%8E%A5%E5%8F%A3.jpg)  
可以发现接线时默认接了 R1、R3、R5的，也就是默认16位驱动。如果我需要SPI串口驱动吗，应该改为R2、R4、R6焊接（此时PIN38-40无用）或者接R7、R8、R9(通过Pin38-40任意调整接口模式)。这里选择后者，这样就可以通过改变外部引脚接线方式改变接口形式，而不需要频繁焊接电阻。

#### 硬件配置

本次驱动采用SPI串口驱动，无触摸（pin1、2、3、4不接），DB0-15可以悬空不用接地。
>注意屏幕背面的电阻（<10R）要仅焊接 R7-9，其余留空。

![图 9](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/LCD_SPI%E6%8E%A5%E7%BA%BF.png)  

#### 初始化配置

沿用上一节并口驱动例程；
- 去除并口例程中的IO驱动引脚配置
- PA0-2 设置位输出引脚
- 添加SPI驱动，配置如下图
![图 8](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/LCD_SPI%E9%85%8D%E7%BD%AE.png)  
- SPI_SCK PA5 接 SCL 
- SPI_MOSI PA7 接 SDA 
- SPI_MISO PA6 接  SDO 
- PB10 LCD背光（实际未接）
- PA0 接 RESET
- PA1 接 CS
- PA2 接 RS

**参考资料**
- [ST7789-STM32](https://github.com/Floyd-Fish/ST7789-STM32/blob/master/ST7789_demo_103c8t6/st7789/st7789.c)
- [分享关于STM32 SPI驱动ST7789 LCD ISP TFT液晶屏幕](https://blog.csdn.net/qq997758497/article/details/105347705)
- [STM32Cube-17 | 使用硬件SPI驱动TFT-LCD（ST7789）](https://cloud.tencent.com/developer/article/1662641)

#### 程序编写

与并口区别主要就是底层数据输出方式，主要函数改动如下：
```

void SPI_WriteByte(u8 Byte)
{
	while(__HAL_SPI_GET_FLAG(&hspi1, SPI_FLAG_TXE) == RESET);//检查接收标志位
	HAL_SPI_Transmit(&hspi1, &Byte, 1, 10);
}

//写数据函数 比放在 .c 文件中减少了33ms显示时间
void LCD_WR_DATA(u8 data){
	LCD_RS=1;
	LCD_CS=0;
	SPI_WriteByte(data);
	LCD_CS=1;
}

//写寄存器函数
//regval:寄存器值，寄存器值都是8位的
void LCD_WR_REG(u8 regval)
{
	LCD_RS=0;//写地址
 	LCD_CS=0;
 	SPI_WriteByte(regval);
 	LCD_CS=1;
}
```
其余初始化及上层画图函数保持不变。

另初始化时要是能SPI
```
void setup()
{
	delay_init(84); /* 延时函数初始化 */
	__HAL_SPI_ENABLE(&hspi1);  // SPI外设使能
	delay_ms(300);
	LCD_Init();//LCD初始化
	delay_ms(1000);
}
```
### ILL9488 SPI驱动

>ILL9488 SPI模式下：数据必须是24位的即RGB666模式。
同时 ILI9488 芯片手册 `Display Data Format`章节中在对SPI的数据数据描述中也仅用3bit和18bie两种形式
一般的颜色数据都是RGB565格式即16位的，所以在使用时需要将16位转为24位的再输出才能正确控制LCD屏。
```
    SPI_WriteByte((color>>8)&0xF8);
    SPI_WriteByte((color>>3)&0xFC);
    SPI_WriteByte(color<<3);
```
>在驱动时间上是16位的1/3倍。而后期使用的LVGL图形库只支持16位或32位格式的颜色，并不支持24位数据，所以无法是哟个
>总之不推荐使用 9488 的屏幕，请使用 ST7796 的替代，此系列可使用16位的SPI模式。

引用外网一句原话：
```
ILI9488 SPI is painful.  You need 3 bytes per pixel.  i.e. 24-bits per pixel.

You can only configure the SAM3X(一款单片机) for 8-16 bits per SPI.    Which works nicely for 565 format 16-bit pixels.   And for DMA.

It would be  pretty straightforward to implement 24-bit pixels on UTFT(一款单片机).    After all,  UTFT is designed to be SLOW.   24-bit pixels can only help to make it SLOWER.

Bodmer（LCD驱动库） supports ILI9488 with TFT_eSPI.   This runs on STM32, ESP8266, ESP32.

I presume that you have already bought your ILI9488 display.   The easiest solution is to buy an STM32, ESP8266 or ESP32 board.

Alternatively,   buy ST7796S or HX8357-D SPI displays.   These support both 16-bit pixels and 24-bit pixels.
Or use ILI9341 SPI displays e.g. with Bodmer's TFT_ILI9341 or Marek's ILI9341_due library.
And of course UTFT supports ILI9341 straight out of the box.
```

```
I have made some heavy modifications, as the typical Adafruit TFT libraries are designed to work with 16bit color (RGB565), and the ILI9488 can only do 24bit (RGB888) color in 4 wire SPI mode. You can still use the library EXACTLY like you would for 16bit mode color, the colors are converted before sending to the display. What this means is, things will be slower than normal. 
```

**参考资料**
- [ili9488还不赖](https://www.eefocus.com/max_huayu/blog/13-03/291938_35e2e.html)
- [有用过ILI9488的RGB接口的朋友吗？ ](https://whycan.com/t_6095.html)
- [jaretburkett/ILI9488](https://github.com/jaretburkett/ILI9488)
- [Ili9488 & lvgl?](https://forum.lvgl.io/t/ili9488-lvgl/2689)
- [Due + ILI9488 SPI - Problem](https://forum.arduino.cc/index.php?topic=683319.0)
- [Need sample code for ILI9488 LCD on SPI Interface](https://www.esp32.com/viewtopic.php?t=1683&start=10)
- [a bug in the ILI9488 driver?](https://community.ugfx.io/topic/1229-a-bug-in-the-ili9488-driver/)
- [3.5寸TFT液晶屏 ILI9488 mcu spi 触摸屏并口/串口模块原子ips全视角液晶屏 显示屏](http://www.lcdgo.com/show-83-93.html)

####  硬件配置

本次驱动采用SPI串口驱动，无触摸（pin1、2、3、4不接），DB0-15可以悬空不用接地。

![图 10](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/ILI9488-SPI%E6%8E%A5%E7%BA%BF.png)  

#### 初始化配置

沿用 ST7789-SPI 的配置，仅改变接线方式
- SPI_SCK PA5 接 SCL 
- SPI_MOSI PA7 接 SDA 
- SPI_MISO PA6 接  SDO 
- PB10 LCD背光（实际未接）
- PA0 接 RESET
- PA1 接 CS
- PA2 接 RS

#### 程序编写

基本和ST7789-SPI驱动类似，不过如开始所述，数据输出要转位24位，所以改动如下；
- 初始化函数改动，这里不再列出，直接看源码即可
注意RGB格式：ILI默认是 BGR 格式，所以 ST7789_MADCTL_RGB 设为 0x08.而ST7789默认是 RGB，随意设为 0x00。
- 数据写入时改为24位：
    ```
    //画点
    //x,y:坐标
    //POINT_COLOR:此点的颜色
    void LCD_DrawPoint(u16 x,u16 y)
    {
        LCD_SetCursor(x,y);		//设置光标位置
        LCD_WriteRAM_Prepare();	//开始写入GRAM
        LCD_WriteRAM(POINT_COLOR);
    }

    //快速画点
    //x,y:坐标
    //color:颜色
    void LCD_Fast_DrawPoint(u16 x,u16 y,u16 color)
    {
        LCD_SetCursor(x,y);		//设置光标位置
        LCD_WriteRAM_Prepare();	//开始写入GRAM
        LCD_WriteRAM(POINT_COLOR);
    }
    ```

>虽然数据接口是24位的，但是在初始化写寄存器时，仍可使用16位，这也就是为什么其它函数未修改的原因。24位格式仅影响 FRAM 的数据输入（即屏幕内部数据缓存），不影响寄存器写入。

**参考资料**
- [LVGL-DemoTest](https://github.com/FASTSHIFT/LVGL-DemoTest/blob/master/LVGL_v7/Libraries/TFT_ILI9488/TFT_ILI9488.cpp)
- [ILI9341(new)SPI library for Due supporting DMA transfer(Uno, Mega,.. compatible)](https://forum.arduino.cc/index.php?topic=265806.0)
- [Adafruit_ILI9341](https://github.com/adafruit/Adafruit_ILI9341/blob/master/Adafruit_ILI9341.cpp)
- [Adafruit-GFX-Library](https://github.com/adafruit/Adafruit-GFX-Library)

### LCD DMA

经实际测试与理论分析：
- DMA可以实现从内容到GPIO的数据控制
- 并口且为普通IO控制时，不适合使用DMA（每传一个数据就要控制WR引脚跳变，就需要没一个字节产生一个DMA中断，程序实现复杂，且速度并无显著提升）
- 并口且为FSMC接口控制是，适合DMA操作（通过FSMC写数据，WR等引脚会自动跳变，无需程序参数，只负责传数据即可，时序由系统自动控制）
- SPI串行接口时，适合DMA，原因同 FSMC 接口。

**参考资料**
- [Connecting a parallel LCD using ST7789 with STM32H743VI](https://electronics.stackexchange.com/questions/485135/connecting-a-parallel-lcd-using-st7789-with-stm32h743vi)
- [STM32 and ILI9341 16bit Parallel](https://www.eevblog.com/forum/microcontrollers/stm32-and-ili9341-16bit-parallel/)
由于这里没有FSMC接口芯片，暂不编写。只编写 SPI 接口的 DMA 例程。
另由于 ILI9488 SPI 数据位24位，而DMA位16位或32位，故该屏的DMA例程不再编写，也不推荐使用该驱动芯片的屏。

#### ST7789_SPI_DMA

##### 硬件配置

硬件配置参考前面的 `ST7789 SPI 四线` 章节内容。

##### 初始化配置

初始化配置基本参考前面的 `ST7789 SPI 四线` 章节内容。仅修改下面内容部分：
- 添加 SPI TX DMA
- 数据位宽 16 位
- SPI中断关闭，DMA中断默认开启

![图 1](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/ST7789_SPI_DMA%E9%85%8D%E7%BD%AE.png)  


##### 程序编写

 这里只编写清屏函数，因为DMA传输实际是要和地址挂钩的（外置Flash存储的图片地址或内部图片数组地址），这里仅演示DMA用法，具体的实际使用在后续例程中在讲解。

```
u16 SendBuff[320*10];  // 缓存数组，10行
static u8 disp_p = 0;  // DMA传输完成标志位


// DMA传输完成回调函数
void HAL_SPI_TxCpltCallback(SPI_HandleTypeDef *hspi)
{
	disp_p = 1;
}

void LCD_DMA_TEST(u16 color)
{
	static int i = 0;

	for(uint32_t j=0 ;j<320*10;j++){
		SendBuff[j] = color;
	}
	LCD_Address_Set(0,0,319,479);//设置显示范围
	LCD_WriteRAM_Prepare();
	LCD_CS =0;

	/* 将spi设置为16位数据传输模式 */
	HAL_SPI_DeInit(&hspi1);
	hspi1.Init.DataSize = SPI_DATASIZE_16BIT;
	HAL_SPI_Init(&hspi1);

	HAL_SPI_Transmit_DMA(&hspi1, (uint8_t *)SendBuff, 320*10);  // 开启一次传输
	while(1){
		if(disp_p){
			disp_p = 0;
			if(i<480/10){ // 一满屏还未传输结束
				i++;
				HAL_SPI_Transmit_DMA(&hspi1, (uint8_t *)SendBuff, 320*10);
			}
			else{
				LCD_CS =1;
				HAL_SPI_DeInit(&hspi1);
				hspi1.Init.DataSize = SPI_DATASIZE_8BIT;
				HAL_SPI_Init(&hspi1);
				i=0;
				break;
			}
		}
	}
}
```

我们定义了一个数组 `SendBuff` 作为数据缓存，并将颜色值写入进去，在开始前我们先将spi的位数转为 16 位，因为DMA的数据位宽是16位的，而LCD初始化配置时是8位的，所以需要切换。

```
/* 将spi设置为16位数据传输模式 */
HAL_SPI_DeInit(&hspi1);
hspi1.Init.DataSize = SPI_DATASIZE_16BIT;
HAL_SPI_Init(&hspi1);
```
然后再开启一轮 DMA 传输，这里该送入数据SendBuff时改为8位，是HAl库的一个bug，但是经对函数内部分析，实际执行时，会将把这两个指针重新变换为( uint16_t *) 。这里8位，但实际还是16位的数据，所以数据宽度仍按16位的计算。

之后会在DMA中断中置位传输完成标志为，循环里检测该标志位，发送下一轮DMA数据，知道一屏幕的数据发送完成，才跳出循环，从而完成刷屏

主程序如下：

```
void setup()
{
	delay_init(84); /* 延时函数初始化 */
	__HAL_SPI_ENABLE(&hspi1);
	delay_ms(300);
	LCD_Init();//LCD初始化
	delay_ms(100);
}


void loop()
{
    LCD_DMA_TEST(WHITE);
	delay_ms(300);
	LCD_Clear(BLUE);
	delay_ms(300);
}
```
这里主程序一个使用DMA刷屏，一个SPI刷屏。可以直观的对比两者速度差异。


这里也可以再配置是将数据位宽改为8位的，不过测试就需要修改以下，主要就是DMA发送数据是的宽度x2，SPI的配置也不要才来回在 8 位和16位之间切换了。
```
// 这是8位的DMA函数
// 需要在初始化配置中将DMA的数据位宽都改为 Byte
u8 SendBuff8[320*10];  // 缓存数组，5行
void LCD_DMA_TEST2(u16 color)
{
	static int i = 0;

	for(uint32_t j=0 ;j<320*10;){  // 一个像素点颜色分两次存储
		SendBuff8[j] = color>>8;
		SendBuff8[j+1] = color&0xFF;
		j += 2;
	}
	LCD_Address_Set(0,0,319,479);//设置显示范围
	LCD_WriteRAM_Prepare();
	LCD_CS =0;

	HAL_SPI_Transmit_DMA(&hspi1, (uint8_t *)SendBuff8, 320*10);  // 开启一次传输
	while(1){
		if(disp_p){
			disp_p = 0;
			if(i<480/10*2){ // 一满屏还未传输结束。这里由于DMA位宽是8位，所以传输次数*2
				i++;
				HAL_SPI_Transmit_DMA(&hspi1, (uint8_t *)SendBuff8, 320*10);
			}
			else{
				LCD_CS =1;
				i=0;
				break;
			}
		}
	}
}
```

**参考资料：**
- [基于stm32 标准库spi驱动st7789(使用DMA)](https://www.codenong.com/cs106298491/)
- [STM32使用SPI DMA加双缓冲区的方式加速LCD显示BMP图片时刷屏速度](https://my.oschina.net/FuXiRuo/blog/471599)
- [cubemx spi 中断_STM32HAL库SPI的16位数据中断发送与接收](https://blog.csdn.net/weixin_39750195/article/details/111751595)
- [STM32F103的GPIO与DMA的终极（没啥用）玩法](http://www.mcublog.cn/stm32/2021_03/stm32f103-gpio-dma/)
- [Using Direct Memory Access (DMA) in STM32 projects](https://embedds.com/using-direct-memory-access-dma-in-stm23-projects/)
- [Particle Photon (STM32F205) DMA Control of GPIO pins](https://www.kasperkamperman.com/blog/particle-photon-stm32f205-dma-control-gpio-pins/)
- [TM32 DMA输出到GPIO问题](https://bbs.21ic.com/icview-368199-1-1.html)
- [STM32并口数据通过DMA传输](https://blog.csdn.net/aaaaa098/article/details/105615573)

## LCD 内部字符显示

使用软件将字符变为数组，存储在内存中，然后调用。
### ASCII 字符

#### 取模
常用ASCII表  
偏移量32  (这句话不知道什么意思)
ASCII字符集（注意首位有个空格不要忘记复制）:

```
 !"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ[\]^_`abcdefghijklmnopqrstuvwxyz{|}~
```

0. 打开PC2LCD2002
1. 模式设为：字符模式
![图 1](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/PC2-%E5%AD%97%E7%AC%A6%E6%A8%A1%E5%BC%8F.png)  

2. 选项设置：
- 阴码+逐列式+顺向+C51格式
- 这里的后缀尽量保持一致，影响最终数组的输出排版和注释
- 点阵：输出的字模每行字节数。大小 = (size/8+((size%8)?1:0))\*(size/2)，size:点阵大小(12/16/24...)。根据计算，12点阵这里应该填写12，但实际测试只要比计算结果大就可以，该值影响数组的输出排版。可以保证每个字符的字符组输出在同一大括号内，不然就会分两行、两个大括号显示，不利于复制使用。
- 索引：每次字模生成在开始时都会有一个索引，但我们不会复制使用这段索引。这里的值也表示每行显示的索引个数，这里任意。
![图 2](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/PC2-%E9%80%89%E9%A1%B9%E8%AE%BE%E7%BD%AE.png)  
3. 选择字体：字宽和字高值和上面的点阵大小保持一致
![图 4](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/PC2-%E5%AD%97%E4%BD%93.png)  
4. 在界面下方输入栏复制粘贴开头的字符集（注意空格）。点击生成字模，然后将字符复制保存，粘贴到程序文件中。
![图 5](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/PC2-%E7%94%9F%E6%88%90%E5%AD%97%E7%AC%A6.png)  
5. 在程序文件中新建数组，名称自定义
    ```
    const unsigned char asc2_1206[95][12]={  // 6x12 宋体
    {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00},/*" ",0*/
    {0x00,0x00,0x00,0x00,0x3E,0x40,0x00,0x00,0x00,0x00,0x00,0x00},/*"!",1*/
    {0x00,0x00,0x20,0x00,0xC0,0x00,0x20,0x00,0xC0,0x00,0x00,0x00},/*""",2*/
    ...
    };
    ```
    数字 95 是固定的，表示95个字符。数字 12 表示字符所占的字节数（也就是下面的行元素个数），计算公式和步骤2一样：
    大小 = (size/8+((size%8)?1:0))\*(size/2)
    12点阵就是12，23点阵就是36.

#### 程序编写
```
/**
 * @brief: 在指定位置显示一个字符
 * @note: 字体色、背景色为宏定义指定
 * @param {u16} x  {u16} y  起始坐标
 * @param {u8} num  要显示的字符，范围：" "--->"~"
 * @param {u8} size 字体大小 12/16/24/32
 * @param {u8} mode  叠加方式(1) ，会导致内容叠加
 *                   非叠加方式(0)，有单独背景，会清除之前内容
 */
void LCD_ShowChar(u16 x,u16 y,u8 num,u8 size,u8 mode)
{
    u16 temp,t1,t;
	u16 y0=y;
	num=num-' ';//得到偏移后的值
	u8 csize=(size/8+((size%8)?1:0))*(size/2);      //得到字体一个字符对应点阵集所占的字节数

	for(t=0;t<csize;t++)  // 轮询单个字符（一行）所有元素
	{
        if(size==12)temp=asc2_1206[num][t];         //查找对应字体
		else if(size==16) temp=asc2_1608[num][t];
		else if(size==24)temp=asc2_2412[num][t];
		else if(size==32) temp=asc2_3216[num][t];
		else ;
		for(t1=0;t1<8;t1++)  // 显示一个元素（每一个位表示一个点）
		{
			if(temp&0x80)LCD_Fast_DrawPoint(x,y,POINT_COLOR);   // 1表示为内容，
			else if(mode==0)LCD_Fast_DrawPoint(x,y,BACK_COLOR); // 0表示无内容（背景）
			temp<<=1;                       // 位偏移，只检测最高位
			y++;                            // 逐列式：从上至下，高位在前（与取模软件一致） 
			if(y>=lcddev.height)return;		// 超区域了
			if((y-y0)==size)                // 一列显示完成
			{
				y=y0;
				x++;
				if(x>=lcddev.width)return;	// 超区域了
				break;
			}
		}
	}
}
/**
 * @brief: m^n函数
 * @param {u8} m 底数; {u8} n 指数
 * @retval: m^n次方.
 */
u32 LCD_Pow(u8 m,u8 n)
{
	u32 result=1;
	while(n--)result*=m;
	return result;
}

/**
 * @brief: 显示数字,高位为0,则不显示
 * @note: 非叠加方式显示，字体色、背景色为宏定义指定
 * @param {u16} x  {u16} y  起始坐标
 * @param {u32} num  数值(0~4294967295);
 * @param {u8} len  数字的个数
 * @param {u8} size  字体大小 12/16/24/32
 */
void LCD_ShowNum(u16 x,u16 y,u32 num,u8 len,u8 size)
{
	u8 t,temp;
	u8 enshow=0;
	for(t=0;t<len;t++)
	{
		temp=(num/LCD_Pow(10,len-t-1))%10;
		if(enshow==0&&t<(len-1))
		{
			if(temp==0)
			{
				LCD_ShowChar(x+(size/2)*t,y,' ',size,0);
				continue;
			}else enshow=1;

		}
	 	LCD_ShowChar(x+(size/2)*t,y,temp+'0',size,0);
	}
}

/**
 * @brief: 显示数字
 * @note: 字体色、背景色为宏定义指定
 * @param {u16} x  {u16} y  起点坐标
 * @param {u32} num  数值(0~999999999);
 * @param {u8} len 长度(即要显示的位数)
 * @param {u8} size  字体大小
 * @param {u8} mode [7]:0,不填充;1,填充0.
 *                  [6:1]:保留
 *                  [0]:0,非叠加显示;1,叠加显示.
 */
void LCD_ShowxNum(u16 x,u16 y,u32 num,u8 len,u8 size,u8 mode)
{
	u8 t,temp;
	u8 enshow=0;
	for(t=0;t<len;t++)
	{
		temp=(num/LCD_Pow(10,len-t-1))%10;
		if(enshow==0&&t<(len-1))
		{
			if(temp==0)
			{
				if(mode&0X80)LCD_ShowChar(x+(size/2)*t,y,'0',size,mode&0X01);
				else LCD_ShowChar(x+(size/2)*t,y,' ',size,mode&0X01);
 				continue;
			}else enshow=1;

		}
	 	LCD_ShowChar(x+(size/2)*t,y,temp+'0',size,mode&0X01);
	}
}

/**
 * @brief: 显示字符串
 * @note: 字体色、背景色为宏定义指定
 * @param {u16} x  {u16} y 起点坐标
 * @param {u16} width  {u16} height  区域大小
 * @param {u8} size  字体大小
 * @param {u8} *p  字符串起始地址
 * @param {u8} mode  叠加方式(1) ，会导致内容叠加
 *                   非叠加方式(0)，有单独背景，会清除之前内容
 */
void LCD_ShowString(u16 x,u16 y,u16 width,u16 height,u8 size,u8 *p,u8 mode)
{
	u8 x0=x;
	width+=x;
	height+=y;
    while((*p<='~')&&(*p>=' '))//判断是不是非法字符!
    {
        if(x>=width){x=x0;y+=size;}
        if(y>=height)break;//退出
        LCD_ShowChar(x,y,*p,size,mode?1:0);
        x+=size/2;
        p++;
    }
}
```

主程序:
```
// C 不支持字符串
char char_test1[8] = {'C', 'T', 'E', 'S', 'T', '!', '1','\0'};  // 由于数组的末尾存储了空字符，所以字符数组的大小比字符串多一个空字符结尾。
char char_test2[] = "CTEST!2"; // 与上面等同，char_test2[0] = 'C'

void setup()
{

	delay_init(84); /* 延时函数初始化 */

	__HAL_SPI_ENABLE(&hspi1);
	delay_ms(300);
	LCD_Init();//LCD初始化
	delay_ms(100);

	LCD_ShowString(10,10,100,16,12,char_test1,1);
	LCD_ShowChar(10,30,char_test2[6],12,1);
	LCD_ShowString(10,50,100,16,12,&char_test2[0],1);
	LCD_ShowString(10,70,100,16,12,"CTEST!3",1);
}
```

### 中文字符

#### 取模
0. 打开PC2LCD2002
1. 模式设为：字符模式
![图 1](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/PC2-%E5%AD%97%E7%AC%A6%E6%A8%A1%E5%BC%8F.png)  

2. 选项设置：
- 阴码+逐列式+顺向+C51格式
- 这里的后缀尽量保持一致，影响最终数组的输出排版和注释
- 点阵：输出的字模每行字节数。大小 = (size/8+((size%8)?1:0))\*(size)，size:点阵大小(12/16/24...)。根据计算，12点阵这里应该填写12，但实际测试只要比计算结果大就可以，该值影响数组的输出排版。可以保证每个字符的字符组输出在同一大括号内，不然就会分两行、两个大括号显示，不利于复制使用。
- 索引：每次字模生成在开始时都会有一个索引，但我们不会复制使用这段索引。这里的值也表示每行显示的索引个数，这里任意。
![![图 2](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/PC2-%E9%80%89%E9%A1%B9%E8%AE%BE%E7%BD%AE.png)  1](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/PC2-%E6%B1%89%E5%AD%97%E9%80%89%E9%A1%B9%E9%85%8D%E7%BD%AE.png)  
3. 选择字体：字宽和字高值和上面的点阵大小保持一致
![图 4](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/PC2-%E5%AD%97%E4%BD%93.png)  
4. 然后在输入栏输入汉字，点击 “生成字模”，生成的字模如下
![图 7](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/PC2-%E6%B1%89%E5%AD%97%E7%94%9F%E6%88%90.png)  
5. 然后将字模复制到程序文件中，新建数组 
    >注：数组为一维数组。每个字的字模前需要添加对应的汉字
    ```
    typedef struct
    {
        unsigned char Index[2];
        unsigned char Msk[24];
    }typFNT_GB12;

    const typFNT_GB12 tfont12_cn[] = {
        "中",0x00,0x00,0x1F,0x80,0x11,0x00,0x11,0x00,0x11,0x00,0xFF,0xF0,0x11,0x00,0x11,0x00,0x11,0x00,0x1F,0x80,0x00,0x00,0x00,0x00,/*"中",0*/
        "文",0x20,0x10,0x20,0x10,0x38,0x20,0x26,0x20,0xA1,0x40,0x60,0x80,0x21,0x40,0x26,0x20,0x38,0x20,0x20,0x10,0x20,0x10,0x00,0x00,/*"文",1*/
        "字",0x30,0x80,0x20,0x80,0x24,0x80,0x24,0x90,0xA4,0x90,0x64,0xF0,0x25,0x80,0x26,0x80,0x24,0x80,0x20,0x80,0x30,0x80,0x00,0x00,/*"字",2*/
        "体",0x08,0x00,0x3F,0xF0,0xC0,0x80,0x11,0x00,0x12,0x40,0x14,0x40,0xFF,0xF0,0x14,0x40,0x12,0x40,0x11,0x00,0x00,0x80,0x00,0x00,/*"体",3*/
        "测",0x44,0x20,0x22,0x40,0x7F,0x90,0x40,0x20,0x5F,0xC0,0x40,0x20,0x7F,0x90,0x00,0x00,0x3F,0x80,0x00,0x10,0xFF,0xF0,0x00,0x00,/*"测",4*/
        "试",0x88,0x00,0x4F,0xF0,0x00,0x20,0x00,0x00,0x24,0x20,0x27,0xE0,0x24,0x40,0x20,0x00,0xFF,0xC0,0x20,0x20,0xA0,0x70,0x00,0x00,/*"试",5*/
    };
    ```

#### 程序编写

```
/**
 * @brief: 显示汉字
 * @note: 字体色、背景色为宏定义指定
 * @param {u16} x  {u16} y  起点坐标
 * @param {u8} *s  字符地址
 * @param {u8} size  字体大小
 * @param {u8} mode  叠加方式(1) ，会导致内容叠加
 *                   非叠加方式(0)，有单独背景，会清除之前内容
 */
void LCD_ShowChinese(u16 x,u16 y,u8 *s,u8 size,u8 mode)
{
	u16 temp,t1,t;
	u16 k;
	u16 y0=y;

	u8 csize=(size/8+((size%8)?1:0))*size;  //得到字体一个字符对应点阵集所占的字节数

	u16 HZnum=sizeof(tfont12_cn)/sizeof(typFNT_GB12);	//统计汉字数目
	for(k=0;k<HZnum;k++)  // 查找汉字在数组中的位置
	{
		if((tfont12_cn[k].Index[0]==*(s))&&(tfont12_cn[k].Index[1]==*(s+1)))
		{
			break;       //查找到对应点阵字库立即退出，防止多个汉字重复取模带来影响
		}
	}
	for(t=0;t<csize;t++)
	{
		if(size==12)temp=tfont12_cn[k].Msk[t];  //调用1206字体
		//else if(size==16) temp=tfont16_cn[k].Msk[0];		 //调用1608字体
		//else if(size==24)temp=tfont24_cn[k].Msk[0];
		//else if(size==32) temp=tfont32_cn[k].Msk[0];
		else ;
		for(t1=0;t1<8;t1++)
		{
			if(temp&0x80)LCD_Fast_DrawPoint(x,y,POINT_COLOR);  // 为1表示为内容，为0表示无内容（背景）
			else if(mode==0)LCD_Fast_DrawPoint(x,y,BACK_COLOR); // 无内容（背景）处理
			temp<<=1;
			y++;
			if(y>=lcddev.height)return;		//超区域了
			if((y-y0)==size)
			{
				y=y0;
				x++;
				if(x>=lcddev.width)return;	//超区域了
				break;
			}
		}
	}
}

/** 
 * @brief:  显示汉字串
 * @note: 字体色、背景色为宏定义指定
 * @param {u16} x  {u16} y  起点坐标
 * @param {u8} *s  字符地址
 * @param {u8} size  字体大小
 * @param {u8} mode  叠加方式(1) ，会导致内容叠加
 *                   非叠加方式(0)，有单独背景，会清除之前内容
 */
void LCD_ShowChineseString(u16 x,u16 y,u8 *s,u8 size,u8 mode)
{
	while(*s!=0)
	{
		LCD_ShowChinese(x,y,s,size,mode);
		s+=3;  // GCC UTF-8 编码，中文字符位3个字节表示。GBK编码则是2个字节
		x+=size;
	}
}

```


主程序：
```
void setup()
{

	delay_init(84); /* 延时函数初始化 */

	__HAL_SPI_ENABLE(&hspi1);
	delay_ms(300);
	LCD_Init();//LCD初始化
	delay_ms(100);
	LCD_ShowChinese(10,90,"中",12,1);
}

```
这里特别说明一下 ` LCD_ShowChineseString` 函数中 s 指针偏移为3，是因为stm32cubeide默认编码是 utf-8，所以导致主程序中的中文会使用**三个字节表示**。

另外我们可以将包含中文字符的源文件格式修改为 GBK 格式，则这里 s偏移就需要修改为2。偏移为2是大多数教程例程的写法，因为他们使用的IDE编码就是GBK格式。这里为了统一、同时也为了兼容后续的中文字库程序，还是修改一下。

1. 使用 vscode 打开该文件，并在vscode右小角单击 UTF-8，在命令栏中选择 `通过编码重新保存`,并选择 GB 2312 格式，保存退出
![图 6](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/vscode%E6%9B%B4%E6%94%B9%E7%BC%96%E7%A0%81%E6%A0%BC%E5%BC%8F.png) 
2. 右键包含中文的文件，单击 `Properties`
3. 在弹出界面中，修改文件编码格式为 GBK。
![图 5](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/%E6%96%87%E4%BB%B6%E7%BC%96%E7%A0%81%E6%A0%BC%E5%BC%8F%E4%BF%AE%E6%94%B9.png)  
4. 记得将上面程序的偏移改为2。则可以下载使用了。

### 图片显示

#### 取模
1. 打开 Img2LCD 软件
2. 打开要取模的图片：以例程的40x40企鹅图片为例
3. 观察左下角的输入图像，如果显示无效的输入图像，那么请使用电脑自带的画图软件将图片转化为16色位的bmp格式图片。
![图 8](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/img-%E6%97%A0%E6%95%88%E5%9B%BE%E5%83%8F.png)  
4. 打开图片后 设置如下
- 输出数据类型：C语言数组
- 扫描模式：水平
- 输出灰度：16位真彩色
- 尺寸请和实际尺寸一致，
  此软件只能缩小图片不能放大图片！缩小是等比例缩小！
  设置好后点击一下![图 10](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/pic_1618883556757.png)  

- 高位在前，其余不勾选

![图 9](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/img-%E8%AE%BE%E7%BD%AE.png)  
5. 然后点击保存，将生成的数组复制到到例程文件内
![图 11](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/img-%E4%BF%9D%E5%AD%98.png)  

#### 程序编写
```
/**
 * @brief: 显示图片
 * @param x,y起点坐标
 * @param {u16} length  图片长度
 * @param {u16} width  图片宽度
 * @param {const u8} pic  图片数组
 */
void LCD_ShowPicture(u16 x,u16 y,u16 length,u16 width,const u8 pic[])
{
	u16 i,j;
	u32 k=0;
    LCD_Set_Window(x,y,length,width);
	for(i=0;i<length;i++)
	{
		for(j=0;j<width;j++)
		{
            LCD_WR_DATA(pic[k*2]);
	        LCD_WR_DATA(pic[k*2+1]);
			k++;
		}
	}
}
```
**主程序**
```
void setup()
{

	delay_init(84); /* ???????????? */

	__HAL_SPI_ENABLE(&hspi1);
	delay_ms(300);
	LCD_Init();//
	delay_ms(100);

    delay_ms(500);
    LCD_ShowPicture(0,0,40,40,gImage_3);

}
```

### 显示flash中的图片

由于图片一般较大，我们一般会将图片存储在外置flash中。

#### 取模

取模方式与上文基本一致，仅需要将输出类型改为 `.bin` 格式。

#### 程序编写

该程序需要使用 usb u功能和fatf是文件系统。请参考`usb U盘` 和 `fatfs文件系统移植` 两个章节内容。这里只讲显示函数编写。

```
uint8_t pic[ 352]; /* 一行真彩色数据缓存 176 * 2 = 352 */
void lcd_show_pic_flash(u16 x,u16 y,u16 length,u16 width,const char* path)
{
	uint16_t k = 0;
	UINT num;
	u16 i,j;
	LCD_Set_Window(x,y,length,width);
	/* Register the file system object to the FatFs module */
	if(f_mount(&USERFatFS, "0:", 0) == FR_OK)
	{
		//* Create and Open a new text file object with write access */
		if(f_open(&USERFile, path, FA_READ) == FR_OK) // FA_WRITE  identity_true_blue.bin
		{
			for(i=0;i<length;i++)
			{
				//f_lseek(&USERFile,i*width*2);
				f_read(&USERFile,pic,width*2,&num);
				k = 0;
				for(j=0;j<width;j++)
				{
					LCD_WR_DATA(pic[k*2]);
					LCD_WR_DATA(pic[k*2+1]);
					k++;
				}
			}
			f_close(&USERFile); // 读完后关闭点阵字库文件
		}
		else{
			lcd_show_str(50,50,200,35,(u8 *)"bin open fail",16,1);
		}

		f_mount(NULL, "0:", 0);
	}
	LCD_Set_Window(0,0,lcddev.width,lcddev.height);
}
```
主函数
```
lcd_show_pic_flash(0,0,240,240,"img_test.bin");
```
下载程序之后，将bin格式图片文件通过usb连接放到flash中。 名称要和程序中的一致 `img_test.bin`。之后程序会查找该文件，并显示。

### 显示falsh中的中文字符

#### 字库制作

1. 打开点阵字库软件 ts3
2. 选择字体。 字符集实测西欧语言和GB2312都可以。这里的字体大小设置于最终字库无关，字体大小由下面几个步骤决定。
![图 7](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/%E4%B8%AD%E6%96%87%E5%AD%97%E5%BA%93%E5%AD%97%E4%BD%93%E9%80%89%E6%8B%A9.png)  
3. 设置宽、高（点阵大小）。字体大小设置请根据实际适应，保证字在方框中即可。一般16点阵字体大小12，24点阵字体大小18，32点阵字体大小24
4. 横向、纵向偏移根据预览调整，保证字体居中方框即可
5. 模式设置，设置`纵向取模方式二`（根据程序适配）
6. 点击创建保存，保存为 GBK16.DZK 。这里的命名适合下面的程序保持一致的，可以自定义，两者保持一致即可。

![图 4](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/%E4%B8%AD%E6%96%87%E5%AD%97%E5%BA%93%E5%88%B6%E4%BD%9C%E9%85%8D%E7%BD%AE.png)  

#### 程序编写

该程序需要使用 usb u功能和fatf是文件系统。请参考`usb U盘` 和 `fatfs文件系统移植` 两个章节内容。这里只讲显示函数编写。
```
//code 字符指针开始
//从字库中查找出字模
//code 字符串的开始地址,GBK码
//mat  数据存放地址 (size/8+((size%8)?1:0))*(size) bytes大小
//size:字体大小
void Get_HzMat(unsigned char *code,unsigned char *mat,u8 size)
{
	 char path[20];
	UINT num;

	unsigned char qh,ql;
	unsigned char i;
	unsigned long foffset;
	u16 csize=(size/8+((size%8)?1:0))*(size);//得到字体一个字符对应点阵集所占的字节数
	qh=*code;
	ql=*(++code);
	if(qh<0x81||ql<0x40||ql==0xff||qh==0xff)//非 常用汉字
	{
	    for(i=0;i<csize;i++)*mat++=0x00;//填充满格
	    return; //结束访问
	}
	if(ql<0x7f)ql-=0x40;//注意!
	else ql-=0x41;
	qh-=0x81;
	foffset=((unsigned long)190*qh+ql)*csize;	//得到字库中的字节偏移量
	sprintf(path,"GBK%d.DZK",size);

	/* Register the file system object to the FatFs module */
	if(f_mount(&USERFatFS, "0:", 0) == FR_OK)
	{
		//* Create and Open a new text file object with write access */
		if(f_open(&USERFile, path, FA_READ) == FR_OK) // FA_WRITE  identity_true_blue.bin
		{
			 f_lseek(&USERFile,foffset);
			 f_read(&USERFile,mat,csize,&num);
			f_close(&USERFile); // 读完后关闭点阵字库文件
		}
		f_mount(NULL, "0:", 0);
	}
}
//显示一个指定大小的汉字
//x,y :汉字的坐标
//font:汉字GBK码
//size:字体大小
//mode:0,正常显示,1,叠加显示
void Show_Font(u16 x,u16 y,u8 *font,u8 size,u8 mode)
{
	u8 temp,t,t1;
	u16 y0=y;
	u8 dzk[512];
	u16 csize=(size/8+((size%8)?1:0))*(size);//得到字体一个字符对应点阵集所占的字节数
	if(size!=12&&size!=16&&size!=24&&size!=32)return;	//不支持的size
	Get_HzMat(font,dzk,size);	//得到相应大小的点阵数据
	for(t=0;t<csize;t++)
	{
		temp=dzk[t];			//得到点阵数据
		for(t1=0;t1<8;t1++)
		{
			if(temp&0x80)LCD_Fast_DrawPoint(x,y,POINT_COLOR);
			else if(mode==0)LCD_Fast_DrawPoint(x,y,BACK_COLOR);
			temp<<=1;
			y++;
			if((y-y0)==size)
			{
				y=y0;
				x++;
				break;
			}
		}
	}
}

//在指定位置开始显示一个字符串
//支持自动换行
//(x,y):起始坐标
//width,height:区域
//str  :字符串
//size :字体大小
//mode:0,非叠加方式;1,叠加方式
void lcd_show_str(u16 x,u16 y,u16 width,u16 height,u8*str,u8 size,u8 mode)
{

	u16 x0=x;
	u16 y0=y;
    u8 bHz=0;     //字符或者中文
    while(*str!=0)//数据未结束
    {
        if(!bHz)
        {
	        if(*str>0x80)bHz=1;//中文
	        else              //字符
	        {
                if(x>(x0+width-size/2))//换行
				{
					y+=size;
					x=x0;
				}
		        if(y>(y0+height-size))break;//越界返回
		        if(*str==13)//换行符号
		        {
		            y+=size;
					x=x0;
		            str++;
		        }
		        else LCD_ShowChar(x,y,*str,size,mode);//有效部分写入
				str++;
		        x+=size/2; //字符,为全字的一半
	        }
        }else//中文
        {
            bHz=0;//有汉字库
            if(x>(x0+width-size))//换行
			{
				y+=size;
				x=x0;
			}
	        if(y>(y0+height-size))break;//越界返回
	        Show_Font(x,y,str,size,mode); //显示这个汉字,空心显示
	        str+=2;
	        x+=size;//下一个汉字偏移
        }
    }

}
```
主程序调用：
```
lcd_show_str(10,90,100,16,"中文字体测试",16,1);	//在指定位置显示一个字符串
```
这里说明以下，stm32cubeide默认编码是 utf-8，所以main.c 中文会无法显示，需要先将包含中文（注释不算）的程序源文件拜编码格式修改为 GBK 格式，才能下载使用。修改方式见上文 `中文字符` 章节

将上述生成的字库通过usb保存到flash中。`Get_HzMat` 函数中会查找该字库，请保持名称一致。

### 图片显示 DMA
```
/**
 * @brief: 显示图片
 * @param x,y起点坐标
 * @param {u16} length  图片长度
 * @param {u16} width   图片宽度
 * @param {const u8} pic  图片数组
 * @detail 由于HAL_SPI_Transmit_DMA的数据size大小是16位的，所以如果满屏刷新 240x320 > 2^16，则需要分多次显示（屏幕小的则不需要）。
 *                 每次传输后都必须等待完成，才能开启下一次传输。
 */
void LCD_ShowPicture(u16 x,u16 y,u16 length,u16 width,const u8 pic[])
{
	uint8_t *p = gImage_3;
	uint32_t i;
	u32 num1=(length)*(width)*2;
	LCD_Set_Window(x,y,length,width);
	LCD_CS=0;
    u32 num_i = num1/65535;
    u32 num_j = num1%65535;
    if(num_i != 0){
    	for(i=0;i<num_i;i++)
    	{
    			while(disp_p==0);
    			disp_p = 0;
    			HAL_SPI_Transmit_DMA(&hspi1, (uint8_t  *)(p), 65535);
    			p += 65535;
    	}
    }

	if(num_j != 0){
		while(disp_p==0);
		disp_p = 0;
		HAL_SPI_Transmit_DMA(&hspi1, (uint8_t  *)(p), num_j);
	}
}
```
这里唯一注意的点就是DMA传输的数据大小是16位的，也就是一次只能传出最多65535个数据。所以对于大的图片需要分次显示。
**主函数:**
```
void setup()
{

	delay_init(84); /* ???????????? */

	__HAL_SPI_ENABLE(&hspi1);
	delay_ms(300);
	LCD_Init();//
	delay_ms(100);

    delay_ms(500);
    LCD_ShowPicture(0,0,40,40,gImage_3);

}
```
### DMA显示flash中的图片

#### 取模

取模方式与上文基本一致，仅需要将输出类型改为 `.bin` 格式。

#### 程序编写

程序结合 `显示flash中的图片` 和 `图片显示 DMA` 两个章节。
```
/**
 * @brief: 显示图片
 * @param x,y起点坐标
 * @param {u16} length  图片长度
 * @param {u16} width   图片宽度
 * @param {const u8} pic  图片数组
 * @detail 由于HAL_SPI_Transmit_DMA的数据size大小是16位的，所以如果满屏刷新 240x320 > 2^16，则需要分多次显示（屏幕小的则不需要）。
 *                 每次传输后都必须等待完成，才能开启下一次传输。
 */
#define pic_buffer_size (LCD_WIDTH*20)
uint8_t pic_buffer[pic_buffer_size]; /* 一行真彩色数据缓存 176 * 2 = 352 */
void lcd_show_pic_flash_dma(u16 x,u16 y,u16 length,u16 width,const char* path)
{
	uint32_t i;
	UINT num;
	u32 num1=(length)*(width)*2; // 两个字节一个像素
    u32 num_i = num1/pic_buffer_size;
    u32 num_j = num1%pic_buffer_size;
	LCD_Set_Window(x,y,length,width);

	/* Register the file system object to the FatFs module */
	if(f_mount(&USERFatFS, "0:", 0) == FR_OK)
	{
		//* Create and Open a new text file object with write access */
		if(f_open(&USERFile, path, FA_READ) == FR_OK) // FA_WRITE  identity_true_blue.bin
		{
	    	for(i=0;i<num_i;i++)
	    	{
				//f_lseek(&USERFile,i*width*2);
	    		f_read(&USERFile,pic_buffer,pic_buffer_size,&num);
	    		LCD_CS=0;
	    		disp_p = 0;
				HAL_SPI_Transmit_DMA(&hspi1,pic_buffer, pic_buffer_size);
    			while(disp_p==0);
    			LCD_CS=1;

			}
	    	if(num_j != 0){
	    		f_read(&USERFile,pic_buffer,num_j,&num);
	    		LCD_CS=0;
	    		disp_p = 0;
	    		HAL_SPI_Transmit_DMA(&hspi1,pic_buffer, num_j);
	    		while(disp_p==0);
	    		LCD_CS=1;
	    	}
			f_close(&USERFile); // 读完后关闭点阵字库文件
		}
		else{
			lcd_show_str(50,50,200,35,(u8 *)"bin open fail",16,1);
		}

		f_mount(NULL, "0:", 0);
	}
	//LCD_Set_Window(0,0,lcddev.width,lcddev.height);

}
```

定义缓存数组`pic_buffer`，存储读取的图片数据。DMA负责显示，检查DMA完成回调函数，判断下一次显示执行。
>这里flash和lcd共用一个SPI接口，导致DMA显示时，无法读取falsh，如果时分开的接口，则可以实现DMA发送显示同时，读取下一次的图片数据，再判断DMA传输完成，这样可以加快显示速度。少了一个等待flash读取时间。

主程序：

```
lcd_show_pic_flash_dma(0,0,240,240,"img_test.bin");
```

## LCD触摸

## LVGl 移植


# LL库

[STM32LL库系列教程（一）—— LL库概览及资料](https://zhuanlan.zhihu.com/p/347459515)
[【stm32cubemx专题教程】ST全外设原理、配置、API使用详解](https://www.bilibili.com/video/BV1Tv411B7Uw)
[STM32 之十一 LL 库（low-layer drivers）详解 及 移植说明](https://blog.csdn.net/ZCShouCSDN/article/details/104174662)
标准库官方已经不更新了，虽然资料很多，所以不再使用。之后学习使用了HAL库，但最近做项目需要使用16和32KB的STM32F0芯片，使用HAL库新建个工程再加上串口，基本就是10KB+了，所以也是被迫重新选择了LL库。

下面是别人做的一个不同编程方式的效率对比：
原文链接：[https://blog.csdn.net/super828/article/details/79078693](https://blog.csdn.net/super828/article/details/79078693)

<!--more-->

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/标准库与Cube,LL,直接写寄存器的效率对比.png)

总的来说：代码效率与移植性成反比的规律是明显的。与HAL相比，LL的效率优势很明显，几乎和直接写寄存器的效率相差无几。而且目前STM32cubeIDE已经支持直接生成LL工程，对于追求效率的开发应用人员来说，非常值得推荐大家使用。


## GPIO操作

配置操作基本和前面的 HAL 一样，只有一处不同，驱动库选择 LL 库，如图所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/advanced_settings.png)

示例：
```
LL_GPIO_SetOutputPin(GPIOB, LL_GPIO_PIN_3); // PB3输出高电平
LL_GPIO_ResetOutputPin(GPIOB, LL_GPIO_PIN_3); // PB3输出低电平
LL_GPIO_TogglePin(GPIOB, LL_GPIO_PIN_3);    /* 翻转PB3输出电平
LL_GPIO_ReadInputPort(GPIO_TypeDef \*GPIOx);  /* 读取引脚电平状态 */

LL_mDelay(500); // ms延时，延时500ms
```
API详细使用请参考官方驱动描述手册：`Description of STM32F4 HAL and low-layer drivers`
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/STM32学习笔记-基于STM32CubeIDE/库参考手册.png)

## USART

不论是重定义和自定义printf函数，若想打印float类型，都需要再IDE中单独开启，否则无法打印，且额外占用内存 18 KB 左右。

http://begild.top/article/a854db16.html

### printf重定义

```c++
/* uart.c */
#ifdef __GNUC__
#define PUTCHAR_PROTOTYPE int __io_putchar(int ch) /* 防⽌重定义， 具体为什么会⽤到GNUC我以
为不知道*/
#else
#define PUTCHAR_PROTOTYPE int fputc(int ch, FILE *f)
#endif
PUTCHAR_PROTOTYPE{
    //LL_USART_TransmitData8(USART1,Usart1_TxBuff[i]);
    USART1->TDR = ch;
    //while(!LL_USART_IsActiveFlag_TXE(USART1));
    while((USART1->ISR&0X40)==0);
return ch;
}
/* main.c*/
printf("233\n"); /* 串⼝打印数据 */

### printf自己编写

/* uart.c */
#include "stdio.h"      //
#include "stdarg.h"        //
#include "string.h"     //
char Usart1_TxBuff[256];
void my_printf(char* fmt,...)
{
    unsigned int i,length;
    va_list ap;
    va_start(ap,fmt);
    vsprintf(Usart1_TxBuff,fmt,ap);
    va_end(ap);
    length=strlen((const char*)Usart1_TxBuff);
    while(!LL_USART_IsActiveFlag_TXE(USART1));
    //while((USART1->ISR&0X40)==0);
    for(i = 0; i < length; i ++)
    {
        
        LL_USART_TransmitData8(USART1,Usart1_TxBuff[i]);
        //USART1->TDR = Usart1_TxBuff[i];
        while(!LL_USART_IsActiveFlag_TXE(USART1));
        //while((USART1->ISR&0X40)==0);
    }
}
/* main.c*/
my_printf("233\n"); /* 串⼝打印数据 */
```
>备注：
1、知识点：va_list
3、自己编写 printf 函数比重定义节省 0.46 KB，但RAM增加了。

其他发送方法

1、只发送字符串数据

```c++
/* uart.c中定义 */
void USART_Print(unsigned char *Send_Text,uint32_t Size_Text)
{
  uint32_t index = 0;
 
  for (index = 0; index < Size_Text; index++)
  {
    while (!LL_USART_IsActiveFlag_TXE(USART1));
     LL_USART_TransmitData8(USART1,Send_Text[index]);
  }
  while (!LL_USART_IsActiveFlag_TC(USART1));
}
/* 使用 */
USART_Print("Ready for Tx\r\n",(uint32_t) sizeof("Ready for Tx\r\n") );
一般接受中断

/* stm32f0xx_it.c声明中断函数 */
void USART1_IRQHandler(void)
{
  /* USER CODE BEGIN USART1_IRQn 0 */
    USART_RxIdleCallback();
  /* USER CODE END USART1_IRQn 0 */
  /* USER CODE BEGIN USART1_IRQn 1 */
  /* USER CODE END USART1_IRQn 1 */
}
/* uart.c中定义 */
void USART_RxIdleCallback(void)
{
    uint8_t tmp;
    if(LL_USART_IsActiveFlag_RXNE(USART1)) //接收中断
    {
        tmp=LL_USART_ReceiveData8(USART1);   //读取出来接收到的数据
        LL_USART_TransmitData8(USART1,tmp);  //把数据再从串口发送出去
    }
}
/* uart.h声明 */
void USART_RxIdleCallback(void);
/* 使用 */
// main.c初始化使能接受中断
LL_USART_EnableIT_RXNE(USART1);
DMA接受中断


/* uart.c中定义 */
void USART_DMA_CONFIG(void)
{
    LL_DMA_SetPeriphAddress(DMA1, LL_DMA_CHANNEL_5, (uint32_t)(&USART1->RDR));// LL_USART_DMA_GetRegAddr(USART1->DR));
    LL_DMA_SetMemoryAddress(DMA1, LL_DMA_CHANNEL_5, (uint32_t)Usart1_RxBuff);
    LL_DMA_SetDataLength(DMA1, LL_DMA_CHANNEL_5, 255);
    LL_DMA_EnableIT_TC(DMA1, LL_DMA_CHANNEL_5);
    LL_DMA_EnableChannel(DMA1, LL_DMA_CHANNEL_5);
    LL_USART_EnableDMAReq_RX(USART1);
    LL_USART_EnableIT_IDLE(USART1);
}
void USART_RxIdleCallback(void)
{
    uint8_t cnt;
    if(LL_USART_IsActiveFlag_IDLE(USART1))
    {
        LL_DMA_DisableChannel(DMA1, LL_DMA_CHANNEL_5); //
        cnt = LL_DMA_GetDataLength(DMA1,LL_DMA_CHANNEL_5);
        u1_printf("data len is:%d\r\n",cnt);
        u1_printf("data rx is:%s\r\n",Usart1_RxBuff);
        LL_DMA_SetDataLength(DMA1, LL_DMA_CHANNEL_5, 255); 
        LL_DMA_EnableChannel(DMA1, LL_DMA_CHANNEL_5);
        LL_USART_ClearFlag_IDLE(USART1);
    }
}
/* uart.h声明 */
void USART_RxIdleCallback(void);
/* 使用 */
// main.c初始化DMA
USART_DMA_CONFIG();
```
>备注：
不可使用DMA3，会出现 "VDD VALUE" redefined 错误。
只要配置了DMA，延时函数LL_mDelay()失效

总结

## ADC

```c++
HAL_ADCEx_Calibration_Start(&hadc);
HAL_ADC_Start(&hadc);
uint16_t ADC_temp1=0;
HAL_ADC_PollForConversion(&hadc,10);
if(HAL_IS_BIT_SET(HAL_ADC_GetState(&hadc),HAL_ADC_STATE_REG_EOC))
    ADC_temp1=HAL_ADC_GetValue(&hadc);//0-4095
return ADC_temp1;
float updateTemperaturesFromRawValues(void)
{
static unsigned char temp_count = 0;
static unsigned long raw_temp_0_value = 0;

  temp_count++;
  raw_temp_0_value +=(ADC_Demo2()>>2);
  if(temp_count >= 16) // per 16 times 锛宑aculate once temp
  {
      temp_count = 0;
      current_temperature = analogtemp(raw_temp_0_value);
  }
  return current_temperature;
}
```


### 参考链接

https://zhuanlan.zhihu.com/p/133874308







 
















 










