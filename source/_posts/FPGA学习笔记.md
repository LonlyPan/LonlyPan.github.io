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

# 软件下载与安装

## Vivado软件

热知识：XILINX 被 AMD 收购了 ~如果你对该事件比较感兴趣，不妨看看： https://www.amd.com/en/corporate/xilinx-acquisition
Vivado 是 FPGA 厂商赛灵思公司（XILINX）于 2012 年发布的集成设计环境。 
其包括高度集成的设计环境和新一代从系统到 IC 级的工具，赛灵思构建的 Vivado 工具把各

### 下载

进入官网下载：[Vivado ML 开发者工具](https://china.xilinx.com/support/download/index.html/content/xilinx/zh/downloadNav/vivado-design-tools.html)
最新版不稳定，推荐下载旧版本，这里选择正点原子用的最新版。进入历史版本中下载旧版本。选择2020.2版本下载，Windows在线安装包
链接：https://pan.baidu.com/s/1KEUWDPlV64QAQfiWLWoA4A?pwd=6666 
提取码：6666
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_20_.jpg)

下载需要登录Xilinx。如果已有Xilinx账户，直接填写账号和密码登录；如果没有账户，则点击“创建账号”即可免费创建一个新账号。如图B.4所示。

 1. 点击创建密码：随后会转到登陆界面，直接点击 "创建密码" 进入注册页面
 2. 账户创建：填写姓名以及邮箱，这里填的邮箱是用来接收验证信息的，所以一定要填一个能用的邮箱。然后选择语言首选项和位置，上面的信息其实除了邮箱都可以随便填。最后进行完谷人机身份验证后，点击 Submit 提交即可。 
 3. 激活账户：访问令牌在你的邮箱中，如果你没有收到邮件，可以点击黑色按钮 "重新发送电子邮件"。 设置密码这块比较烦，长度必须包含10且需要包含 1个大写字母，1个小写字母，1个数字，1个特殊字符。这里不得不吐槽一下，是真的很麻烦。
 4. 登陆账号：激活账户后，会跳转到登录页面。此时输入刚才的邮箱和密码即可登陆。

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_21_.jpg)
认证个人信息：下载之前还需要再次填写个人信息，简单地填一下，然后点击 Download 按钮就可以下载了。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_22_.jpg)

### 安装

1. 打开安装包：下完完毕后我们打开安装包进行安装：
2. 如果弹出 Windows 安全中心警报，点击 允许访问 即可：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_23_.jpg)
3. 点击 Next >  
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_24_.jpg)
4. 再次输入账号密码：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_25_.jpg)
5. 选择要安装的软件：这里选择Viavad
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_26_.jpg)
6. 选择版本：这里选择Design Edition版本即可
  a. System Edition是面向大型系统设计的版本，具有高级功能，如高速原理图布局和高速系统级设计。System Edition包含有system generator for dsp with matlab工具。
  b. Design Edition是面向常规设计的版本，提供了大多数设计所需的功能。
  c. HL WebPACK Edition是免费版本，限制了一些高级功能。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_27_.jpg)
7. 选择工具组件和器件库。为了节省存储空间，我们将用不到的工具组件和器件库去掉，如下图所示： 
最下面的“Disk Space Required”表示在当前选项下 Vivado 在安装完成后所占用的磁盘空间大小， Vivado 对硬盘存储空间的占用相对来说还是挺大的。
点击 Next，进入安装目录设置页面，如下图所示： 
 ![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_28_.jpg)
8. 同意许可
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_29_.jpg)
9. 图中红色方框内是对安装目录的设置，可以点击后面的三个点来修改安装目录（注意，安装路径只能够包含字母、数字、下划线，否则安装程序有可能出问题）。其他的设置保持默认即可。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_30_.jpg)
10. 进入 Summary 界面，该界面总结了前面所有安装的配置信息，供用户浏览确认。确认无误后，点击“Install”开始安装 Vivado 设计套件，如下图所示。（由于 Vivado 在安装期间会占用大量的电脑CPU 资源和内存资源，所以笔者建议在开始安装之前，尽量关闭电脑中其他的不必要的应用软件）
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_31_.jpg)
11. 等待软件安装完成
这可能会是一个漫长的等待，长到令人窒息，就像是安装核弹发射系统一样。
期间可能会弹出下载失败，重试后依旧提示下载失败，此时可以点击 Cancel取消安装，重新安装即可。软件会保留已下载的文件，所以不会重头开始。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_32_.jpg)
12. 在这期间可能会弹出 Windows 安全中心：信任安装即可。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_33_.jpg)

### License注册

下载License文件，并解压

链接：https://pan.baidu.com/s/1TZAkoJbpQlLIIuMnWuXdCg?pwd=6666 
提取码：6666

运行Vivado 软件，进入License管理
点击copy license，选择下载的文件
在View License Status 中查看是否成功，有效期至2037
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_34_.jpg)

### 参考资料：[（一） vivado2018.3安装注册指南](https://blog.csdn.net/weixin_42668358/article/details/125512721)


## Modelsim软件

### 下载：

一定要下载2020.4版本的，和Vivado2020.2配套，后面会联合仿真。如果版本不对应，可能会联合仿真失败
链接：https://pan.baidu.com/s/1v4AMXQhexjFzHtdmdWaOkQ?pwd=6666 
提取码：6666

### 安装

https://zhuanlan.zhihu.com/p/646172508

### Tab缩进修改

Tools->edit preferences->by name->source->tabs
双击 tabs，软件默认为8，我改成了4
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693140353582.png)

# FPGA 开发流程

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1692973462775.png)


![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693036157272.png)

# LED工程抢先体验
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

### 软件技巧

#### Wave窗口独立

wave右上角点击 dock/undock 图标，就可以将Wave独立出来，更加方便查看波形
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_2_.jpg)

#### Wave窗口变量名路径隐藏

先将Wave窗口独立出来，然后在菜单栏的 Format 菜单中修改

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_3_.jpg)

## Vivado工程

### 1、新建工程
- 名称要能反应出工程所实现的功能，本次工程实现了点亮 LED 的功 能，因此项目名称命名为“led”。
- 工程路径是我们在 7.2 小结中新建的 led/prj 文件夹，工程路径不能包含中文、空格或者其它一些特殊的符 号，尽量使用英文、数字和下划线，否则工程会创建失败。
- 取消默认勾选了 “Create project subdirectory”选项。如果勾选，Vivado 会在所选工程目录下自动创建一个与工程名同名的文件夹，用于存放工程内的各种文件。我们在前面已经做了统一的工程管理，所以我们取消该选项的勾选状态。

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_18_.jpg)

