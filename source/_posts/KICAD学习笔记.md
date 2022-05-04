---
layout: post
title: "KICAD学习笔记"
index_img: https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/kicad.png
date: 2020-05-21
categories: Cad
hide: true
---

[KiCad使用笔记（01）-介绍及设置](https://blog.csdn.net/Naisu_kun/article/details/80589942)

KiCad 是一个开源软件工具，用于创建电子原理图和 PCB 图形。  

有充分的理由认为, KiCad 已足够成熟, 并可以用于开发和维护复杂的电路板。 KiCad 对电路板的大小不做任何限制, 它可以轻松地处理多达 32 个铜层、多达 14 个技术层和多达 4 个辅助层的电路 板。KiCad 可以创建制造印刷电路板所需的所有文件、用于照片绘图仪的 Gerber 文件、钻孔文件、元件位置文件等 等。 

作为开源 (GPL 许可) 软件, KiCad 是面向有意创建开源电子硬件的项目的工程师的理想工具。 在互联网上, KiCad 的主页是:   
https://kicad-pcb.org/

<!--more-->
# 官方入门文档翻译

## 1、KiCad安装

下载链接：  
[https://kicad-pcb.org/download/](https://kicad-pcb.org/download/)  
Windows 版本下镜像站：  
[https://mirror.tuna.tsinghua.edu.cn/kicad/windows/stable/](https://mirror.tuna.tsinghua.edu.cn/kicad/windows/stable/)

## 2、KiCad流程

 KiCad 的特点是独特的工作流程，其中**原理图元件和封装是分开的。仅在创建原理图后才会为元件分配封装。**

KiCad 工作流由两个主要任务组成: **绘制原理图和布置电路板。原理图元件库和 PCB 封装库对于这两个任务都是必需的**。

在下图中, 您将看到一个表示 KiCad 工作流的流程图。 流程图说明了您需要采取哪些步骤, 以及以何种顺序采取这些步骤。（必看）

![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/kicad_flowchart.png)

有关 **库编辑器** 和 **封装编辑器** 请见后文详述。

## 3、使用KICad

### 3.1 快捷键

KiCad 有两种相关但不同的快捷键: 快捷键 (accelerator keys) 和热键 (hotkeys)。

### 3.1.1 快捷键

快捷键的效果与单击菜单或工具栏图标的效果相同, 但在单击鼠标左键之前不会发生任何情况。快捷键显示在所有菜单窗格的右侧：

![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/gsik_accelerator_keys.png)

### 3.1.2 热键

热键 = 快捷键 + 鼠标左键单击。使用热键会立即在当前光标位置启动该命令。使用热键可以在不中断工作流的情况下快速更改命令。 要查看任何 KiCad 工具中的热键，请转到 **帮助 → 列出热键或按 Ctrl+F1** ：

![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/热键列表.png)

您可以从 **首选项 → 首选项 → 热键选项** 菜单编辑热键的分配，并导入或导出它们。

>**注意**  
>在软件的中文界面，这里的热键都被翻译为快捷键，其实就是上述的热键。热键是可编辑且可单独查看的。  
>而实际的快捷键是无法编辑的，且查看的方式只有鼠标左键菜单栏，显示在所有菜单窗格的右侧。除了菜单栏外，其它选择窗口（如鼠标右键弹窗）也会在右侧显示键值，但这里的都是热键，快捷键仅限于菜单栏。  
>在本文档中，热键用括号表示：\[a]。 如果看到 \[a]，只需在键盘上键入 a 键即可。

**例子**

以在原理图中添加走线的示例。

要使用快捷键, 请按 `Shift + W` 调用 `添加导线` 命令 (请注意光标将会更改)。接下来, 左键单击所需的导线开始位置, 开始绘制导线。

使用热键，只需按 `[w]`，线将立即从当前光标位置开始。

### 3.2 鼠标操作

鼠标滚轮，缩小放大视图。按下滚轮 (中键) 鼠标按钮将可以水平和垂直平移视图。


## 4、绘制原理图

1. 运行KiCad。下图是 KiCad 项目管理器的主窗口。 从这里您可以访问八个独立的软件工具：**Eeschema，原理图库编辑器，Pcbnew，PCB 封装编辑器，GerbView，Bitmap2Component，PCB计算器 和 图框编辑器**。请参阅工作流程图，了解如何使用主要工具。
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/kicad_main_window.png)

2. 创建新工程：**文件 → 新建 → 工程**。 将项目文件命名为 `test1`。 项目文件将自动采用扩展名 .pro。
3. 从创建原理图开始。开始原理图编辑器 eeschema , ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/eeschema.png)。它是左边的第一个按钮。或者在左侧文件浏览区双击 `.sch` 文件。
4. 单击顶部工具栏上的 `页面设置` 图标 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/sheetset.png)。 设置适当的 `纸张尺寸`（'A4'，'8.5x11’等）并输入标题为 `test1` 。 如有必要，您将在此处看到可以输入更多信息， 此信息将填充右下角的原理图信息表，需使用鼠标滚轮放大查看。单击 `确定`，退出 `页面设置`。保存整个原理图：**文件 → 保存**
5. 我们现在将放置第一个元件。 单击右侧工具栏中的 `放置符号` 图标 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/add_component.png)。 您也可以按 `添加符号` 热键 [a]。
6. 单击原理图中任意空白部分。 屏幕上将出现 `选择符号` 窗口。 我们要放一个电阻器。 搜索/过滤 **R**esistor 的 `R`。 您可能会注意到电阻器上方的 `Device` 标题。 此 `Device` 标题是元件所在库的名称，这是一个非常通用且有用的库。
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/choose_component.png)
7. 双击 符号名 `R`。这将关闭 `选择符号` 窗口。 通过单击元件在原理图中的的位置将其放置在原理图中。
8. 将鼠标悬停在元件 `R` 上（无需单击）, 然后按下 \[R] 旋转元件。此时如果出现元件叠加的情况，就会弹出菜单明确选择对应的元件。
9. 右键单击元件选择 **属性 -> 编辑值** 。请注意, 下面的右键单击菜单显示的快捷方式都是热键。
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/edit_component_dropdown.png)
也可以通过将鼠标悬停在元件上并按 \[V] 来实现相同的结果。或者，按下 \[E] 将打开到更通用的 `属性` 窗口。
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/value_and_attribute.png)
10. 在文本输入域当中。 用 1k 替换当前值 R。 单击确定。
    >不要更改参考字段（R?），稍后会自动完成。 电阻器上方的值现在应为 1k 。
    >工具 → 编辑符号字段 可以批量修改元件相关数据
    >
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/resistor_value.png)

11. 如果您输入错误并想要删除元件，请右键单击该元件并单击 `删除 `。 这将从原理图中删除元件。 或者，您可以将鼠标悬停在要删除的元件上，然后按 \[Delete]。
12. 您还可以通过将鼠标悬停在原理图页上并按 \[C] 来复制已经存在于原理图页上的元件。单击要放置新复制元件的位置。
13. 右键单击电阻、标识符、编号等，选择移动 或 \[m] 可以实现相同的功能。可以**单独移动**任何符号到满意的位置。
    >右键单击 → 拖动 或 [G]，也可以移动元件，该方式可以在保持导线连接的情况下移动元件。

14. 更改网格大小。 您可能已经注意到，在原理图表上，所有元件都被捕捉到大间距网格上。 您可以通过 **右键单击 → 网格** 轻松更改网格的大小。 `通常，建议使用 50.0mils 的网格`。
15. 重复添加元件步骤，组织原理图纸上的所有元件，如下所示。
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/gsik_tutorial1_010.png)
16. 我们现在需要创建原理图元件 “MYCONN3”。 您可以跳转到标题为 《make-schematic-symbols-in-kicad（制作-原理图-符号-在-kicad），Make Schematic Symbols in KiCad（在 KiCad 中制作原理图符号）》 的部分，了解如何从头开始制作该元件，然后返回本节继续下一步。
17. 您现在可以放置新制作的元件。 按 \[a] 并选择 myLib 库中的 MYCONN3 元件。
18.  单击右侧工具栏上的 `放置电源端口` 按钮 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/add_power.png)。 或者，按 [p]。 在元件选择窗口中，向下滚动并从 `Power`库中选中并且放置 VCC 和 GND，此时原理图效果呈现的如下： 
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/gsik_tutorial1_020.png)

19. 接下来，我们将连接所有元件。 单击右侧工具栏上的 `放置电线` 图标 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/add_line.png)。或者使用 Shift + W 或 \[W]。
    >小心不要选择 放置总线，它位于此按钮的正下方，但线条较粗。
    >
    ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/gsik_tutorial1_040.png)


20. 我们现在将考虑使用标签建立连接的另一种方法。 通过单击右侧工具栏上的 放置网络标签 图标 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/add_line_label.png) 来选择网络标签工具。 你也可以用 \[l]。下面是最终的结果。
    >您不必标记 VCC 和 GND 线，因为标签是从它们所连接的电源对象中隐含的。

    ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/gsik_tutorial1_050.png)
21. 单击右侧工具栏上的 放置 `无连接标志` 图标 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/noconn.png) 。 单击悬空引脚2,3,4和5，表示缺少引脚未连接是故意的。这样在之后的 KiCad 检查时，就不会出现警告。（任何未连接的引脚或电线都会产生警告）
22. 某些元件具有不可见的电源引脚。 您可以通过单击左侧工具栏上的 显示隐藏的引脚 图标 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/hidden_pin.png) 使其可见。 一般隐藏的引脚会自动连接（多为VCC，GND或NC悬空）。 同时你应该尽量不要制作隐藏的电源引脚。
23. 现在需要添加一个 `Power Flag` 来向 KiCad 表明电源是从某个地方进来的。 按 \[a] 并搜索 电源 库中的 PWR_FLAG 。 放置其中两个。 将它们连接到 GND 引脚和 VCC，如下所示。
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/gsik_tutorial1_070.png)

    >这将避免经典的原理图检查警告： 引脚连接到其他一些引脚但没有引脚来驱动它 。

24. 注释原理图符号。 事实上，我们的许多元件仍被命名为 R? 还是 J? 。 通过单击顶部工具栏上的 注释原理图符号 图标 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/annotate.png) ，在注释原理图窗口中，选择 `使用整个原理图` 并单击` 注释` 按钮，将自动完成标识符分配。 点击 关闭 退出操作。
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/标注原理图.png)
    >注意，如果选择 **重置现有标注**，将会打乱PCB电路板中的原有布局。慎重选择！！！

25. 检查原理图的错误。 单击顶部工具栏上的 执行电气规则检查 图标 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/erc.png) 。 单击 运行 按钮。 生成一个报告，通知您任何错误或警告，例如断开的电线。 你应该有 0个错误 和 0个警告。 如果出现错误或警告，原理图中将出现一个小绿色箭头，指示错误或警告所在的位置。 选中 创建ERC文件报告 并再次按 运行 按钮以接收有关错误的更多信息。
26. 编辑封装。右键单击符号 → 属性 → 编辑封装 双击符号，或右键单击符号 → 属性 → 编辑属性 → Foorprint 为每个元件单独编辑封装。（不推荐使用）  
 工具 → 分配封装 或者 单击顶部工具栏上的图标![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/cvpcb.png) 。批量分配PCB封装到原理图符号。
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/批量分配封装.png)
 
     右侧窗格可能只显示可用封装的一个子集。点击图标 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/module_filtered_list.png)，![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/module_library_list.png) ，![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/module_pin_filtered_list.png)以启用或禁用这些筛选器。
 
     你也可以通过 工具 → 编辑符号字段 或者 单击顶部工具栏上的图标 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/编辑符号字段.png)，批量编辑封装（不推荐）
27. 如果您有兴趣知道您选择的封装是什么样的，您可以单击 查看所选封装 图标 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/show_footprint.png) 以预览当前封装。
28. 完成以后，可以通过点击 文件 → 保存原理图 或使用 应用、保存原理图和继续 按钮保存原理图
29. 要创建物料清单（BOM），请转到 Eeschema 原理图编辑器，然后单击顶部工具栏上的 `生成物料清单` 图标![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/bom_(1).png)。 默认情况下，没有处于活动的插件。 您可以通过单击 添加插件 按钮添加一个。这里，我们选择 bom2csv.xsl。
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/bom.png)
    >注意下面的命令行：  
    ` xsltproc -o "%O" "/home/<user>/kicad/eeschema/plugins/bom2csv.xsl" "%I"`  
"%O"  需要修改为 "%O.csv"，表示输出文件格式为 .csv，这样我们就可以使用相关软件正常打开并编辑 BOM 文件了。
完整命令如下：  
`xsltproc -o "%O.csv" "/home/<user>/kicad/eeschema/plugins/bom2csv.xsl" "%I"`

    按 `帮助` 按钮获取更多信息
30. 现在按 `生成`。 该文件（与项目同名）位于项目文件夹中（与.pro文件同级目录）。 使用 LibreOffice Calc 或 Excel 打开 * .csv 文件，进行查看或编辑。

您现在可以转到 PCB 布局部分，这将在下一节中介绍。 但是，在继续之前，让我们快速了解如何使用总线连接元件引脚。

### KiCad 的总线连接

有时您可能需要将元件 A 的多个顺序引脚与元件 B 的其他顺序引脚连接。在这种情况下，您有两个选项：我们已经看到的标记方法或使用总线连接。 让我们看看如何做到这一点。

1. 让我们假设您有三个4针连接器，您想要将引脚连接在一起。 使用标签选项（按 \[L]）标记 P4 部件的引脚4。 将此标签命名为 a1 。 现在按 [Insert] 将相同的项目自动添加到引脚4（引脚3）下方的引脚上。 注意标签是如何自动重命名为 a2 。
    >通过 \[Insert] 可重复上一项 选项，并自动递增编号。用于重复期间项目插入。

2. 在另外两个连接器 CONN_2 和 CONN_3 上重复相同的标签操作，您就完成了。 如果继续制作 PCB，您将看到三个连接器相互连接。 下图2 显示了我们描述的结果。 出于美观目的，还可以使用`将电线放入总线`图标图像![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/add_line2bus.png) 添加一系列入口，将电线放入总线入口和使用`总线线路`图标图像：![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/add_bus2bus.png)将总线放到总线入口 ，如下图3 所示。但是，请注意，对 PCB 没有影响。
3. 应该指出的是，连接到 图2 中的引脚的短导线不是严格必需的。 实际上，标签可以直接应用于引脚。
4. 让我们更进一步，假设你有一个名为 CONN_4 的第四个连接器，无论出于什么原因，它的标签恰好有点不同（b1，b2，b3，b4）。 现在我们想要以引脚到引脚的方式将_Bus a_ 与 Bus b 连接起来。 我们希望不使用引脚标记（这也是可能的），而是使用总线上的标签，每个总线一个标签。
5. 使用之前说明的标记方法连接并标记 CONN_4。 将引脚命名为b1，b2，b3和b4。 使用图标 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/add_line2bus.png) 将引脚连接到一系列 `电线到总线入口` ，并使用图标 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/add_bus.png)连接到总线。见下图4。
6. 在 CONN_4 的总线上放一个标签（按[l]）并命名为 b\[1..4] 。
7. 在前一个总线上放一个标签（按[l]）并将其命名为 a\[1..4] 。
8. 我们现在可以做的是使用带有按钮图像的总线连接总线 a[1..4] 和总线 b[1..4] ： ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/add_bus.png)。
9. 通过将两条总线连接在一起，引脚a1 将自动连接到 引脚b1，a2将连接到b2，依此类推。 图4 显示了最终结果的样子。
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/gsik_bus_connection.png)


## 布局电路板

1. 从 KiCad 项目管理器，单击 Pcb布局编辑器 图标 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/pcbnew.png) 。 您还可以使用 Eeschema 中的相应工具栏按钮。 Pcbnew 窗口将打开。 如果您收到一条消息，指出 \*.kicad_pcb 文件不存在并询问您是否要创建它，只需单击是。
    >也可以直接在Eeschema  原理图 界面中菜单栏上的 **工具 -> 从原理图更新 PCB..** ，直接基于原理图生成 PCB ，操作完成以后将会自动打开如下的 Pcbnew 编辑界面。若如此，可跳过步骤5。

2. 首先输入一些原理图信息。 单击顶部工具栏上的 页面设置 图标 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/sheetset.png) 。 将相应的 纸张尺寸（ A4，8.5x11 等）和 标题 设置为 教程1 。
3. 最好将 间距 和 最小布线宽度 设置为PCB制造商要求的宽度。 通常，您可以将间隙设置为 0.25mm，将最小轨道宽度设置为 0.25mm 。 单击 设置 → 设计规则 菜单。 如果它尚未显示，请单击 网络类编辑器 选项卡。 将窗口顶部的 间距 字段更改为 0.25mm，将 布线宽度 字段更改为 0.25mm，如下所示。 这里的测量单位是 mm。
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/design_rules.png)
4. 单击 全局设计规则 选项卡，将 最小布线宽度 设置为 0.25mm 。 单击 确定 按钮以提交更改并关闭 设计规则编辑器 窗口。
5. 单击菜单栏上的 **工具 -> 从原理图更新 PCB..** ，导入元件。将元件移动到板的中间。
6. 所有元件都通过称为 `飞线` 的一组细线体现引脚连接关系。 按下 显示/隐藏板飞线 按钮 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/general_ratsnest.png) 可以切换显示状态。
7. 您可以通过将每个元件悬停在其上并按 \[m] 来移动它们。 单击要放置它们的位置。 或者，您可以通过单击选择元件然后拖动它。 按 \[r] 旋转元件。 移动所有元件，直到最小化电线交叉的数量。
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/gsik_tutorial1_080.png)
8. 现在我们将定义 PCB 的板子形状。 从顶部工具栏的下拉菜单中选择 边切（边缘切割） 图层。 单击右侧工具栏上的 添加图形线 图标 ![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/add_dashed_line.png) 。 沿着电路板边缘，在每个角落点击，并记住在绿色边缘和 PCB 边缘之间留一个小间隙。
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/select_edge_cuts.png)
10. 接下来，连接除 GND 之外的所有电线。 事实上，我们将使用放置在电路板底部铜线（称为 B.Cu ）的接地层一次连接所有 GND 连接。
11. 如果您决定改为使用 4层 PCB，请转到 设置 → 层设置 并将 铜层 更改为 4.在 层 表中，您可以命名图层并确定它们的含义 用于。 请注意，可以通过 预设图层分组 菜单选择非常有用的预设。
12. 现在我们必须选择我们想要处理的铜层。 单击右侧工具栏上的 布线 图标![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/add_tracks.png) 。 单击开始布线，双击结束。布线宽度将默认为 0.250mm。 您可以从顶部工具栏的下拉菜单中更改布线宽度。 请注意，默认情况下，您只有一个可用的布线宽度。
13. 如果您想添加更多的布线宽度，请转到：文件 → 电路板设置 或 单击顶部工具栏上的电路板设置按钮![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/电路板设置.png)，在此设置窗口的 **设计规则 →  导线和过孔** 选项中，添加您希望可用的任何其他宽度。
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/step-7.png)
14. 然后，您可以在布置电路板时从下拉菜单中选择布线的宽度。 请参阅下面的示例（英寸）。
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/step-8.png)
15. 或者，您可以添加一个 网络类。 转到 设计规则 → 网络类编辑器 并添加新类。 这样当我们需要批量指定一类相同类型的布线（如电源类，特殊信号类），就可以在该界面选择该类，并指定该类导线所属网络类，自动批量应用布线规则而不影响到其他布线。
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/网络列表.png)
16. 如果要更改网格大小，右键单击 → 网格。 在放下元件并将它们与轨道连接在一起之前或之后，请务必选择合适的网格尺寸。
17. 重复此过程，直到连接除 J1 的引脚3之外的所有电线。 您的电路板应如下所示。
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/gsik_tutorial1_090.png)
18. 现在我们将制作一个连接到所有 GND 引脚的接地层。 单击右侧工具栏上的 添加填充区域 图标 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/add_zone.png) 。 放置覆铜。
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/gsik_tutorial1_100.png)
19. 单击顶部工具栏上的 执行设计规则检查 图标 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/drc.png) 运行设计规则检查器。 点击 开始DRC 。 应该没有错误。 点击 列表未连接 。 应该没有未连接的项目。 单击 确定 关闭 DRC控制 对话框。
20. 单击 文件 → 保存 保存文件。 要以 3D 方式观察您的电路板，请单击 视图 → 3D 查看器。
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/pcbnew_3d_viewer.png)
21. 你的板是完整的。 要将其发送给制造商，您需要生成所有 Gerber 文件。

### 生成 Gerber 文件

完成 PCB 后，您可以为每一层生成 Gerber 文件，并将它们发送给您最喜欢的 PCB 制造商，他们将为您制作电路板。

1. 从 KiCad，打开 Pcbnew 板编辑器。
2. 点击 **文件 → 绘制** 。 选择 Gerber 作为 绘制格式 并选择放置所有 Gerber 文件的文件夹。 单击 绘制 按钮继续。
3. 单击 `生成钻孔文件`，生成钻孔文件。 
4. 这些是您制作典型Gerber 文件的导出配置
   
![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/print.png)

>注意：KiCad 输出的 Gerber 文件当中，后缀名为.dbr的文件为Gerber文件，后缀名为.drl的为钻孔文件，后缀名.pos则代表位置文件。

**PCB层详述**

| 层  |  KiCad层名 | Gerber扩展名  |  作用  |
|---|---|---|---|
| Bottom Layer（底层）|  B.Cu（底层 | 	.GBR  |  铜箔层 |
| Top Layer（顶层）   | F.Cu （顶层） |  .GBR  | 铜箔层  |
| Top Overlay （顶层丝印层）| F.SilkS （顶层丝印层）|	.GBR  | 丝印 |
| Bottom Overlay （底层丝印层）| B.SilkS （顶层丝印层）|	.GBR  | 丝印 |
| Bottom Solder Resist（底层阻焊层）| B.Mask （底层阻焊层） |	.GBR  | 焊盘区，不盖油 |
| Top Solder Resist（顶层阻焊层）| F.Mask（顶层阻焊层） |	.GBR  | 焊盘区，不盖油 |
| Edges（边缘（板框）层） | Edge.Cuts （边缘（板框）层） |	.GBR  |  定义板子形状 |
|  | .drl文件 钻孔层 |  | 非金属和金属 |
|  | .Paste 助焊层 |  |  开钢网用  |

>边框，铜箔曾，阻焊层，丝印层，钻孔 一定要有

### 使用 Gerbview

1. 要查看所有 Gerber 文件，请转到 KiCad 项目管理器并单击 GerbView 图标。 在下拉菜单或图层管理器中，选择 图形图层1 。 单击 **文件 → 打开 Gerber 文件** 或单击图标 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/gerber_file.png) 。 选择并打开所有生成的 Gerber 文件。 注意它们如何一个显示在另一个之上。
2. 使用 **文件 → 打开 Excellon 钻孔文件** 打开钻孔文件。
3. 使用右侧的图层管理器选择/取消选择要显示的图层。 在发送生产之前仔细检查每一层。
4. 该视图与 Pcbnew 类似。 在视图内右键单击并单击 网格 以更改网格。

### 使用 FreeRouter 自动布线

手动布线板既快速又有趣，但对于具有大量元件的电路板，您可能希望使用自动布线器。 请记住，您应首先手动路由关键迹线，然后设置自动布线器以执行无聊位。 它的工作只会解释未布线的痕迹。 我们将在这里使用的自动布线器是 FreeRouting。

1. 从 *Pcbnew* 单击 **文件 → 导出 → Specctra DSN** 并在本地保存文件。 启动 FreeRouter 并单击 *打开您自己的设计* 按钮，浏览 *dsn* 文件并加载它。
2. FreeRouter 具有 KiCad 目前不具备的一些功能，包括手动路由和自动路由。 FreeRouter 主要有两个步骤：首先，对电路板进行布线，然后对其进行优化。 完全优化可能需要很长时间，但您可以随时停止它。
3. 您可以通过单击顶部栏上的 *自动布线器* 按钮来启动自动路由。 底栏为您提供有关正在进行的路由过程的信息。 如果 *通过* 计数超过30，则您的电路板可能无法使用此路由器自动执行。 更多地展开您的元件或更好地旋转它们并再试一次。 零件的旋转和位置的目标是降低速率的交叉走线的数量。
4. 左键单击鼠标可以停止自动路由并自动启动优化过程。 另一次左键单击将停止优化过程。 除非你真的需要停下来，否则最好让 FreeRouter 完成它的工作。
5. 单击 **文件 → 导出 Specctra 会话文件** 菜单并使用 .ses 扩展名保存板文件。 您实际上不需要保存 FreeRouter 规则文件。
6. 回到 *Pcbnew*。 您可以通过单击 **文件 → 导入 → Spectra 会话** 并选择 .ses 文件来导入刚刚布线的电路板。

如果有任何您不喜欢的布线，可以使用 \[Delete] 和路由工具删除它并重新路由它，这是 *布线* 图标 ![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/add_tracks.png)在右侧工具栏上添加。


## 制作原理图符号

有时，您想要放置在原理图中的符号不在 KiCad 库中。 这很正常，没有理由担心。 在本节中，我们将了解如何使用 KiCad 快速创建新的原理图符号。 不过，请记住，您始终可以在互联网上找到 KiCad 元件。

在 KiCad 中，符号是一段以 DEF 开头并以 ENDDEF 结尾的文本。 一个或多个符号通常放在库文件中，扩展名为 .lib。 如果要将符号添加到库文件，只需使用文本编辑器的剪切和粘贴命令即可。

