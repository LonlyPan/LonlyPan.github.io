---
title: "ATSAME54 SERCOM硬件设计问题"
index_img: https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/ATSAME54_SERCOM硬件设计问题/ATSAME54.png
data: 2019-11-20 16:2
hide: false
# sticky: 100 #置顶，数字越大越靠前
# banner_img: #/img/post_banner.jpg
# comment: false
categories: 01-专业
---



## USART引脚&问题描述

官方芯片手册 ==SAM D5x/E5x Family Data Sheet== 在描述**SERCOM  USART**章节的**信号说明 Signal_Description**小结处，对引脚信号只给了一个表格，如下：

![USART_Signal_Description_](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/ATSAME54_SERCOM硬件设计问题/USART_Signal_Description_.png)
<!--more-->
其余上下文并没有对**SERCOM引脚与USART信号**对应关系有约束说明，而在**SERCOM I2C**相同小结处的表格却做了明确约束，如下：
![IIC_Signal_Description](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/ATSAME54_SERCOM硬件设计问题/IIC_Signal_Description.png)


对于一个英语非母语的使用者，在无法通读手册全文的情况下，会很容易产生  
**误会：UASART信号（RX、TX、RTS、CTS）可以随意配置到SERCOM组的物理引脚。**

然而事实并非如此，在随后的**寄存器说明-CTRLA**寄存器处，给出了下面的配置表格：
![USART_CTRLA](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/ATSAME54_SERCOM硬件设计问题/USART_CTRLA.png)


从该表可以得出，**RX**引脚是可以随意配置到SERCOM组的任意物理引脚（PAD[0-4]），而**TX**引脚只能配置到SERCOM组 **PAD[0]** 物理引脚，**XCK、RTS、CTS**亦有约束。

只能说隐藏的太深，对于一个刚接触该芯片的人来说，要了解到寄存器层面才能正确完成硬件设计，实则很难。

## SPI引脚

与串口引脚类似，只在寄存器描述处给了明确约束，如下：

![SPI_CTRLA](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/ATSAME54_SERCOM硬件设计问题/SPI_CTRLA.png)
## I2C引脚

I2C的引脚信号说明在**UASRT引脚**已经列出表格，请回溯上文。另外其在寄存器说明处不再有如USART和SPI那样的引脚配置表格。

## 结论

### USART 

 - RX：PAD[0-4]
 - TX：PAD[0]
 - XCK：PAD[1]
 - RTS：PAD[2]
 - CTS：PAD[3]

### SPI

- SCK：PAD[1]
- SS：PAD[2]
 
**主模式（master）**
 - MISO：PAD[0-4]
 - MOSI：PAD[0]、PAD[3]
 
**从模式（slave）**
 - MOSI：PAD[0-4]
 - MISO：PAD[0]、PAD[3]

### I2C

 - SDA：PAD[0]
 - SCL：PAD[1]
 - SDA_OUT：PAD[2]
 - SCL_OUT：PAD[3]


