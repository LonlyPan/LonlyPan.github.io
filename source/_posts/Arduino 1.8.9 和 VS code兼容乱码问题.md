---
layout: post
title:  "Arduino 1.8.9 和 VS code兼容乱码问题"
index_img: https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/Arduino_1.8.9_和_VS_code兼容乱码问题/下载.png
date:   2019-05-17 20:12
hide: false
# sticky: 100 #置顶，数字越大越靠前
# banner_img: #/img/post_banner.jpg
# comment: false
categories: 01-专业
---

 近期Arduino推出了新版本1.8.9，兴奋的去官网进行了升级替换了老版本1.8.8。然后问题出现了：
 
 我是使用VS code + Arduino 进行Arduino开发的，VS code负责编写代码，然后调用Arduino进行编译和下载，具体实现方法请自行查找，教程很多。开始正题：
 
 <!--more-->
 
 更新软件后，正常操作，编译之前的一个工程文件，突然发现 调试输出栏出现了乱码：

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Arduino_1.8.9_和_VS_code兼容乱码问题/20190517输出乱码.png)

但是可以正常的编译和下载，仅是乱码问题。下张图片显示上传成功。

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Arduino_1.8.9_和_VS_code兼容乱码问题/20190517输出乱码2.png)

一开始自己查找各种解决方法，最后还卸载重装了VS code，删除了其用户数据，但问题依旧。时隔两天，突然想到是不是Arduino的问题，于是将软件重装为1.8.8版本，然后，然后......问题就解决了。

最后附一张VS code和Arduino联合使用的配置参数，作为备忘。
 
 ```cpp
	// "D:\\Program Files (x86)\\Arduino\\**",
	"C:\\Users\\LonlyPan\\Documents\\Arduino\\libraries\\U8glib\\src",
	"D:\\Program Files (x86)\\Arduino\\tools\\**",
	"D:\\Program Files (x86)\\Arduino\\hardware\\arduino\\avr\\**"
	
    // "D:/Program Files (x86)/Arduino/hardware/tools/avr/lib/gcc/avr/5.4.0/include",
    // "D:/Program Files (x86)/Arduino/hardware/tools/avr/avr/include"
 ```
