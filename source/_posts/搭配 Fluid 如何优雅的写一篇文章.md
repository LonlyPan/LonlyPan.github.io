---
title: 搭配 Fluid 如何优雅的写一篇文章
index_img: https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/%E6%90%AD%E9%85%8D_Fluid_%E5%A6%82%E4%BD%95%E4%BC%98%E9%9B%85%E7%9A%84%E5%86%99%E4%B8%80%E7%AF%87%E6%96%87%E7%AB%A0/hexo%2Bfluid.png
date: 2021-06-12 14:33
updated: 2020-05-08 14:33
hide: false
# sticky: 100 #置顶，数字越大越靠前
# banner_img: #/img/post_banner.jpg
# comment: false
categories: 03-瞎折腾
---



{% note success %}
文章参考：https://hexo.fluid-dev.com/posts/fluid-write/
{% endnote %}


基于Hexo-Fluid，记录当前博客的一些常用语法以及进阶使用配置。
<!--more-->


Fluid官网文档：[Fuild 配置指南](https://hexo.fluid-dev.com/docs/guide/)  
Hexo官网文档：[hexo文档](https://hexo.io/zh-cn/docs/)


# 配图

**常用配图资源**
- [wallpaperhub](https://wallpaperhub.app/)
- [Wallhaven](https://wallhaven.cc/)
- [Unsplash](https://unsplash.com/)
 
文章首页封面图推荐大小 540x320px
 
# Fluid主题语法
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


## 页内跳转

<a href="#demo">点击到达跳转位置演示</a> 
```
<a href="#demo">点击到达跳转位置演示</a> # 在需要跳转的地方添加此代码。
<a id="demo">跳转位置演示（跳转位置设置点）</a> # 在跳转位置添加次代码。
```
<a id="demo">跳转位置演示（跳转位置设置点）</a> 

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


## iframe 页面镶套 

iframe 页面镶套可以帮助我们更好的展示一个页面。

```
<iframe src="https://lonlypan.com" width="100%" height="500" name="topFrame" scrolling="yes"  noresize="noresize" frameborder="0" id="topFrame"></iframe>
```
`src=" "`为网页链接，必须是 https，实测 http 不显示， `width="100%"` 为宽度自适应，高度请根据实际需求调整，注意移动端页面是否匹配。 scrolling 为滚动条参数。frameborder 为边框参数。noresize 属性规定用户无法调整框架的大小。

**最终效果**
<iframe src="https://lonlypan.com/" width="100%" height="480" name="topFrame" scrolling="yes"  noresize="noresize" frameborder="0" id="topFrame"></iframe>

# 进阶配置

## 哔哩哔哩视频嵌入

需要修改主题配置文件即_config.fluid.yml文件：
```
# 指定自定义 .css 文件路径，用法和 custom_js 相同
# The usage is the same as custom_js
custom_css: /css/custom.css
```

在主题的 `fluid\source\css` 文件夹下新建 `custom.css` 文件，并添加以下css代码：
```c
/*哔哩哔哩视频适配*/
.bilibili{
    position: relative;
    width: 100%;
    height: 0;     ``
	/*高度设置这里无效，设置为0，用padding撑开div*/
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
`<iframe ...></iframe>`所有内容，在bilibili网页端点击视频下方分享图标->嵌入代码，直接获取。追加 `&high_quality=1`可设置默认清晰度为最高

>该方法也适用于所有iframe 页面镶套 ，做到页面自适应和显示调整。

**最终效果**
<div class="bilibili">
    <iframe src="////player.bilibili.com/player.html?aid=248260059&bvid=BV1Qv411G76v&cid=343709769&page=1&high_quality=1"  scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>
</div>

**参考资料：**
- [hexo博客插入图片与视频方法](https://zhuanlan.zhihu.com/p/104996801)
- [Hexo中插入Bilibili视频](https://liuzhihang.com/2019/09/14/hexo-inserts-bilibili-video.html)
- [HEXO博客引用B站视频并自动适配](https://hongcyu.cn/posts/hexo-bilibili.html)


## 图片加速

用   [jsDelivr CDN官网](https://www.jsdelivr.com/?docs=gh) 加速访问。

图片
```
https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@main/
```
`LonlyPan/LonlyPan.github.io`是 存储图片的 `hexo_images` 文件夹地址，`@main`  必须要加。

**参考资料：**
- [使用 jsDelivr CDN 对 Github 图床进行加速，带给你如丝滑般的图片体验](https://zhuanlan.zhihu.com/p/138582151)
- [GitHub 图床的正确用法，通过 jsDelivr CDN 全球加速](https://ld246.com/article/1583894928771)
- [hexo博客插入图片与视频方法](https://zhuanlan.zhihu.com/p/104996801)


## Fusion360 模型在线展示
Fusion 软件中，文件->共享，生成分享链接，不过只能看不能下载，按如下格式编写即可：
```
<iframe src="https://a360.co/38PcGXr" width="100%" height="480" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe
```
`src=" "`  填写的就是软件生成的共享链接，`width="100%" height="480"` 指定了显示大小，也可以自定义参数，随便填。

**最终效果**
<iframe src="https://a360.co/38PcGXr" width="100%" height="480" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe
