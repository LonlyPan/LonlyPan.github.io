---
layout: post
title:  "VS Code使用经验总结"
index_img: https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/彻底删除vscode及安装的插件和个人配置信息/1200px-Visual_Studio_Code_1.35_icon.svg.png
date:   2019-05-17 11:12
updated: 2019-05-17 11:12
hide: false
# sticky: 100 #置顶，数字越大越靠前
# banner_img: #/img/post_banner.jpg
# comment: false
categories: 01-专业
---

# 彻底删除vscode及安装的插件和个人配置信息
原文在：https://www.cnblogs.com/muou2125/p/10388440.html
此贴完全复制粘贴，作为个人备份使用。感谢作者。

1、卸载vscode应用软件（在控制面板里面找不到改软件，所以只能进入应用所在文件夹进行卸载）
<!--more-->

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/彻底删除vscode及安装的插件和个人配置信息/1.png)

此步骤虽然删掉了应用软件，但是此时重新安装会发现之前下载的插件和个人配置信息都还会重新加载出来，所以继续进行以下步骤：

2、找到下图中文件夹的目录，然后将之删除，即可彻底清除已安装的插件个个人配置信息

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/彻底删除vscode及安装的插件和个人配置信息/。vscode.png)

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/彻底删除vscode及安装的插件和个人配置信息/code.png)


经过以上两步操作之后，再次重新安装软件，将是最原始的状态

# 使用技巧

## 注释
- shift+alt+a：选中内容，会使用 `/* */` 注释内容。如果是空行，则自动输入`/* */`，方便快速写注释
- shift+/：选中内容，会使用 `//` 注释内容，多行也可以。如果是空行，则自动输入`//`，方便快速写注释

# 插件

## Settings Sync 同步 VSCode 设置插件

- [Settings Sync插件主页](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)

**快捷键**
1. Upload Key : Shift + Alt + U
2. Download Key : Shift + Alt + D
(on macOS: Shift + Option + U / Shift + Option + D)

## koro1FileHeader 注释插件

目的：编写程序时可以快速添加注释。规范写作  

* [注释模板的设置](https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE#%E5%A4%B4%E9%83%A8%E6%B3%A8%E9%87%8A%E7%AD%89%E5%AE%BD%E8%AE%BE%E7%BD%AEwidesame)
* [配置](https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE#%E8%87%AA%E5%AE%9A%E4%B9%89%E6%B3%A8%E9%87%8A%E7%AC%A6%E5%8F%B7)
* [配置字段](https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE%E5%AD%97%E6%AE%B5)
* [Visual Studio Code插件koro1FileHeader使用记录](https://www.rumosky.com/archives/176.html)
* [代码注释插件koroFileHeader](https://maizitoday.github.io/post/vscode%E5%BC%80%E5%8F%91%E5%B7%A5%E5%85%B7-%E4%BB%A3%E7%A0%81%E6%B3%A8%E9%87%8A%E6%8F%92%E4%BB%B6/)
* [VScode文件自动添加头部注释](https://www.yuque.com/purequant/vscode/shrch6)

## background 插件

目的：好看
>如果你的图片链接地址是错的（没有该图片），插件可能不会正常工作。你的设置也是不会生效的（实际设置已经更改保存了）

* [shalldie/vscode-background](https://github.com/shalldie/vscode-background)
* [VSCode——修改VSCode背景图片](https://blog.csdn.net/yukinoai/article/details/84564949)

## git与guithub

目的：使用这个插件的目的是能够编辑github文档（我的博客存储在github），这样我就可以使用vscode编写文档，并同步到GitHub上。

1. 安装git，保证你的git能够正常使用（clone、push等）
2. 安装 GitHub Pull Requests and Issues 扩展，处理Pull Requests and Issues。

* [How to Get Visual Studio Code GitHub Setup Going!](https://adamtheautomator.com/visual-studio-code-github-setup/)
* [Working with GitHub in VS Code](https://code.visualstudio.com/docs/editor/github)
* [使用GitHub（三）：使用VSCode+GitHub进行版本控制](https://segmentfault.com/a/1190000013762818)

使用git进行push时，vscode会提示  错误
`fatal unable to access https://github.com LibreSSL SSL_connect SSL_ERROR_SYSCALL in connection to github.com 443`
我是由于使用代理软件导致的，现在必须开全局代理才行，而且不开代理也不能正常工作。貌似每安装过代理软件的电脑时可以正常使用的。

* [【已解决】mac中git提交出错：fatal unable to access https://github.com LibreSSL SSL_connect SSL_ERROR_SYSCALL in connection to github.com 443](https://www.crifan.com/mac_git_push_fatal_unable_to_access_https_github_libressl_ssl_error_syscall/)
* [Visual Studio Code使用代理解决下载插件失败的问题](https://www.crifan.com/mac_git_push_fatal_unable_to_access_https_github_libressl_ssl_error_syscall/)
* [使用git clone或push出现SSL错误的解决](https://www.dreamoftime0.com/2018/10/21/%E4%BD%BF%E7%94%A8git-clone%E6%88%96push%E5%87%BA%E7%8E%B0ssl%E9%94%99%E8%AF%AF%E7%9A%84%E8%A7%A3%E5%86%B3/)
* [git push github失败，提示：SSL_connect: SSL_ERROR_SYSCALL in connection to github.com:443](https://blog.csdn.net/daerzei/article/details/79528153)

## Mardown编辑相关（已弃用）

### Markdown Preview Enhanced
目的：预览 MarkDown 文档

- [markdown preview enhanced文档（简体中文版）](https://www.bookstack.cn/read/mpe/zh-cn-extra.md)
- [关于插件Markdown Preview Enhanced的使用技巧](https://my.oschina.net/u/4410490/blog/3583542)

### markdown-image

目的：编写 MarkDown 文档，上传图片并保存到github中。
我的博客文章的图片时直接保存在博客所在仓库下的，所以我需要插图图片同时还要保存到github中。其实是先保存到本地，然后通过 push 保存到仓库中。这个插件主要功能就是指定图片的存储位置并在文档中生成链接。设置如下：
![ex-image set](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/VS_Code使用经验总结/ex-image_set.png)
- [Markdown Image
](https://marketplace.visualstudio.com/items?itemName=hancel.markdown-image)
- [vscode 粘贴图片自动上传到多吉图床](https://blog.allwens.work/uploadImageToDogedoge/)

### Markdown Shortcuts
目的：编写 MarkDown 文档，通过快捷键和鼠标右键菜单快速生成 Markdown 标记符。

### Markdown All in One

以使用 `Markdown Preview Enhanced` + `Markdown Shortcuts` 组合代替。
它的渲染功能和效果与`Markdown Preview Enhanced` 比较的话还是稍有不足，且无pdf导出操作

* [用VSCode打造宇宙最强Markdown编辑器【插件篇】vscode+MPE等插件+PigGo图床+格式化导出+最佳实践+技巧
](https://my.oschina.net/leozhangit/blog/4688394)