## 制作元件封装

## 关于KiCad项目文件的可移植性的注意事项

您需要将哪些文件发送给某人才能完全加载和使用您的KiCad项目？  

当你有一个KiCad项目与某人分享时，重要的是原理图文件.sch，板文件.kicad_pcb，项目文件.pro和网表文件.net，与两个原理图一起发送库文件.lib和封装文件.kicad_mod。只有这样，人们才能完全自由地修改原理图和电路板。

使用KiCad原理图，人们需要包含符号的.lib文件。需要在Eeschema首选项中加载这些库文件。另一方面，使用板（.kicad_pcb文件），封装可以存储在.kicad_pcb文件中。您可以向某人发送.kicad_pcb文件，而不是其他任何内容，他们仍然可以查看和编辑该板。但是，当他们想要从网表加载元件时，脚本库（.kicad_mod文件）需要存在并加载到Pcbnew首选项中，就像原理图一样。此外，有必要在Pcbnew的首选项中加载.kicad_mod文件，以便在Cvpcb中显示这些封装。

如果有人向您发送了一个带有封装的.kicad_pcb文件，您可以在另一个板上使用，您可以打开封装编辑器，从当前板上加载封装，并将其保存或导出到另一个封装库中。您也可以通过Pcbnew→文件→压缩→封装-创建封装压缩，这将创建一个新的一次导出.kicad_pcb文件中的所有封装。.kicad_mod文件包含所有板的封装。

最重要的是，如果PCB是你想要分发的唯一东西，那么电路板文件.kicad_pcb就足够了。但是，如果您想让人们完全能够使用和修改您的原理图，其元件和PCB，强烈建议您压缩并发送以下项目目录：
```
教程1/
|--教程1.pro
|--教程1.sch
|--教程1.kicad_pcb
|--教程1.net
|--library/
|    |--myLib.lib
|    |--myOwnLib.lib
|    \--myQuickLib.lib
|
|--myfootprint.pretty/
|    \--MYCONN3.kicad_mod
|
\--gerber/
|-- ...
|-- ...
```

## 更多文档信息


## 参考连接


不要在符号编辑和封装编辑中添加中文，否则在使用  Library Loader 时会出错，导致无法加载库。

当你使用Library Loader 下载新元件库时，软件会提醒如下

![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/warring.png)

当你打开软件，加载符号编辑时，则会提醒

![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/error.png)

此时你的 SamacSys_Parts.lib 库是无法打开的，没有任何符号，但其实是有的  

原因是使用Library Loader 下载新元件时，其会自动修改 lib 库文件，将新原件添加进去，完成全自动操作。但如果存在中文，则可能会使中文乱码，导致数据错误。如下图，红色是原来的数据，绿色则是修改后的乱码数据。

![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/reson.png)

所以当出现以上错误时，我们可以使用文档编辑软件（推荐 VS code）打开 SamacSys_Parts.lib 库文件，找到乱码处（错误弹窗中会提示错误行号，这里是 327），手动修改回来。这样就能正常加载了，再在符号编辑处将中文删除，避免下次再错。

另外可能 SamacSys_Parts.dcm 文件也会乱码，这里我没复现成功，原因和上面类似，手动修改即可。

### 阵列

选择阵列时，特征如果是依附与某个实体（圆角，凹坑基于立方体），则应先阵列该实体（立方体），否则特征就无法阵列（无实体可依附，即悬空，软件判断为错误，无法阵列）

有些圆角无法阵列，就很离谱。好像只有拉伸和切割可以阵列


### 与KiCAD


#### kicadStepUpMod

可以导入Kicad的PCB模型，并且还可以导入布线层和丝印层，使得模型更加逼真。