1. “RTL Project”是指按照正常设计流程所选择的类型，这也是常用的一种类型。
  a. “Do not specify sources at this time”：用于设置是否在创建工程向导的过程中添加设计文件，如果勾选后，则不创建或者添加设计文件，我们后续需要添加设计文件，所以不勾选该选项。该选项勾选则结果就是直接跳过下一步：添加源文件。
  b. “Project is an extensible Vitis platform”：创建的工程是否需要扩展 Vitis 开发平台，点亮 LED等实验不需要 Vitis 开发平台。所以这个选项我们也不需要勾选。
2. “Post-synthesis Project”在导入第三方工具所产生的综合后网表时才选择；
3. “I/O Planning Project”一般用于在开始 RTL 设计之前，创建一个用于早期 IO 规划和器件开发的空工程；
4. “Imported Project” 用于从 ISE、 XST 或 Synopsys Synplify 导入现有的工程源文件；
5. “Example Project”是指创建一个 Vivado 提供的工程模板。

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_19_.jpg)


选择了“RTL Project”后，我们点击“Next”，进入添加源文件页面。注意，如果勾选中图 7.3.53 中“RTL Project”下的“Do not specify sources at this time”，则不会出现添加源文件的界面。
1. 源文件编程语言有“ VHDL”与“Verilog”两种，我们所有工程的编程语言都选择“Verilog”语言。
2. 仿真文件的编程语言选择有“VHDL”、“Verilog”与“Mixed”三种选择。这里选择默认的混合编程语言“Mixed”。
3. 点击上图中“ +”后会下拉三个选项，分别是添加源文件（Add Source Files）、添加源文件夹（Add Source Directories）以及新建源文件（Create Source File），与④处三个选项的功能是一样的。
>添加源文件的作用：有的设计可能是先使用Modesim，进行设计和仿真，然后再使用Vivado进行完整工程开发。这样我们就可以直接添加设计文件，不需要再重新编写了。
源文件的添加时链接方式，不是复制，所以最好将文件复制到工程目录下

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_35_.jpg)
 
因为我们已经编写好了源文件，所以这里我们直接选择“Add Files”按钮添加，
如下图所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_40_.jpg)

添加约束文件，与添加源文件一样。一般是创建完工程后再创建/添加约束文件
 ![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_41_.jpg)
 
> 源文件的添加是链接方式，不是复制，所以最好将文件复制到工程目录下

接下来选择开发板的芯片型号，我们可以直接在搜索框中输入完整的芯片型号，大家根据自己所使用 的 ZYNQ 核心板型号进行选择，这里我们输入“xc7z020clg400-2”，也可以根据芯片本身的所在系列、封装、速度等级以及工/商业级等信息进行选择。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_42_.jpg)

下面介绍 Vivado 工程主界面中的几个主要子窗口：
1. Flow Navigator。 Flow Navigator 提供对命令和工具的访问，其包含从设计输入到生成比特流的整个过程。 在点击了相应的命令时，整个 Vivado 工程主界面的各个子窗口可能会作出相应的更改。功能窗口。
2. 数据窗口区域。默认情况下， Vivado IDE 的这个区域显示的是设计源文件和数据相关的信息。类似与工程目录
	1. Sources 窗口： 显示层次结构（Hierarchy）、 IP 源文件（IP Sources）、库（Libraries）和编译顺序（Compile Order）的视图
	2. Netlist 窗口： 提供分析后的（elaborated）或综合后的（synthesized）逻辑设计的分层视图。
3. Properties 窗口： 显示有关所选逻辑对象或器件资源的特性信息。
4. 工作空间（Workspace）： 工作区显示了具有图形界面的窗口和需要更多屏幕空间的窗口，包括：
● Project Summary。提供了当前工程的摘要信息，它在运行设计命令时动态地更新。
● 用于显示和编辑基于文本的文件和报告的 Text Editor。
● 原理图（Schematic）窗口。
● 器件（Device）窗口。
● 封装（Package）窗口。
5. 结果窗口区域：在 Vivado IDE 中所运行的命令的状态和结果，显示在结果窗口区域中，这是一组子窗口的集合。在运行命令、生成消息、创建日志文件和报告文件时，相关信息将显示在此区域。默认情况下，此区域包括以下窗口：
● Tcl Console： 允许您输入 Tcl 命令，并查看以前的命令和输出的历史记录。
● Messages： 显示当前设计的所有消息，按进程和严重性分类，包括“Error”、“CriticalWarning”、“Warning”等等
● Log： 显示由综合、实现和仿真 run 创建的日志文件。
● Reports： 提供对整个设计流程中的活动 run 所生成的报告的快速访问。
● Designs Runs： 管理当前工程的 runs。
6. 主工具栏： 主工具栏提供了对 Vivado IDE 中最常用命令的单击访问。
7. 主菜单： 主菜单栏提供对 Vivado IDE 命令的访问。
8. 窗口布局（Layout）选择器： Vivado IDE 提供预定义的窗口布局，以方便调用设计过程中的各种窗口。布局选择器使您能够轻松地更改窗口布局。或者，可以使用菜单栏中的“Layout”菜单来更改窗口布局。

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_43_.jpg)


这里着重讲解以下Flow Navigator。其包含从设计输入到生成比特流的整个过程。也就意味着这是一般的设计流程

1. Project manager：工程管理，新建文档等，也就是源码
2. IP INTEGRATOR：IP核设计，类似与设计好的库，这个一般在后期用到，可选。这个本质上也是Verilog源码
3. Simulation：仿真，可选。正点原子的设计流程是在Modelsim中仿真，所以Vivado中就不需要这一步了。这里的仿真其实就是使用的Modelsim，是厂家集成到Vivado的，当然功能后速度都会相对差一些
4. RTL analysis：RTL分析，会将我们编写的代码转换成RTL原理图（理论上的原理图，和FPGA硬件无关的）
   ![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693060912071.png)
5. I/O约束：对RTL中的信号映射到实际引脚中。可以在综合后
  ![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_45_.jpg)
6. Systhesis：综合，其实就是将RTL的原理图转成FPGA芯片已有资源的对应原理图，这个是跟FPGA实体挂钩的。所以就存在实现综合失败的问题，也就是RTL设计所需要的资源FPGA没有（比如不可综合的Verilog代码）
可以看出RTL和综合后是不一样的。I/O输出输出都有BUF，而逻辑部分则是由LUT查找表实现的，具体可以学习FPGA相关书籍：这里推荐**FPGA原理和结构**，日本**天野英晴**编写，这里是一定要学的，不然后面的综合会看不懂
**综合过后还要进行时序约束，即添加时钟，纯组合电路不需要添加**
 ![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693061127971.png)
7. Implementation：设计实现。就是将综合的原理图在FPGA芯片上实现。因为FPGA又喝多资源，单元性质的，所以同一个综合电路可以在FPGA不同的位置实现。这就引出后面的布局布线问题，类似于PCB设计。
 ![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_46_.jpg)
8. Program and Debug：程序下载与调试


### 2、设计输入

双击 Sources→Design Sources 下的点亮 LED 灯文件，在 Vivado 软件中显示的界面如下图所示。

每次保存后，Vivado 都会对源文件进行部分语法的检查，如果有语法的错误，Vivado 会给出提示。另外，在大多数情况下，Vivado IDE 会自动识别设计的顶层模块，当然，用户也可以手动指定顶层模块。从 “Sources”窗口的右击菜单中选择“Set as Top”来手动定义顶级模块。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_44_.jpg)

### 3、RTL分析

代码输入完毕之后，就可以对设计进行分析（Elaborated）了。点击“Flow Navigator”窗口中的 “Open Elaborated Design”按钮，如下图所示。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693062258069.png)

此时，Vivado 会编译 RTL 源文件并进行全面的语法检查，并在 Messages 窗口中给出相应的“Error” 和“Warning”。打 开分析后（Elaborated）的设计，Vivado 会生成顶层原理图视图，并在默认 view layout 中显示设计，如下 图所示：
可以看到，此时窗口布局已经发生了变化，新增了 Schematic（原理图）、Netlist（网表）等窗口。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_47_.jpg)
此时，底部的 Messages 窗口会显示分析阶段产生的消息，我们点亮 LED 实验的代码并没有产生错误提示，图中的一个警告信息是因为我们为了方便工 程管理，没有使用 Vivado 软件新建源文件自带的路径导致的警告，该警告不造成影响。 
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_48_.jpg)
### 4、I/O约束

I/O约束 在综合之前任意步骤都可以。因为综合涉及到实际硬件，再设之前都没涉及到。

在右上角的窗口布局（Layout）选择器中选择“I/O Planing”，如 下图所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_49_.jpg)

此时，窗口布局会打开 IO 相关的子窗口，在其中的“I/O Ports”窗口中，就可以进行 IO 的分配了。
- Name：工程中顶层端口的名称。 
- Direction：说明管脚是输入还是输出。 
- Neg Diff Pair：负差分对，差分信号在 I/O Ports 窗口中只显示在一行里中（只会显示 P 端信号，N 端 信号显示在 Neg Diff Pair 属性栏中）。 
- Package Pin：配置管脚封装。
-  Fixed：每一个端口都有 Fixed 属性，表明该逻辑端口是由用户赋值的。端口必须保持锁定状态，才能 避免生成比特流时不会发生错误。 
-  Bank，I/O Std，Vcco，Slew Type，Drive Strength：显示 I/O 端口的参数值。 
-  Bank：显示管脚所在的 Bank。 
-  I/O Std：配置管脚的电平标准，常用电平标准有 LVTTL 和 LVCMOS、SSTL、LVDS 与 HSTL 等。
-  Vcco：选择的管脚的电压值。
-  Vref：在我们的设计中，硬件上 VREF 引脚悬空。 
-  Drive Strength：驱动强度，默认 12mA。 
-  Slew Type：指上升下降沿的快慢，设置快功耗会高一点，默认设置慢（slow）。 
-  Pull Type：管脚上下拉设置，有上拉、下拉、保持与不设置。 
-  Off-Chip Termination：终端阻抗，默认 50Ω。 
-  IN-TERM：是用于 input 的串联电阻。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693062578219.png)

领航者开发板提供了 IO 引脚总表，在如下图所示位置
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_50_.jpg)

这里更具开发板原理图、引脚电压等设计管教约束如下：
  ![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_45_.jpg)

### 5、综合

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693064461762.png)
综合完成后，弹出如下窗口
- “Run lmplementation”选项直接进行**设计实现**步骤
- “Open Synthesized Design”选项打开综合设计。即查看综合后的原理图
- 查看报告

这里选中“Open Synthesized Design”后点击“OK”
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_52_.jpg)

点击“OK”之后会出现一个弹框，这个弹框中选择“YES”就会打开综合后的原理图，如下图所示：
可以清晰地看到代码和电路图的对应关系，IBUF 是输入缓存，一般 VIVADO 会自动给输入信号 加上，作用是将输入管脚输入到 FPGA 内部；OBUF 是输出缓存，一般 VIVADO 会自动给输出信号加上， 作用是将输出信号送出 FPGA。代码中的取反是通过一个 LUT1 实现的。 

打开综合后设计除了自动弹出的原理图界面外，还有一些其它选项，如下图所示
- Constraints Wizard：约束向导。 
- Edit Timing Constraints：编辑时间约束。 
- Set Up Debug：引导创建在线调试。
- Report Timing Summary：报告时序摘要并运行时序分析。 
- Report Clock Networks：时钟网络报告。 
- Report Clock Interaction：时钟交互报告。 
- Report Methodology：检查符合 UltraFast 设计方法的设计。 
- Report DRC：设计规则检查。 
- Report Noise：噪声分析报告。 
- Report Utilization：资源利用率报告。 
- Report Power：电源报告。 
- Schematic：打开综合后的原理图设计。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_53_.jpg)
### 6、时钟约束

- 时钟是在**综合后的**
- 组合逻辑电路可以不要时钟约束

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/20230829-1.jpg)

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/20230829-2.jpg)

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/20230829-3.jpg)
### 7、设计实现

约束、综合完毕之后，就可以开始实现设计了。我们点击“Flow Navigator”窗口中的“Run Implementation”按钮，如下图所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693064855050.png)

实现完成后会弹出如下提示窗口：
和前面的类似，该窗口有三个选项，从上到下分别是
- 打开实现设计
- 生成下载文件
- 查看报告。
 
这里我们打开实现设计
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_54_.jpg)

进入实现设计界 面，如下图所示：左边的“Netlist”窗口中有“Nets”与“Leaf Cells”，点击“Nets”与“Leaf Cells”下面的选项， 右边的器件图会高亮对应模块，所以实现设计将代码映射到了 FPGA 底层资源上。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_55_.jpg)

 这时我们再次查看“Design Runs”窗口中的实现结果，如下图所示：已经全部实现完成，没有错误
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693065880313.png)

