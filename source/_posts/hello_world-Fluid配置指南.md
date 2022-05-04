---
title: Hello World & Fluid 配置指南
index_img: https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Hello_World_&_Fluid_配置指南/hexo+fluid.png
date: 2021-06-12 03:14:33
hide: false
# sticky: 100 #置顶，数字越大越靠前
# banner_img: #/img/post_banner.jpg
# comment: false
categories: 03-瞎折腾
---

---
title: "未命名文件"
index_img:  #/img/example.jpg
date: 2022-05-04 14
hide: false
# sticky: 100 #置顶，数字越大越靠前
# banner_img: #/img/post_banner.jpg
# comment: false
categories: [00-项目,01-专业,02-文学,03-瞎折腾]
---


fluid官网文档：[Fuild 配置指南](https://hexo.fluid-dev.com/docs/guide/)  
hexo官网文档：[hexo文档](https://hexo.io/zh-cn/docs/)

**常用配图资源**
- [wallpaperhub](https://wallpaperhub.app/)
- [Wallhaven](https://wallhaven.cc/)
- [Unsplash](https://unsplash.com/)
 
# HEXO指令

更多指令：[hexo指令](https://hexo.io/zh-cn/docs/commands.html)
## 新建一篇博客文章

``` bash
$ hexo new "My New Post"
```
也可以直接在文件夹下新建 .md 文档。  
More info: [Writing](https://hexo.io/docs/writing.html)

## 启动服务
一般是启动本地服务器预览。远程服务器端不需要执行该指令。
``` bash
$ hexo server
或者
$ hexo s
```
More info: [Server](https://hexo.io/docs/server.html)

## 生成静态文件

hexo的机制是将 .md  文档渲染成 html 文档，提供给浏览器访问。所以如果不执行这个命令，网站的内容就会保持原样。  
所以每次有新文章发布，都需要执行该指令。
``` bash
$ hexo generate
或者
$ hexo g
```
More info: [Generating](https://hexo.io/docs/generating.html)

## 部署到远程站点
执行该命令执行前，请执行`hexo g`指令。
``` bash
$ hexo deploy
或者
$ hexo d
```
也可使用组合命令，生成加部署。
``` bash
$ hexo g -d
```
More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)

# 搭配 Fluid 优雅的写一篇文章

## 头部信息

```
---
layout: about
title: 文章标题
index_img: /img/example.jpg
banner_img: /img/post_banner.jpg
date: 2019-10-10 10:00:00
comment: 'valine'
hide: true
sticky: 100
categories: [Hexo, Fluid]
---
以下是文章内容
```
- **layout**布局
可不写，默认使用post布局
- **index_img:**  文章在首页的封面图
 /img/example.jpg 对应的是存放在 /source/img/example.jpg 目录下的图片（目录也可自定义，但必须在 source 目录下）。
也可以使用外链 Url 的绝对路径。
如果想统一给文章设置一个默认图片（文章不设置 index_img 则默认使用这张图片），可在主题配置中设置：
	```
	post:
	  default_index_img: /img/example.jpg
	```
	当 default_index_img 和 index_img 都为空时，该文章在首页将不显示图片。
	>文档首页封面推荐大小 540x320px
	
- **banner_img** 文章页顶部大图
默认显示主题配置中的 post.banner_img，如需要设置单个文章的 Banner，在 Front-matter (opens new window)中指定 banner_img 属性。
本地图片存放位置同上。
>文档首页封面推荐大小 540x320px
- **comment** 关闭评论
如果想在某个文章页关闭评论，或者想在某个自定义页面开启评论，可以通过在 Front-matter (opens new window)设置 comment: bool 来控制评论开关，或者通过 comment: 'type' 来开启指定的评论插件。
例如在关于页开启并指定评论插件：
	```
	---
	title: 关于页
	comment: 'valine'
	---
	以下是正文内容
	```
- **hide: true** 隐藏文章
如果想把某些文章隐藏起来，不在首页和其他分类里展示，可以在文章开头 Front-matter (opens new window)中配置 hide: true 属性。
	>TIP
	隐藏会使文章在分类和标签类里都不显示
	隐藏后依然可以通过文章链接访问
- **sticky: 100** 文章排序
如果想手动将某些文章固定在首页靠前的位置，可以在文章开头 Front-matter (opens new window)中配置 sticky 属性
sticky 数值越大，该文章越靠前，达到类似于置顶的效果，其他未设置的文章依然按默认排序。
当文章设置了 sticky 后，主题会默认在首页文章标题前增加一个图标，来标识这是一个置顶文章


## 文章摘要
文章摘要是会显示在首页封面的内容。

开关自动摘要（默认开启）：
```
index:
  auto_excerpt:
    enable: true
```
若要手动指定摘要，使用 <!-- more --> MD文档里划分，如：
```
正文的一部分作为摘要
<!-- more -->
余下的正文
```
或者在 Front-matter (opens new window)里设置 excerpt 字段，如：
```
---
title: 这是标题
excerpt: 这是摘要 （将摘要单独卸载头部信息中）
---
```

>TIP
优先级: 手动摘要 > 自动摘要
如果关闭自动摘要，并且没有设置手动摘要，摘要区域空白
无论哪种摘要都最多显示 3 行，当屏幕宽度不足时会隐藏部分摘要。


## 便签
表示强调、提示，类似 ">" 语法。
在 markdown 中加入如下的代码来使用便签：
```
{% note success %}
文字 或者 `markdown` 均可
{% endnote %}
```
或者使用 HTML 形式：
```
<p class="note note-primary">标签</p>
```
可选便签：
{% note primary %}
primary
{% endnote %}

{% note secondary %}
secondary
{% endnote %}

{% note success%}
success
{% endnote %}

{% note danger %}
danger
{% endnote %}

{% note warning%}
warning
{% endnote %}

{% note info %}
info
{% endnote %}

{% note light %}
light
{% endnote %}

{% note warning%}
WARNING
使用时 ```{% note primary %}``` 和 ```{% endnote %}``` 需单独一行，否则会出现问题
{% endnote %}


## 行内标签

类似加粗，高亮。
在 markdown 中加入如下的代码来使用 Label：
```
{% label primary @text %}
```
或者使用 HTML 形式：
```
<span class="label label-primary">Label</span>
```
可选 Label：

{% label primary @primary %}{% label  default @default%}{% label  info @info%}{% label  success @success %}{% label  warning @warning %}{% label  danger @danger %}

{% note warning%}
WARNING
若使用```{% label primary @text %}```，text 不能以 @ 开头
{% endnote %}

## 勾选框
在 markdown 中加入如下的代码来使用 Checkbox：
```
{% cb text, checked?, incline? %}
```
text：显示的文字
checked：默认是否已勾选，默认 false
incline: 是否内联（可以理解为后面的文字是否换行），默认 false

示例：
```
{% cb 普通示例 %}
{% cb 默认选中, true %}
{% cb 内联示例, false, true %} 后面文字不换行
{% cb false %} 也可以只传入一个参数，文字写在后边（这样不支持外联）
```
{% cb 普通示例 %}
{% cb 默认选中, true %}
{% cb 内联示例, false, true %} 后面文字不换行
{% cb false %} 也可以只传入一个参数，文字写在后边（这样不支持外联）

## 按钮
你可以在 markdown 中加入如下的代码来使用 Button：
```
{% btn url, text, title %}
```
或者使用 HTML 形式：
```
<a class="btn" href="url" title="title">text</a>
```
url：跳转链接
text：显示的文字
title：鼠标悬停时显示的文字（可选）

{% btn url, text, title %}


## 组图

如果想把多张图片按一定布局组合显示，你可以在 markdown 中按如下格式：
```
{% gi total n1-n2-... %}
  ![](url)
  ![](url)
  ![](url)
  ![](url)
  ![](url)
{% endgi %}
```
total：图片总数量，对应中间包含的图片 url 数量
n1-n2-...：每行的图片数量，可以省略，默认单行最多 3 张图，求和必须相等于 total，否则按默认样式

如下图为 `{% gi 5 3-2 %}` 示例，代表共 5 张图，第一行 3 张图，第二行 2 张图。
![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Hello_World_&_Fluid_配置指南/group_image.c1b58381.png)

## 页内跳转

<a href="#demo">点击到达跳转位置演示</a> 
```
<a href="#demo">点击到达跳转位置演示</a> # 在需要跳转的地方添加此代码。
<a id="demo">跳转位置演示（跳转位置设置点）</a> # 在跳转位置添加次代码。
```
<a id="demo">跳转位置演示（跳转位置设置点）</a> 

## iframe 页面镶套 
iframe 页面镶套可以帮助我们更好的展示一个页面。比如以下演示页面。
<iframe src="http://lonlypan.com" width="100%" height="500" name="topFrame" scrolling="yes"  noresize="noresize" frameborder="0" id="topFrame"></iframe>

```
<iframe src="http://lonlypan.com" width="100%" height="500" name="topFrame" scrolling="yes"  noresize="noresize" frameborder="0" id="topFrame"></iframe>
```
一些参数说明，width="100%" 为宽度自适应，高度请根据实际需求跳转，注意移动端页面是否匹配。 scrolling 为滚动条参数。frameborder 为边框参数。noresize 属性规定用户无法调整框架的大小。

## 哔哩哔哩视频嵌入

<div class="bilibili">
    <iframe src="////player.bilibili.com/player.html?aid=248260059&bvid=BV1Qv411G76v&cid=343709769&page=1&high_quality=1"  scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>
</div>

修改主题配置文件：
custom_css: /css/custom.css
在主题的 `fluid\source\css` 文件夹下新建 `custom.css` 文件，并添加以下css代码：
```c
/*哔哩哔哩视频适配*/
.bilibili{
    position: relative;
    width: 100%;
    height: 0;              /*高度设置这里无效，设置为0，用padding撑开div*/
    padding-bottom: 75%;    /*68%到80%都可以*/
}
.bilibili iframe {
    position: absolute;
    width: 100%;
    height: 100%;
    left: 0;
    top: 0;
}
```
插入时写成如下形式即可：
```
<div class="bilibili">
    <iframe src="////player.bilibili.com/player.html?aid=248260059&bvid=BV1Qv411G76v&cid=343709769&page=1&high_quality=1"  scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>
</div>
```
`<iframe></iframe>`中内容点击视频下方分享按钮，获取。追加 `&high_quality=1`可设置默认清晰度为最高

>该方法也适用于所有iframe 页面镶套 ，做到页面自适应和显示调整。

**参考资料：**
- [hexo博客插入图片与视频方法](https://zhuanlan.zhihu.com/p/104996801)
- [Hexo中插入Bilibili视频](https://liuzhihang.com/2019/09/14/hexo-inserts-bilibili-video.html)
- [HEXO博客引用B站视频并自动适配](https://hongcyu.cn/posts/hexo-bilibili.html)

## details 标签
用于展示代码较多需要折叠或折叠相关内容，以下为演示。summary 填写显示名称。

<details>
<summary>Demo</summary>
<pre><code>
coding...
</pre></code>
</details>

```
<details>
<summary>Demo</summary>
<pre><code>
  coding...
</pre></code>
</details>
```

## 图床绑定

hexo自身图片存储地址是指定的，并不好用。决定使用 Github 图床存储，并用   [jsDelivr CDN官网](https://www.jsdelivr.com/?docs=gh) 加速访问。


只需要修改文件名生成规则
```
hexo_images/{{title}}/{{filename}}
```
和url前缀即可，
```
https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/
```
`LonlyPan/LonlyPan.github.io`是 存储图片的 `hexo_images` 文件夹地址，`@master`  必须要加。

**参考资料：**
- [使用 jsDelivr CDN 对 Github 图床进行加速，带给你如丝滑般的图片体验](https://zhuanlan.zhihu.com/p/138582151)
- [GitHub 图床的正确用法，通过 jsDelivr CDN 全球加速](https://ld246.com/article/1583894928771)
- [hexo博客插入图片与视频方法](https://zhuanlan.zhihu.com/p/104996801)


## 扩展语法
- [使用 ECharts 插件绘制炫酷图表](https://hexo.fluid-dev.com/posts/hexo-echarts/) 

## Fusion360 模型在线展示

测试结论：
- 使用**640\*480像素**+css自适应代码约束约束。
- 添加提示：手机端建议点最后边图标 ![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Hello_World_&_Fluid_配置指南/1628332366748.png) 全屏显示观看。


>手机端建议点最后边图标 ![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Hello_World_&_Fluid_配置指南/1628332366748.png) 全屏显示观看。

**原链接1024\*768像素+css自适应约束**
<div class="bilibili">
<iframe src="https://myqq12509.autodesk360.com/shares/public/SH919a0QTf3c32634dcf08c6dee1f4cfca2a?mode=embed" width="1024" height="768" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe>
</div>

**原链接800\*600像素+css自适应约束**
<div class="bilibili">
<iframe src="https://myqq12509.autodesk360.com/shares/public/SH919a0QTf3c32634dcf004d92fd38c51ead?mode=embed" width="800" height="600" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe>
</div>

**原链接640\*480像素+css自适应约束**
<div class="bilibili">
<iframe src="https://myqq12509.autodesk360.com/shares/public/SH919a0QTf3c32634dcf004d92fd38c51ead?mode=embed" width="640" height="480" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe>
</div>

**原链接640\*480像素**
<iframe src="https://myqq12509.autodesk360.com/shares/public/SH919a0QTf3c32634dcf004d92fd38c51ead?mode=embed" width="640" height="480" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe