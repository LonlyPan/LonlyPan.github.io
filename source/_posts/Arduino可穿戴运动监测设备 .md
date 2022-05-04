---
layout: post
title:    "Arduino-可穿戴运动监测设备"
index_img: https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Arduino-可穿戴运动监测设备/arduino_logo.png
date:   2019-6-23  23:18 
categories: 单片机
---

## 项目简介

### 功能：

设计一款可穿戴设备，能够检测佩戴者是否处在运动或休息状态，从而能够在佩戴者长时间（30min）连续运动（步）后，提醒其饮水并适当休息（亮灯和串口打印提醒语句）；在长时间休息（60min）后，提醒其适当运动（灯闪烁和串口打印提醒语句）。

<!--more-->

### 硬件设备：

LilyPad Arduino 328 Main Board ，[资料链接](https://www.sparkfun.com/products/13342)  
LilyPad Accelerometer - ADXL335， [资料链接](https://www.sparkfun.com/products/9267)

### 软件&语言

Arduino IDE  
C/C++

## 项目方案

利用 ADXL335三轴加速度传感器获取佩戴者的实时数据，Main Board通过分析采集到的数据来判断佩戴者是否在运动或者休息， 再根据多次连续的运动数据进一步判断佩戴者是否处在连续运动状态，从而做出相对应的处理（亮灯或闪烁并打印消息）。

### 硬件连接：

ADXL335 分别连接 Main Board 的2，3，4引脚。（不是 a2,a3,a4 ？前者是Digital I/O Pin，后者是 Analog Input Channels，ADX输出信号属于 Analog 模拟信号）  
const int accelX = 2;                  // x-axis pin of the accelerometer  
const int accelY = 3;                  // y-axis pin  
const int accelZ = 4;                  // z-axis pin (only on 3-axis models)  

### 预备知识

更具 [ADI官方资料](https://www.analog.com/cn/analog-dialogue/articles/pedometer-design-3-axis-digital-acceler.html) 我们知道：无论如何穿戴计步器，总有至少一个轴具有相对较大的周期性加速度变化，那么我们就可以从这里着手，进行数据分析，判断步伐。

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Arduino-可穿戴运动监测设备/ADX运动数据.png)

### 算法

#### 1，均值滤波器---滤波

均值滤波器实现均值滤波，其实就是拿到多组x,y,z三轴数据，相加再求平均值。最后的平均值作为输出结果（采样值）。目的是使输出结果更加平滑。完成初步滤波。  

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Arduino-可穿戴运动监测设备/均值滤波.png)

```cpp
// Reads the acces and returns an 'energy' value
// @brief    均值滤波：获取多次三轴数据取平均值，所得平均值作为一次采样值
// @return    avgMag 作为一次采样值
// @more 这个函数在loop里会一直被循环调用的，不会停止
float filter_calculate(){    // 定义一个返回值为浮点数的函数
  float avgMag = 0;
  for(int x = 0 ; x < FILTER_CNT ; x++) //  获取 FILTER_CNT 次数据
  {
    float aX = analogRead(accelX);      // 读取 X轴数据 （0-1023）
    avgMag += aX;                       // 求和
  }
  avgMag /= FILTER_CNT;                 // 取平均
  return(avgMag);                       // 返回平均值
}
```

这里我们只使用了 x轴 一个轴。但从实验和理论上来说，任意方向动作都是可以，仅仅有幅度（原始数值）差别，但并不影响计步的判断。

#### 2、运动分析

有了滤波后的值，我们可以将其输出到串口，以图形显示运动状态下的数据特征。下图为AD官方的数据图，显示了来自一名步行者所戴计步器的最活跃轴的滤波数据。对于跑步者，峰峰值（每个山峰的最大值也可以看作最大值）会更高。

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Arduino-可穿戴运动监测设备/最活跃轴的滤波数据.png)

FILTERD DATA：**1** 滤波后的值  
THRESHOLD：阈值（步判断的参考值），阈值获取 **3** 再讲解，目前可以看作是一段时间所有最大最小值（滤波值）的中间值（平均值）  

图像分析：绿色线条是运动的实时数据，类似于一个一个三角波（一次运动即一次摆臂来回），可以看到，每步运动绿色线条会有两次跨过橙色线（阈值），那么我们就可以设法先后获取两次滤波值（不相等），判断是否有一个值 > 阈值，另一个值 < 阈值，就可以认为佩戴者是走了一步。 

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Arduino-可穿戴运动监测设备/摆臂运动对比图.png)

步伐迈出的条件定义为：当加速度曲线跨过动态阈值下方时，加速度曲线的斜率为负值.  


#### 3、动态阈值（获取阈值）

系统持续采样，不断更新3轴加速度的最大值和最小值，每50次更新一次平均值(Max + Min)/2称为"阈值"。由于此阈值每50次采样更新一次，因此它是动态的，称为"动态阈值"。这种选择具有自适应性。

```cpp
// Reads the acces and returns an 'energy' value
// @brief    动态阈值，连续采样50次，期间不断对比数据，更新最大最小值，最后取平均获得阈值
// @parameter    sample：一次采样值
// @more     这个函数在loop里会一直被循环调用的，不会停止
void peak_update(float sample){ 
  static unsigned int sample_size = 0;  // 采样次数计数器
  sample_size ++;   // 每执行一次函数，采样次数 +1（值是static类型，不会消失和改变）
  if (sample_size > SAMPLE_SIZE) {  // 采样达到50个，更新一次
    sample_size = 1;             // 计数器复位
    peak_oldmax = peak_newmax;   // 保存最大值
    peak_oldmin = peak_newmin;   // 保存最小值（这里实际还没计算阈值，后面程序里再计算）
  }
  peak_newmax = max(peak_newmax, sample);  // 每次采样都对比，取其中最大或最小值
  peak_newmin = min(peak_newmin, sample); 
}
```