### 8、下载

在下载程序之前，首先要先生成用于下载到器件中的比特流文件，该文件的后缀为“.bit”。我们点击 “Flow Navigator”窗口中的“Generate Bitstream”按钮，如下图所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693065950884.png)
此时我们可以看到在“Design Runs”窗口中显示正在生成比特流，如下图所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693065962491.png)
比特流生成完毕之后，Vivado 会弹出提示窗口，如下图所示：
图中有四个选项，分别是
- 打开实现设计
- 查看报告
- 打开硬件管理
- 生成固化文件。
 
我们可以直接选 择上图的“Open Hardware Manager”进入硬件管理界面，
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693065989636.png)
也可以如上图所示，选择“Cancel”关闭该页面， 按照下面的描述进入硬件管理页面。 接下来我们开始下载比特流，点击“Flow Navigator”窗口中的“Open Hardware Manager”按钮，如 下图所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693066029064.png)
接着 Vivado 就会打开 Hardware Manager，同时窗口布局也跟着发生了变化，如下图所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693066061542.png)

接下来我们就要用到开发板和 Xilinx 下载器了。首先将 Xilinx 下载器一端连接电脑，另一端与开发板 上的 JTAG 接口相连接；然后连接开发板电源线，并打开电源开关。下图为领航者开发板的实物连接图
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693066083718.png)

开发板连接完成并打开电源开关后，点击“Hardware”子窗口中的“Auto Connect”按钮，如下图所 示：

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693066094117.png)
在“Hardware”子窗口中出现如下界面就表示 Vivado 就已经和下载器连接成功了，如下图所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693066108345.png)
我们点击上图中的“Program Device”，弹出的界面如下图所示
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693066118701.png)

此时 Bitstream File 一栏会自动识别到工程的比特流文件，我们直接点击“Program”按钮下载程序。 
“Enable end of startup check”勾选就是使用下载完成校验，如果下载失败就会返回一个错误提示，一 般这里我们都是默认勾选的。

程序下载完成后，我们可以看到位于底板上的 PL_LED0 灯是常灭状态，此时按下 PL_KEY0 按键，PL_LED0 灯会被点亮，松开按键，LED 灯被熄灭，如下图所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693066170758.png)

> 需要说明的是，下载完比特流后，如果开发板断电，程序会丢失。如果想要程序断电不丢失的话，需要将程序固化至开发板中，这个需要在嵌入式 Vitis 软件中完成，ZYNQ 芯片无法单独固化比特流文件 （PL 的配置文件）。**这是由于 ZYNQ 非易失性存储器的引脚（如 SD 卡、QSPI Flash）是 ZYNQ PS 部分 的专用引脚，这些非易失性存储器由 PS 的 ARM 处理器进行驱动，需要将 bit 流文件和 elf 文件（软件程 序的下载文件）合成一个 BOOT.BIN，才能进行固化，因此需要学习 ZYNQ 嵌入式 VITIS 的开发流程。** 在 《领航者 ZYNQ 之嵌入式开发指南.pdf》文档中“第七章 程序固化实验”，会有一个单独的章节向大家介 绍程序固化的方法。 

> 本章内容有限，Vivado 工具还有更多的使用规则本章没有进行介绍，用户可以查找 Xilinx 官方的使用 手册进行学习。 
> 1．Vivado Design Suite User Guide:System-Level Design Entry (UG895)。
> 2．Vivado Design Suite User Guide: DesignAnalysis and Closure Techniques(UG906

### 程序固化

使用vivado烧录下载后，会掉电丢失，这里讲解两种方法固化程序

#### boot程序文件制作


#### 1、QSPI启动

#### 2、SD卡启动

# Verilog语法

## Verilog 和 C 的区别 
Verilog 是硬件描述语言，在编译下载到 FPGA 之后，会生成电路，所以 Verilog 全部是**并行处理与运行**的；C 语言是软件语言，编译下载到单片机/CPU 之后，还是**软件指令**，而不会根据你的代码生成相应的硬件电路，而**单片机/CPU 处理软件指令需要取址、译码、执行，是串行执行的**。 Verilog 和 C 的区别也是 FPGA 和单片机/CPU 的区别，由于 FPGA 全部并行处理，所以处理速度非常快，这个是 FPGA 的最大优势，这一点是单片机/CPU 替代不了的。

## Verilog 基础知识

### Verilog 的逻辑值
逻辑 0：表示低电平，也就是对应我们电路的 GND； 逻辑 1：表示高电平，也就是对应我们电路的 VCC； 逻辑 X：表示未知，有可能是高电平，也有可能是低电平； 逻辑 Z：表示高阻态，外部没有激励信号是一个悬空状态。

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_56_.jpg)

### Verilog 的标识符 
- 定义 
标识符(identifier）用于定义模块名、端口名和信号名等。Verilog 的标识符可以是任意一组字母、数 字、$和_(下划线)符号的组合，但标识符的第一个字符必须是字母或者下划线。另外，标识符是区分大小 写的。以下是标识符的几个例子： 
Count 
COUNT //与 Count 不同。 
R56_68 
FIVE$ 
- 推荐写法如下： 
  count fifo_wr 不建议大小写混合使用，普通内部信号建议全部小写，参数定义建议大写，另外信号命名最好体现信 号的含义。 规范建议 以下是一些书写规范的要求： 
	- 1、用有意义的有效的名字如 sum、cpu_addr 等。 
	- 用下划线区分词语组合，如 cpu_addr。 
	- 采用一些前缀或后缀，比如：时钟采用 clk 前缀：clk_50m，clk_cpu；低电平采用_n 后缀： enable_n； 
	- 统一缩写，如全局复位信号 rst。 
	- 同一信号在不同层次保持一致性，如同一时钟信号必须在各模块保持一致。 
	- 自定义的标识符不能与保留字（关键词）同名。 
	- 参数统一采用大写，如定义参数使用 SIZE。

### Verilog 的数字进制格式 
Verilog 数字进制格式包括二进制、八进制、十进制和十六进制，一般常用的为二进制、十进制和十六 进制。 
- 二进制表示如下：4’b0101 表示 4 位二进制数字 0101； 
- 十进制表示如下：4’d2 表示 4 位十进制数字 2（二进制 0010）； 
- 十六进制表示如下：4’ha 表示 4 位十六进制数字 a（二进制 1010），十六进制的计数方式为 0，1， 2…9，a，b，c，d，e，f，最大计数为 f（f：十进制表示为 15）。 

当代码中没有指定数字的位宽与进制时，**默认为 32 位的十进制**，比如 100，实际上表示的值为 32’d100。

### Verilog 的数据类型
在 Verilog 语法中，主要有三大类数据类型，即寄存器类型、线网类型和参数类型。从名称中，我们可以看出，真正在数字电路中起作用的数据类型应该是寄存器类型和线网类型。

1. 寄存器类型
- 寄存器类型表示一个抽象的**数据存储单元**（但不一定就是寄存器），它**只能在 always 语句和 initial 语句中被赋值**。
- 如果语句描述的是**时序逻辑**，即 always 语句带有时钟信号，则该**寄存器变量对应为寄存器**
- 如果语句描述的是组合逻辑，即 always 语句不带有时钟信 号，则该**寄存器变量对应为硬件连线**
- 寄存器类型的**缺省值是 x**（未知状态）。
- 寄存器数据类型有很多种，如 reg、integer、real 等，其中最常用的就是 reg 类型，它的
- **always语句中的数据类型必须是reg型**

使用方法如 下

``` verilog
//reg define
reg [31:0] delay_cnt; //延时计数器 32位的
reg key_flag ; //按键标志 默认1bit
```

2. 线网类型
- 线网表示 Verilog 结构化元件间的物理连线。
- 它的值由驱动元件的值决定，例如连续赋值或门的输出。如果没有驱动元件连接到线网，线网的缺省值为 z（高阻态）。
- 线网类型同寄存器类型一样也是有很 多种，如 tri 和 wire 等，其中最常用的就是 wire 类型，它的使用方法如下：

``` verilog
//wire define
wire data_en; //数据使能信号
wire [7:0] data ; //数据
```

3. 参数类型 
- 参数其实就是一个常量，常被用于定义状态机的状态、数据位宽和延迟大小等
- 可以在运行时修改参数的值，因此它又常被用于一些参数可调的模块中，使用户在实例化模块时，可以根据需要配置参数。
- 在定义参数时，我们可以一次定义多个参数，参数与参数之间需要用逗号隔开。
- 参数的定义是局部的，只在当前模块中有效。
它的使用方法如下：

``` verilog
//parameter define
parameter DATA_WIDTH = 8; //数据位宽为8位
```

## Verilog 的运算符

### 算术运算符
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_57_.jpg)

### 关系运算符
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_58_.jpg)
### 逻辑运算符
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_59_.jpg)
### 条件运算符
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_60_.jpg)
### 位运算符
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_61_.jpg)
### 移位运算符
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_62_.jpg)
### 拼接运算符
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_63_.jpg)

``` Verilog
A = 4'b1010 ;
B = 1'b1 ;
Y1 = {B, A[3:2], A[0], 4'h3 };  //结果为Y1='b1100_0011
Y2 = {4{B}, 3'd4};  //结果为 Y2=7'b111_1100
Y3 = {32{1'b0}};  //结果为 Y3=32h0，常用作寄存器初始化时匹配位宽的赋初值
```

### 运算符的优先级
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_64_.jpg)

### 关键字
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693106957428.png)

##  程序框架

``` verilog
1 module led(
2   input sys_clk , //系统时钟
3   input sys_rst_n, //系统复位，低电平有效
4   output reg [3:0] led //4位LED灯
5 );
6
7 //parameter define
8 parameter WIDTH = 25 ;
9 parameter COUNT_MAX = 25_000_000; //板载50M时钟=20ns，0.5s/20ns=25000000，需要25bit
10 //位宽
11
12 //reg define
13 reg [WIDTH-1:0] counter ;
14 reg [1:0] led_ctrl_cnt;
15
16 //wire define
17 wire counter_en ;
18
19 
20 //** main code
21 
22
23 //计数到最大值时产生高电平使能信号
24 assign counter_en = (counter == (COUNT_MAX - 1'b1)) ? 1'b1 : 1'b0;
25
26 //用于产生0.5秒使能信号的计数器
27 always @(posedge sys_clk or negedge sys_rst_n) begin
28   if (sys_rst_n == 1'b0)
29     counter <= 1'b0;
30   else if (counter_en)
31     counter <= 1'b0;
32   else
33     counter <= counter + 1'b1;
34   end
35
36 //led流水控制计数器
37 always @(posedge sys_clk or negedge sys_rst_n) begin
38   if (sys_rst_n == 1'b0)
39     led_ctrl_cnt <= 2'b0;
40   else if (counter_en)
41     led_ctrl_cnt <= led_ctrl_cnt + 2'b1;
42   end
43
44 //通过控制IO口的高低电平实现发光二极管的亮灭
45 always @(posedge sys_clk or negedge sys_rst_n) begin
46   if (sys_rst_n == 1'b0)
47     led <= 4'b0;
48   else begin
49     case (led_ctrl_cnt)
50       2'd0 : led <= 4'b0001;
51       2'd1 : led <= 4'b0010;
52       2'd2 : led <= 4'b0100;
53       2'd3 : led <= 4'b1000;
54     default: ;
56   end 
57 end 
58 
59 endmodule
```

第 1 行为模块定义，模块定义以 module 开始，endmodule 结束，如 59 行所示。 
其次 2 到 5 行为端口定义，需要定义 led 模块的输入信号和输出信号，此处输入信号为系统时钟和复位信号，输出为 led 控制信号。 
7 到 9 行为参数 parameter 定义，语法如 7 到 9 行所示，定义 parameter 的好处是可以灵活改变参数数字就能控制一些计数器最大计数值或者信号位宽的最大位宽。 
12 到 14 行为 reg 信号定义，reg 信号一般情况下代表寄存器，比如此处控制 0.5 秒使能信号的计数器 counter。 
16 到 17 行为 wire 信号定义，wire 信号就是硬件连线，比如此处的 counter_en，代表计数到最大值时 产生高电平使能，本质上是一个硬件连线，其实代表的是一些计数器/寄存器做逻辑判断的结果。 
19 到 21 行为 moudle 开始的注释，不添加工具综合也不会报错，但是我们推荐添加，作为一个良好的 编程规范。 
23 到 24 行为 assign 语句的样式，条件成立选择 1，否则选择 0。 
26 到 34 行是 always 语句的样式，
27 行代表在时钟上升沿或者复位的下降沿进行信号触发。begin/end 代表语句的开始和结束。
28 到 33 行为 if/else 语句，和 C 语言是比较类似的。
29 行的“<=”标记代表信号 是非阻塞赋值，信号赋值有非阻塞赋值和阻塞赋值两个方式，这个我们后面会详细解释。 
36 和 42 行也是一个 always 语句，和 26 到 34 行类似。 
44 和 57 行也是一个 always 语句，不过这个 always 语句中嵌入了一个 case 语句，case 语句的语法如 49 到 55 行所示，需要一个 case 关键字开始，endcase 关键字结束，default 作为默认分支，和 C 语言也是 类似的。当然 case 语句也可以用在不带时钟的 always 语句中，不过本例子的 always 都是带有时钟的。不 带时钟的 always 和带时钟的 always 语句的差异这个我们后面也会详细解释。 
59 行是 endmodule 标记，代表模块的结束。 

对于if条件超过一条赋值语句的情况，必须添加begin和end，代码如下所示：

``` verilog
if(en == 1'b1) begin
  b <= 1'b1;
  c <= 1'b1;
end：
```

## 阻塞与非阻塞赋值

### 阻塞赋值，
阻塞赋值，顾名思义，即在一个 always 块中，后面的语句会受到前语句的影响，具体来说，在同一个 always 中，一条阻塞赋值语句如果没有执行结束，那么该语句后面的语句就不能被执行，即被“阻塞”。也 就是说 always 块内的语句是一种顺序关系，这里和 C 语言很类似。符号“=”用于阻塞的赋值（如:b = a;），阻塞赋值“=”在 begin 和 end 之间的语句是顺序执行，属于串行语句。 
在这里定义两个缩写： 
- RHS：赋值等号右边的表达式或变量可以写作 RHS 表达式或 RHS 变量； 
- LHS：赋值等号左边的表达式或变量可以写作 LHS 表达式或 LHS 变量； 

阻塞赋值的执行可以认为是只有一个步骤的操作，即计算 RHS 的值并更新 LHS，此时不允许任何其他语句的干扰，所谓的阻塞的概念就是值在同一个 always 块中，其后面的赋值语句从概念上来讲是在前面 一条语句赋值完成后才执行的。

使用阻塞赋值为例来实现这样一个功能：在复位的时候，a=1，b=2，c=3；而在没有复位的时候，a 的值清零， 同时将 a 的值赋值给 b，b 的值赋值给 c，代码以及信号波形图如下图所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693107402640.png)
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693107407809.png)
从波形图中可以看到，在复位的时候（rst_n=0），a=1，b=2，c=3； 而结束复位之后（波形图中的 0 时刻），当 clk 的上升沿到来时（波形图中的 2 时刻），a=0，b=0，c=0。 
这是因为阻塞赋值是在当前语句执行完成之后，才会执行后面的赋值语句，因此首先执行的是 a=0，赋值完成后将 a 的值赋值给 b，由于此时 a 的值已经为 0，所以 b=a=0，最后执行的是将 b 的值赋值给 c，而 b 的值已经赋值为 0，所以 c 的值同样等于 0。

### 非阻塞赋值（Non-Blocking）
符号“<=”用于非阻塞赋值（如:b <= a;），非阻塞赋值是由时钟节拍决定，在时钟上升到来时，执行赋值语句右边，然后**将 begin-end 之间的所有赋值语句同时赋值到赋值语句的左边**，**注意：是 begin—end 之间的所有语句，一起执行，且一个时钟只执行一次，属于并行执行语句**。这个是和 C 语言最大的一个差 异点，大家要逐步理解并行执行的概念。 
非阻塞赋值的操作过程可以看作两个步骤：

 1. 赋值开始的时候，计算 RHS；
 2. 赋值结束的时候，更新 LHS。

所谓的非阻塞的概念是指，在计算非阻塞赋值的 RHS 以及 LHS 期间，允许其它的非阻塞赋值语句同时计算 RHS 和更新 LHS。 
下面使用非阻塞赋值同样来实现这样一个功能：在复位的时候，a=1，b=2，c=3；而在没有复位 的时候，a 的值清零，同时将 a 的值赋值给 b，b 的值赋值给 c，代码以及信号波形图如下图所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693107555685.png)
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693107559585.png)
从波形图中可以看到，在复位的时候（rst_n=0），a=1，b=2， c=3；而结束复位之后（波形图中的 0 时刻），当 clk 的上升沿到来时（波形图中的 2 时刻），a=0，b=1， c=2。这是因为非阻塞赋值在计算 RHS 和更新 LHS 期间，允许其它的非阻塞赋值语句同时计算 RHS 和更新 LHS。在波形图中的 2 时刻，RHS 的表达是 0、a、b，分别等于 0、1、2，这三条语句是同时更新LHS，所以 a、b、c 的值分别等于 0、1、2。 

总结如下。 在**描述组合逻辑电路的时候，使用阻塞赋值**，比如 assign 赋值语句和不带时钟的 always 赋值语句，这种电路结构只与输入电平的变化有关系，代码如下：

``` verilog
示例1：assign赋值语句
assign data = (data_en == 1'b1) ? 8'd255 : 8'd0;

示例2：不带时钟的always语句
always @(*) begin
  if (en) begin
    a = a0;
    b = b0;
  end
  else begin
    a = a1;
    b = b1;
  end
end
```

在描述**时序逻辑的时候，使用非阻塞赋值**，综合成时序逻辑的电路结构，比如带时钟的 always 语句；这种电路结构往往与触发沿有关系，只有在触发沿时才可能发生赋值的变化，代码如下：

``` verilog
always @(posedge sys_clk or negedge sys_rst_n) begin
if (!sys_rst_n) begin
	a <= 1'b0;
	b <= 1'b0;
end
else begin
	a <= c;
	b <= d;
end
end
```

## 带时钟和不带时钟的always
always 语句可以带时钟，也可以不带时钟。在 always 不带时钟时，逻辑功能和 assign 完全一致，虽然

``` verilog
产生的信号定义还是 reg 类型，但是该语句产生的还是组合逻辑。
44 reg [3:0] led；
45 always @(*) begin
49 case (led_ctrl_cnt)
50 2'd0 : led = 4'b0001;
51 2'd1 : led = 4'b0010;
52 2'd2 : led = 4'b0100;
53 2'd3 : led = 4'b1000;
54 default : led = 4'b0000;
55 endcase
57 end
```
在 always 带时钟信号时，这个逻辑语句才能产生真正的寄存器，如下示例 counter 就是真正的寄存器。
``` verilog
26 //用于产生 0.5 秒使能信号的计数器
27 always @(posedge sys_clk or negedge sys_rst_n) begin
28 if (sys_rst_n == 1'b0)
29 counter <= 1'b0;
30 else if (counter_en)
31 counter <= 1'b0;
32 else
33 counter <= counter + 1'b1;
34 end
```

## latch锁存器

锁存器，是一种对**脉冲电平敏感**的存储单元电路。锁存器和寄存器都是基本存储单元，**锁存器是电平触发的存储器，寄存器是边沿触发的存储器**。两者的基本功能是一样的，都可以存储数据。**锁存器是组合逻辑产生的，而寄存器是在时序电路中使用**，由时钟触发产生的。
latch 的主要危害是会产生毛刺（glitch），这种毛刺对下一级电路是很危险的。并且其隐蔽性很强， 不易查出。因此在设计中，**应尽量避免 latch 的使用**。 
代码里面出现 latch 的两个原因：在组合逻辑中，if 或者 case 语句不完整的描述，比如 if 缺少 else 分 支，case 缺少 default 分支，导致代码在综合过程中出现了 latch。解决办法就是 if 必须带 else 分支，case 必须带 default 分支。

只有不带时钟的 always 语句 if 或者 case 语句不完整才会产生 latch，带时钟的if语句或者 case 语句不完整描述不会产生 latch。
下面为缺少 else 分支的带时钟的 always 语句和不带时钟的 always 语句，通过实际产生的电路图可以看到第二个是有一个 latch 的，第一个仍然是普通的带有时钟的寄存器。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693108087767.png)

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693108239893.png)

## 状态机
Verilog 是硬件描述语言，硬件电路是并行执行的，当需要按照流程或者步骤来完成某个功能时，代码 中通常会使用很多个 if 嵌套语句来实现，这样就增加了代码的复杂度，以及降低了代码的可读性，这个时候就可以使用状态机来编写代码。

状态机相当于一个控制器，它将一项功能的完成分解为若干步，每一步对应于二进制的一个状态，通过预先设计的顺序在各状态之间进行转换，状态转换的过程就是实现逻辑功 能的过程。 

状态机，全称是有限状态机（Finite State Machine，缩写为 FSM），是一种在有限个状态之间按一定 规律转换的时序电路，可以认为是组合逻辑和时序逻辑的一种组合。状态机通过控制各个状态的跳转来控制流程，使得整个代码看上去更加清晰易懂，在控制复杂流程的时候，状态机优势明显，因此基本上都会用到状态机，如 SDRAM 控制器等。

根据状态机的输出是否与输入条件相关，可将状态机分为两大类，
- 米勒 (Mealy)型状态机：组合逻辑的输出不仅取决于当前状态，还取决于输入状态。 
- 摩尔(Moore)型状态机：组合逻辑的输出只取决于当前状态。 

### Mealy 状态机 

米勒状态机的模型如下图所示，模型中第一个方框是指产生下一状态的组合逻辑 F，F 是当前状态和 输入信号的函数，状态是否改变、如何改变，取决于组合逻辑 F 的输出；第二框图是指状态寄存器，其由 一组触发器组成，用来记忆状态机当前所处的状态，状态的改变只发生在时钟的跳边沿；第三个框图是指 产生输出的组合逻辑 G，状态机的输出是由输出组合逻辑 G 提供的，G 也是当前状态和输入信号的函数。

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_65_.jpg)

### Moore 状态机 

摩尔状态机的模型如下图所示，对比米勒状态机的模型可以发现，其区别在于米勒状态机的输出由当 前状态和输入条件决定的，而摩尔状态机的输出只取决于当前状态。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_66_.jpg)

## 模块化设计

FPGA 逻辑设计中通常是一个大的模块中 包含了一个或多个功能子模块，Verilog 通过模块调用或称为**模块实例化**的方式来实现这些子模块与高层模 块的连接，有利于简化每一个模块的代码，易于维护和修改。 下面以一个实例(静态数码管显示实验)来说明模块和模块之间的例化方法。
在静态数码管显示实验中，我们根据功能将 FPGA 顶层例化了以下两个模块：计时模块 （time_count）和数码管静态显示模块（seg_led_static），如下图所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693108804140.png)

计时模块部分代码如下所示：

``` verilog
1 module time_count(
2 	input clk , // 时钟信号
3 	input rst_n , // 复位信号
4
5 	output reg flag // 一个时钟周期的脉冲信号
6 );
7
8 //parameter define
9 parameter MAX_NUM = 25000_000; // 计数器最大计数值
……
34 endmodule
```

数码管静态显示模块部分代码如下所示：

``` verilog
1 module seg_led_static (
2 input clk , // 时钟信号
3 input rst_n , // 复位信号（低有效）
4
5 input add_flag, // 数码管变化的通知信号
6 	output reg [5:0] sel , // 数码管位选
7 	output reg [7:0] seg_led // 数码管段选
8 );
……
66 endmodule
```

顶层模块代码如下所示：
代码的大致功能就是，
- 计数子模块根据系统时钟计数，到TIME_SHOW 最大计数后，更新flag
- 然后显示子模块收到flag(add_flag)变换后，就会将数码管显示数据+1，更新一次显示
- 顶层模块，就是将输入输出信号，通过例化子模块的方式传递给子模块，同时将各自模块的信号链接

``` basic
1 module seg_led_static_top (
2 	input sys_clk , // 系统时钟
3 	input sys_rst_n, // 系统复位信号（低有效）
4
5 	output [5:0] sel , // 数码管位选
6 	output [7:0] seg_led // 数码管段选
7
8 );
9
10 //parameter define
11 parameter TIME_SHOW = 25'd25000_000; // 数码管变化的时间间隔0.5s
12
13 //wire define
14 wire add_flag; // 数码管变化的通知信号
15
16 //*****************************************************
17 //** main code
18 //*****************************************************
19
20 //例化计时模块
21 time_count #(
22 	.MAX_NUM (TIME_SHOW)
23 ) u_time_count(
24 	.clk (sys_clk ),
25 	.rst_n (sys_rst_n),
26
27 	.flag (add_flag )
28 );
29
30 //例化数码管静态显示模块
31 seg_led_static u_seg_led_static (
32 	.clk (sys_clk ),
33 	.rst_n (sys_rst_n),
34
35 	.add_flag (add_flag ),
36 	.sel (sel ),
37 	.seg_led (seg_led )
38 );
39
40 endmodule
```
上面贴出了顶层模块的完整代码，子模块只贴出了模块的端口和参数定义的代码。这是因为顶层模块对子模块做例化时，只需要知道子模块的端口信号名，而不用关心子模块内部具体是如何实现的。如果子模块内部使用 parameter 定义了一些参数，Verilog 也支持对参数的例化（也叫参数的传递），即顶层模块可以通过例化参数来修改子模块内定义的参数。 我们先来看一下顶层模块是如何例化子模块的，例化方法如下图所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_67_.jpg)
上图右侧是例化的数码管静态显示模块，子模块名是指被例化模块的模块名，而例化模块名相当于标识，当例化多个相同模块时，可以通过例化名来识别哪一个例化，我们一般命名为“u_”+“子模块名”。信号 列表中“.”之后的信号是数码管静态显示模块定义的端口信号，括号内的信号则是顶层模块声明的信号，这 样就将顶层模块的信号与子模块的信号一一对应起来，同时需要注意信号的位宽要保持一致。 

