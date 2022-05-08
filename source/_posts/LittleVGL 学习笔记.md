---
layout: post
title: "LittleVGL 学习笔记"
index_img: https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/logo3_lvgl.png
date: 2020-11-05
updated: 2020-11-05
hide: false
# sticky: 100 #置顶，数字越大越靠前
# banner_img: #/img/post_banner.jpg
# comment: false
categories: 01-专业
---

**版本：** v7.7.2-dev  
**平台：** windows 10   
**模拟平台：** CodeBlocks on win10  

<!--more-->

# 移植

## Tick 接口

LittlevGL 需要一个系统嘀嗒 (tick) 来了解动画和其他任务的运行时间。

您需要定期调用 ` lv_tick_inc(tick_period)` 函数，并以毫秒为单位告知调用周期。例如，` lv_tick_inc(1)` 是每毫秒调用一次。

`lv_tick_inc `应该在一个比 lv_task_handler() 更高优先级的例程中调用（例如在一个中断中），以精确地知道经过的毫秒数，即使 `lv_task_handler`的执行需要更长的时间。

在 FreeRTOS 中 ‘ lv_tick_inc ‘ 可以在 ‘ vApplicationTickHook ‘ 中调用。

基于 Linux 的操作系统 (例如在Raspberry Pi上) 可以在线程中调用 ‘ lv_tick_inc ‘，如下所示:
```
void * tick_thread (void *args)
{
      while(1) {
        usleep(5*1000);   /*Sleep for 5 millisecond*/
        lv_tick_inc(5);      /*Tell LittlevGL that 5 milliseconds were elapsed*/
    }
}
```

### API

以1毫秒的分辨率提供对系统刻度的访问

#### 函数

**uint32_t lv_tick_get(void)**
- 获取自启动以来经过的毫秒数
- **return**
	- 经过的毫秒数


**uint32_t lv_tick_elaps(uint32_t prev_tick)**
-获取自上一个时间戳以来经过的毫秒数
- **return**
	- 自“ prev_tick”以来经过的毫秒数
- **Parameters**
	  - prev_tick：上一个时间戳（systick_get（）的返回值）

# 概述

## Object

ittleVGL是以对象（Object）为概念的，在LVGL中，用户界面的**基本构建块**是对象（Object），也称为控件。例如，按钮，标签，图像，列表，图表或文本区域。‘