#### 3、动态阈值（获取两次不等的滤波值）

```cpp
// Reads the acces and returns an 'energy' value
// @brief    动态精度，连续采样，判断前后差值是否 > 动态精度值（预设好），从而更新两个数据变量
// @parameter    sample：一次采样值
// @return    前后值有变化啦，你可以去尝试判断一次步了（两次采样点可能都在阈值线的上方或下方，所有我们只要达到判断条件（差值 > 动态精度值），我们就判断一次，一次摆臂会判断很多很多次，但合格的只有一次）
char slid_update(float sample){
  char res = 0;
  if (abs((sample - new_sample)) > PRECISION) {  // 值变化大于一定预设值（滤除高频噪声：自身手环不动时数值也会有微小的浮动（忽略它））
    old_sample = new_sample;       // 保存上一次的值
    new_sample = sample;           // 保存本次的采样值（最新值）
    res =1;                        // 值改变达到判断条件了，下面的程序语句不在执行
  } 
  else {
    old_sample = new_sample;      // 没有就保持两者相等，这样就不会误判
    res = 0;                      // 没准备好，不能判断
  }
  return(res);                    // 返回值
}
```

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Arduino-可穿戴运动监测设备/动态精度.png)

#### 4、步判断

```cpp
// @brief    判断是否走步，有则步数 +1 ，无则不变
// @return    从开机到现在的总运动步数
int detect_step(){
  
  static int step_cnt = 0;                   // 总步数
  
  float value = filter_calculate();          // 调用均值滤波函数获得 采样值
  peak_update(value);                        // 将采样值个 动态阈值函数，获得最大最小值（50次更新一次，期间保持为上一次比较的值）
  if(slid_update(value)){                    // 将采样值传递给 动态精度 函数，让其对比预设值，返回是否达到比较条件
     float threshold = (peak_oldmax + peak_oldmin) / 2;  // 进入if，表示 差值 > 精度值。获得阈值
     if (old_sample > threshold && new_sample < threshold){  // 最大最小值和阈值进行比较
        step_cnt++;                                        // 满足步的判断条件，步数 +1
        Serial.println(step_cnt);                          //  串口打印输出
        return( step_cnt);                                 // 返回步数值
     }
  }
}
```

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Arduino-可穿戴运动监测设备/步判断.png)

## 主程序

每隔3s获取一次步数值，并保存。下一个3s再次获取步数值，和上一次保存的步数值对比，如果有变化，说明佩戴者是出于运动状态的，否则处于休息状态。每判断为运动状态，就更新一次运动时间，否则更新休息时间，然后比较运动时间和休息时间的差值，从而得出是长时间运动还是休息。  

```cpp
void loop() {
  static int new_steps = 0;  // 最新的步数
  static int old_steps = 0;  // 上一次保存的步数
  static int newgap_time = 0;  // 最新的时间
  static int oldgap_time = 0;  // 上一次保存的时间

  new_steps = detect_step();  //  获取步数
  newgap_time = millis();  // 更新时间
  if((newgap_time - oldgap_time) > 3000){  //时间差为 3s(不可改)，比较一次步数值，看是否有变化，没变化，说明不在运动，相反则在运动
    oldgap_time = newgap_time;  // 更新旧时间值，作为下一次3s比较值
    if(new_steps==old_steps)
      nmove_time = millis();     // 步数没改变，更新没有移动的时间
    else{
      old_steps = new_steps;     // 旧步数 = 最新步数，最为下一个3s 比较值
      move_time = millis();      // 步数改变，更新运动的时间
    }
    if(move_time > nmove_time){  // 当前运动的时间比不运动的大，说明处在运动状态
      if(abs(move_time - nmove_time) > 30000)  // 30s  看运动的持续时间是否达标（可更改）
         Serial.println("moving to long !");
    }
    else{                       // 当前运动的时间比不运动的小，说明处在休息状态
      if(abs(nmove_time - move_time) > 30000)  //  30s  不运动的持续时间是否达标（可更改）
         Serial.println("please have a Exercise !");  
    } 
  }
}
```

## 流程图

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Arduino-可穿戴运动监测设备/流程图.png)

## 参考资料链接：

[主要程序参考](https://blog.csdn.net/dancer__sky/article/details/81504778#comments)  
[所有LilyPan配套产品资料](https://www.sparkfun.com/search/results?term=LilyPad)  
[Arduino官网-LilyPan介绍](https://www.arduino.cc/en/Main/ArduinoBoardLilyPad/)  
[Arduino官网-LilyPan入门资料](https://www.arduino.cc/en/Guide/ArduinoLilyPad)  
[Arduino官方编程语言参考](https://www.arduino.cc/reference/en/#functions)  
[LilyPan原理图](https://www.arduino.cc/en/uploads/Main/LilyPad_schematic_v18.pdf)  
[基于ADXL335的闪光礼帽制作](https://learn.sparkfun.com/tutorials/das-blinken-top-hat?_ga=2.148003764.2073164159.1559885624-1893861135.1559885624)  