接下来再来介绍一下参数的例化，参数的例化是在模块例化的基础上，增加了对参数的信号定义，如 下图所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_68_.jpg)
在对参数进行例化时，在模块名的后面加上“#”，表示后面跟着的是参数列表。计时模块定义的 MAX_NUM 和顶层模块的 TIME_SHOW 都是等于 25000_000，当在顶层模块定义 TIME_SHOW=12500_000 时，那么子模块的 MAX_NUM 的值实际上是也等于 12500_000。当然即使子模 块包含参数，在做模块的例化时也可以不添加对参数的例化，这样的话，子模块的参数值等于该模块内部实际定义的值。

值得一提的是，Verilog 语法中的 localparam 代表的意思同样是参数定义，用法和 parameter 基本一 致，区别在于 parameter 定义的参数可以做例化，而 localparam 定义的参数是指本地参数，上层模块不可 以对 localparam 定义的参数做例化。

# 数字电路基础

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693135061532.png)

# 实战

## Modelsim联合仿真

教程:
https://blog.csdn.net/weixin_42837669/article/details/107829499

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_1_.jpg)

## 流水灯

系统时钟 sys_clk 的时钟周期为 20ns（对应开发板板载的晶振频率为 50Mhz），计数器计时 0.5s 需要 0.5s/20ns=500000000ns/20ns = 25000000 个时钟周期，由于计数器是从 0 开始计数，所以计数器最大计数到 25000000-1，刚好是 0.5s。
由此绘制出 flow_led 模块的波形图如下图所示：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693140534588.png)

### 编写代码 
计数器 cnt 实现计时的功能，其赋值在时序逻辑 always 语句中完成。对 led 的赋值同样在时序逻辑 always 语句中完成，由于流水灯的效果是依次按顺序点亮其中一个 LED 灯，所以可以通过移位的方式实 现。当 cnt 计数到最大值时，对 led 端口实现一次移位的操作。流水灯（flow_led.v）代码编写如下：


## 按键LED


## 按键蜂鸣器与分层设计

vivado  自动识别顶层文件

## 触摸按键LED

## 呼吸灯&在线逻辑分析仪

### 在线逻辑分析仪


 1. HDL 实例化调试探针流程，这是集成层次最高的方法，但其灵活性也较差。在调试工作完毕之后，还需要在 HDL 源代码中删除 ILA IP 核，然后重新综合以生成最终的比特流。 
 2.使用 Debug 标记创建 ILA 调试环境。用户设 置的调试信息会以 Tcl XDC 调试命令的形式保存到 XDC 约束文件中，在实现阶段，Vivado 会读取这些 XDC 调试命令，并在布局布线时加入这些 ILA IP 核。在调试工作完毕之后，用户可以在 HLD 代码中删除 之前添加的“Mark Debug”综合属性，并且在 XDC 文件中删除调试命令，然后再对设计进行重新编译， 以生成最终的比特流。 
3. 网表插入调试探针流程。第三种方法与第二种方法的使用区别很小，只是标记信号的方式不同，第二种是在综合前标记，第三种是在综合后标记。 4. 第四种方法是手动地在 XDC 约束文件中书写对应的 Tcl XDC 调试命令，这种方法集成层次最低，一般不会使用这种方法。 

 本小节同样以呼吸灯实验的工程为例，介绍前三种 ILA 调试流程，即“HDL 实例化调试探针流 程”、“使用 Debug 标记创建 ILA 调试环境”和“网表插入调试探针流程”。
 
#### 1、HDL 实例化调试探针流程
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_4_121.jpg)

General Options（常规选项）主要有如下三部分： 
1. Monitor Type：ILA 探针接口类型设置。Native 通常是用来测量电平或一定位宽信号，AXI 直接测量 AXI 总线的信号。
	1. Number of Probes：探针数量设置，在 GUI 界面最大可设置 64 个，可以通过 TCL 脚本去生成 IP Core 超过 64 个，或者可以通过生成多个 ILA IP Core 去调试更多信号。每个探针还可以设置位宽，你可以测量一个多位的信号，也可以测量硬件的多个位，比如AD的14位数据。
	2. Sample Data Depth：采样数据深度，设置的数值越大，采样的数据越多，看到的波形数据越多，但最终占用的资源也会越多，并不是设置的越大越好。

探针无法探测时钟信号。
	
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_6_23345.jpg)

图中 Synthesis Options（综合选项）我们选择的是 Out of context per IP，简称 OOC。
对于顶层设计，Vivado使用自顶向下的全局（Global）综合方式，将顶层之下的所有逻辑模块都进行 综合，但是设置为 OOC 方式的模块除外，它们独立于顶层设计而单独综合。通常在整个设计周期中，顶层设计会被多次修改并综合，但有些子模块在创建完毕之后不会因为顶层设计的修改而被修改，如 IP，它们被设置为 OOC 综合方式。OOC 模块只会在综合顶层之前被综合一次，这样在顶层的设计迭代过程中， OOC 模块就不必跟随顶层模块而一次次产生相同结果的多余综合了，所以 OOC 流程减少了设计的周期， 并消除了设计迭代，使您可以保存和重用综合结果。 
在对顶层进行综合时，OOC 模块会被视为 黑盒子，并且不会参与到顶层的综合中来。在综合之后的编译过程中，OOC 模块的黑盒子才会被打开，这 时其网表才是可见的，并参与到全局设计的实现过程中来。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/1693491185010.png)


## IP核PLL