在[对象类型](https://docs.lvgl.io/v6/zh-CN/html/object-types/index.html)这里查看所有对象类型。

### Attributes

#### 基础属性

所有的控件对象都具有一些共同的属性,如下所示

 - 位置(Position)
 - 大小(Size)
 - 父类(Parent)
 - 是否可拖拽(Drag enable)
 - 是否可点击(Click enable)等等

你可以通过 `lv_obj_set_...`和 `lv_obj_get_...`这样的API接口对来读写这些属，就比如下面这样：
```
/*设置对象基本属性*/ 
lv_obj_set_size(btn1, 100, 50); //按钮大小
lv_obj_set_pos(btn1, 20,30); //按钮位置
```
要查看所有可用函数请访问基本对象的[文档](https://docs.lvgl.io/v6/zh-CN/html/object-types/obj.html).

#### 专属属性

对象类型也有本身的专有属性，比如，一个滑动条有：

 - 最小最大值(Min. max. values)
 - 当前值(Current value)
 - 自定义样式(Custom styles)
  
对于这些属性，每个对象类型都有唯一的API函数。例如一个滑块：
```
/*设置滑动条专有属性*/
lv_slider_set_range(slider1, 0, 100);	   /*设置最大最小值*/
lv_slider_set_value(slider1, 40, LV_ANIM_ON);	/*设置当前值（位置）*/
lv_slider_set_action(slider1, my_action);     /*设置回调函数*/
```
这些对象类型的API已经描述在他们的 [文档](https://docs.lvgl.io/latest/en/html/widgets/index.html) 中，但是你也可以在他们各自的头文件中核查 (例如 lv_objx/lv_slider.h)

### 对象工作机制

#### 父子结构

父对象可以被看作是其子对象的容器，每个对象只有一个父对象（screen对象除外）,父对象可以有无限数量的子对象。同时父对象的类型是没有限制，但是有一些典型的父对象（例如按钮）和典型的子对象（例如标签）。

父对象和子对象之间具有如下2点特性:

#### 一起移动(Moving together)

如果父对象的位置更改,则子对象将随父对象一起移动,因此子对象的坐标位置是以父对象的左上角而言的,而不是以屏幕的左上角

(0;0) 坐标表示对象的位置会保持在他们各自父对象的左上角

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/1606962374407.png)
```
lv_obj_t * par = lv_obj_create(lv_scr_act(), NULL);  /*在当前屏幕上创建一个父对象*/
lv_obj_set_size(par, 100, 80);	               /*设置父对象大小*/

lv_obj_t * obj1 = lv_obj_create(par, NULL);	       /*在前面创建的父对象上创建一个子对象*/
lv_obj_set_pos(obj1, 10, 10);	                 /*为新的子对象设置位置*/
```
Modify the position of the parent:

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/1606962536983.png)
```
lv_obj_set_pos(par, 50, 50);	/*移动父对象，子对象也会随之移动*/
```
(为简单演示，例子中没有给出调整对象颜色的代码)

#### 子对象只可显示在父对象上

如果子对象的一部分在父对象的外面,那么子对象的这一部分将不会被显示出来
![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/1606962601758.png)
```
lv_obj_set_x(obj1, -30);	/*在父对象移动子对象的位置，-30 已经超出了父对象的大小范围*/
```

#### 创建-删除对象

在littleVGL中,对象可以在运行时被创建和删除，这意味着仅当前创建的对象消耗RAM。例如，如果需要图表（chart），则可以在需要时创建它，并在不可见或不必要时将其删除。


每一种对象都有其专属的 **create** 创建函数，他需要2个参数
- 指向父对象的指针。要创建屏幕（screen），请以NULL作为父级。
- （可选）用于复制具有相同类型的对象的指针。该复制对象可以为NULL，以避免复制操作。

所有对象使用 `lv_obj_t` 指针（C语言）作为句柄进行引用。以后可以使用该指针设置或获取对象的属性。
 
创建函数看起来如下面这样:
```
lv_obj_t * lv_ <type>_create(lv_obj_t * parent, lv_obj_t * copy);
```
所有对象类型都有一个通用的**删除**函数。它删除对象及其所有子对象。
```
void lv_obj_del(lv_obj_t * obj);
```
lv_obj_del will delete the object immediately. If for any reason you can't delete the object immediately you can use lv_obj_del_async(obj). It is useful e.g. if you want to delete the parent of an object in the child's LV_EVENT_DELETE signal.

`lv_obj_del`会立即删除对象。如果你由于某种原因不想立即删除的话，你可以使用`lv_obj_del_async(obj)`接口来异步删除，删除动作会在下一个`lv_task_handler` 调用时被执行。这很有用，例如，如果要在子对象的LV_EVENT_DELETE信号删除对象的父对象。

另外你可以通过 `lv_obj_clean(obj)`接口来清除某个obj对象下的所有子对象（但不能删除对象本身）：
```
void lv_obj_clean(lv_obj_t * obj);
```
### Screens

确保不要混淆显示器和屏幕：

 - 显示（Displays）是绘制像素的物理硬件。
 - 屏幕（Screens ）是与特定显示器关联的高级根对象。一个显示器可以有多个与之关联的屏幕，但反之亦然。

#### 创建 Screens

屏幕对象是一个特殊的对象，因为他自己没有父对象，所以它以这样的方式来创建：
```
lv_obj_t * scr1 = lv_obj_create(NULL, NULL);
```
屏幕对象可以被创建为任何对象类型。例如，一个 基本对象或一个用一个图片作为壁纸

#### 获取活跃的screen

默认情况下，littleVGL会为显示器创建一个lv_obj类型的基础对象来作为它的屏幕,即最顶层的父类,可以通过lv_scr_act()接口来获取当前活跃的屏幕对象,通过lv_scr_load()接口来设置一个新的活跃屏幕对象

每一次显示中始终有一个活动屏幕。默认情况下，littleVGL为每个显示创建并加载一个“基础对象(Base object)”作为屏幕。要获取当前活动的屏幕，请使用函数 `lv_scr_act()` 来获取活跃屏幕。

#### 加载screens

要加载新屏幕，如以下函数原型 `lv_scr_load(scr1)`.

#### 动画加载screens

可以使用动画加载新屏幕（类似PPT切换页面的动画,如：淡入淡出等）
```
lv_scr_load_anim(scr, transition_type, time, delay, auto_del)
```
transition_type存在以下转换类型：

 - LV_SCR_LOAD_ANIM_NONE: 延时 time 毫秒后立即切换
 - LV_SCR_LOAD_ANIM_OVER_LEFT/RIGHT/TOP/BOTTOM：将新屏幕移到给定方向上
 - LV_SCR_LOAD_ANIM_MOVE_LEFT/RIGHT/TOP/BOTTOM：将旧屏幕和新屏幕都移至给定方向
 - LV_SCR_LOAD_ANIM_FADE_ON：使新屏幕淡入覆盖屏幕

Setting `auto_del` to `true` 会在动画结束时自动删除旧屏幕.

在延迟time之后开始动画播放时，新屏幕将变为活动状态（(由 lv_scr_act（）返回创建）。

#### 处理多个显示器 displays

屏幕在当前选择的默认显示上创建。默认显示是使用 `lv_disp_drv_register`注册的最后一个显示屏，也可以使用`lv_disp_set_default(disp)`显式地选择一个新的默认显示。

`lv_scr_act()`，`lv_scr_load()`和`lv_scr_load_anim()`在默认屏幕上进行操作。

查看 [Multi-display support](https://docs.lvgl.io/latest/en/html/overview/display.html) 了解更多

### Parts

控件（ widgets）可以包含多个零部件。例如，[按钮button](https://docs.lvgl.io/latest/en/html/widgets/btn.html)仅具有主要部分，而[滑块Slider](https://docs.lvgl.io/latest/en/html/widgets/slider.html)则由背景，指示器和旋钮组成。

各零部件的名称构造类似 `LV_ + <TYPE> _PART_ <NAME>`。例如 `LV_BTN_PART_MAIN` or `LV_SLIDER_PART_KNOB`。通常在将 样式styles 添加到对象时使用零部件。可以将不同的样式分配给对象的不同零部件。

要了解有关零部件的更多信息，请阅读“[样式](https://docs.lvgl.io/latest/en/html/overview/style.html)”概述的相关部分。

### States

对象可以处于以下状态的组合

 - LV_STATE_DEFAULT：正常，已发布
 - LV_STATE_CHECKED：切换或选中 Toggled or checked
 - LV_STATE_FOCUSED：通过键盘或编码器聚焦或通过触摸板/鼠标单击Focused via keypad or encoder or clicked via touchpad/mouse
 - LV_STATE_EDITED  由编码器编辑Edit by an encoder
 - LV_STATE_HOVERED 鼠标悬停（现在不支持）Hovered by mouse (not supported now)
 - LV_STATE_PRESSED 已按下Pressed
 - LV_STATE_DISABLED 禁用或不活动Disabled or inactive

当用户按下、释放、聚焦等对象时，状态通常由库自动更改。但是，状态也可以手动更改。要完全覆盖当前状态，请使用 `lv_obj_set_state(obj, part, LV_STATE...)`。要设置或清除给定状态（但保持其他状态不变）使用 `lv_obj_add/clear_state(obj, part, LV_STATE_...)`，在两种情况下都可以使用 **或|** 状态值来组合。例如`lv_obj_set_state(obj, part, LV_STATE_PRESSED | LV_PRESSED_CHECKED)`。

要了解有关零部件的更多信息，请阅读“[样式](https://docs.lvgl.io/latest/en/html/overview/style.html)”概述的相关部分。

## Layers 层

### 创建顺序

默认情况下，LVGL在背景上绘制旧对象，在前景上绘制新对象。

例如，假设我们向一个名为button1的父对象添加了一个按钮，然后向另一个名为button2的按钮添加了一个按钮。然后button1（及其子对象）将在背景中，并且可以被button2及其子对象覆盖。
![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/1606973910550.png)
```
/*创建一个屏幕*/
lv_obj_t*scr=lv_obj_create(NULL,NULL);
lv_scr_load(scr);   /*加载屏幕*/

/*创建两个按钮*/
lv_obj_t*btn1=lv_btn_create(scr,NULL);/*创建一个按钮1在屏幕中*/
lv_btn_set_fit(btn1,true,true);/*使能自动设置大小根据内容*/
lv_obj_set_pos(btn1,60,40);/*设置该按钮的位置*/

lv_obj_t*btn2=lv_btn_create(scr,btn1);/*复制第一个按钮*/
lv_obj_set_pos(btn2,180,80);/*设置该按钮的位置*/

/*添加标签到按钮中*/
lv_obj_t*label1=lv_label_create(btn1,NULL);/*创建一个标签于按钮1*/
lv_label_set_text(label1,"Button1");/*设置标签内容*/
lv_obj_t*label2=lv_label_create(btn2,NULL);/*创建标签于按钮2*/
lv_label_set_text(label2,"Button2");/*设置标签的内容*/

/*删除第二个按钮标签*/
lv_obj_del(label2);
```

### 成为前景

有几种方法可以将对象置于前景：Cai Xuefeng 

 - 使用 `lv_obj_set_top(obj,true)`。如果对象或它的任何子对象被点击，那么LVGL将自动将该对象带到前景。它的工作原理类似于PC上的典型GUI。当点击背景中的一个窗口时，它会自动来到前景
 - 使用 `lv_obj_move_foreground(obj)`显式地告诉库将一个对象放到前景。类似地，使用`lv_obj_move_background(obj)`移动到背景
 - 当 `lv_obj_set_parent(obj,new_parent)`被使用时，对象将在new_parent父对象的前景

### 顶层和系统层

LVGL使用两个特殊的层，分别名为 `layer_top` 和 `layer_sys`。两者在显示器的所有屏幕上都是可见的和常见的。但是，**它们不能在多个物理显示器之间共享**。`layer_top`总是位于默认屏幕的顶部(`lv_scr_act()`)，而 `layer_sys` 位于 `layer_top`的顶部（不可更改，适合做 一些弹窗）

用户可以使用 `layer_top` 创建一些随处可见的内容。例如，一个菜单栏，一个弹出窗口等等。如果单击click属性是启用的，那么 `layer_top` 将吸收所有用户的单击，并处理。
```
lv_obj_set_click(lv_layer_top(),true);
```

layer_sys也用于类似的目的在LVGL上。例如，它将鼠标光标放在那里，以确保它总是可见的

## Events 事件

当发生用户可能感兴趣的事情时，事件将在LVGL中触发。例如对象：

 - 被点击
 - 被拖动
 - 其值已被更改，等等。

用户可以将回调函数分配给对象以查看这些事件。实际上，它看起来像这样：
```
lv_obj_t * btn = lv_btn_create(lv_scr_act(), NULL);
lv_obj_set_event_cb(btn, my_event_cb);   /*指定一个事件回调函数*/

...

static void my_event_cb(lv_obj_t * obj, lv_event_t event)
{
    switch(event) {
        case LV_EVENT_PRESSED:	/*事件被按下*/
            printf("Pressed\n");
            break;

        case LV_EVENT_SHORT_CLICKED:  /*短点击*/
            printf("Short clicked\n");
            break;

        case LV_EVENT_CLICKED:  /*点击*/
            printf("Clicked\n");
            break;

        case LV_EVENT_LONG_PRESSED:
            printf("Long press\n");
            break;

        case LV_EVENT_LONG_PRESSED_REPEAT:
            printf("Long press repeat\n");
            break;

        case LV_EVENT_RELEASED:  /*释放*/
            printf("Released\n");
            break;
    }

       /*Etc.*/
}
```
注意：多个对象可以使用同一回调函数。

### Event类型

存在以下事件类型：

#### 一般事件

所有对象（例如Buttons/Labels/Sliders等）都将接收这些通用事件，而不管它们的类型如何。

##### 相关的输入设备

当用户按下/释放对象时发送这些消息。它们不仅用于鼠标指针，还可以用于键盘，编码器和按钮输入设备。访问[输入设备概述](https://docs.lvgl.io/latest/en/html/overview/indev.html)部分以了解有关它们的更多信息。

- LV_EVENT_PRESSED  该对象已被按下
- LV_EVENT_PRESSING  对象被按下（在按下时连续发送）
- LV_EVENT_PRESS_LOST  仍在按下输入设备，但不再在对象上
- LV_EVENT_SHORT_CLICKED  在 `LV_INDEV_LONG_PRESS_TIME` 时间之前释放按键。如果拖动则不调用。
- LV_EVENT_LONG_PRESSED 持续按下 `LV_INDEV_LONG_PRESS_TIME`时间。如果拖动则不调用。
- LV_EVENT_LONG_PRESSED_REPEAT   在`LV_INDEV_LONG_PRESS_TIME`事件之后每`LV_INDEV_LONG_PRESS_REP_TIME`毫秒调用一次。如果拖动则不调用。
- LV_EVENT_CLICKED  释放时调用（无论长按），如果拖动则不调用。
- LV_EVENT_RELEASED 无论什么情况下，只要对象被释放时都会被调用，即使是拖动。如果在按下后从对象外部释放时，则不会调用。在这种情况下，`LV_EVENT_PRESS_LOST` 会被触发。

##### 相关指针

这些事件仅由类似指针的输入设备（例如鼠标或触摸板）发送
- LV_EVENT_DRAG_BEGIN 开始拖动对象
- LV_EVENT_DRAG_END  拖动完成（包括拖动并甩出（拖拽））
- LV_EVENT_DRAG_THROW_BEGIN 开始拖拽（拖动后释放“动量”）（控件会因为惯性仍会移动一段距离）
 
##### 键盘和编码器

这些事件由键盘和编码器输入设备发送。在 \[overview/indev](Input devices) 了解更多。

- LV_EVENT_KEY  将按键发送到对象。通常在按下它或在长按之后重复时。可以通过以下方式检索按键`uint32_t*key=lv_event_get_data()`
- LV_EVENT_FOCUSED  对象集中在其组中
- LV_EVENT_DEFOCUSED 对象在其组中散焦
  
##### 一般事件库

发送的其他一般事件。
- LV_EVENT_DELETE 正在删除对象。释放相关的用户分配数据。

#### 特别事件

这些事件特定于特定的对象类型。
- LV_EVENT_VALUE_CHANGED  对象值已更改（例如，对于Slider）
- LV_EVENT_INSERT 某物被添加到对象中。（通常到文本区域）
- LV_EVENT_APPLY 单击“确定”，“应用”或类似的特定按钮。（通常来自键盘对象）LV_EVENT_CANCEL 单击“关闭”，“取消”或类似的特定按钮。（通常来自键盘对象）LV_EVENT_REFRESH 查询以刷新对象。永远不会由库发送，但可以由用户发送。

请访问[enter description here](https://docs.lvgl.io/latest/en/html/widgets/index.html)的文档，以了解对象类型使用哪些事件。

### 自定义的数据

一些事件可能包含自定义数据。例如，`LV_EVENT_VALUE_CHANGED` 在某些情况下会告诉新值。有关更多信息，请访问[enter description here](https://docs.lvgl.io/latest/en/html/widgets/index.html)的文档。要在事件回调中获取自定义数据，请使用 `lv_event_get_data()`。

自定义数据的类型取决于发送对象，但如果是
- single number （数据）则是 uint32_t* 或 int32_t*
- 文字则是char* 或 constchar*

### 手动发送事件

#### 任意事件

要将事件手动发送到对象，请使用 `lv_event_send(obj,LV_EVENT_...,&custom_data)`

例如，它可以通过模拟按钮按下来手动关闭消息框（尽管有更简单的方法）：
```
/*Simulatethepressofthefirstbutton(indexesstartfromzero)*/
uint32_tbtn_id=0;
lv_event_send(mbox,LV_EVENT_VALUE_CHANGED,&btn_id);
```
#### 刷新事件

`LV_EVENT_REFRESH` 是一个特殊事件，因为它旨在供用户用来通知对象刷新自身。一些例子：
- 通知标签根据一个或多个变量（例如当前时间）刷新其文本
- 语言更改时刷新标签
- 如果满足某些条件，则启用按钮（例如，输入正确的PIN）
- 如果超出限制，则将样式添加到对象/从对象删除样式，等等。
  
处理类似情况的最简单方法是使用以下函数。

`lv_event_send_refresh(obj)`只是 `lv_event_send(obj,LV_EVENT_REFRESH,NULL)`一个包装。因此，它只向对象发送一个 `LV_EVENT_REFRESH`.

`lv_event_send_refresh_recursive(obj)`向一个对象及其所有子对象发送`LV_EVENT_REFRESH`事件。如果将`NULL`作为参数传递，则所有显示的所有对象都将刷新。

## LVGL的样式

样式用于设置对象的外观。lvgl中的样式受CSS启发很大。简而言之，概念如下：
- 样式是一个 `lv_style_t`变量， 可以保存属性，例如边框宽度，文本颜色等。与classCSS类似。
- 并非必须指定所有属性。未指定的属性将使用默认值。
- 可以将样式分配给对象以更改其外观。
- 样式可以被任意数量的对象使用。
- 样式可以级联，这意味着可以将多个样式分配给一个对象（一个对象由多个零部件，会用到不同的样式），并且每种样式可以具有不同的属性。例如，`style_btn` 可能会是导致默认的灰色按钮，但`style_btn_red` 只需添加一个 `background-color=red` 就可以覆盖背景色。
- 以后添加的样式具有更高的优先级。这意味着，如果以两种样式指定属性，则将使用后面添加的样式。
- 如果未在对象中指定，则某些属性（例如，文本颜色）可以从父级继承。
- 对象可以具有比“普通”样式更高优先级的局部样式local styles。
- 与CSS（伪类描述不同的状态，例如:hover）不同，在lvgl中，将属性分配给给定的状态。（即“类”与状态无关，但是每个属性都有一个状态）
- 当对象更改状态时可以应用过渡Transitions 。


### States

对象可以处于以下状态：
- LV_STATE_DEFAULT（0x00）：正常，已释放
- LV_STATE_CHECKED（0x01）：切换或选中
- LV_STATE_FOCUSED（0x02）：通过键盘或编码器聚焦或通过触摸板/鼠标单击LV_STATE_EDITED（0x04）：由编码器编辑
- LV_STATE_HOVERED（0x08）：鼠标悬停（现在不支持）
- LV_STATE_PRESSED（0x10）：已按下
- LV_STATE_DISABLED（0x20）：禁用或不禁用，
- 
状态的组合也是可能的。`LV_STATE_FOCUSED|LV_STATE_PRESSED`

可以在每种状态和状态组合中定义样式属性。例如，为默认和按下状态设置不同的背景颜色。如果未在状态中定义属性，则将使用最佳匹配状态的属性。通常，它表示带有 `LV_STATE_DEFAULT` 状态的属性。 ̨如果即使对于默认状态也未设置该属性，则将使用默认值。（请参阅稍后）

但是“最佳匹配状态的属性”到底意味着什么？States的值代表着显示的的优先级（请参见上面的列表）。值越高，优先级越高。为了确定要使用哪个状态state的属性，我们举一个例子。让我们来看看背景色是这样定义的：

- LV_STATE_DEFAULT：白色
- LV_STATE_PRESSED：灰色
- LV_STATE_FOCUSED：红色

1. 默认情况下，对象处于默认状态，因此很简单：该属性在对象的当前状态中完美定义为白色
2. 按下对象时，有2个相关属性：默认为白色（默认与每个状态有关）和按下为灰色。按下状态的优先级为0x10，高于默认状态的0x00优先级，因此将使用灰色。
3. 当物体聚焦时，会发生与按下状态相同的事情，并且将使用红色。（焦点状态的优先级高于默认状态）。
4. 聚焦并按下对象时，灰色和红色都可以使用，但是按下状态的优先级高于聚焦，因此将使用灰色。
5. 可以为设置例如玫瑰色通过组合 `LV_STATE_PRESSED | LV_STATE_FOCUSED`。在这种情况下，此组合状态的优先级为0x02+0x10=0x12，该优先级高于按下状态的优先级，因此将使用玫瑰色。
6. 选中对象后，没有属性可以设置此状态的背景色。因此，在缺少更好的选择的情况下，对象在默认状态的属性中仍为白色。

一些实用说明：

- 如果要为所有状态设置属性（例如红色背景色），只需将其设置为默认状态即可。如果对象找不到其当前状态的属性，它将回退到默认状态的属性。
- 使用 `或|` 来描述复杂情况的属性。（例如，按+选中+集中）
- 对不同的状态使用不同的样式元素可能是一个好主意。例如，很难找到 释放，按下，选中+按下，聚焦，聚焦+按下，聚焦+按下+选中等状态的背景颜色。相反，例如，将背景色用于按下和选中状态，并使用不同的边框颜色指示聚焦状态

### 级联样式

不需要将所有属性设置为一种样式。可以向对象添加更多样式，然后让后来添加的样式修改或扩展其他样式的属性。例如，创建常规的灰色按钮样式，并为仅设置新的背景色的红色按钮创建新的样式。

当在CSS中列出所有使用的类时，这是相同的概念。`<divclass=".btn.btn-red">`

较晚添加的样式优先于较早的样式。因此，在上面的灰色/红色按钮示例中，应首先添加常规按钮样式，然后再添加红色样式。但是，仍然考虑来自状态State的优先级。因此，让我们研究以下情况：
- 基本的按钮样式定义了默认状态为深灰色和按下状态为浅灰色
- 红色按钮样式仅在默认状态下将背景色定义为红色
- 
在这种情况下，释放按钮时（它处于默认状态）它将是红色的，因为在最后添加的样式（红色样式）中找到了完美的匹配。按下按钮时，浅灰色是更好的搭配，因为它完美地描述了当前状态，因此按钮将是浅灰色的。

### 继承

某些属性（通常与文本相关）可以从父对象的样式继承。仅当未以对象的样式（即使在默认状态下）设置给定属性时，才应用继承。在这种情况下，如果该属性是可继承的，则该属性的值也将在父级中搜索，直到一部分可以告诉该属性的值为止。父母会用自己的状态来说明价值。按下按钮后，文字颜色就从这里来，将使用按下的文字颜色。

###  小部件

对象可以具有可以具有自己样式的小部件。例如，页面包含四个部分：
- 背景
- 可卷动
- 滚动条
- 边缘闪光灯
- 
有三种类型的对象部分的**主体，虚拟和现实**。

主要零部件通常是对象的背景和最大的部分。某些对象只有一个主要部分。例如，一个按钮只有一个背景。

虚拟零部件是实时绘制到主体零部件的其他零部件。它们后面没有“真实”对象。例如，页面的滚动条不是真实的对象，仅在绘制页面背景时才绘制。虚拟零部件始终具有与主要零部件相同的状态。如果可以继承该属性，则在转到父级之前还将考虑主体部分。

真实零部件是由主对象创建和管理的真实对象。例如，页面的可滚动部分是真实对象。实际零部件的状态可能与主要零部件的状态不同。要查看对象具有哪些部分，请访问其文档页面

### 初始化样式并设置/获取属性

样式存储在 `lv_style_t`变量中。样式变量应为`static`，全局或动态分配。换句话说，它们不能是函数中的局部变量，在函数存在时销毁。在使用样式之前，应使用进行初始化`lv_style_init(&my_style)`。初始化后，可以设置样式属性。属性集函数格式如下所示：`lv_style_set_<property_name>(&style,<state>,<value>);`。例如，上面提到的示例如下所示：

```
staticlv_style_tstyle1;
lv_style_set_bg_color(&style1,LV_STATE_DEFAULT,LV_COLOR_WHITE);
lv_style_set_bg_color(&style1,LV_STATE_PRESSED,LV_COLOR_GRAY);
lv_style_set_bg_color(&style1,LV_STATE_FOCUSED,LV_COLOR_RED);
lv_style_set_bg_color(&style1,LV_STATE_FOCUSED|LV_STATE_PRESSED,lv_color_hex(0xf88));
```

可以使用`lv_style_copy(&style_destination,&style_source)`复制样式。复制后，仍然可以自由添加属性。

要删除属性，请使用：
```
lv_style_remove_prop(&style,LV_STYLE_BG_COLOR|(LV_STATE_PRESSED<<LV_STYLE_STATE_POS));
```
要从给定状态的样式中获取值，可以使用以下原型的功能：`_lv_style_get_color/int/opa/ptr(&style, <prop>, <result buf>);.`。将选择最匹配的属性，并返回其优先级。如果找不到该属性，则将返回-1

函数（...color/int/opa/ptr）的形式应根据的类型使用\<prop\>。例如：
```
lv_color_t color;
int16_t res;
res=_lv_style_get_color(&style1,LV_STYLE_BG_COLOR|(LV_STATE_PRESSED<<LV_STYLE_STATE_POS),&color);
if(res>=0){
	//the bg_color is loaded into `color`
}
```

要重置样式（释放所有数据），请使用Cai Xuefeng 
```
lv_style_reset(&style);
```
### 管理样式列表

样式本身没有那么有用。应该将其分配给对象才能生效。对象的每个部分都存储一个样式列表，该列表是已分配样式的列表。

要将样式添加到对象，请使用`lv_obj_add_style(obj,<part>,&style)`，例如：
```
lv_obj_add_style(btn,LV_BTN_PART_MAIN,&btn); /*Default button style*/
lv_obj_add_style(btn,LV_BTN_PART_MAIN,&btn_red);/*Overwrite only a some colors to red*/
```

可以使用以下方式重置对象样式列表`lv_obj_reset_style_list(obj,<part>)`

如果已经分配给对象的样式发生更改（即，其属性之一设置为新值），则应通过以下方式通知使用该样式的对象：`lv_obj_refresh_style(obj)`

要获得属性的最终值，包括级联，继承，局部样式和过渡（请参见下文），可以使用以下类似的get函数：`lv_obj_get_style_<property_name>(obj, <part>)`。这些函数使用对象的当前状态，如果没有更好的候选者，则返回默认值。例如：
```
lv_color_tcolor=lv_obj_get_style_bg_color(btn,LV_BTN_PART_MAIN);
```

### 本地风格
在对象的样式列表中，也可以存储所谓的局部属性。与CSS的概念相同。局部样式与普通样式相同，但是它仅属于给定的对象，不能与其他对象共享。要设置本地属性，请使用`lv_obj_set_style_local_<property_name>(obj, <part>, <state>, <value>); `例如以下：
```
lv_obj_set_style_local_bg_color(btn,LV_BTN_PART_MAIN,LV_STATE_DEFAULT,LV_COLOR_RED);`
```
### 转换

默认情况下，当对象更改状态（例如，按下状态）时，会立即设置新状态下的新属性。但是，通过过渡，可以在状态改变时播放动画。例如，在按下按钮后，可以在300毫秒内将其背景色设置为所按下的颜色。

过渡的参数存储在样式中。可以设置
- 过渡时间
- 开始过渡之前的延迟
- 动画path（也称为计时功能）
- 要设置动画的属性

可以为每个状态定义过渡属性。例如，将500ms过渡时间设置为默认状态将意味着当对象进入默认状态时，将应用500ms过渡时间。在按下状态下设置100ms过渡时间将意味着在进入按下状态时100ms过渡时间。因此，此示例配置将导致快速进入工作状态而缓慢回到默认状态。


### 属性

样式中可以使用以下属性。

#### 混合属性
- radius（lv_style_int_t）：设置背景的半径。0：无半径，LV_RADIUS_CIRCLE：最大半径。默认值：0。
- clip_corner（bool）：true:可以将溢出的内容剪切到圆角（半径>0）上。默认值：false。
- size（lv_style_int_t）：小部件内部元素的大小。是否使用此属性，请参见窗口小部件的文档。默认值：LV_DPI / 20。
- transform_width（lv_style_int_t）：使用此值使对象在两侧更宽。默认值：0
- transform_height（lv_style_int_t）使用此值使对象在两侧都较高。默认值：0
- transform_angle（lv_style_int_t）：旋转类似图像的对象。它的uinit为0.1度，对于45度使用450。默认值：0。
- transform_zoom（lv_style_int_t）缩放类似图像的对象。LV_IMG_ZOOM_NONE正常大小为256（或），一半为128，一半为512，等等。默认值：LV_IMG_ZOOM_NONE
- opa_scale（lv_style_int_t）：继承。按此比例缩小对象的所有不透明度值。由于继承了子对象，因此也会受到影响。默认值：LV_OPA_COVER。

#### 填充和边距属性

填充可在边缘的内侧设置空间。意思是“我不要我的孩子们离我的身体太近，所以要保留这个空间”。  
填充内部设置了孩子之间的“差距”。边距在边缘的外侧设置空间。意思是“我想要我周围的空间”。

如果启用了布局或自动调整，则这些属性通常由Container对象使用。但是，其他小部件也使用它们来设置间距。有关详细信息，请参见小部件的文档。
- pad_top（lv_style_int_t）：在顶部设置填充。默认值：0。
- pad_bottom（lv_style_int_t）：在底部设置填充。默认值：0。
- pad_left（lv_style_int_t）：在左侧设置填充。默认值：0。
- pad_right（lv_style_int_t）：在右侧设置填充。默认值：0。
- pad_inner（lv_style_int_t）：设置子对象之间对象内部的填充。默认值：0。
- margin_top（lv_style_int_t）：在顶部设置边距。默认值：0。
- margin_bottom（lv_style_int_t）：在底部设置边距。默认值：0。
- margin_left（lv_style_int_t）：在左边设置边距。默认值：0。
- margin_right（lv_style_int_t）：在右边设置边距。默认值：0。
- 
#### 背景属性

背景是一个可以具有渐变和radius舍入的简单矩形。
- bg_color（lv_color_t）指定背景的颜色。默认值：LV_COLOR_WHITE。
- bg_opa（lv_opa_t）指定背景的不透明度。默认值：LV_OPA_TRANSP。
- bg_grad_color（lv_color_t）指定背景渐变的颜色。右侧或底部的颜色是 ` bg_grad_dir != LV_GRAD_DIR_NON`。默认值：LV_COLOR_WHITE。
- bg_main_stop（uint8_t）：指定渐变应从何处开始。0：最左/最上位置，255：最右/最下位置。默认值：0。
- bg_grad_stop（uint8_t）：指定渐变应在何处停止。0：最左/最上位置，255：最右/最下位置。预设值：255。
- bg_grad_dir（lv_grad_dir_t）指定渐变的方向。可以LV_GRAD_DIR_NONE/HOR/VER。默认值：LV_GRAD_DIR_NONE。
- bg_blend_mode（lv_blend_mode_t）：将混合模式设置为背景。可以LV_BLEND_MODE_NORMAL/ADDITIVE/SUBTRACTIVE）。默认值：LV_BLEND_MODE_NORMAL。
- 
![](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/1606979430122.png)

```
#include "../../lv_examples.h"

/**
 * Using the background style properties
 */
void lv_ex_style_1(void)
{
    static lv_style_t style;
    lv_style_init(&style);
    lv_style_set_radius(&style, LV_STATE_DEFAULT, 5);

    /*Make a gradient*/
    lv_style_set_bg_opa(&style, LV_STATE_DEFAULT, LV_OPA_COVER);
    lv_style_set_bg_color(&style, LV_STATE_DEFAULT, LV_COLOR_SILVER);
    lv_style_set_bg_grad_color(&style, LV_STATE_DEFAULT, LV_COLOR_BLUE);
    lv_style_set_bg_grad_dir(&style, LV_STATE_DEFAULT, LV_GRAD_DIR_VER);

    /*Shift the gradient to the bottom*/
    lv_style_set_bg_main_stop(&style, LV_STATE_DEFAULT, 128);
    lv_style_set_bg_grad_stop(&style, LV_STATE_DEFAULT, 192);


    /*Create an object with the new style*/
    lv_obj_t * obj = lv_obj_create(lv_scr_act(), NULL);
    lv_obj_add_style(obj, LV_OBJ_PART_MAIN, &style);
    lv_obj_align(obj, NULL, LV_ALIGN_CENTER, 0, 0);
}
```

#### 边框属性

边框绘制在背景上方。它具有radius舍入。
- border_color（lv_color_t）指定边框的颜色。默认值：LV_COLOR_BLACK
- border_opa（lv_opa_t）指定边框的不透明度。默认值：LV_OPA_COVER
- border_width（lv_style_int_t）：设置边框的宽度。默认值：0。
- border_side（lv_border_side_t）指定要绘制边框的哪一侧。可以LV_BORDER_SIDE_NONE/LEFT/RIGHT/TOP/BOTTOM/FULL。ORed值也是可能的。默认值：LV_BORDER_SIDE_FULL。
- border_post（bool）：如果true在绘制完所有子级之后绘制边框。默认值：false
- border_blend_mode（lv_blend_mode_t）：设置边框的混合模式。可以LV_BLEND_MODE_NORMAL/ADDITIVE/SUBTRACTIVE）。默认值：LV_BLEND_MODE_NORMAL。

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/1606979543577.png)
```
#include "../../lv_examples.h"

/**
 * Using the border style properties
 */
void lv_ex_style_2(void)
{
    static lv_style_t style;
    lv_style_init(&style);

    /*Set a background color and a radius*/
    lv_style_set_radius(&style, LV_STATE_DEFAULT, 20);
    lv_style_set_bg_opa(&style, LV_STATE_DEFAULT, LV_OPA_COVER);
    lv_style_set_bg_color(&style, LV_STATE_DEFAULT, LV_COLOR_SILVER);

    /*Add border to the bottom+right*/
    lv_style_set_border_color(&style, LV_STATE_DEFAULT, LV_COLOR_BLUE);
    lv_style_set_border_width(&style, LV_STATE_DEFAULT, 5);
    lv_style_set_border_opa(&style, LV_STATE_DEFAULT, LV_OPA_50);
    lv_style_set_border_side(&style, LV_STATE_DEFAULT, LV_BORDER_SIDE_BOTTOM | LV_BORDER_SIDE_RIGHT);

    /*Create an object with the new style*/
    lv_obj_t * obj = lv_obj_create(lv_scr_act(), NULL);
    lv_obj_add_style(obj, LV_OBJ_PART_MAIN, &style);
    lv_obj_align(obj, NULL, LV_ALIGN_CENTER, 0, 0);
}
```

#### 轮廓属性

轮廓类似于边框，但绘制在对象外部。
- outline_color（lv_color_t）指定轮廓的颜色。默认值：LV_COLOR_BLACK
- outline_opa（lv_opa_t）指定轮廓的不透明度。默认值：LV_OPA_COVER
- outline_width（lv_style_int_t）：设置轮廓的宽度。默认值：0。
- outline_pad（lv_style_int_t）设置对象和轮廓之间的空间。默认值：0
- outline_blend_mode（lv_blend_mode_t）：设置轮廓的混合模式。可以LV_BLEND_MODE_NORMAL/ADDITIVE/SUBTRACTIVE）。默认值：LV_BLEND_MODE_NORMAL。
 
![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/1606979605995.png)
```
#include "../../lv_examples.h"

/**
 * Using the outline style properties
 */
void lv_ex_style_3(void)
{
    static lv_style_t style;
    lv_style_init(&style);

    /*Set a background color and a radius*/
    lv_style_set_radius(&style, LV_STATE_DEFAULT, 5);
    lv_style_set_bg_opa(&style, LV_STATE_DEFAULT, LV_OPA_COVER);
    lv_style_set_bg_color(&style, LV_STATE_DEFAULT, LV_COLOR_SILVER);

    /*Add outline*/
    lv_style_set_outline_width(&style, LV_STATE_DEFAULT, 2);
    lv_style_set_outline_color(&style, LV_STATE_DEFAULT, LV_COLOR_BLUE);
    lv_style_set_outline_pad(&style, LV_STATE_DEFAULT, 8);

    /*Create an object with the new style*/
    lv_obj_t * obj = lv_obj_create(lv_scr_act(), NULL);
    lv_obj_add_style(obj, LV_OBJ_PART_MAIN, &style);
    lv_obj_align(obj, NULL, LV_ALIGN_CENTER, 0, 0);
}
```

#### 阴影属性

阴影是对象下方的模糊区域。
- shadow_color（lv_color_t）指定阴影的颜色。默认值：LV_COLOR_BLACK
- shadow_opa（lv_opa_t）指定阴影的不透明度。默认值：LV_OPA_TRANSP
- shadow_width（lv_style_int_t）：设置轮廓的宽度（模糊大小）。默认值：0
- shadow_ofs_x（lv_style_int_t）：设置阴影的X偏移量。默认值：0
- shadow_ofs_y（lv_style_int_t）：设置阴影的Y偏移量。默认值：0
- shadow_spread（lv_style_int_t）：在每个方向上使阴影大于背景的值达到此值。默认值：0
- shadow_blend_mode（lv_blend_mode_t）：设置阴影的混合模式。可以LV_BLEND_MODE_NORMAL/ADDITIVE/SUBTRACTIVE）。默认值：LV_BLEND_MODE_NORMAL。
![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/1606979690907.png)
```
#include "../../lv_examples.h"

/**
 * Using the Shadow style properties
 */
void lv_ex_style_4(void)
{
    static lv_style_t style;
    lv_style_init(&style);

    /*Set a background color and a radius*/
    lv_style_set_radius(&style, LV_STATE_DEFAULT, 5);
    lv_style_set_bg_opa(&style, LV_STATE_DEFAULT, LV_OPA_COVER);
    lv_style_set_bg_color(&style, LV_STATE_DEFAULT, LV_COLOR_SILVER);

    /*Add a shadow*/
    lv_style_set_shadow_width(&style, LV_STATE_DEFAULT, 8);
    lv_style_set_shadow_color(&style, LV_STATE_DEFAULT, LV_COLOR_BLUE);
    lv_style_set_shadow_ofs_x(&style, LV_STATE_DEFAULT, 10);
    lv_style_set_shadow_ofs_y(&style, LV_STATE_DEFAULT, 20);

    /*Create an object with the new style*/
    lv_obj_t * obj = lv_obj_create(lv_scr_act(), NULL);
    lv_obj_add_style(obj, LV_OBJ_PART_MAIN, &style);
    lv_obj_align(obj, NULL, LV_ALIGN_CENTER, 0, 0);
}
```
#### 图案属性

图案是在背景中间绘制或重复以填充整个背景的图像（或符号）。
- pattern_image（）：指向变量的指针，图像文件或符号的path。默认值：。constvoid*lv_img_dsc_tNULL
- pattern_opa（lv_opa_t）：指定图案的不透明度。默认值：LV_OPA_COVER
- pattern_recolor（lv_color_t）：将此颜色混合到图案图像中。如果是符号（文本），它将是文本颜色。默认值：LV_COLOR_BLACK。
- pattern_recolor_opa（lv_opa_t）：重着色的强度。默认值：（LV_OPA_TRANSP不重新着色）
- pattern_repeat（bool）：：true图案将作为马赛克重复。false：将图案放置在背景中间。默认值：false。
- pattern_blend_mode（lv_blend_mode_t）：设置图案的混合模式。可以LV_BLEND_MODE_NORMAL/ADDITIVE/SUBTRACTIVE）。默认值：LV_BLEND_MODE_NORMAL。

![](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/1606979811531.png)
```
#include "../../lv_examples.h"

/**
 * Using the pattern style properties
 */
void lv_ex_style_5(void)
{
    static lv_style_t style;
    lv_style_init(&style);

    /*Set a background color and a radius*/
    lv_style_set_radius(&style, LV_STATE_DEFAULT, 5);
    lv_style_set_bg_opa(&style, LV_STATE_DEFAULT, LV_OPA_COVER);
    lv_style_set_bg_color(&style, LV_STATE_DEFAULT, LV_COLOR_SILVER);

    /*Add a repeating pattern*/
    lv_style_set_pattern_image(&style, LV_STATE_DEFAULT, LV_SYMBOL_OK);
    lv_style_set_pattern_recolor(&style, LV_STATE_DEFAULT, LV_COLOR_BLUE);
    lv_style_set_pattern_opa(&style, LV_STATE_DEFAULT, LV_OPA_50);
    lv_style_set_pattern_repeat(&style, LV_STATE_DEFAULT, true);

    /*Create an object with the new style*/
    lv_obj_t * obj = lv_obj_create(lv_scr_act(), NULL);
    lv_obj_add_style(obj, LV_OBJ_PART_MAIN, &style);
    lv_obj_align(obj, NULL, LV_ALIGN_CENTER, 0, 0);
}
```

#### 数值属性

值是绘制到背景的任意文本。它可以是创建标签对象的轻量级替代。
- value_str（）：指向要显示的文本的指针。仅保存指针！（不要将局部变量与lv_style_set_value_str一起使用，而应使用静态，全局或动态分配的数据）。默认值：NULL
- value_color（lv_color_t）：文本的颜色。默认值：LV_COLOR_BLACK。
- value_opa（lv_opa_t）：文本的不透明度。默认值：LV_OPA_COVER。
- value_font（）：指向文本字体的指针。默认值：NULL
- value_letter_space（lv_style_int_t）：文本的字母空间。默认值：0
- value_line_space（lv_style_int_t）：文本的行距。默认值：0。
- value_align（lv_align_t）：文本的对齐方式。可以LV_ALIGN_...。默认值：LV_ALIGN_CENTER。
- value_ofs_x（lv_style_int_t）：与路线原始位置的X偏移量。默认值：0。
- value_ofs_y（lv_style_int_t）：从路线的原始位置偏移Y。默认值：0。
- value_blend_mode（lv_blend_mode_t）：设置文本的混合模式。可以LV_BLEND_MODE_NORMAL/ADDITIVE/SUBTRACTIVE）。默认值：LV_BLEND_MODE_NORMAL。
![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/1606979916503.png)

```
#include "../../lv_examples.h"

/**
 * Using the value style properties
 */
void lv_ex_style_6(void)
{
    static lv_style_t style;
    lv_style_init(&style);

    /*Set a background color and a radius*/
    lv_style_set_radius(&style, LV_STATE_DEFAULT, 5);
    lv_style_set_bg_opa(&style, LV_STATE_DEFAULT, LV_OPA_COVER);
    lv_style_set_bg_color(&style, LV_STATE_DEFAULT, LV_COLOR_SILVER);

    /*Add a value text properties*/
    lv_style_set_value_color(&style, LV_STATE_DEFAULT, LV_COLOR_BLUE);
    lv_style_set_value_align(&style, LV_STATE_DEFAULT, LV_ALIGN_IN_BOTTOM_RIGHT);
    lv_style_set_value_ofs_x(&style, LV_STATE_DEFAULT, 10);
    lv_style_set_value_ofs_y(&style, LV_STATE_DEFAULT, 10);

    /*Create an object with the new style*/
    lv_obj_t * obj = lv_obj_create(lv_scr_act(), NULL);
    lv_obj_add_style(obj, LV_OBJ_PART_MAIN, &style);
    lv_obj_align(obj, NULL, LV_ALIGN_CENTER, 0, 0);

    /*Add a value text to the local style. This way every object can have different text*/
    lv_obj_set_style_local_value_str(obj, LV_OBJ_PART_MAIN, LV_STATE_DEFAULT, "Text");
}

```

#### 文字属性

文本对象的属性。
- text_color（lv_color_t）：文本的颜色。默认值：LV_COLOR_BLACK。
- text_opa（lv_opa_t）：文本的不透明度。默认值：LV_OPA_COVER。
- text_font（）：指向文本字体的指针。默认值constlv_font_t*NULL
- text_letter_space（lv_style_int_t）：文本的字母空间。默认值：0
- text_line_space（lv_style_int_t）：文本的行距。默认值：0。
- text_decor（lv_text_decor_t）：添加文字修饰。可以LV_TEXT_DECOR_NONE/UNDERLINE/STRIKETHROUGH。默认值：LV_TEXT_DECOR_NONE。
- text_sel_color（lv_color_t）：设置文本选择的背景色。默认值：LV_COLOR_BLACK
- text_blend_mode（lv_blend_mode_t）：设置文本的混合模式。可以LV_BLEND_MODE_NORMAL/ADDITIVE/SUBTRACTIVE）。默认值：LV_BLEND_MODE_NORMAL。

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/1606980103626.png)

```
#include "../../lv_examples.h"

/**
 * Using the text style properties
 */
void lv_ex_style_7(void)
{
    static lv_style_t style;
    lv_style_init(&style);

    lv_style_set_radius(&style, LV_STATE_DEFAULT, 5);
    lv_style_set_bg_opa(&style, LV_STATE_DEFAULT, LV_OPA_COVER);
    lv_style_set_bg_color(&style, LV_STATE_DEFAULT, LV_COLOR_SILVER);
    lv_style_set_border_width(&style, LV_STATE_DEFAULT, 2);
    lv_style_set_border_color(&style, LV_STATE_DEFAULT, LV_COLOR_BLUE);

    lv_style_set_pad_top(&style, LV_STATE_DEFAULT, 10);
    lv_style_set_pad_bottom(&style, LV_STATE_DEFAULT, 10);
    lv_style_set_pad_left(&style, LV_STATE_DEFAULT, 10);
    lv_style_set_pad_right(&style, LV_STATE_DEFAULT, 10);

    lv_style_set_text_color(&style, LV_STATE_DEFAULT, LV_COLOR_BLUE);
    lv_style_set_text_letter_space(&style, LV_STATE_DEFAULT, 5);
    lv_style_set_text_line_space(&style, LV_STATE_DEFAULT, 20);
    lv_style_set_text_decor(&style, LV_STATE_DEFAULT, LV_TEXT_DECOR_UNDERLINE);

    /*Create an object with the new style*/
    lv_obj_t * obj = lv_label_create(lv_scr_act(), NULL);
    lv_obj_add_style(obj, LV_LABEL_PART_MAIN, &style);
    lv_label_set_text(obj, "Text of\n"
                            "a label");
    lv_obj_align(obj, NULL, LV_ALIGN_CENTER, 0, 0);
}
```

#### 线属性

线的属性。
- line_color（lv_color_t）：线条的颜色。默认值：LV_COLOR_BLACK
- line_opa（lv_opa_t）：直线的不透明度。默认值：LV_OPA_COVER
- line_width（lv_style_int_t）：线的宽度。默认值：0。
- line_dash_width（lv_style_int_t）：破折号的宽度。仅对水平或垂直线绘制虚线。0：禁用破折号。默认值：0。
- line_dash_gap（lv_style_int_t）：两条虚线之间的间隙。仅对水平或垂直线绘制虚线。0：禁用破折号。默认值：0。
- line_rounded（bool）：：true绘制圆角的线尾。默认值：false。
- line_blend_mode（lv_blend_mode_t）：设置线条的混合模式。可以LV_BLEND_MODE_NORMAL/ADDITIVE/SUBTRACTIVE）。默认值：LV_BLEND_MODE_NORMAL。

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/1606980179758.png)

```c
#include "../../lv_examples.h"

/**
 * Using the line style properties
 */
void lv_ex_style_8(void)
{
    static lv_style_t style;
    lv_style_init(&style);

    lv_style_set_line_color(&style, LV_STATE_DEFAULT, LV_COLOR_GRAY);
    lv_style_set_line_width(&style, LV_STATE_DEFAULT, 6);
    lv_style_set_line_rounded(&style, LV_STATE_DEFAULT, true);
	
    /*Create an object with the new style*/
    lv_obj_t * obj = lv_line_create(lv_scr_act(), NULL);
    lv_obj_add_style(obj, LV_LINE_PART_MAIN, &style);

    static lv_point_t p[] = {\{10,30\}, \{30,50\}, \{100,0\}};  // 我也不知道为啥这句话会导致github page报错，此处6个 '\'为多余，请自行删除
    lv_line_set_points(obj, p, 3);

    lv_obj_align(obj, NULL, LV_ALIGN_CENTER, 0, 0);
}
```
#### 图片属性

图像的属性。
- image_recolor（lv_color_t）：将此颜色混合到图案图像中。如果是符号（文本），它将是文本颜色。默认值：LV_COLOR_BLACK
- image_recolor_opa（lv_opa_t）：重新着色的强度。默认值：（LV_OPA_TRANSP不重新着色）。默认值：LV_OPA_TRANSP
- image_opa（lv_opa_t）：图像的不透明度。默认值：LV_OPA_COVER
- image_blend_mode（lv_blend_mode_t）：设置图像的混合模式。可以LV_BLEND_MODE_NORMAL/ADDITIVE/SUBTRACTIVE）。默认值：LV_BLEND_MODE_NORMAL。

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/1606980309881.png)
```c
#include "../../lv_examples.h"

/**
 * Using the image style properties
 */
void lv_ex_style_9(void)
{
    static lv_style_t style;
    lv_style_init(&style);

    /*Set a background color and a radius*/
    lv_style_set_radius(&style, LV_STATE_DEFAULT, 5);
    lv_style_set_bg_opa(&style, LV_STATE_DEFAULT, LV_OPA_COVER);
    lv_style_set_bg_color(&style, LV_STATE_DEFAULT, LV_COLOR_SILVER);
    lv_style_set_border_width(&style, LV_STATE_DEFAULT, 2);
    lv_style_set_border_color(&style, LV_STATE_DEFAULT, LV_COLOR_BLUE);

    lv_style_set_pad_top(&style, LV_STATE_DEFAULT, 10);
    lv_style_set_pad_bottom(&style, LV_STATE_DEFAULT, 10);
    lv_style_set_pad_left(&style, LV_STATE_DEFAULT, 10);
    lv_style_set_pad_right(&style, LV_STATE_DEFAULT, 10);

    lv_style_set_image_recolor(&style, LV_STATE_DEFAULT, LV_COLOR_BLUE);
    lv_style_set_image_recolor_opa(&style, LV_STATE_DEFAULT, LV_OPA_50);

#if LV_USE_IMG
    /*Create an object with the new style*/
    lv_obj_t * obj = lv_img_create(lv_scr_act(), NULL);
    lv_obj_add_style(obj, LV_IMG_PART_MAIN, &style);
    LV_IMG_DECLARE(img_cogwheel_argb);
    lv_img_set_src(obj, &img_cogwheel_argb);
    lv_obj_align(obj, NULL, LV_ALIGN_CENTER, 0, 0);
#endif
}
```

#### 转换特性

用于描述状态更改动画的属性。
- transition_time（lv_style_int_t）：过渡时间。默认值：0。
- transition_delay（lv_style_int_t）：转换前的延迟。默认值：0。
- transition_prop_1（propertyname）：应在其上应用过渡的属性。将属性名称`LV_STYLE_`与大写字母一起使用，例如`LV_STYLE_BG_COLOR`。默认值：0（无）。
- transition_prop_2（propertyname）：与transition_1相同，只是另一个属性。默认值：0（无）。
- transition_prop_3（propertyname）：与transition_1相同，只是另一个属性。默认值：0（无）。
- transition_prop_4（propertyname）：与transition_1相同，只是另一个属性。默认值：0（无）。
- transition_prop_5（propertyname）：与transition_1相同，只是另一个属性。默认值：0（无）。
- transition_prop_6（propertyname）：与transition_1相同，只是另一个属性。默认值：0（无）。
- transition_path（lv_anim_path_t）：过渡的动画path。（需要为静态或全局变量，因为仅保存了其指针）。默认值：（lv_anim_path_def线性path）。

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/Transition_properties.gif)
```
#include "../../lv_examples.h"

/**
 * Using the transitions style properties
 */
void lv_ex_style_10(void)
{
    static lv_style_t style;
    lv_style_init(&style);

    /*Set a background color and a radius*/
    lv_style_set_radius(&style, LV_STATE_DEFAULT, 5);
    lv_style_set_bg_opa(&style, LV_STATE_DEFAULT, LV_OPA_COVER);
    lv_style_set_bg_color(&style, LV_STATE_DEFAULT, LV_COLOR_SILVER);

    /*Set different background color in pressed state*/
    lv_style_set_bg_color(&style, LV_STATE_PRESSED, LV_COLOR_GRAY);

    /*Set different transition time in default and pressed state
     *fast press, slower revert to default*/
    lv_style_set_transition_time(&style, LV_STATE_DEFAULT, 500);
    lv_style_set_transition_time(&style, LV_STATE_PRESSED, 200);

    /*Small delay to make transition more visible*/
    lv_style_set_transition_delay(&style, LV_STATE_DEFAULT, 100);

    /*Add `bg_color` to transitioned properties*/
    lv_style_set_transition_prop_1(&style, LV_STATE_DEFAULT, LV_STYLE_BG_COLOR);

    /*Create an object with the new style*/
    lv_obj_t * obj = lv_obj_create(lv_scr_act(), NULL);
    lv_obj_add_style(obj, LV_OBJ_PART_MAIN, &style);
    lv_obj_align(obj, NULL, LV_ALIGN_CENTER, 0, 0);
}
```

#### 比例属性

鳞片状元素的辅助属性。体重秤具有正常区域和末端区域。顾名思义，结束区域是标度的结束，可以是临界值或void值。正常区域在结束区域之前。两个区域可能具有不同的属性
- scale_grad_color（lv_color_t）：在正常区域中，在比例尺线上对该颜色进行渐变。默认值：LV_COLOR_BLACK。
- scale_end_color（lv_color_t）：结束区域中刻度线的颜色。默认值：LV_COLOR_BLACK。
- scale_width（lv_style_int_t）：比例尺的宽度。默认值：。默认值：。LV_DPI/8LV_DPI/8
- scale_border_width（lv_style_int_t）：在标准区域的比例尺外侧绘制的边框的宽度。默认值：0。
- scale_end_border_width（lv_style_int_t）：在结束区域的刻度外侧上绘制边框的宽度。默认值：0。
- scale_end_line_width（lv_style_int_t）：结束区域中比例线的宽度。默认值：0。

![](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/1606981343856.png)
```
#include "../../lv_examples.h"

/**
 * Using the scale style properties
 */
void lv_ex_style_11(void)
{
    static lv_style_t style;
    lv_style_init(&style);

    /*Set a background color and a radius*/
    lv_style_set_radius(&style, LV_STATE_DEFAULT, 5);
    lv_style_set_bg_opa(&style, LV_STATE_DEFAULT, LV_OPA_COVER);
    lv_style_set_bg_color(&style, LV_STATE_DEFAULT, LV_COLOR_SILVER);

    /*Set some paddings*/
    lv_style_set_pad_inner(&style, LV_STATE_DEFAULT, 20);
    lv_style_set_pad_top(&style, LV_STATE_DEFAULT, 20);
    lv_style_set_pad_left(&style, LV_STATE_DEFAULT, 5);
    lv_style_set_pad_right(&style, LV_STATE_DEFAULT, 5);

    lv_style_set_scale_end_color(&style, LV_STATE_DEFAULT, LV_COLOR_RED);
    lv_style_set_line_color(&style, LV_STATE_DEFAULT, LV_COLOR_WHITE);
    lv_style_set_scale_grad_color(&style, LV_STATE_DEFAULT, LV_COLOR_BLUE);
    lv_style_set_line_width(&style, LV_STATE_DEFAULT, 2);
    lv_style_set_scale_end_line_width(&style, LV_STATE_DEFAULT, 4);
    lv_style_set_scale_end_border_width(&style, LV_STATE_DEFAULT, 4);

    /*Gauge has a needle but for simplicity its style is not initialized here*/
#if LV_USE_GAUGE
    /*Create an object with the new style*/
    lv_obj_t * obj = lv_gauge_create(lv_scr_act(), NULL);
    lv_obj_add_style(obj, LV_GAUGE_PART_MAIN, &style);
    lv_obj_align(obj, NULL, LV_ALIGN_CENTER, 0, 0);
#endif
}
```
在小部件的文档中，您将看到诸如“小部件使用典型的背景属性”之类的句子。“典型背景”属性是：
- 背景
- 边境
- 大纲
- 阴影
- 模式
- 值

### 主题

主题是样式的集合。始终有一个活动主题，在创建对象时会自动应用其样式。它为UI提供了默认外观，可以通过添加其他样式来对其进行修改。

默认的主题被设定在`lv_conf.h`与`LV_THEME_...`定义。每个主题都具有以下属性

- 原色primary color
- 二次色secondary color
- 小字体
- 普通字体
- 字幕字体
- 标题字体
- 标志（特定于给定主题）

如何使用这些属性取决于主题。

有3个内置主题：
- empty空：未添加默认样式
- material材质：令人印象深刻的现代主题-单声道：用于黑白显示的简单黑白主题
- template模板：一个非常简单的主题，可以将其复制以创建自定义主题

#### 扩展主题 

内置主题可以通过自定义主题进行扩展。如果创建了自定义主题，则可以选择“基本主题”。基本主题的样式将添加到自定义主题之前。可以链接任何数量的主题。例如，材料主题->自定义主题->黑暗主题。这是有关如何基于当前活动的内置主题创建自定义主题的示例

```
 /*Get the current theme (e.g. material). It will be the base of the custom theme.*/   
lv_theme_t * base_theme = lv_theme_get_act();

/*Initialize a custom theme*/
static lv_theme_t custom_theme;                         /*Declare a theme*/
lv_theme_copy(&custom_theme, )base_theme;               /*Initialize the custom theme from the base theme*/                           
lv_theme_set_apply_cb(&custom_theme, custom_apply_cb);  /*Set a custom theme apply callback*/
lv_theme_set_base(custom_theme, base_theme);            /*Set the base theme of the csutom theme*/

/*Initialize styles for the new theme*/
static lv_style_t style1;
lv_style_init(&style1);
lv_style_set_bg_color(&style1, LV_STATE_DEFAULT, custom_theme.color_primary);

...

/*Add a custom apply callback*/
static void custom_apply_cb(lv_theme_t * th, lv_obj_t * obj, lv_theme_style_t name)
{
    lv_style_list_t * list;

    switch(name) {
        case LV_THEME_BTN:
            list = lv_obj_get_style_list(obj, LV_BTN_PART_MAIN);
            _lv_style_list_add_style(list, &my_style);
            break;
    }
}
```

### Example

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/1606981634954.png)

```
#include "../../lv_examples.h"


/**
 * Create styles from scratch for buttons.
 */
void lv_ex_get_started_2(void)
{
    static lv_style_t style_btn;
    static lv_style_t style_btn_red;

    /*Create a simple button style*/
    lv_style_init(&style_btn);
    lv_style_set_radius(&style_btn, LV_STATE_DEFAULT, 10);
    lv_style_set_bg_opa(&style_btn, LV_STATE_DEFAULT, LV_OPA_COVER);
    lv_style_set_bg_color(&style_btn, LV_STATE_DEFAULT, LV_COLOR_SILVER);
    lv_style_set_bg_grad_color(&style_btn, LV_STATE_DEFAULT, LV_COLOR_GRAY);
    lv_style_set_bg_grad_dir(&style_btn, LV_STATE_DEFAULT, LV_GRAD_DIR_VER);

    /*Swap the colors in pressed state*/
    lv_style_set_bg_color(&style_btn, LV_STATE_PRESSED, LV_COLOR_GRAY);
    lv_style_set_bg_grad_color(&style_btn, LV_STATE_PRESSED, LV_COLOR_SILVER);

    /*Add a border*/
    lv_style_set_border_color(&style_btn, LV_STATE_DEFAULT, LV_COLOR_WHITE);
    lv_style_set_border_opa(&style_btn, LV_STATE_DEFAULT, LV_OPA_70);
    lv_style_set_border_width(&style_btn, LV_STATE_DEFAULT, 2);

    /*Different border color in focused state*/
    lv_style_set_border_color(&style_btn, LV_STATE_FOCUSED, LV_COLOR_BLUE);
    lv_style_set_border_color(&style_btn, LV_STATE_FOCUSED | LV_STATE_PRESSED, LV_COLOR_NAVY);

    /*Set the text style*/
    lv_style_set_text_color(&style_btn, LV_STATE_DEFAULT, LV_COLOR_WHITE);

    /*Make the button smaller when pressed*/
    lv_style_set_transform_height(&style_btn, LV_STATE_PRESSED, -5);
    lv_style_set_transform_width(&style_btn, LV_STATE_PRESSED, -10);
#if LV_USE_ANIMATION
    /*Add a transition to the size change*/
    static lv_anim_path_t path;
    lv_anim_path_init(&path);
    lv_anim_path_set_cb(&path, lv_anim_path_overshoot);

    lv_style_set_transition_prop_1(&style_btn, LV_STATE_DEFAULT, LV_STYLE_TRANSFORM_HEIGHT);
    lv_style_set_transition_prop_2(&style_btn, LV_STATE_DEFAULT, LV_STYLE_TRANSFORM_WIDTH);
    lv_style_set_transition_time(&style_btn, LV_STATE_DEFAULT, 300);
    lv_style_set_transition_path(&style_btn, LV_STATE_DEFAULT, &path);
#endif

    /*Create a red style. Change only some colors.*/
    lv_style_init(&style_btn_red);
    lv_style_set_bg_color(&style_btn_red, LV_STATE_DEFAULT, LV_COLOR_RED);
    lv_style_set_bg_grad_color(&style_btn_red, LV_STATE_DEFAULT, LV_COLOR_MAROON);
    lv_style_set_bg_color(&style_btn_red, LV_STATE_PRESSED, LV_COLOR_MAROON);
    lv_style_set_bg_grad_color(&style_btn_red, LV_STATE_PRESSED, LV_COLOR_RED);
    lv_style_set_text_color(&style_btn_red, LV_STATE_DEFAULT, LV_COLOR_WHITE);
#if LV_USE_BTN
    /*Create buttons and use the new styles*/
    lv_obj_t * btn = lv_btn_create(lv_scr_act(), NULL);     /*Add a button the current screen*/
    lv_obj_set_pos(btn, 10, 10);                            /*Set its position*/
    lv_obj_set_size(btn, 120, 50);                          /*Set its size*/
    lv_obj_reset_style_list(btn, LV_BTN_PART_MAIN);         /*Remove the styles coming from the theme*/
    lv_obj_add_style(btn, LV_BTN_PART_MAIN, &style_btn);

    lv_obj_t * label = lv_label_create(btn, NULL);          /*Add a label to the button*/
    lv_label_set_text(label, "Button");                     /*Set the labels text*/

    /*Create a new button*/
    lv_obj_t * btn2 = lv_btn_create(lv_scr_act(), btn);
    lv_obj_set_pos(btn2, 10, 80);
    lv_obj_set_size(btn2, 120, 50);                             /*Set its size*/
    lv_obj_reset_style_list(btn2, LV_BTN_PART_MAIN);         /*Remove the styles coming from the theme*/
    lv_obj_add_style(btn2, LV_BTN_PART_MAIN, &style_btn);
    lv_obj_add_style(btn2, LV_BTN_PART_MAIN, &style_btn_red);   /*Add the red style on top of the current */
    lv_obj_set_style_local_radius(btn2, LV_BTN_PART_MAIN, LV_STATE_DEFAULT, LV_RADIUS_CIRCLE); /*Add a local style*/

    label = lv_label_create(btn2, NULL);          /*Add a label to the button*/
    lv_label_set_text(label, "Button 2");                     /*Set the labels text*/
#endif
}
```

## 动画

您可以使用动画在开始值和结束值之间自动更改变量的值。动画将通过定期调用带有相应value参数的“animator”函数来发生。  
该动画功能具有以下原型： 
```
voidfunc(void*var,lv_anim_var_tvalue);
```
该原型与LVGL的大多数设置功能兼容。例如
```lv_obj_set_x(obj, value) or lv_obj_set_width(obj, value)```
### 创建动画

要创建动画，必须初始化变量 lv_anim_t并使用lv_anim_set_...()功能对其进行配置。
```
/*初始化动画*----------------------*/
lv_anim_t a;
lv_anim_init(&a);

/*强制设置*------------------*/

/*设置“动画”时要调用的函数*/
lv_anim_set_exec_cb(&a,(lv_anim_exec_xcb_t)lv_obj_set_x);

/*Setthe"animator"function*/
lv_anim_set_var(&a,obj);

/*动画时间【ms】*/
lv_anim_set_time(&a,duration);

/*设置开始和结束值。.E.g.0,150*/
lv_anim_set_values(&a,start,end);

/*可选设置*------------------*/

/*开始动画之前要等待的时间[ms]*/
lv_anim_set_delay(&a,delay);

/*设置路径（曲线）。默认为线性*/
lv_anim_set_path(&a,&path);

/*设置回调以在动画准备好时调用.*/
lv_anim_set_ready_cb(&a,ready_cb);

/*设置在动画开始时调用的回调(afterdelay).*/
lv_anim_set_start_cb(&a, start_cb);

/*在此持续时间内也向后播放动画. Default is 0 (disabled) [ms]*/
lv_anim_set_playback_time(&a, wait_time); 

/*播放前延迟. Default is 0 (disabled) [ms]*/
lv_anim_set_playback_delay(&a, wait_time);

/*重复次数. Default is 1.  LV_ANIM_REPEAT_INFINIT for 无限重复*/
lv_anim_set_repeat_count(&a, wait_time);

/*延迟再重复. Default is 0 (disabled) [ms]*/
lv_anim_set_repeat_delay(&a, wait_time);

/*true（默认）：立即应用开始值，false：延迟设置动画后再应用开始值。真正开始。 */
lv_anim_set_early_apply(&a, true/false);

/* START THE ANIMATION
 *------------------*/
lv_anim_start(&a);                             /*Start the animation*/
```
您可以同时在同一变量上应用多个不同的动画。例如，使用lv_obj_set_x和lv_obj_set_y设置x和y坐标的动画。但是，只有一个动画可以存在给定的变量和函数对。因此lv_anim_start()将删除已经存在的可变功能动画。

### 动画path

您可以确定动画的path。在最简单的情况下，它是线性的，这意味着开始和结束之间的当前值线性变化。path主要是其计算基于所述动画的当前状态中的下一个值集的函数。当前，有以下内置path功能：
- lv_anim_path_linear线性动画
- lv_anim_path_step最后一步更改
- lv_anim_path_ease_in开头缓慢
- lv_anim_path_ease_out最后慢
- lv_anim_path_ease_in_out在开始和结束时也很慢
- lv_anim_path_overshoot超出最终值
- lv_anim_path_bounce从最终值反弹一点（就像撞墙一样）
  
 可以这样初始化path：
 ```
 lv_anim_path_t path;
lv_anim_path_init(&path);
lv_anim_path_set_cb(&path, lv_anim_path_overshoot);
lv_anim_path_set_user_data(&path, &foo); /*Optional for custom functions*/

/*Set the path in an animation*/
lv_anim_set_path(&a, &path);
```
# 通过CodeBlocks模拟运行LittlevGL

其实官方有使用教程：[lvgl/lv_sim_codeblocks_win](https://github.com/lvgl/lv_sim_codeblocks_win)
```
Tutorial for Windows
Download and install Git
Download CodeBlocks in latest version. It is recommended to use the version which includes MinGW, as otherwise you will have to install and configure it separately.
Clone this repository.
Open Command prompt (Win key + R -> cmd -> Enter) or PowerShell (Win key + R -> powershell -> Enter) (if you want different folder than C:/Users/username you have to navigate to it) and type: git clone https://github.com/lvgl/lv_sim_codeblocks_win.git
In that folder type: cd lv_sim_codeblocks_win and press Enter. Here type: git submodule update --init --recursive and press Enter. This should download files to lvgl, lv_examples and lv_drivers folders.
Open CodeBlocks and select Open an existing project. Navigate to the lv_sim_codeblocks_win folder and select LittlevGL.cbp.
Click on Build and Run or press F9.
If everything goes well, you should see your simulator running.
```
下面就跟着官方教程走。

1. 下载安装 git：[1.5 起步 - 安装 Git](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git)
比较简单，跟着git官方教程走就行了。这是为了后边使用命令下载 LVGL 仿真文件。
2. 下载安装 CodeBlocks：[CodeBlocks下载地址](http://www.codeblocks.org/downloads/26)
选择带编译器的版本：codeblocks-20.03mingw-setup。
安装一路默认即可，可更改安装路径
3. 打开 cmd，输入命令:
    ```
    git clone https://github.com/lvgl/lv_sim_codeblocks_win.git
    ```
    文件默认会下载到目录 `C:/Users/username`。如果自己指定目录，请提前进入指定目录下，再输入上述命令下载
4. 使用命令
    ```
    cd lv_sim_codeblocks_win
    ```
    回车进入该文件目录下，并输入命令：
    ```
    git submodule update --init --recursive
    ```
    回车，将自动下载lvgl, lv_examples and lv_drivers 文件内容
5. 打开 codeblocks，开始界面点击 `Open an existing project`，进入我们刚下载的 `lv_sim_codeblocks_win`文件夹并选择 ittlevGL.cbp` ，确认打开。
6. 单击 `Click on Build` 图标或者按 `F9` 编译运行工程。
如果编译时提示找不到编译器，出现无法编译的情况，则需对程序的编译环境进行重新配置
![图 1](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/codeblock_settings.png) 
这里先点击 `Auto-detect`自动检测看自己电脑上有没有，如果没有的话， 则需要单独下载mingw：[mingw-w64](http://mingw-w64.org/doku.php)。再设置mingw路径。
![图 2](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/settings_compiler.png)  
7. 开始使用仿真。

>这里下载仿真的 LVGL 版本比较老，7.6.0 版本，不过这与最新版差别并不大，新版主要时修复了一些bug。
关于最新版的仿真，目前并无方法。

**参考资料**
- [LVGL：Win7基于CodeBlocks的PC模拟仿真](https://blog.csdn.net/qq_33661199/article/details/108199967)
- [【LittleVGL】Windows环境下利用CodeBlocks搭建LittleVGL的PC模拟器环境配置问题](https://blog.csdn.net/SSA_ming/article/details/107875678)
- [AN0018_LittlevGL_on_AT32_MCU_ZH_V1.0.3](www.arterytek.com/download/AN0018_LittlevGL_on_AT32_MCU_ZH_V1.0.3.pdf)

# 使用PlatformIO运行LittlevGL

>该方案不可行，不可行！不可行！原因未知。
>已在 windows 和 linux 都测试过，均不可用，源码编译有错误。

该演示应帮助您使用出色的PlatformIO IDE组织项目。
它会自动安装所有内容-只需在vscode中打开此项目的文件夹，并同意安装它提供的所有功能
它包含可运行的LittlevGL演示，可在PC上运行（通过SDL驱动程序）。
它有示例，说明如何创建多个构建目标。例如：
native 快速在PC上建立原型接口
其他，为裸机构建固件


## 如何安装和使用演示

重要！native 构建（带有SDL2驱动程序的模拟器）仅在Linux上进行了测试！但是您仍然可以构建MCU目标。如果有人有兴趣改善其他OS支持（Windows，macOS）-欢迎PR。

### Install Visual Studio Code

https://code.visualstudio.com/ 

### Install SDL drivers

#### Windows

先安装 [MSYS2](https://www.msys2.org/)，记住软件安装路径，后面用到。
之后打开 MSY32，只命令行输入下面语句：
>国内网速安装较慢，搭梯子请设置全局梯子，再重新打开 MSY32 并输入下面语句。
```
pacman -S mingw-w64-x86_64-gcc mingw-w64-x86_64-SDL2
```
将 Mingw-w64  安装路径下的 bin 文件夹(默认为 C:\msys64\mingw64\bin) 加载到 Windows 系统的 PATH 环境变量中
 1. 在win10搜索框输入 `设置`
 2. 输入`环境变量`,选中`编辑账户的环境变量`
 3. 再弹窗中 变量 栏中找到 `Path` 并单击选中，单击 `编辑`按钮，弹窗中 单击 `新建`，紧接着单击 `浏览`,找到  Mingw-w64  安装路径下的 bin 文件夹
 4. 一步步单击 `确定` 推出弹窗保存设置，之后重新打开 MSY32
 5. 在MSY32中分别输入命令
  ```
  g++ --version
  gdb --version
  ```
如果看不到预期的输出或g ++或gdb无法识别的命令，请检查安装（Windows控制面板>程序），并确保PATH条目与Mingw-w64 bin文件夹位置匹配

### 构建/运行

下载 [lv_platformio 文件](https://github.com/lvgl/lv_platformio)

打开 vscode，File -> Open Folder 打开下载的文件
- 如果你是第一次使用platformio，如果 vscode 弹窗提醒你要安装 PlatformIO 插件，请点击 `同意`。然后等待PlatformIO安装。

构建/执行：
- 仿真: Terminal（终端） -> Run Task...（） -> PlatformIO: Execute (native)
Bare metal: Terminal -> Run Build Task... -> PlatformIO: Build (stm32f429_disco)

## 参考文件

[Run LittlevGL via PlatformIO](https://github.com/lvgl/lv_platformio)
[Using GCC with MinGW](https://code.visualstudio.com/docs/cpp/config-mingw#_prerequisites)
[LittlevGL基础控件学习](https://littlevgl.readthedocs.io/en/latest/Doc/01.LittelvGL%E5%9F%BA%E6%9C%AC%E7%9F%A5%E8%AF%86%E5%AD%A6%E4%B9%A0/LittlevGL.html#ainmation)
[LittlevGL在AT32上的移植说明](http://www.arterytek.com/download/AN0018_LittlevGL_on_AT32_MCU_ZH_V1.0.3.pdf)
# STM32F4 SPI DMA显示
[GUuiLite-MCU SPI屏也能跑这么炫酷的特效？来，移植起来秀一秀](https://mp.ofweek.com/ee/a545693620996)
[STM32---SPI通信的总结(库函数操作)](https://www.cnblogs.com/linxw-blog/p/12488682.html)
[【STM32】使用DMA+SPI传输数据](https://my.oschina.net/u/4314849/blog/3447441)
[【LVGL学习之旅 01】移植LVGL到STM32](https://blog.csdn.net/qq_40831286/article/details/107633216)
[ScarsFun/lvgl_STM32F103_encoder_rtx5](https://github.com/ScarsFun/lvgl_STM32F103_encoder_rtx5/blob/master/ili9341/dma.c)
[(三)stm32之串口通信DMA传输完成中断](https://www.cnblogs.com/zhangshenghui/p/5340483.html)
[使用 spi dma 传输和键盘时绘图区域错误？](https://forum.lvgl.io/t/drawing-area-error-when-using-spi-dma-transmit-and-keypad/288)
[weefnn/pandora](https://github.com/weefnn/pandora/blob/179ae468ab176c86b8b10732e22507869abeb56d/pandora_lvgl/pandora_lvgl/LVGL/hal_stm_lvgl/tft/tft.c#L32)
[lvgl/lv_port_stm32f746_disco](https://github.com/lvgl/lv_port_stm32f746_disco/blob/master/hal_stm_lvgl/tft/tft.c#L428-L429)
[两天移植littlevgl](https://www.firebbs.cn/thread-28679-1-1.html) 
[lvgl/lv_port_stm32f429_disco](https://github.com/lvgl/lv_port_stm32f429_disco)
[LVGL 优化帧率技巧](https://blog.csdn.net/weixin_43862847/article/details/109318017)
[f429 discovery开发版 LVGL移植（带操作系统）](https://blog.csdn.net/weixin_43862847/article/details/109056321)
[【STM32】HAL库 STM32CubeMX教程十一---DMA (串口DMA发送接收)](https://blog.csdn.net/as480133937/article/details/104827639)
[STM32 DMA传输笔记（HAL库版）](https://www.cnblogs.com/sovagxa/p/9135653.html)
[STM32f103c8cb HAL library with SPI over DMA](https://community.st.com/s/question/0D50X00009XkXmS/stm32f103c8cb-hal-library-with-spi-over-dma)
[STM32 cubeMX: triggering SPI DMA interrupt using interrupt](https://stackoverflow.com/questions/53999597/stm32-cubemx-triggering-spi-dma-interrupt-using-interrupt)
[HAL_SPI_TransmitReceive_DMA() transmit interrupt always triggered after receive interrupt](https://community.st.com/s/question/0D50X0000APclcf/halspitransmitreceivedma-transmit-interrupt-always-triggered-after-receive-interrupt)
[STM32CubeMx. SPI and DMA usage example for STM32 MCU.](https://microtechnics.ru/en/stm32cube-spi-and-dma-example/)

# LVGL 移植

## 文件移植与修改
跟随官方教程移植：[v7.11.0-dev](https://docs.lvgl.io/latest/en/html/get-started/index.html)

```
The following steps show how to setup LVGL on an embedded system with a display and a touchpad.

Download or Clone the library from GitHub with git clone https://github.com/lvgl/lvgl.git

Copy the lvgl folder into your project

Copy lvgl/lv_conf_template.h as lv_conf.h next to the lvgl folder, change the first #if 0 to 1 to enable the file's content and set at least LV_HOR_RES_MAX, LV_VER_RES_MAX and LV_COLOR_DEPTH defines.

Include lvgl/lvgl.h where you need to use LVGL related functions.

Call lv_tick_inc(x) every x milliseconds in a Timer or Task (x should be between 1 and 10). It is required for the internal timing of LVGL. Alternatively, configure LV_TICK_CUSTOM (see lv_conf.h) so that LVGL can retrieve the current time directly.

Call lv_init()

Create a display buffer for LVGL. LVGL will render the graphics here first, and seed the rendered image to the display. The buffer size can be set freely but 1/10 screen size is a good starting point.
```
1. 下载 LVGL 最新发布版 [Release v7.11.0](https://github.com/lvgl/lvgl/releases/tag/v7.11.0)。这里并不下载 master 仓库的。并将文件夹重命名为 `lvgl`
2. 将 lvgl 文件夹复制到工程目录下，并在IDE中将 `lvgl` 文件夹添加到编译路径中
3. 将lvgl文件夹中的lv_conf_template.h复制并重命名为lv_conf，与lvgl同级目录
4. 打开lv_conf.h，将#if 0改为 # if 1，设置屏幕的宽高像素，以及颜色模式，如下代码所示
    ```
    #if 1 /*Set it to "1" to enable content*/

    #ifndef LV_CONF_H
    #define LV_CONF_H
    /* clang-format off */

    #include <stdint.h>

    /*====================
    Graphical settings
    *====================*/

    /* Maximal horizontal and vertical resolution to support by the library.*/
    #define LV_HOR_RES_MAX          (240)  // 水平像素
    #define LV_VER_RES_MAX          (320)  // 垂直像素

    /* Color depth:
    * - 1:  1 byte per pixel
    * - 8:  RGB332
    * - 16: RGB565
    * - 32: ARGB8888
    */
    #define LV_COLOR_DEPTH     16  // 这里选择 RGB565模式
    ```
5. 在你需要用到的littlevGL的文件添加#include "lvgl.h"
6. 调用lv_init();函数
在一个定时任务中调用`lv_tick_inc（x）`每x毫秒（x应在1到10之间）。 LVGL的内部时间需要。或者配置lv_tick_custom（请参阅LV_CONF.H）直接使用库函数自动调用`lv_tick_inc（x）`。
7. 调用 lv_init() 初始化
8. 设置显示缓冲区。LVGL首先将在此缓存图像数据，并将渲染图像作为显示器进行。缓冲区大小可以自由设置，但1/10屏幕尺寸是一个很好的起点。
   ```
    static lv_disp_buf_t disp_buf;
    static lv_color_t buf[LV_HOR_RES_MAX * LV_VER_RES_MAX / 10];                     /*Declare a buffer for 1/10 screen size*/
    lv_disp_buf_init(&disp_buf, buf, NULL, LV_HOR_RES_MAX * LV_VER_RES_MAX / 10);    /*Initialize the display buffer*/
    ```
9. 实现并注册可以将渲染图像复制到显示区域的函数：
    ```
    lv_disp_drv_t disp_drv;               /*Descriptor of a display driver*/
    lv_disp_drv_init(&disp_drv);          /*Basic initialization*/
    disp_drv.flush_cb = my_disp_flush;    /*Set your driver function*/
    disp_drv.buffer = &disp_buf;          /*Assign the buffer to the display*/
    lv_disp_drv_register(&disp_drv);      /*Finally register the driver*/

    void my_disp_flush(lv_disp_drv_t * disp, const lv_area_t * area, lv_color_t * color_p)
    {
        int32_t x, y;
        for(y = area->y1; y <= area->y2; y++) {
            for(x = area->x1; x <= area->x2; x++) {
                set_pixel(x, y, *color_p);  /* Put a pixel to the display.*/
                color_p++;
            }
        }

        lv_disp_flush_ready(disp);         /* Indicate you are ready with the flushing*/
    }
    ```
    其中的set_pixel就是LCD顶层驱动的画点函数，这里直接替换成自己的画点函数即可。
10. 如果需要触摸输入就进行输入函数注册和实现，不需要就算了
    ```
    lv_indev_drv_t indev_drv;                  /*Descriptor of a input device driver*/
    lv_indev_drv_init(&indev_drv);             /*Basic initialization*/
    indev_drv.type = LV_INDEV_TYPE_POINTER;    /*Touch pad is a pointer-like device*/
    indev_drv.read_cb = my_touchpad_read;      /*Set your driver function*/
    lv_indev_drv_register(&indev_drv);         /*Finally register the driver*/

    bool my_touchpad_read(lv_indev_t * indev, lv_indev_data_t * data)
    {
        data->state = touchpad_is_pressed() ? LV_INDEV_STATE_PR : LV_INDEV_STATE_REL;
        if(data->state == LV_INDEV_STATE_PR) touchpad_get_xy(&data->point.x, &data->point.y);

        return false; /*Return `false` because we are not buffering and no more data to read*/
    }
    ```
    这里 `touchpad_get_xy` 和 `touchpad_is_pressed()`也需要我们自己实现，目的就是将触摸坐标赋值给`data->point.x, &data->point.y,data->state`。因此这里可以自由发挥，只要把坐标值给到这俩变量就行了。例如
    ```
    bool my_touchpad_read(lv_indev_drv_t * drv, lv_indev_data_t*data)
    {
        data->point.x = touchpad_x;
        data->point.y = touchpad_y;
        data->state = LV_INDEV_STATE_PR or LV_INDEV_STATE_REL;
        return false; /*现在没有缓冲，所以没有更多的数据读取*/
    }
    ```
11. 定期调用lv_task_handler（）在主机中断或操作系统任务中每隔几毫秒。如果需要，它将重绘屏幕，处理输入设备等。

For a more detailed guide go to the[ Porting ](https://docs.lvgl.io/v7/en/html/porting/index.html)section.

以上的 9-10 步其实官方已经有例程了，我们不用再自己编写，只需要修改即可。例程在 `lvgl\examples\porting`，这里面存放的就是对外接口适配例程。用哪一个，就更改那个名字，并在相应文件中修改 `#if 0` 启用。
![图 3](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/porting%E6%9B%B4%E5%90%8D.png)  

- lv_port_disp：显示相关。
- lv_port_indev：输入相关。
- lv_port_fs：文件系统相关。

这里我们只使用 `lv_port_disp` 其它保持不动。并将 `lv_port_disp.c/h`文件复制到 lvgl 同级的 `lvgl_porting`目录下。 然后删除其它无关文件夹，最终文件结构如下：
![图 4](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/LVGL%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84png.png)  

这里我们按照上面的 8-10 步骤修改 lv_port_disp.c/h 文件

1. .c/h 文件中的 `#if 0` 改为 `# if 1` , .c 文件的头文件包含改为 `#include "lv_port_disp.h"` 
    ```
    /*Copy this file as "lv_port_disp.c" and set this value to "1" to enable content*/
    #if 1

    /*********************
    *      INCLUDES
    *********************/
    #include "lv_port_disp.h"
    ```
2. 缓存数组已经在 `lv_port_disp_init(void)` 定义好了，这里我们仅使用一级缓存，将其他数组定去注释掉，不然会占用RAM，导致RAM不够用。
    ```
        /* Example for 1) */
        static lv_disp_buf_t draw_buf_dsc_1;
        static lv_color_t draw_buf_1[LV_HOR_RES_MAX * 10];                          /*A buffer for 10 rows*/
        lv_disp_buf_init(&draw_buf_dsc_1, draw_buf_1, NULL, LV_HOR_RES_MAX * 10);   /*Initialize the display buffer*/

        /* Example for 2) */
    //    static lv_disp_buf_t draw_buf_dsc_2;
    //    static lv_color_t draw_buf_2_1[LV_HOR_RES_MAX * 10];                        /*A buffer for 10 rows*/
    //    static lv_color_t draw_buf_2_2[LV_HOR_RES_MAX * 10];                        /*An other buffer for 10 rows*/
    //    lv_disp_buf_init(&draw_buf_dsc_2, draw_buf_2_1, draw_buf_2_2, LV_HOR_RES_MAX * 10);   /*Initialize the display buffer*/

        /* Example for 3) */
    //    static lv_disp_buf_t draw_buf_dsc_3;
    //    static lv_color_t draw_buf_3_1[LV_HOR_RES_MAX * LV_VER_RES_MAX];            /*A screen sized buffer*/
    //    static lv_color_t draw_buf_3_2[LV_HOR_RES_MAX * LV_VER_RES_MAX];            /*An other screen sized buffer*/
    //    lv_disp_buf_init(&draw_buf_dsc_3, draw_buf_3_1, draw_buf_3_2, LV_HOR_RES_MAX * LV_VER_RES_MAX);   /*Initialize the display buffer*/
    ```
    同时注释掉下面屏幕尺寸赋值语句。之前我们已经在 lv_conf.h 中设置过了，下面语句的优先级最高即会覆盖掉 lv_conf.h 中的设置。但为了保持入口一致，我们统一在lv_conf.h中设置，这里的注释掉。
    ```
        /*Set the resolution of the display*/
    //disp_drv.hor_res = 240;
    //disp_drv.ver_res = 320;
    ```
3. 修改 `disp_flush()`。`LCD_Fast_DrawPoint`就是我们之前编写的画点函数。
    ```
    #include "lcd.h"

    static void disp_flush(lv_disp_drv_t * disp_drv, const lv_area_t * area, lv_color_t * color_p)
    {
        /*The most simple case (but also the slowest) to put all pixels to the screen one-by-one*/

        int32_t x;
        int32_t y;
        for(y = area->y1; y <= area->y2; y++) {
            for(x = area->x1; x <= area->x2; x++) {
                /* Put a pixel to the display. For example: */
                /* put_px(x, y, *color_p)*/
                LCD_Fast_DrawPoint(x,y,color_p->full);
                color_p++;
            }
        }

        /* IMPORTANT!!!
        * Inform the graphics library that you are ready with the flushing*/
        lv_disp_flush_ready(disp_drv);
    }
    ```
4. 触摸接口在 `lv_port_indev` 文件中，这里没用到，不添加。 
5. 添加定时器，设置为1ms定时。在中断函数中调用 `lv_tick_inc(1)`。注意这里的 "1" 必须和中断事件移植，中断间隔几ms，这个数字就是几。且只能取 1-10。
    ```
    #include "lvgl.h"
    /* 中断回调函数 */
    void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
    {
        lv_tick_inc(1);//lvgl的1ms心跳
    }
    ```
6. 编译，检查错误。主要就是头文件包含错误。注意在STM32CubeIDE中一定要将GUI文件夹设置源文件夹
![图 6](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/LVGL_GUI_Sr_Loc.png)  
其余头文件包含如下：
![图 5](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/LittleVGL_学习笔记/LVGL_%E5%A4%B4%E6%96%87%E4%BB%B6.png)  

7. 编写主函数，驱动例程。
    ```
    void test_screen(void)
    {
        lv_obj_t * btn = lv_btn_create(lv_scr_act(), NULL);     /*Add a button the current screen*/
        lv_obj_set_pos(btn, 10, 10);                            /*Set its position*/
        lv_obj_set_size(btn, 120, 50);                          /*Set its size*/

        lv_obj_t * label = lv_label_create(btn, NULL);          /*Add a label to the button*/
        lv_label_set_text(label, "Button");                     /*Set the labels text*/
    }

    void setup()
    {
        delay_init(84); /* 延时函数初始化 */
        __HAL_SPI_ENABLE(&hspi1);
        HAL_TIM_Base_Start_IT(&htim10); /* 使能定时器  */
        LCD_Init();//LCD初始化
        delay_ms(100);

        lv_init();				//lvgl系统初始化, 放在 LCD_Init() 后面
        lv_port_disp_init();	//lvgl显示接口初始化,放在lv_init()的后面
        test_screen(); /* 初始化界面后立即会显示 */

        run_times = millis();
    }



    void loop()
    {
        lv_task_handler();
    }
    ```
    `test_screen` 函数作用是创建一个带字符的按钮控件，主函数通过调用 `lv_task_handler()` 显示创建的按钮控件。
    loop 循环中一定不能有阻塞（延时函数），必须保证 `lv_task_handler()` 能够及时调用，或者放在中断中定时调用。

## LVGL快速入门


# LVGL应用

## Screen
[Screen transitions and display background](https://forum.lvgl.io/t/screen-transitions-and-display-background/2582)
[Multiple screen question #394](https://github.com/lvgl/lvgl/issues/394)
[The most efficient way to structure a multi screen LittlevGL project](https://forum.lvgl.io/t/the-most-efficient-way-to-structure-a-multi-screen-littlevgl-project/1680)
[button to change screen #93](https://github.com/lvgl/lvgl/issues/93)
[LittlevGL 切换界面的演示](https://blog.csdn.net/yunjie167/article/details/105488356)
[零知增强板-TFT扩展板 3.5寸 UI界面示例(lvgl)](http://www.lingzhilab.com/lzbbs/resources.html?ecid=454)



## LVGL优化

该优化测试实在 240*320 的屏幕测试的，测试显示按钮+屏幕背景刷新
```
void test_screen(void)
{
    lv_obj_t * btn = lv_btn_create(lv_scr_act(), NULL);     /*Add a button the current screen*/
    lv_obj_set_pos(btn, 10, 10);                            /*Set its position*/
    lv_obj_set_size(btn, 120, 50);                          /*Set its size*/

    lv_obj_t * label = lv_label_create(btn, NULL);          /*Add a label to the button*/
    lv_label_set_text(label, "Button");                     /*Set the labels text*/
}

void setup()
{
	delay_init(84); /* 延时函数初始化 */
	__HAL_SPI_ENABLE(&hspi1);
	HAL_TIM_Base_Start_IT(&htim10); /* 使能定时器  */
	LCD_Init();//LCD初始化
	delay_ms(100);

	lv_init();				//lvgl系统初始化
	lv_port_disp_init();	//lvgl显示接口初始化,放在lv_init()的后面
	test_screen(); /* 初始化界面后立即会显示 */
	run_times = millis();   // 记住显示前时间
	lv_task_handler();
	delay_ms(50);  // 延时50ms。等待显示完成
	run_times = (millis()-run_times-50);   // 得到显示时间
	LCD_ShowNum(50,100,run_times,5,16);
}


```
不同配置下显示该同一内容画面所消耗的时间、RAM、ROM对比。

### DMA+中断+单buf 
```
static lv_disp_buf_t disp_buf_1;
static lv_color_t buf_1[LV_HOR_RES_MAX * 10];                      /*A buffer for 10 rows*/
lv_disp_buf_init(&disp_buf_1, buf_1,  NULL,  LV_HOR_RES_MAX * 10);   /*Initialize the display buffer*/
```
- 耗时：67ms
- RAM占用 73.55%  
- FLASH占用 65.12%

**`LV_HOR_RES_MAX * 10` 改为 `LV_HOR_RES_MAX * 20`**
```
static lv_disp_buf_t disp_buf_1;
static lv_color_t buf_1[LV_HOR_RES_MAX * 20];                      /*A buffer for 10 rows*/
lv_disp_buf_init(&disp_buf_1, buf_1, NULL, LV_HOR_RES_MAX * 20);   /*Initialize the display buffer*/
```
- 耗时：55ms
- RAM占用  80.87%  
- FLASH占用 65.12%


### 双buf 
```
static lv_disp_buf_t disp_buf_1;
static lv_color_t buf_1[LV_HOR_RES_MAX * 10];                      /*A buffer for 10 rows*/
static lv_color_t buf_2[LV_HOR_RES_MAX * 10];
lv_disp_buf_init(&disp_buf_1, buf_1, buf_2, LV_HOR_RES_MAX * 10);   /*Initialize the display buffer*/
```
- 耗时：48ms
- RAM占用  80.87%  
- FLASH占用 65.12%

**`LV_HOR_RES_MAX * 10` 改为 `LV_HOR_RES_MAX * 20`**
```
static lv_disp_buf_t disp_buf_1;
static lv_color_t buf_1[LV_HOR_RES_MAX * 20];                      /*A buffer for 10 rows*/
static lv_color_t buf_2[LV_HOR_RES_MAX * 20];
lv_disp_buf_init(&disp_buf_1, buf_1, buf_2, LV_HOR_RES_MAX * 20);   /*Initialize the display buffer*/
```
- 耗时：42ms
- RAM占用  95.52%  
- FLASH占用 65.12%

### 减小RAM占用
修改以下内容对于显示时间几乎无影响，但可以大幅减小RAM暂用  
上述测试都是在下属设置中(lv_conf.h)
```
#  define LV_MEM_SIZE    (32U * 1024U)
```
下述为dma 单buffer的显示参数
```
#  define LV_MEM_SIZE    (32U * 1024U)

static lv_disp_buf_t disp_buf_1;
static lv_color_t buf_1[LV_HOR_RES_MAX * 10];                      /*A buffer for 10 rows*/
lv_disp_buf_init(&disp_buf_1, buf_1,  NULL,  LV_HOR_RES_MAX * 10);   /*Initialize the display buffer*/
```
- 耗时：67ms
- RAM占用 73.55%  
- FLASH占用 65.12%

修改 LV_MEM_SIZE 大小

```
#  define LV_MEM_SIZE    (8U * 1024U)
```
- 耗时：66ms
- RAM占用 36.05%  
- FLASH占用 65.12%

### 插件宏定义设为0
lv_conf.h设置不用的插件为0，速度无影响，内存增加
上述测试都是在下属设置中(lv_conf.h)
```
#define LV_USE_ARC      1
#define LV_USE_BAR      0
#define LV_USE_BTN      1
#define LV_USE_BTNMATRIX     0
#define LV_USE_CALENDAR 0
#define LV_USE_CANVAS   0
#define LV_USE_CHECKBOX       0
#define LV_USE_CHART    0
#define LV_USE_CONT     1
#define LV_USE_CPICKER   0
#define LV_USE_DROPDOWN    0
#define LV_USE_GAUGE    0
#define LV_USE_IMG      0
#define LV_USE_IMGBTN   0
#define LV_USE_KEYBOARD       0
#define LV_USE_LABEL    1
#define LV_USE_LED      0
#define LV_USE_LINE     1
#define LV_USE_LIST     0
#define LV_USE_LINEMETER   0
#define LV_USE_OBJMASK  0
#define LV_USE_MSGBOX     0
#define LV_USE_PAGE     0
#define LV_USE_SPINNER      0
#define LV_USE_ROLLER    0
#define LV_USE_SLIDER    0
#define LV_USE_SPINBOX       0
#define LV_USE_SWITCH       0
#define LV_USE_TEXTAREA       0
#define LV_USE_TABLE    0
#define LV_USE_TABVIEW      0
#define LV_USE_TILEVIEW     0
#define LV_USE_WIN      0
```
在上述配置下，下述为dma 单buffer的显示参数
```
#  define LV_MEM_SIZE    (32U * 1024U)

static lv_disp_buf_t disp_buf_1;
static lv_color_t buf_1[LV_HOR_RES_MAX * 10];                      /*A buffer for 10 rows*/
lv_disp_buf_init(&disp_buf_1, buf_1,  NULL,  LV_HOR_RES_MAX * 10);   /*Initialize the display buffer*/
```
- 耗时：67ms
- RAM占用 73.55%  
- FLASH占用 65.12%

如果将上述宏定义全部改为1（lvgl默认全为1）,显示参数为
- 耗时：67ms
- RAM占用 73.55%  
- FLASH占用 72.38%

**参考资料**
- [【LVGL学习之旅 01】移植LVGL到STM32](https://blog.csdn.net/qq_40831286/article/details/107633216)
- [STM32 移植 littlevGL GUI](https://www.jianshu.com/p/456fb71ec798)
- [lvgl最新版本在STM32上的移植使用](https://www.eet-china.com/mp/a40316.html)
- [stm32 DMA2D使用中断LVGL,提高LVGL帧率](https://www.codeleading.com/article/56705058674/)
- [LVGL 优化帧率技巧](https://blog.csdn.net/weixin_43862847/article/details/109318017)