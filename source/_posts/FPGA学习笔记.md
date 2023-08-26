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

运行Vivado 软件，进入License管理
点击copy license，选择下载的文件
在View License Status 中查看是否成功，有效期至2037
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_34_.jpg)

### 参考资料：[（一） vivado2018.3安装注册指南](https://blog.csdn.net/weixin_42668358/article/details/125512721)


## Modelsim软件

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

1. Project manager：工程管理，新建文档等
2. IP INTEGRATOR：IP核设计，类似与设计好的库，这个一般在后期用到，可选
3. Simulation：仿真，可选。正在原子的设计流程视在Modelsim中仿真，所以Vivado中就不需要这一步了。这里的仿真其实就是使用的Modelsim，是厂家集成到Vivado的，当然功能后速度都会相对差一些，所以选择直接使用
### 2、设计输入

双击 Sources→Design Sources 下的点亮 LED 灯文件，在 Vivado 软件中显示的界面如下图所示。

每次保存后，Vivado 都会对源文件进行部分语法的检查，如果有语法的错误，Vivado 会给出提示。另外，在大多数情况下，Vivado IDE 会自动识别设计的顶层模块，当然，用户也可以手动指定顶层模块。从 “Sources”窗口的右击菜单中选择“Set as Top”来手动定义顶级模块。
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/FPGA学习笔记/image_44_.jpg)

### 3、分析与综合

### 4、设计实现

### 5、下载