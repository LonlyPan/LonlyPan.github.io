---
layout: post
title: "Marlin 使用自定义Serial1/2/3额外串口"
index_img: https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/Marlin_使用自定义Serial1_2_3额外串口/Marlin-Logo-GitHub.png
data: 2019-6-14 9:23
updated: 2019-6-14 9:23
hide: false
# sticky: 100 #置顶，数字越大越靠前
# banner_img: #/img/post_banner.jpg
# comment: false
categories: 01-专业
---

按照Arduio官方[串口文档教程](https://www.arduino.cc/reference/en/language/functions/communication/serial/)  ，Mege系列主板是支持最多4路硬件串口的。
而我的Marlin主板正是使用了Mega2560（最常见的3D打印主板），因此也是可以添加额外的自定义串口，并且还有官方软件库支持。

<!--more-->

按照官方例程：[Arduino MultiSerialMega](https://www.arduino.cc/en/Tutorial/MultiSerialMega)
在Marlin的`setup()`函数中添加了如下测试代码：

```cpp
setup(){
	......
  	Serial2.begin(9600);
	Serial2.write("serial2 is ready!\n");
  	......
}
 ```
 
然而编译时却报错了，提示：
- 'Serial2' was not declared in this scope
- 'SERIAL_8E1' was not declared in this scope

简单的操作就是额外包含头文件 `arduino.h`，因为该处程序并并不是在 `.ino` 文件中编写的。然而包含之后并没有用，查询了多方资料加测试，才找到最终解决办法：  
需要添加头文件 `#include <hardwareSerial.h> `（arduino.h不用添加）
是不是贼拉简单<i class="fas fa-smile"></i>，害我折腾了一个小时......

另外多说一句 Arduino还支持模拟串口，可以任意指定引脚。[SoftwareSerial Library](https://www.arduino.cc/en/Reference/SoftwareSerial)  
不过经过测试，板子只能发送数据但无法接受，这个原因比较深奥，关乎到引脚中断和数接受原理。但官方的库和例程看似是可行的，不知道为什么（用的是硬件串口，但使用的模拟方式控制？可是我也测试了，也不行啊。暂且不深究了）

参考资料：
- [Serial1 or Serial2 in external library reference](https://github.com/espressif/arduino-esp32/issues/407)
- [Arduino, ESP32 and 3 hardware serial ports](https://quadmeup.com/arduino-esp32-and-3-hardware-serial-ports/)