[Github地址]( https://github.com/easyw/kicadStepUpMod)
[使用文档kicadStepUp-starter-Guide](https://github.com/easyw/kicadStepUpMod/blob/master/demo/kicadStepUp-starter-Guide.pdf)

1. 配置KiCAD元件模型库路径

不配置的化，导如模型是，会提示找不到wrl模型文件（默认支持step文件），模型就会缺少3d模型，这里配置路径，就是让插件再导入时将wrl文件自动转为step文件，

2. 导入dxf

https://www.youtube.com/watch?v=b3NoAOxOGxA&feature=emb_rel_end

https://forum.kicad.info/t/kicad-to-dxf-or-dwg/7994

https://github.com/easyw/kicadStepUpMod

https://www.youtube.com/watch?v=KSeicvr6Rog

https://www.youtube.com/watch?v=6R6UEUScjgA

https://www.youtube.com/watch?v=n04M6nFvdxs
## 参考链接

1. [KiCad官方文档](https://docs.kicad-pcb.org/5.1.6/zh/)
2. [开源 EDA 工具 KiCad 5.1.5 电路设计手册](https://uinika.github.io/Electronics/KiCad.html)
3. [KiCad基本使用](https://an_kang.gitee.io/2020/ck93pvj620008u0rhh0qaa55i/)  
4. [KiCad-CN](https://gitee.com/KiCAD-CN)  
5. [开源 KiCad EDA 中文资料收集整理 (2020-05-15)](https://www.cnblogs.com/F4NNIU/p/KiCad.html)6
6. [Setting up Library Loader for use with KiCad](https://www.samacsys.com/kicad/)  

## PCB生产注意事项

### 没有封装被放置

在 KICAD 导出制造输出 - 封装位置文件，会提示：没有封装被放置（No footprint for automated placement）  
 
原因：KICAD默认不会给 通孔 元件输出位置文件，所以如果你的板子恰好全是 通孔 元件，就会出现这样的错误。

解决：双击通孔元器件，将其 **制造属性** 手动修改为 **表面贴装**。

![enter description here]( https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/KICAD学习笔记/制造属性.png)

**参考链接**    
[1. KiCad: “No footprint for automated placement”](https://electronics.stackexchange.com/questions/456671/kicad-no-footprint-for-automated-placement)




### KiCad 插件
https://my.oschina.net/taotieren/blog/3233534


## 辅助工具

https://componentsearchengine.com/   模型库下载

# KICAD-嘉立创SMT生产经验
原文：
https://notes.glab.online/#/posts/4

以下为全文转载，留作备份

要想圆满的完成贴装，从设计PCB文件之前就要注意以下几点：

1、原理图中原理图符号要正确的关联封装。也就是要填写正确的封装、还有元件的值。如封装：0805，值：10KΩ。

<!--more-->

2、关联封装的时候为了确保最后贴装正确，要确保你的封装是正确的。但是网上各种封装画得五花八门的，我就曾经被不标准的封装害的翻车，所以最好的是用嘉立创自己提供的封装库。同时一些带极性的元件，比如贴片LED灯，贴片开关二极管，也要确保正确的封装0度方向，所以为了确保万无一失，从设计之初就选择嘉立创的封装吧。点击下载：嘉立创Kicad封装库

3、要能实现SMT贴装，你还需要通过Kicad导出gerber文件、BOM清单、元件位置文件。其中gerber文件导出可以放心的提交生产。而Kicad原生导出的BOM清单和元件位置文件不符合嘉立创的格式标准。这个可以用KiCAD生产文件生成器，直接生成符合嘉立创格式标准的BOM清单和元件位置文件。

还需要注意的是，这里生成的BOM清单是否准确，依赖你最初画原理图时填写的信息是否准确，如果你填写本身不准确，这时候的BOM清单就是错误的。

总体来说，只要注意以上三点，你就能很方便的实现在嘉立创完成SMT贴装。也不必有太多顾虑，整个SMT下单过程中，会让你确定贴装的元件，贴装元件的位置，贴装元件的极性，最终还会给你一张贴装工艺图，这样多次检查下来，基本能避免错误导致翻车。第一次后，你就会非常熟悉流程，节约你大量的手焊时间了。

PCB设计中检查要点：

1、原理图的检查。执行电气规则检查可以检查出一些错误，但是一些错误必须人工确认，比如：针脚是否有错，因为针脚和封装的焊盘对应，如果出错，电气规则检查是查不出来的， 只能人工确认。特别是采用了未确认过的的原理图符号，最好反复确认是否有错。

2、PCB文件的检查。DRC检查可以检查出连接问题，连线重合等等，但是一些错误也必须人工确认，比如：PCB边框是否封闭；元件的丝印是否被遮挡；覆铜是否正确（覆铜的网络设定是否准确）；贴片元件是否会遮挡住插件元件（最好加上3D模型确认）；元件移动后是否相应的设置作了移动，如PCB天线的禁布区在PCB天线移动后随之移动；有极性元件的方向是否正确；焊盘的网络名是否有错；新的封装是否验证过。
