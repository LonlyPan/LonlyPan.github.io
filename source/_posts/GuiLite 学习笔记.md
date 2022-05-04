---
layout: post
title: GuiLite 学习笔记
index_img: https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/GuiLite_学习笔记/logo.png
date: 2021-04-15
hide: false
# sticky: 100 #置顶，数字越大越靠前
# banner_img: #/img/post_banner.jpg
# comment: false
categories: 01-专业
---

**版本：** 
**平台：** windows 10   
**模拟平台：** Vscode  

官方github地址：[idea4good/GuiLite](https://github.com/idea4good/GuiLite)

<!--more-->

## 在vscode模拟运行

官方教程地址：[idea4good/GuiLitePreviewer](https://github.com/idea4good/GuiLitePreviewer)

1. Open VS Code，Click Extensions
2. Search "GuiLite" and install "guilite-previewer"
3. Download npm, and install
https://nodejs.org/en/download/
node包含npm，也可以使用命令行单独下载 npm。安装好后，重启vscode
4. 在Windows command或者vscode 终端 cd 进入 "guilite-previewer" 插件目录下，一般在 `C:\Users\23714\.vscode\extensions\niaoge.guilite-previewer-0.0.2`。 ??确保你在 “niaoge.guilite-previewer-0.0.2” 文件夹下??
5. 接着输入命令: `npm install`
6. 接着输入命令: `code .` 将使用 vscode 打开当前目录，Press F5 to build/debug the extension
7. You will get an new VS Code window with the extension you build above
8. In the new VS Code window, open your source code(e.g. test.cpp 这个文件是自己提前单独新建的，里面内容为空，就是一个文件夹)。一定要在当前新窗口打开（窗口的标题是：扩展开发宿主）。在文件中输入以下内容
```
WND_TREE s_main_widgets[] =
{
	{ &s_edit,		ID_EDIT,	"Input2",	275, 10, 100, 50},

	{ &s_label_1,	ID_LABEL_1,	"label 1",	150, 100, 100, 50},
	{ &s_label_2,	ID_LABEL_2,	"label 2",	150, 170, 100, 50},
	{ &s_label_3,	ID_LABEL_3,	"label 3",	150, 240, 100, 50},

	{ &s_button,	ID_BUTTON,	"Dialog",	400, 100, 100, 50},
	{ &s_spin_box,	ID_SPIN_BOX,"spinBox",	400, 170, 100, 50},
	{ &s_list_box,	ID_LIST_BOX,"listBox",	400, 240, 100, 50},

	{NULL, 0 , 0, 0, 0, 0, 0}
};
```
9. ctrl + shift + p, and input GuiLite: preview layout。回车运行
10. You will see your GUI layout in preview page

## STM32CubeIDE 移植

1. 你的项目一定要是基于C++。否则 GuiLite.h 中的class类定义无法编译通过。
在项目名上右键将项目转为 C++ 也不可行。必须重新建立工程，从一开始就选择 C++ 工程。
2. 下载 GuiLiteSamples/HelloStar/ 使用实例，将 `GuiLite.h` 和 `UIcode.c` 两个文件添加到工程。
3. 提前实现 delay_ms 函数。然后将 GuiLite.h 的`thread_sleep`函数前的 `extern "C"` 删除 ，最终结果如下：
    ```
    extern void delay_ms(unsigned short nms);
    void thread_sleep(unsigned int milli_seconds)
    {//MCU alway implemnet driver code in APP.
            delay_ms(milli_seconds);
    }
    ```
4. 删除 `UIcode.c`中 `startHelloStar`函数前的 `extern "C"`，最终结果如下：
    ```
    void startHelloStar(void* phy_fb, int width, int height, int color_bytes, struct EXTERNAL_GFX_OP* gfx_op) {
        create_ui(phy_fb, width, height, color_bytes, gfx_op);
    }
    ```
5. 提前实现底层画点函数。这个是你自己的LCD底层驱动，必须自己适配LCD编写好。
最终要给 GuiLite 调用，
    ```
    //快速画点
    //x,y:坐标
    //color:颜色
    void LCD_Fast_DrawPoint(u16 x,u16 y,u16 color)
    {
        LCD_SetCursor(x,y);		//设置光标位置
        LCD_WriteRAM_Prepare();	//开始写入GRAM
        LCD_WriteRAM(color);
    }
    ```
6. 在主函数添加代码。这里的  `startHelloStar()` 是一个使用例程函数（星空效果），里面有死循环，实际使用时请修改该函数底层代码跳出循环。
    ```
    //Transfer GuiLite 32 bits color to your LCD color
    #define GL_RGB_32_to_16(rgb) (((((unsigned int)(rgb)) & 0xFF) >> 3) | ((((unsigned int)(rgb)) & 0xFC00) >> 5) | ((((unsigned int)(rgb)) & 0xF80000) >> 8))
    //Encapsulate your LCD driver:
    void gfx_draw_pixel(int x, int y, unsigned int rgb)
    {
        LCD_Fast_DrawPoint(x, y, GL_RGB_32_to_16(rgb));  // 这个函数必须自己提前实现，底层LCD画点函数
    }
    //Implement it, if you have more fast solution than drawing pixels one by one.
    //void gfx_fill_rect(int x0, int y0, int x1, int y1, unsigned int rgb){}

    //UI entry
    struct EXTERNAL_GFX_OP
    {
        void (*draw_pixel)(int x, int y, unsigned int rgb);
        void (*fill_rect)(int x0, int y0, int x1, int y1, unsigned int rgb);
    } my_gfx_op;
    extern void startHelloStar(void* phy_fb, int width, int height, int color_bytes, struct EXTERNAL_GFX_OP* gfx_op);

    void setup() {

        /**************** 硬件初始化 *************/
        ......

        /**************** GuiLite 绘图开始 *************/
        //Link your LCD driver & start UI:
        my_gfx_op.draw_pixel = gfx_draw_pixel;
        my_gfx_op.fill_rect = NULL;//gfx_fill_rect;
        startHelloStar(NULL, 240, 320, 2, &my_gfx_op);
    }
    ```

### fill_rect 支持

fill_rect 是串口填充函数，如果是一个区块颜色相同下，使用DMA填充，速度会提升不少。
首先我们需要编写 DMA 填充函数。
```
void GuiLite_DMA_LCD_Fill(u16 xsta,u16 ysta,u16 xend,u16 yend,u16 color)
{

	static uint16_t color_temp[1] = {0};
	color_temp[0] = color;

	u32 num1;
	num1=(xend-xsta+1)*(yend-ysta+1);
	LCD_Address_Set(xsta,ysta,xend,yend);//设置显示范围
	LCD_SetCursor(xsta,ysta);      				//设置光标位置
	LCD_WriteRAM_Prepare();
	LCD_CS=0;

	HAL_SPI_DeInit(&hspi1);
	hspi1.Init.DataSize = SPI_DATASIZE_16BIT; /* 将spi设置为16位数据传输模式 */
	HAL_SPI_Init(&hspi1);
    if(num1 > 65536){
    	while(dma_flag == 0); //等待DMA传输完成
    	dma_flag = 0;
    	HAL_SPI_Transmit_DMA(&hspi1, (uint8_t *)color_temp, num1/2);
    	while(dma_flag == 0); //等待DMA传输完成
    	dma_flag = 0;
    	HAL_SPI_Transmit_DMA(&hspi1, (uint8_t *)color_temp, num1/2);

    }
    else{
    	while(dma_flag == 0); //等待DMA传输完成
    	dma_flag = 0;
    	HAL_SPI_Transmit_DMA(&hspi1, (uint8_t *)color_temp, num1);
    }
    while(dma_flag == 0); //等待DMA传输完成
	HAL_SPI_DeInit(&hspi1);
	hspi1.Init.DataSize = SPI_DATASIZE_8BIT;
	HAL_SPI_Init(&hspi1);

}
```
由于`HAL_SPI_Transmit_DMA`是16位的，所以如果满屏刷新  240x320 > 2^16，则需要分两次显示（屏幕小的则不需要）。每次传输后都必须等待完成，才能开启下一次传输。
>这里的等待可以优化，本次测试刷屏时间29ms，故不再优化，足够使用了。
>从这里也可以得出：虽然DMA传输有一个循环等待完成，阻塞了程序运行，但仅仅只是用DMA传输数据就已经比一般SPI传输的快很多了。

修改主函数：
```
//Transfer GuiLite 32 bits color to your LCD color
#define GL_RGB_32_to_16(rgb) (((((unsigned int)(rgb)) & 0xFF) >> 3) | ((((unsigned int)(rgb)) & 0xFC00) >> 5) | ((((unsigned int)(rgb)) & 0xF80000) >> 8))
//Encapsulate your LCD driver:
void gfx_draw_pixel(int x, int y, unsigned int rgb)
{
	LCD_Fast_DrawPoint(x, y, GL_RGB_32_to_16(rgb));
}
//Implement it, if you have more fast solution than drawing pixels one by one.
void gfx_fill_rect(int x0, int y0, int x1, int y1, unsigned int rgb){
	GuiLite_DMA_LCD_Fill(x0,y0,x1,y1,GL_RGB_32_to_16(rgb));
}

//UI entry
struct EXTERNAL_GFX_OP
{
	void (*draw_pixel)(int x, int y, unsigned int rgb);
	void (*fill_rect)(int x0, int y0, int x1, int y1, unsigned int rgb);
} my_gfx_op;
extern void startHelloStar(void* phy_fb, int width, int height, int color_bytes, struct EXTERNAL_GFX_OP* gfx_op);

void setup() {

	__HAL_SPI_ENABLE(&hspi1);
	HAL_TIM_Base_Start_IT(&htim10); /* 使能定时器  */
	LCD_Init();//LCD初始化
	delay_ms(100);

	//Link your LCD driver & start UI:
	my_gfx_op.draw_pixel = gfx_draw_pixel;
	my_gfx_op.fill_rect = gfx_fill_rect;  //NULL;//
	startHelloStar(NULL, 240, 320, 2, &my_gfx_op);
}
```

## GuiLite设计原理及代码注释

### 基本原理
GuiLite只作两个工作：界面元素管理和图形绘制。

**界面管理包括：**
- 添加/删除界面元素（例如：按钮，标签，对话框等控件），设置对应的文字及位置信息
- 用户输入消息传递：根据用户输入寻找受影响的界面元素，并回调响应的处理的处理函数
- 用户自定义消息传递：用户可以自定义消息响应函数，并自主产生消息；当消息产生时，对应的响应函数会被调用

**图形绘制包括：**

- 基本的点线绘制，例如：画点，矩形，横线，竖线等
- 设置绘制图层，如果需要多个图层，在基本点线绘制时，需要给出图层的索引值
- 图层处理，在图层界面发生变化的时候(例如：打开/关闭对话框)，GuiLite将决定各个图层上的像素点，哪个会被最终显示在屏幕上

?注意：图形绘制不依赖界面管理，可以独立的存在，例如，在资源有限的单片机环境，有时候不需要界面元素管理，而直接进行图形，文字的绘制。

### 扩展方法

GuiLite只给出了基本控件（例如：按钮，标签，键盘，选择框）的实现方法，旨在说明控件的实现方法。对于扩展控件，可以选择下面的方式：

- 如果开发者需要调整基本控件的细节，可以直接在源代码中修改
- 如果开发者需要构建全新的控件，可以参考基本控件的实现方法，重新实现

对于扩展绘制，例如：画圆，画曲线，可以直接在surface.cpp文件中添加响应的函数接口。

### 代码目录结构

>?注意：
Guilite.h 文件已经包含以下所有文件夹及其下面的内容。我们在使用时，只需要包含该头文件即可。下面的文件夹只是 Guilite.h 里代码的分类代码，仅用来便于阅读和理解。

**core:**
- 实现了底层绘制，图层管理和消息传递
- adapter实现了各个平台（例如：Windows, Linux，Android，iOS，macOS,未知OS或无OS）的封装。

**widgets:**
- 实现了各种常规控件（例如：按钮，标签，键盘，波形）及容器（例如：视窗，对话框，滑动页面），开发者可以根据自己的需要，直接在相应的代码上进行修改或重绘，开发出有自己风格，特色的界面
- 实现了用户的手势识别（例如：手指滑动，鼠标按下/释放）的消息传递，将用户的输入信息传递到整个GUI体系树中，并调用相应的响应回调函数；开发者可以根据自己的需要添加/修改响应回调函数。

**参考资料**
- [GuiLite 3.3 发布：要灵活扩展，还要简单粗暴](https://www.oschina.net/news/115945/guilite-3-3-released)