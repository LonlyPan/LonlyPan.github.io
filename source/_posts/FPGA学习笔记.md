---
layout: post
title: FPGA学习笔记
index_img: https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_3_.jpg
date: 2023-08-25
updated: 2023-08-25 15:19
hide: false
## sticky: 100 #置顶，数字越大越靠前
## banner_img: #/img/post_banner.jpg
## comment: false
categories: 01-专业
---

<!--more-->

# FPGA 开发流程

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1692973462775.png)


![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693036157272.png)

# LED前线体验
## 工程管理

参考正点原子

 1. doc：开发过程中使用的辅助文档文件（如绘图软件绘制的波形图文件等）
 2. prj：新建工程及产生的文件（Vivado工程）
 3. rtl：开发过程中的 RTL 代码文件
 4. sim 仿真工程与仿真文件

工程路径除了英文、数字以及下划线等，不要出现中文或者其它特殊字符，否则 FPGA 开发工具无法识别工程路径

> 正点原子开发方式是先编写RTL代码，再modelsim中仿真，最终Vivado新建工程

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_1_.jpg)

## 需求分析-系统设计-模块设计-波形图

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_4_.jpg)

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_5_.jpg)

## RTL代码-Modelsim仿真代码

建议直接在Modelsim编写代码，可以实时编译检测错误
- 一般RTL文件名：工程名.v
- 仿真文件名：vtf_工程名_test.v

led.v RTL代码
```
module led(
    input key,
    output led
);

assign led = ~key;

endmodule
```

vtf_led_test.v 仿真代码
```
`timescale 1ns/1ns

module vtf_led_test();

reg key;
wire led;

initial begin 
    key <= 1'b1;

    #200
    key  <= 1'b1;
    #1000
    key  <= 1'b0;
    #600
    key  <= 1'b1;
    #1000
    key  <= 1'b0;
end

led uut(
    .key(key),
    .led(led)
);

endmodule
```

## Modelsim仿真

### 1、新建工程
在 modelsim 中建立 project，选择 File->New->Project
1. 在“Project Name”栏中填写工程名，建议和仿真的文件一样
2. “Project Location”是Modelsim仿真工程路径，这里统一路径为工程下的 sim 文件夹
3. 下面两部分是用来设置仿真库名称和路径的，这里我们使用默认
4. 点击【OK】

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_7_.jpg)

### 2、导入文件
选择窗口中共有四种操作：
- Create New File（创建新文件）
- Add Existing File（添加已有文件）
- Create Simulation（创建仿真）
- Create New Folder（创建新文件夹）
 
1. 这里我们先选择“Add Existing File”（添加已有文件）
2. 点击“Browse”按钮选择“led.v”文件，选择 Reference from current location 来引用设计文件，其他的保持默认设置，最后点击【OK】按钮。
3. 接下来以同样的方法添加仿真文件
4. 点击“Close”关闭文件创建/添加框。

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_8_.jpg)

### 3、 编译

我们可以在菜单栏 【Compile】中找到编译相关命令，也可以在快捷工具栏或者在工作区中的右键弹出的菜单中找到编译相关命令。

- Compile Selected（编译所选）
- Compile All（编译全部）
- Compile Order：文件编译顺序，可以调整编译的.v 文件的编译顺序。
-  Compile Report：编译报告，内容为当次编译的详细报告。 
-  Compile Summary：编译摘要，执行过的编译操作都在编译摘要有记录。
-  Compile out-of-data：
 我们单击 Compile All（编译全部）
  编译完成后，结果如下图所示：

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_11_.jpg)

