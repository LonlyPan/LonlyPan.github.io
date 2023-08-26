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

我们在仿真一些复杂的工程的时候往往需要对工程里的 IP 核进行仿真，当我们需要对 IP 核进行仿真时一定要事先将 IP 核的库文件加载到Modelsim 库中去。

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
-  Compile out-of-data：只重新compile有修改過的檔案 (比較節省時間，故也較常用)
 
我们单击 Compile All（编译全部）

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_11_.jpg)

文件编译后“Status”列可能会有三个不同状态。除了上图的用“√”表示的通过状态外，还有两个在 设计中不希望出现的状态：编译错误（显示红色的“×”）和包含警告的编译通过（对号的后面会出现一个 黄色的三角符号）。

- 编译错误即 Modelsim 无法完成文件的编译工作。通常这种情况是因为被编译文件中包 含明显的语法错误
- 编译结果中包含警告信息是一种比较特殊的状态，表示被编译的文件没有明显的语法错误，但可能包含一些影响最终输出结果的因素。这种状态在实际使用中较少出现，这类信息一般在功能仿真的时候不会带来明显的影响，不过可能会在后续的综合和时序仿真中造成无法估计的错误

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_12_.jpg)

### 4、仿真

在 ModelSim 菜单栏中找到【Simulate】→【Start Simulation...】菜单并点击，弹出如下右图所示页面

 - Design Optimization：优化设计设置页面。
 - Runtime Options：运行选项配置，例如波形格式配置、仿真时间设置等。
 - Restart：重启仿真。
 - Break/End Simulation：终止仿真运行。

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_13_.jpg)

配置仿真功能页面包含 6 个标签，分别是：Design、VHDL、Verilog、Libraries、SDF 和 Others。我们用的最多的是 Design、Libraries 和 SDF 这三个标签了
- Design 标签，该标签内居中的部分是 Modelsim 中当前包含的全部库，这些库和单元是为仿真服务的。可以使用 Ctrl 和 Shift 键来选择多个文件。右侧是 Resolution 选项，可以选择仿真的时间精度。这个选项一般都是设置在默认状态，这时 Modelsim 依照仿真设计文件中指定的最小时间刻度来进行仿真，如果设计文件中没有指定，则按 1ns 来进行仿真。最下方的区域是 Optimization 区域，可以在仿真开始的时候使能优化。
- Libraries 标签，我们可以设置搜索库。Search Libraries 和 Search Libraries First 的功能基本一致，唯一不同的是 Search Libraries First 中指定的库会在指定的用户库之前被搜索。
-  SDF 标签，SDF 是 Standard Delay Format（标准延迟格式）的缩写，内部包含了各种延迟信息，也是用于时序仿真的重要文件。SDF Files 区域用来添加 SDF 文件，可以选择 Add 按钮进行添加，选择 Modify 按钮进行修改，选择 Delete 按钮删除添加的文件。
SDF Options 区域设置 SDF 文件的 warning 和 error 信息。第一个“Disable SDF warning”是禁用 SDF 警告，第二个“Reduce SDF errors to warnings”是把所有的 SDF 错误信息变成警告信息。区域 Multi-Sourcedelay 中可以控制多个目标对同一端口的驱动，如果有多个控制信号同时控制同一个端口或互连，且每个信号的延迟值不同，可以使用此选项统一延迟。下拉列表中可供选择的有三个选项：latest、min 和 max。latest选项选择最后的延迟作为统一值，max 选项选择所有信号中延迟最大的值作为统一值，min 选项选择所有信号中延迟最小的值作为统一值。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_14_.jpg)

接下来我们在 Design 标签页面中选择 work 库中的 tb_led 模块，在Optimization 一栏中勾选“Enable optimization”。
> 注意：如果不进行上面的优化选项配置，Modelsim SE-64 2020.4 仿真会报如下截图所示错误：![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693040774865.png)

然后点击上图的右下角的“Optimization Options”对优化选项进行如下图设置，设置完成后点击“OK”退出优化选项设置页面
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_15_.jpg)

弹出下图所示界面：
鼠标右键单击“u_led”，选择“Add Wave”选项，如下图所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_16_.jpg)

仿真软件的几个仿真按钮，如下图所示：
- Restart：复位仿真，点击该按钮会有一个弹框如下图。勾选不同的选项会复位对应的设置；我们修改代码编译成功后，不需要重新打开仿真界 面，直接使用仿真复位按键，上图弹窗全部勾选后点击“OK”，就可以重新开始运行仿真。
- Run Length：设置仿真时间，配合运行仿真按钮使用； 
- Run：运行仿真，配合设置仿真时间一起使用，会按照设置仿真时长进行仿真；
- ContinueRun：继续仿真，在停止仿真后需要继续运行仿真，可以使用继续仿真按钮
- Run-All：一直仿真，点击仿真复位后，再点击一直仿真，仿真会一直运行，直到点击 Stop 停止仿真；
- Break：中断当前编译或者仿真； 
- Stop：在下一步或者下一时间之前停止仿真。 
 
注意：使用“Run -All”与“ContinueRun”时，如果仿真工程是只有组合电路，没有使用时钟，就只能跑到仿真文件的激励的节点，不能一直运行仿真。此时如果在仿真文件添加持续的时钟输入，点击“Run -All”与“ContinueRun”时就会一直仿真。 

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693040965836.png)

本例程我们选择仿真时间为 10us，如下图所示，单击右边的运行按钮。
运行后的结果如下图所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_17_.jpg)

这里将会对 ModelSim 软件中几个常用小工具进行简单的讲解。
- 上图红框中的几个放大镜模样的工具分别是放大、缩小和全局显示功能，鼠标放到图标上会显示出它们的快捷键。
- 上图红框中黄色图标是用来在波形图上添加用来标志的黄色竖线，紧跟着的是将添加的黄色竖线对齐到信号的下降沿和上升沿。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693041130450.png)

## Vivado工程