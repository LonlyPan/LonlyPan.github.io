---
title: "HTML&CSS_设计与构建网站"
index_img: https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/HTML&CSS_设计与构建网站/HTML&CSS_设计与构建网站.jpg
date: 2019-12-14
updated: 2020-05-15
isbook: true
author: "[美]Jon Duckett"
translator: "刘涛、陈学敏"
press: "清华大学出版社"
publishdate: "2017-8"
readdate: "2019-12"
stars: "★★★★★" 
categories:  02-文学
---

系统讲述到了关于HTML和CSS的相关基础知识，运用大量图片代替枯燥的语言，加上精美的排版，不仅很好的讲述了这门设计语言，还能够激发初学者的阅读兴趣，同时配有大量实例进行练习，是一本对于初学者很友好的工具类丛书。

<!--more-->
https://xiaohuochai.site/JS/JS.html
[小火柴的前端小册子](https://xiaohuochai.site/)
[使用vue全家桶制作博客网站](https://www.cnblogs.com/xiaohuochai/p/9228543.html)
[CSS揭秘](https://book.douban.com/subject/26745943/)
[一个「学渣」从零Web前端自学之路](https://juejin.im/post/6844903778164949000)
[qianguyihao/Web](https://github.com/qianguyihao/Web)
https://yelog.org/2020/12/28/3-hexo-add-icon/
https://wujun234.github.io/01%20%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/01%20%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1%20%E7%BA%BF%E6%80%A7%E8%A1%A8%EF%BC%88List%EF%BC%89/
http://blog.geekaholic.cn/2017/02/22/%E5%88%B6%E4%BD%9CHexo%E4%B8%BB%E9%A2%98%E8%AF%A6%E7%BB%86%E6%95%99%E7%A8%8B%EF%BC%881%EF%BC%89/
https://esappear.github.io/clover/
https://www.bilibili.com/video/BV1Yr4y1P7hS/?spm_id_from=333.788.recommend_more_video.0
https://www.bilibili.com/video/BV1XV411n7Qx?p=1
https://www.youtube.com/watch?v=dtTWD0ystG0&list=RDCMUCkjoHfkLEy7ZT4bA2myJ8xA&index=19
https://www.youtube.com/watch?v=z_fcO7Iwfyk&list=PLwit80FmuM4P-0txNvOWw-7maU7yWl84I&index=7
https://www.youtube.com/watch?v=viEJQPVCoLU&list=PLLAZ4kZ9dFpOMJR6D25ishrSedvsguVSm&index=20
# 前言

这是自己学习《HTML&CSS设计与构建网站》一书的学习笔记，按照文章目录结构顺序，记录一些个人学习内容。
**书本简介：** 全书有大量例程和图示，很适合这种网页设计学习，且教学循序渐进，值得一看。

<!--more-->

## 本书的组织结构

书本一共分三部分：

### 1.HTML

- HTML语言负责**创建网页，划分整个界面的结构和章节。**
- 涉及**标签**和**元素**概念：用于告诉浏览器文本中那些是标题，那些是段落，那些是图片等等。
- 标签类型：文本、列表、链接、图像、表格、表单、Flash、视频、音频等。

### 2.CSS

- CSS语言负责**控制网页的显示样式和布局**，相当于美化排版。注意和HTML的区别：一个是为了能显示，一个是为了显示的更好看。
- 涉及到**属性**概念：
  - 表现属性：控制文本颜色和字体；设置字体大小、添加背景色及背景图像
  - 布局属性：控制**HTML中各元素**在屏幕的位置

>HTML语言也是有属性概念的：也可以控制文本的颜色，字体大小等，包括一部分排版功能，和CSS在功能上有些交叉。**但是：**  
HTML的属性只能作用域局部，而网页设计它的排版通常会涉及到很多重复内容（比如文章标题通常是一样格式，用HTML就得在每个标题内容中重复书写），这时CSS就相当于将所有HTML中相同的属性抽出来统一在一起。所以在学习HTML时，**侧重了解其便签概念，即显示内容上，只要做到能显示即可。**

### 3.实用信息

- 介绍HTML5中的新标签。
-  创建新站点应该遵循的涉及过程
-  如何将内容上传到网站、构建搜索引擎优化（SEO）以及分析工具来跟踪网站的访问者和访问目的。

## 如何访问网络

### 浏览器

访问网站时用到的软件称为WEB浏览器。比较流行的有Firefox、IE、Chrome、Safari和Opera。

### WEB服务器

是一种特殊的始终联网的计算机，专门用来托管网站，用来像请求者发送网页。

### 屏幕阅读器

用来将计算机屏幕上的内容读给用户的程序，用于具有视觉障碍的人。因此本书中涉及屏幕阅读器的提示，有助于确保使用此类软件得人也能访问你的网站。

## 如何创建网站

### 你所看到的

当你浏览网站时，浏览器会从托管此网站的WEB服务器上接受HTML和CSS，然后解释这些代码并渲染成你所看到的。（网页实际是一连串的代码，你看到的一切其实是浏览器将代码解释得图像和文字）。有的网站会用到JavaScript（不是Java，完全不同语言）或Flash，属于更高级别的内容，放在HTML和CSS之后学习。

### 如何创建它

小型网站使用HTML和CSS足够。更大、更复杂的会使用数据库来存储，并在服务器端使用如PHP、ASP.Net、Java或Ruby等编程语言。

### HTM5和CSS3

我们本书所要学习的内容，目前仍在开发中，但许多浏览器已经支持新特性。

## 网络访问机制

当你访问一个网站时，托管此网站的WEB服务器可能位于世界的任何位置，为了能够找到这台服务器位置，浏览器首先会连接到一个域名系统（DNS）服务器。  
1. 你通过Internet服务提供商（ISP）连接到网络，并通过浏览器输入域名或网址来访问网站。例如：google.com  
2. 然后你的计算机会连接到一个称为域名系统（DNS）的服务器网络。DNS类似于电话簿存储姓名（网址）对应的号码（IP地址：即存放你要访问的那个网站的Web服务器地址），然后告诉你的计算机， 
3. 返回的IP地址能让你电脑上的浏览器连接到所要访问的的WEB服务器。 
4. 最后，WEB服务器将请求的页面发送到你的浏览器中。浏览器解释代码，显示成网页形式。

# 第1章 结构
 
## 创建网页显示

你可以使用任何文本编译器，如：记事本、写字板。将上述代码复制过去，并保存为 **xxx.html**，确保**保存类型**选择**所有文件**。  

![add_html](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/HTML&CSS_设计与构建网站/add_html.png)

然后右键该文件，选择打开方式，选择浏览器打开，即可查看显示效果。

网页其实就像各种各样的文档、报纸、医疗保单及商品目录这些文档的电子版本。不论哪种文档，文档的结构都是非常重要的，它能帮助读者理解你所要传达的信息并对文档起到导航作用。而HTML正是对网页文档进行结构划分的语言。

HTML的 全称是 HyperText Markup Language（超文本标记语言）。 
**超文本**指的是 HTML 允许你通过建立链接，使访问者从一个页面跳转到另一个页面。  
**标记语言**允许你对文本进行注释即代码，然后浏览器借助这些代码将文本已一定格式正确显示出来。而我们添加的注释（即下文所述的标签）就是标记

## HTML描述页面的结构

HTML描述页面的结构与一般的文档结构是一个道理：使用标题和子标题反应出信息的层次性，段落用于编写正文。  
下面的代码中，白色字体是我们想要显示在屏幕上的内容，红色则是HTML代码，传递结构信息。

```html
<html>
  <body>
    <h1>这是主标题</h1>
    <p>本文可能是对本页其余部分的介绍。如果这一页很长，它可能会被分为几个小标题。</p>
    <h2>这是一个子标题</h2>
    <p>许多长篇文章都有子标题，以帮助您遵循正在编写的内容的结构。 
       甚至可能有子子标题（或更低级别的标题）。</p>
    <h2>另一个子标题</h2>
    <p>在这里，您可以看到另一个子标题。</p>
  </body>
</html>
```
**上面代码中：**  
- 左右尖括号 + 其中的代码 = **HTML标签**。
- 起始标签 + **内容** + 结束标签 = **HTML元素** 
- 结束标签 只比 起始标签 多个斜杠，标签不一定成对出现
- 元素用来传递**内容**的结构信息

>“标签” 和 “元素” 两个术语经常混用，但仍要注意区分

![页面结构](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/HTML&CSS_设计与构建网站/页面结构.png)

## 元素的特性

书中翻译为特性，但通俗的翻译为属性，我也习惯称之为属性。  

属性提供了有关**元素内容**的附加信息（字体、大小、颜色等）。出现在元素的起始标签中。

属性组成： **属性名称 + 属性值**，中间由等号 “=” 隔开。名称小写，属性值在双引号中。

```
  属性名  属性值
<p lang = "en-us" > paragraph in english </p>
|                 |
 --- 起始标签 ---
```
特性名称和特性值基本都是预定义好的。

上面的 “lang” 属性说明元素中使用的语言，后面的属性值 “en-us” 表示语言时美式英语。

只有少量的属性可以在所有的元素中使用，大部分属性是和元素绑定的，即只能某个元素使用。

## body、head和title

请自行输入下面代码，使用浏览器查看效果。

```html
<html>
	<head>
		<title>This is the Title of the Page</title>
	</head>
	<body>
		<h1>This is the Body of the Page</h1>
		<p>Anything within the body of a web page is displayed in the main browser window.</p>
	</body>
</html>
```
 **\<body>**

前面已经说过，这个元素中的所有内容都会显示在浏览器的主窗口中。

**\<head>**

在 \<body> 前面经常会看到 \<head> 元素，它包含有关这个页面的一些信息，是不显示在主页面的内容。主要是给浏览器分析用的。其中经常会出现 \<title> 元素。

**\<title>**

\<title> 元素中的内容要么显示在浏览器的顶端，即页面的 URL 地址栏的上方；要么显示在页面所在的标签页上。

![title](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/HTML&CSS_设计与构建网站/title.png)

## 小结

 - HTML页面就是文本文档
 - HTML使用标签为包含在其中的内容提供结构信息
 - 标签与元素经常互换使用
 - 起始标签可以附加属性，属性告诉我们更多有关于内容的信息
 - 学习HTML其实就是学习哪些标签可用，它们有什么作用以及用在何处。

# 第2章 文本

本章节详细介绍标签（标记）的使用，主要重点介绍：

 - 标题和段落
 - 粗体、斜体、强调
 - 结构化标记和语义化标记

## 标题

HTML标题一共有六个级别，h 即 heading 的首字母
 
![标题](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/HTML&CSS_设计与构建网站/标题.png)

虽然各标题显示有大小之分，但这是各浏览器自己的默认显示样式（其本身是无大小之分的）。所以其显示大小也因浏览器而异，后期还需要使用 CSS 来具体控制其大小。

## 段落

标签 \<p> 和 \</p>  之间的文字内容都属于一个段落。p 即 paragraph 的首字母。

![paragraphs](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/HTML&CSS_设计与构建网站/paragraphs.png)

默认情况下，浏览器在显示段落时会另起一行并于后续段落保持一定的距离。

## 空白

在文本内容中，我们通常会添加空格或换行；但在 HTML 中，如果有多个空格和一个换行，浏览器都会将其解释为**一个空格**；其存在的意义仅是为了增加代码的可读性，并不影响实际显示。所以不要试图使用空格来表缩进，用换行表示另起一行等等。

![bold](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/HTML&CSS_设计与构建网站/bold.png)

## 空元素

空元素指只含有单个标签的元素，注意斜线前有一个空格（没有也没事，编码习惯）。如：
- \<br />：表示回车
- \<hr />：表示水平线

## 引用

表示引用了一段或一句别人的话，名言。 \<blockquote> 表示一长段引用，通常会显示整段缩进。\<q> 表示短引用，之间的内容会自动添加双引号。

![quote](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/HTML&CSS_设计与构建网站/quote.png)

## 缩写词

缩写词表示这之间的文本是缩写形式，如果你使用 title 属性指定了原词，则当鼠标停留在该缩写词上时，浏览器会自动显示原词内容。  
\<abbr> 和 \<acronym> 完全相同，所以在 HTML5 中只保留使用 \<abbr> 

![abbr](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/HTML&CSS_设计与构建网站/abbr.png)

## 引文和定义

引文 \<cite> 表示引用一部作品，区别引用（引用其中的内容），通常斜体显示。定义 \<dfn> 表示一个新术语的定义，通常斜体。

![cite_dfn](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/HTML&CSS_设计与构建网站/cite_dfn.png)

## 其它元素

| 元素  | 含义 | 元素 | 含义 |
| ----- | ---- | ---- | ---- |
| \<b>   |  粗体    |   \<strong>   |   表示重要，有粗体效果   |
| \<i>   |   斜体   |   \<em>   |   强调，有斜体效果   |
| \<sup> |   上标   |  \<address>    |   设计者的信息   |
| \<sub> |   下标   |   \<ins>   |    插入的内容，带下划线  |
| \<del>   |  删除的内容，带删除线  |   \<s>  |   不准确或不应该却不应当删除的内容，带中间删除|

## 结构化标记和语义化标记

书中作者多次提起上述两个词，也做了解释和说明，但始终模棱两可，导致困惑，遂查资料了解。

**参考资料：**
- [对HTML语义化的一些理解和记录](https://juejin.im/post/5ae029bcf265da0b7155f15d)
- [Difference between Structural and Semantic markup anyone?](https://www.reddit.com/r/computerscience/comments/8cer6c/difference_between_structural_and_semantic_markup/)

综合上述两文的内容，结构化就是代表结构划分的标记，而语义化则表示通俗易懂的标记。在自己看来，并无区分的必要，都看作标记（标签）即可。  
深入的理解就是在日后的网页设计中，我们的 HTML 代码应更趋向于语义化，即能让人一眼就看明白这其中的内容属于什么结构和类型，而不是完全自定义。其实本章所提的标签基本都属于该范畴。

# 第3章 列表

本章节介绍三种列表的使用：
- 有序列表
- 无序列表
- 定义列表

## 有序列表

列表中的每个项目都按顺序标有标号，表示按一定步骤。  
注意凡是属于列表，显示上都会有一定的缩进，后同。

\<ol> 表示创建一个有序列表，\<li> 表示列表项目（一行内容）

显示效果见**无序列表**内。

## 无序列表

列表中的每个项目都以点状符号作为开头

\<ul> 表示创建一个无序列表

![list](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/HTML&CSS_设计与构建网站/list.png)

## 定义列表

定义专业术语的列表

\<dl> 表示创建一个定义列表，\<dt> 表示被定义的术语，\<dd> 表示被、具体定义。一个术语可以有多个定义，反之也可。

![dfnlist](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/HTML&CSS_设计与构建网站/dfnlist.png)

# 第4章 链接

链接允许你快速在网站或者网页之间跳转，达到网上冲浪的目的。本站将学习：

- 链接到其他网站
- 在页面之间建立链接
- 电子邮件链接

## 链接编写

链接是由 \<a> 元素建立的，其间添加属性 href 来指点链接的地址（属性及属性值要写在起始标签中），中间的内容是显示出来供读者点击实现跳转的。一般默认，该段文字会显示为蓝色并带有下划线。

链接的显示文字应尽量表达清楚要跳转到何处，而不至于让读者困惑。

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/HTML&CSS_设计与构建网站/link.png)

**在新窗口打开链接**

默认链接是在当前窗口刷新的，如果希望在新窗口打开，就需要添加 target 属性，并将属性值设为 \_blank.
```html
<a href = http://www.imdb.com" target = "_blank">Internet Movie Database</a>
```

## 指向其他网站的链接

如果希望添加其他网站的链接，只需要把属性值设为该网站的网址（域名）即可，如：http://wwww.xxx.com ，但记住一定要使用绝对地址。

## 同一网站中其他页面的链接

当链接时指向同一网站中其他页面或内容时，一般采用较短的相对地址（不需要网站和域名）。
相对地址同时对于在本地建立网站时很有用，不依赖于网络环境。

下面将详述何为相对地址。

### 目录结构

通常较大的网站，都会采用不同类别的文件夹来保存相应的网站文件。称之为目录。  
下图是一个网站的目录结构。

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/HTML&CSS_设计与构建网站/struture.png)

**结构**  
最顶端的文件夹称为根文件夹（图中examplearts文件夹），根文件夹包含了网站中其他所有的文件夹及文件。

**关系**  
文件与文件之间的关系是使用家谱关系的术语来描述的，如图中所示。


**主页**  
主页其实就是最主要的网页界面（首页则是其中最重要的一页，一般都是进入网站看到的第一页界面），主页一般是同一类型内容的引导界面，所以通常命名为 index.html。同一网站可以有不同的主页。

如果我们输入网站中某个文件夹的地址并进入，浏览器会优先打开该文件夹下的 index.html 文件并显示（如果指定了文件则打开指定文件）。所以当我们输入一个网站的网址时，其实是打开了根目录下的 index.html 文件。

### URL 

URL：Uniform Resource Locator（统一资源定位器，即地址）。每个网页都有自己的 URL。
- 绝对URL：域名 + 文件路径
- 相对 URL：只有文件路径

#### 相对URL

| 相对链接类型      | 示例（依据上图）                                                                           |
| ------------------------------- | -------------------------------------- |
| 相同的文件夹<br />要链接到同一文件夹内的文件，只需使用文件名（其余都不需要） | 从 music 主页链接到 music 板块中的 review 页面：<br /> \<a href = "review.html">Reviews</a>       |
| 子文件夹<br />对于子文件夹，使用子文件夹名后面加**正斜杠**再加文件名的形式   | 从网站主页链接到 music 板块中的 listings 页面：<br /> \<a href = "music/listings">Listings</a>    |
| 孙子文件夹<br />子文件夹名，加正斜杠，加孙子文件夹名，加正斜杠，再加文件名   | 从网站主页链接到 DVD 板块中的 review 页面：<br /> \<a href = "movies/dvd/review.html">Reviews</a> |
| 父文件夹<br />使用 ../ 来表示当前文件夹的上一级文件夹，然后再加上文件名      | 从music 板块中的 review 页面链接到网站主页：<br /> \<a href = "../index.html">Home</a>            |
| 祖父文件夹<br />重复 ../ 表示你要到达上面两级文件夹，然后加上文件名          | 从 DVD 板块中的 review 页面链接到网站主页：<br />\<a href = "../../index.html">Home</a>                |

## 邮件链接

邮件链接会当用户点击时，启动用户电脑上的邮件程序，并自动添加链接中指定的邮件地址为收件人。  
邮件链接只需要在 href 属性中再添加一个 mailto 属性值即可。

```html
<a href = "mailto:lonlypan@example.com">Email Lonlypan</a>
```

## 链接到特定位置

前面已经学习了如何跳转到指定网站或页面，那么如何在同一页面中实现跳转呢？最常见的就是标题之间的跳转。

这里我们需要用到 id 属性（可以用于所有的HTML元素中）。  
id 属性相当于一个标记，表明可以通过寻找这个标记就可以跳转到这里，其属性值可以自定义

有了标记后，我们就需要编写链接跳转到这里，同样需要使用 \<a> 元素和 href 属性。不同的是这次的 href 属性值是以 # 开头，后面紧跟要链接元素的 id 属性值。

下图中的示例，当用户点击 Top 时，就会跳转到文章开头位置。

```html
<h1 id="top">Film-Making Terms</h1>
<a href="#arc_shot">Arc Shot</a><br />
<a href="#interlude">Interlude</a><br />
<a href="#prologue">Prologue</a><br /><br />
<h2 id="arc_shot">Arc Shot</h2>
<p>A shot in which the subject is photographed by an encircling or moving camera</p>
<h2 id="interlude">Interlude</h2>
<p>A brief, intervening film scene or sequence, not specifically tied to the plot, that appears within a film</p>
<h2 id="prologue">Prologue</h2>
<p>A speech, preface, introduction, or brief scene preceding the the main action or plot of a film; contrast to epilogue</p>
<p><a href="#top">Top</a></p>
```

**链接到其他页面的特定位置**

其实就是页面地址 + 特定地址的组合形式。href 属性只要依次包含页面的地址（绝对或相对）、# 符号及目标元素的 id 属性值。

例如上面代码保存在一个网站的名为 test-posts 文件中，我们跳转到该文件的开头位置：

```html
<a href="http://www.xxx.com/test-posts/#top">
```

## 小结

- 使用 \<a> 元素创建链接，href 属性指定链接地址
- 网站内部跳转可使用相对链接
- 邮件链接可自动打开软件发送邮件
- id 属性可实现页面中元素间的跳转

# 第5章 图像

一图胜千言，好的图像可以更好的呈现网站内容，本章将学习：

- 如何添加图像
- 选择合适的图像格式
- 优化图像让页面加载更快

## 选择图像

以下列出一些开源且没有版权的网站地址，在这里提醒大家：在使用图像时，要有版权意识。

- [Unsplash](https://unsplash.com/)
- [pixabay](https://pixabay.com/)
- [pexels](https://www.pexels.com/)
- [pngimg](http://pngimg.com/)
- [piqsels](https://www.piqsels.com/)


## 添加图像

使用 \<img> 元素（空元素），当必须包含以下两个属性：

- src：图像的地址，相对或绝对 URL
- alt：图像内容说明，在图片无法显示时会用该文字替换（正常时不显示出来）,属性值可以留空，即空引号

示例见下文。

### 其它属性

**title**   
有关图像的附加信息，当用户将光标悬停在图像上时显示出来（正常时不显示出来）

**height**  
以像素为单位指定图像的高度

**width**  
以像素为单位指定图像的宽度

>指定图像尺寸，可以让浏览器加载时提前预留空间，从而继续显示其他文本
>建议使用 CSS 约束尺寸，统一的图像大小利于管理和风格统一

### 代码示例

注意 alt 和 title 属性值正常时不会显示出来。

![add_img](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/HTML&CSS_设计与构建网站/add_img.png)

## 在代码中插入图像

上述都只是图像单独占用一行内容显示的效果，如果将上述代码插入文本中的任意位置会有不同的效果。以下示例三个位置：

![img_side](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/HTML&CSS_设计与构建网站/img_side.png)

## 旧代码 align

指 HTML5 中不再支持的属性，而采用 CSS 代替。但一些网站或编译器可能仍然会采用

**align**  
控制页面其他部分相对图片的位置，有以下几个属性值
- left：图像左对齐，文本在右侧
- right：图像右对齐，文本在左侧
- top：文本的第一行与图像的顶端对齐
- middle：文本的第一行与图像中间对齐
- bottom：文本的第一行与图像底端对齐

**代码示例：**

![align](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/HTML&CSS_设计与构建网站/align.png)

## 创建图像的三条原则

**1. 使用正确的格式保存图像**  
  - jpeg：色彩丰富，有层次感，一般为自然景色
  - png：色彩单一，大面积同色，一般为几何图
  - gif：简单的插画（不要当视频使用），每一帧多余的图像都会增加文件大小增加加载时间

**2. 以正确的大小保存图像**  
根据网页显示的大小来设置图像的原始尺寸，过小会被拉伸变形，过大会增大加载时间。

**3. 以像素衡量图像（不是分辨率）**  
分辨率是会随着用户自己设备设置而不同，而像素是设备的最小基本单位，所以应使用像素值衡量图像的尺寸，而不是厘米或英寸为单位。

>在网页中使用图像时（对图像质量无太高要求），最好再使用压缩软件进行压缩，可以显著减小图片内存大小，同时质量也不会降低太多，从而加快加载速度

## 图像工具推荐

- [krita](https://krita.org/zh/)：开源免费，专业绘画软件，代替Ps
- [picpick](https://picpick.app/zh/)：个人免费，屏幕截图+编辑，有着强大的编辑功能，平时也作为简单图片编辑使用。
- [pngyu](https://pngquant.org/)：开源免费，png格式图片压缩，嵌入到博客文章里，提升加载速度。

具体请参考文章：[办公软件使用合集](https://lonlypan.com/archivers/办公软件使用合集)

# 第6章 表格


## 表格概述

HTML中，表格按照**行的顺序**逐行进行编写

## 表格基本结构

\<table>
创建表格，表示该标签内部都是表格。表格内容逐行编写

\<tr>
创建表格行，标签内部都属于一行。内含多个单元格

\<td>
创建单元格，标签内部是单元各内容

```c
		<table>
			<tr>
				<td>15</td>
				<td>15</td>
				<td>30</td>
			</tr>
			<tr>
				<td>45</td>
				<td>60</td>
				<td>45</td>
			</tr>
			<tr>
				<td>60</td>
				<td>90</td>
				<td>90</td>
			</tr>
		</table>
```

显示效果：
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/HTML&CSS_设计与构建网站/table_base.png)

\<th>
用法和 \<td> 一样，也会占用一个单元格，但其代表意义不同其表示行或列的标题（一般会粗体显示）。