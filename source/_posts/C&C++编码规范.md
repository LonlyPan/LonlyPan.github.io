---
layout: post
title: "C&C++编码规范"
index_img: https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/C&C++编码规范/C_C++.png
data: 2019-9-3 15:15
updated: 2022年05月08日 14时32分34秒
hide: false
# sticky: 100 #置顶，数字越大越靠前
# banner_img: #/img/post_banner.jpg
# comment: false
categories: 01-专业
---

## 自己需要改正的地方

* 建议使用 `/*...*/` 进行注释，多行单行都适用
* 全局变量应增加”g_“前缀，静态增加 `s_` 前缀，下划线连接
* 定义函数时左花括号”{“单独占一行，其他的 `if`、`while`、`for` 等语句建议紧随其后
* 组成 `switch、while、do...while` 或 `for` 结构体的语句应该是复合语句。即使该复合语句只包含一条语句也要扩在 `{}`里
* `const、virtual、inline、case` 等关键字之后至少要留一个空格；函数名之后不要留空格，紧跟左括号 `(`，以与关键字区别
<!--more-->

>**具体使用规则参考文后的使用示例**

## 版本信息

**version：** V0.3.1  
**date：**  2021-02-03  
**Modification：**    
- 部分内容优化

**version：** V0.3  
**date：**  2020-04-13  
**Modification：**    
- 修改代码示例

**version：** V0.2  
**date：**  2020-04-08  
**Modification：**    
- 删除一些冗余、繁杂规范
- 修改部分规范

## 目录结构
**建议将工程按照功能模块划分子目录，子目录再定义头文件和源文件目录**

## 设计原则
* 为了防止头文件被重复引用，应当用`ifndef/define/endif`结构产生预处理块
* 用 `#include <...>` 格式来引用标准库的头文件，用 `#include "..."` 引用非标准库的头文件
* 头文件应当职责单一
* **头文件中适合放置接口的声明，不适合放置实现**
  - 类的 public 函数若小于 20行，且是重要函数，可选择写在 `.h` 文件里  
* **不要在头文件中定义变量**
* 建议每一个`.c`文件应有一个同名`.h`文件，用于声明需要对外公开的接口
* **一个模块通常包含多个`.c`文件，建议放在同一个目录下，目录名即为模块名；如果一个模块包含多个子模块，则建议每一个子模块提供一个对外的`.h`，文件名为子模块名**
* 禁止循环依赖
* 禁止包含用不到的头文件
* 只能通过头文件.h使用`extern`
* 禁止在`extern "C" `包含头文件

## 版权声明
 * 头文件版权声明一致，放在头文件置顶位置
 * 如果提交的代码是在开源软件基础上修改所编写或衍生的代码，请遵循开源许可协议要求，并且已完成履行被修改软件的许可证义务
 * MajorVersion 主版本号：从 0 开始的，从 alpha（0）到 beta（1）一直到现在的正式版本，主版本号表示一个全新的框架版本，例如：重大的重构、重大的feature改动、重大的不兼容性的变化，但往往不会发生；
 * MinorVersion 子版本号：发布较大的新feature功能，或者较大的重构或者模块变化，或者出现不兼容性改动，会增加子版本号；子版本的发布会伴随着完整的change log，算是一个较大的版本发布，有仪式感；
 * Revision 修正版本号：往往是bug fix，或者增加较小的feature，较小的功能改进或者模块变化，在保证完整向后兼容的前提下，会增加修正版本号；
 * 当主版本号增加时，子版本号及修正版本号置0；
 * 当子版本号增加时，修正版本号置0

## 命名规则
* 版本命名规则： MajorVersion . MinorVersion . Revision 即： 主版本号 . 子版本号 . 修正版本号  如： v0.0.1, v1.1.0, 1.7.1
* 通用规则：**unix like风格，单词小写，用下划线连接，例如：text_mutex。**
* 不使用缩写和汉语拼音
* 避免名字中出现数字编号
* 标识符前不应添加模块、项目、产品、部门的名称作为前缀
* **全局变量应增加"g_"前缀，静态增加"s_"前缀，指针增加"p_"前缀**
*   注意在局部作用域中（单个文件内全局访问）的“全局变量”应视为局部变量，且建议使用静态前缀"s_"加以限定。
* 类名、结构体、类型定义（typedef）首字母大写，其余小写，下划线连接
* 类的数据成员在通用规格上后接下划线”_“，这样可以避免数据成员与成员函数的参数同名。
* **宏、数值、字符串、枚举等等常量的定义，建议采用全大写字母，使用下划线"_"连接**
* 禁止单字节变量，除局部循环使用
* 不建议使用匈牙利命名法
* **除了头文件或编译开关等特殊表示定义，宏定义不能使用下划线"_"开头和结尾**

## 函数
* **重复代码应该尽可能提炼成函数**
* 避免函数过长，**新增函数不超过40-50行**
* 避免函数的代码块嵌套过深,不超过5级
* **函数应避免使用全局变量、静态局部变量和 I/O 操作，不可避免的地方应集中使用**
* 设计高扇入（被其它函数调用），合理扇出（调用其它函数）（小于7）的函数
* **不变参数使用`const`**
* 函数参数不超过5个
* **除打印类函数外，不要使用可变长参函数**
* **在源文件范围内声明和定义的所有函数，除非外部可见 ，都应增加`static`关键字**
* **不建议使用递归函数调用**
* 用正确的反义词组命名具有互斥意义的变量或相反动作的函数等
* 在多重循环中，应将最忙的循环放在最内层

## 变量
* **一个变量只有一个功能，不要把一个变量用作多种用途**
* **不用或者少用全局变量**
* **在首次使用前初始化变量**
* **使用面向接口编程思想，通过API访问数据；若本模块数据需要对外开放，应提供接口函数来设置、获取**
* 初始化变量离使用地方越近越好
* 明确全局变量初始化顺序
* **尽量减少没有必要的数据类型默认转换与强制转换**

## 宏、常量
* **宏定义表达式时，使用完备的括号**
* 将宏定义的多条表达式放在大括号中。使用`do-while` 结构
* **不允许直接使用魔鬼数字**
* 除非必要，应尽可能使用函数代替宏,除了`do-while` 结构
* **常量建议使用`const`代替宏**
* 将多次被调用的“小函数”改为`inline`函数或者宏实现。

## 注释
* 注释风格要统一，注释内容要清楚、明了、含义准确、防止注释二义性
* 建议使用"/*...*/"进行注释，多行单行都适用
* **注释应放在起代码上方相邻位置或右方**
* 避免在注释中使用缩写，除非是业界通用或子系统内标准化的缩写
* **文件头部要注释，建议注释列出：版权说明、版本号、生成日期、作者姓名、功能说明、与其他文件的关系、修改日志等，头文件的注释还应有函数功能的简要说明。**
* 在代码的功能、意图层次上进行注释，即**注释解释代码难以表达的意图，而不是仅仅重复描述代码**
* **函数声明处注释函数的功能、性能即用法，包括输入输出函数、函数返回值、可重入的要求等；定义处详细描述函数功能和实现要点，如实现的简要步骤、实现的理由、设计约束等**
* **对于函数名能够自解释的，可以不写注释。**
* 在程序块的结束行右方加注释标记，以表明某程序块的结束。
* **全局变量要有详细的注释，包括对其功能、取值范围以及存取时注意事项等的说明**

## 排版与格式
* 程序块采用缩进风格编写，**每级缩进为4格空格,不要使用tab键**
* **相对独立的程序块之间、变量说明之后必须加空行**
* 一条语句不能过长 如必能拆分需要分行写
* 一行只写一条语句
* if、for、do、while、case、switch、default等语句独占一行
* 源程序中关系较为紧密的代码尽可能相邻
* 注释符(包括„/*‟„//‟„*/‟)与注释内容之间要用一个空格进行分隔
* **定义函数时左花括号”{“单独占一行，其他的`if`、`while`、`for`等语句建议紧随其后**
* **组成switch、while、do...while 或for 结构体的语句应该是复合语句。即使该复合语句只包含一条语句也要扩在{}里**
* **const、virtual、inline、case 等关键字之后至少要留一个空格；函数名之后不要留空格，紧跟左括号‘（’，以与关键字区别**
* 逗号、分号只在后面加空格
* **二元操作符：“=”、“+=” “>=”、“<=”、“+”、“\*”、“%”、“&&”、“\|\|”、“<<”,“^”等的前后应加空格**
* **一元操作符：“!”、“~”、“++”、“--”、“&”（地址运算符）等前后不加空格**
* if、for、while、switch等与后面的括号间应加空格，使if等关键字更为突出、明显

## 可移植性
* 不使用与硬件或操作系统关系很大的语句，而使用建议的标准语句，以提高软件的可移植性和可重用性

## 业界编程规范
* C语言编程规范参考资料较多，大家可以自行了解，不再赘述。

## 编写示例

### 常见单词缩写表

|单词|缩写|单词|缩写|
|------------|-----------|--------|----------------|
|  argument  |  arg  | buffer   | 	buf   |
| clock | 	clk  | command  | 	cmd   |
|  compare  |  cmp  | configuration   | 	cfg   |
|  device |  dev  |  error  | 	err  |
|  hexadecimal  |  	hex  | increment  | 	inc  |
|  initialize  |  init  | 	maximum  | 	max   |
|  message  |  msg  | 	minimum  | min   |
|  parameter  |  param  | 	previous  | 	prev   |
|  register  |  reg  | 	semaphore  | 	sem   |
|  statistic  |  stat  | 	synchronize  | 	syn  |
|  temp  |  tmp  | 	  | 	   |


### 常用缩写及含义

```
@file              文件名
@author            作者
@brief             简要说明
@param             用于函数说明中，后面接参数名字，然后再接关于该参数的说明
@retval            对函数返回值进行说明
@note              注解信息
@attention         注意信息
@warning           警告信息
@see               表示交叉参考
@TODO              计划内容
```

### 头文件

```c
/**
  * Copyright (c)   LonlyPan . 1998-2011.  All rights reserved.  // 1998为创建日期，2011为最新文件修改日期
  * @file:    lcdmenu.c
  * @author:  LonlyPan
  * @version: V0.1
  * @date:    2019-09-04
  * @brief:   Header file of xxx module  // 用于详细说明此程序文件完成的主要功能，与其他模块  
  *                                     // 或函数的接口，输出值、取值范围、含义及参数间的控
  *                                     // 制、顺序、独立或依赖等关系
  * @attentio:        注意事项
  * @Modification:    本次版本修改添加了xxxx
  * @History:         // 修改历史记录表
  *   1.Version: 历史版本号
  *   Author:
  *     date: 历史版本日期
  *     Modification: 历史版本修改内容
  *   2.Version: 历史版本号
  *   ......
  */
#ifndef __XXX_XX_H  /* 定义以防止递归包含  */
#define __XXX_XX_H

#include <math.h>       /* 引用标准库的头文件 */
...
#include “myheader.h”   /* 引用非标准库的头文件，若有重复，优先使用用户目录下的 */

/* 禁止在extern "C" 包含头文件 */
#ifdef __cplusplus
extern "C" {   
#endif

class Box               /* 类声明，首字母大写 */
{
  private: /* 缩进两格 */
    /* 数据成员最好全私有，使用函数访问修改 */
    string table_name_;  /*  缩进两格，后加下划线. */
    
  public:
   /**
   * @brief     xxx  // 简介、功能
   * @note   // 注意事项
   * @param xxx // 参数
   * @retval     xxx  // 返回值
   */
   /* 函数声明处注释函数的功能、性能及用法，包括输入输出函数、函数返回值、可重入的要求等 */
    void set_table_name();  // 通用该规则命名  
    ...
};

#define PI 3.14   // 全大写
const int MI = 1356;  // 全大写
extern int day_number  = 0;  // 通用规则

/* 使用完备的 {...} ，并使用 do...while 结构 */
#define FOO(x) do{ \         /* 尽量少用define、使用const代替define */
        printf("arg is %s\n", x); \
        do_something_useful(x); \
} while(0)


#if defined(xxx)
...
#endif  /* xxx */


#ifdef __cplusplus
}
#endif  /* __cplusplus */
 
#endif /* __XXX_XX_H */
```

### 源文件

```c++
/** 和源文件相同，但删除版本记录
  * Copyright (c)   LonlyPan . 1998-2011.  All rights reserved.  // 1998为创建日期，2011为最新文件修改日期
  * @file： lcdmenu.c
  * @author： LonlyPan
  * @version： V0.1
  * @date：  2019-09-04
  * @brief：  Source file of xxx module  // 用于详细说明此程序文件完成的主要功能，与其他模块  
  *                                     // 或函数的接口，输出值、取值范围、含义及参数间的控
  *                                     // 制、顺序、独立或依赖等关系
  */
#include <math.h>       // 引用标准库的头文件
...
#include “myheader.h”   // 引用非标准库的头文件

/* 禁止在extern "C"包含头文件 */
#ifdef __cplusplus
extern "C" {
#endif

int day_number  = 0;  // 通用规则

/**
 * @brief   // 简介、功能
 * @note   // 注解信息
 * @attention // 注意事项
 * @detail  // 细节
 */
/* 定义处详细描述函数功能和实现要点，如实现的简要步骤、实现的理由、设计约束等 */
void function1(char *dest,const char *src)  /* 全小写，下划线连接，指针仅作输入用，加const */
{
    ...
}

#ifdef __cplusplus
}
#endif  /* __cplusplus */
```

### 全局变量

全局变量要有较详细的注释，包括对其功能、取值范围、哪些函数或过程存取它以及
存取时注意事项等的说明。

```c 
/* @brif The ErrorCode when SCCP translate */  // 变量作用、含义
/* @value Global Title failure, as follows */ 
/* 0 － SUCCESS 1 － GT Table error */  // 变量取值范围
/* 2 － GT error Others － no use */  
/* only function SCCPTranslate() in */ // 使用方法
/* this modual can modify it, and other */
/* module can visit it through call */
/* the function GetGTTransErrorCode() */ 
BYTE g_GTTranErrorCode;
```

### 反义词组

|  词组  |  词组   |    词组  |
|--------------------|----------------|-------------------------|
|  add / remove  | begin/end  |  create / destroy  |
|  insert / delete  |  first / last  |  get / release  |
|  increment / decrement  |  put / get  |  add / delete  |
|  lock / unlock  |  open / close  | min / max  | 
|  old / new  |  start / stop  | next / previous  |
|  source / target  |  show / hide  | send / receive  | 
| source / destination  | cut / paste  |  up / down  |

示例：
```
int min_sum;  
int max_sum;  
int add_user( BYTE \*user_name );  
int delete_user( BYTE \*user_name );  
```

## 参考文档

1、[华为-C语言编程规范](http://discourse-production.oss-cn-shanghai.aliyuncs.com/original/3X/f/6/f6453c68ba08158a78363c4379c3131bb92f3a1f.pdf)  
2、[高质量C++/C编程指南](https://www.jianshu.com/p/654fea4bfd4f)  
3、[Google 开源项目风格指南 (中文版)](https://zh-google-styleguide.readthedocs.io/en/latest/)  
4、[UNIX 风格的命名习惯](https://blog.csdn.net/querw/article/details/5467438)
5、[嵌入式C语言编程规范（个人规约）](https://blog.csdn.net/zhanglianpin/article/details/46544431)  

## Github 垃圾代码书写准则

在 GitHub 上有一个新项目，它描述了「最佳垃圾代码」的十九条关键准则。从变量命名到注释编写。这些准则将指导你写出最亮眼的烂代码。

为了保持与原 GitHub 项目一致的风格，下文没有进行转换。读者们可以以**相反的角度来理解所有观点**，这样就能完美避免写出垃圾代码。

这是一个你的项目应该遵循的垃圾代码书写准则的列表：


### 💩 以一种代码已经被混淆的方式命名变量
如果我们键入的东西越少，那么就有越多的时间去思考代码逻辑等问题。

Good 👍🏻

     let a = 42;
	 
Bad 👎🏻

    let age = 42;
	
### 💩 变量/函数混合命名风格
为不同庆祝一下。

Good 👍🏻

     let wWidth = 640;
     let w_height = 480;

Bad 👎🏻

     let windowWidth = 640;
     let windowHeight = 480;
### 💩 不要写注释
反正没人会读你的代码。

Good 👍🏻

     const cdr = 700;
	 
Bad 👎🏻

更多时候，评论应该包含一些“为什么”，而不是一些“是什么”。如果“什么”在代码中不清楚，那么代码可能太混乱了。

     // 700ms的数量是根据UX A/B测试结果进行经验计算的。
     // @查看: <详细解释700的一个链接>
     const callbackDebounceRate = 700;
### 💩 使用母语写注释
如果您违反了“无注释”原则，那么至少尝试用一种不同于您用来编写代码的语言来编写注释。如果你的母语是英语，你可能会违反这个原则。

Good 👍🏻

     // Закриваємо модальне віконечко при виникненні помилки.
     toggleModal(false);
	 
Bad 👎🏻

     // 隐藏错误弹窗
     toggleModal(false);
### 💩 尽可能混合不同的格式
为不同庆祝一下。

Good 👍🏻

     let i = ['tomato', 'onion', 'mushrooms'];
     let d = [ "ketchup", "mayonnaise" ];
	 
Bad 👎🏻

     let ingredients = ['tomato', 'onion', 'mushrooms'];
     let dressings = ['ketchup', 'mayonnaise'];
### 💩 尽可能把代码写成一行
Good 👍🏻

     document.location.search.replace(/(^\?)/,'').split('&').reduce(function(o,n){n=n.split('=');o[n[0]]=n[1];return o},{})

Bad 👎🏻

     document.location.search
       .replace(/(^\?)/, '')
       .split('&')
       .reduce((searchParams, keyValuePair) => {
         keyValuePair = keyValuePair.split('=');
         searchParams[keyValuePair[0]] = keyValuePair[1];
         return searchParams;
       },
       {}
     )
### 💩 不要处理错误
无论何时发现错误，都没有必要让任何人知道它。没有日志，没有错误弹框。

Good 👍🏻

     try {
       // 意料之外的情况。
     } catch (error) {
       // tss... 🤫
     }

Bad 👎🏻

     try {
       // 意料之外的情况。
     } catch (error) {
       setErrorMessage(error.message);
       // and/or
       logError(error);
     }
### 💩 广泛使用全局变量
全球化的原则。

Good 👍🏻

     let x = 5;

     function square() {
       x = x ** 2;
     }

     square(); // 现在x是25

Bad 👎🏻

     let x = 5;

     function square(num) {
       return num ** 2;
     }

     x = square(x); // 现在x是25
### 💩 创建你不会使用的变量
以防万一。

Good 👍🏻

     function sum(a, b, c) {
       const timeout = 1300;
       const result = a + b;
       return a + b;
     }

Bad 👎🏻

     function sum(a, b) {
       return a + b;
     }
### 💩 如果语言允许，不要指定类型和/或不执行类型检查。
Good 👍🏻

     function sum(a, b) {
       return a + b;
     }

     // 在这里享受没有注释的快乐
     const guessWhat = sum([], {}); // -> "[object Object]"
     const guessWhatAgain = sum({}, []); // -> 0

Bad 👎🏻

     function sum(a: number, b: number): ?number {
       // 当我们在JS中不做置换和/或流类型检查时，覆盖这种情况。
       if (typeof a !== 'number' && typeof b !== 'number') {
         return undefined;
       }
       return a + b;
     }

     // 这个应该在转换/编译期间失败。
     const guessWhat = sum([], {}); // -> undefined
###  💩 你应该有不能到达的代码
这是你的 "Plan B".

Good 👍🏻

     function square(num) {
            if (typeof num === 'undefined') {
    return undefined;
       }
       else {
         return num ** 2;
       }
       return null; // 这就是我的"Plan B".
     }

Bad 👎🏻

     function square(num) {
       if (typeof num === 'undefined') {
         return undefined;
       }
       return num ** 2;
     }
### 💩 三角法则
就像鸟巢，鸟巢，鸟巢。

Good 👍🏻

     function someFunction() {
       if (condition1) {
         if (condition2) {
           asyncFunction(params, (result) => {
             if (result) {
               for (;;) {
                 if (condition3) {
                 }
               }
             }
           })
         }
       }
     }

Bad 👎🏻

     async function someFunction() {
       if (!condition1 || !condition2) {
         return;
       }
  
       const result = await asyncFunction(params);
       if (!result) {
         return;
       }
  
       for (;;) {
         if (condition3) {
         }
      }
     }
### 💩 混合缩进
避免缩进，因为它们会使复杂的代码在编辑器中占用更多的空间。如果你不喜欢回避他们，那就和他们捣乱。

Good 👍🏻

     const fruits = ['apple',
       'orange', 'grape', 'pineapple'];
       const toppings = ['syrup', 'cream', 
                         'jam', 
                         'chocolate'];
     const desserts = [];
     fruits.forEach(fruit => {
     toppings.forEach(topping => {
         desserts.push([
     fruit,topping]);
         });})

Bad 👎🏻

     const fruits = ['apple', 'orange', 'grape', 'pineapple'];
     const toppings = ['syrup', 'cream', 'jam', 'chocolate'];
     const desserts = [];

     fruits.forEach(fruit => {
       toppings.forEach(topping => {
         desserts.push([fruit, topping]); 
       });
     })
### 💩 不要锁住你的依赖项
以非受控方式更新每个新安装的依赖项。为什么坚持使用过去的版本，让我们使用最先进的库版本。

Good 👍🏻

     $ ls -la

     package.json

Bad 👎🏻

     $ ls -la

     package.json
     package-lock.json
### 💩 函数长的比短的好
不要把程序逻辑分成可读的部分。如果IDE的搜索停止，而您无法找到所需的文件或函数，该怎么办?

- 一个文件中10000行代码是OK的。

- 一个函数体有1000行代码是OK的。

- 在一个' service.js ' 中处理许多服务(第三方库和内部库、一些工具、手写的数据库ORM和jQuery滑块)? 这是OK的。

### 💩 不要测试你的代码
这是重复且不需要的工作。

### 💩 避免代码风格统一
编写您想要的代码，特别是在一个团队中有多个开发人员的情况下。这是“自由”原则。

### 💩 构建新项目不需要 README 文档
一开始我们就应该保持。

### 💩 保存不必要的代码
不要删除不用的代码，最多注释掉。

>地址：
https://github.com/trekhleb/state-of-the-art-shitcode


