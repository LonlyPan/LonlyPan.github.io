---
layout: post
show_title:  "GPIO_Analog 引脚模式-STM32CubeMX"
title:  "2019-05-17-GPIO_Analog 引脚模式-STM32CubeMX"
date:   2019-05-17 21:12
categories: arm
---

最近开始学习使用STM最新的编译器STM32cubeIDE，直接从Keil转移过来的。之前也没用过CubeMX。

这里在配置引脚模式时，遇到了一个问题：**GPIO_Analog** 是什么模式？之前似乎从没遇见过。

<!--more-->

![enter description here](https://LonlyPan.github.io/images/Posts/2019-05-17-GPIO_Analog_引脚模式-STM32CubeMX/引脚选择.png)

上网搜了一通，找到ST官方社区的两个问答，才解决了疑惑。

问答原址：  

[1、Question about ADC versus GPIO Analog](https://community.st.com/s/question/0D50X00009XkfqtSAB/question-about-adc-versus-gpio-analog)  
[2、What pins can I use for Analog Input/Output? STM32CubeMX allows every GPIO to be set to ''GPIO_Analog''?](https://community.st.com/s/question/0D50X00009XkWkeSAF/what-pins-can-i-use-for-analog-inputoutput-stm32cubemx-allows-every-gpio-to-be-set-to-gpioanalog)  

总结起来就两个：

1、**GPIO_Analog**和**A/D**没有啥关系，就是名字是这个（第一个问答后面就一直在探讨两者关系，最终的答案是：当开启ADC功能时，后台默认同时开启Analog功能。但开启Analog功能，就是Analog功能。但笔者的理解：两者其实就没有联系）。  
2、这个功能实际作用是**降低功耗**。通过关闭引脚的上拉、下拉和施密特触发器，减小电流损耗。

实际STM32中文参考手册的**GPIO功能描述**也有介绍，不过这是个坑。我们看一下对比。

![参考手册对比](https://LonlyPan.github.io/images/Posts/2019-05-17-GPIO_Analog_引脚模式-STM32CubeMX/20190517参考手册对比.png)

官方的描述是**Analog**，翻译过来就是**模拟输入**了。当阅读的时候直接照字面理解成立类似ADC的模拟输入引脚了，但实际不是这个意思。当然中文版是**V10**，我的这篇英文是**V20**版，存在改版可能性。好了，看一下手册上对**Analog**模式的解释吧。

![参考手册英](https://LonlyPan.github.io/images/Posts/2019-05-17-GPIO_Analog_引脚模式-STM32CubeMX/20190517参考手册英.png)
