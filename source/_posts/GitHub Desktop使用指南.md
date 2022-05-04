---
layout: post
title:    "GitHub Desktop使用指南"
index_img: https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/GitHub_Desktop使用指南/github_desktop_logo.png
date:   2019-12-07  9:33 
categories: 编程语言
---

简单介绍如何安装和使用 **GitHub Desktop** 软件，完成线上仓库的克隆，并将本地更改内容同步更新到线上仓库。主要配合 **Jekyll** 更新博客文档使用。

- 官方[软件地址](https://desktop.github.com/)
- 官方[帮助文档](https://help.github.com/en)

## 安装

进入 [Github Desktop官网](https://desktop.github.com/) 下载软件，单击安装包，再登陆账户，安装完成。

![001-sign_in.](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/GitHub_Desktop使用指南/001=sign_in.png)

## 克隆

首次打开软件，主界面会显示下图左半界面，共四个选项，字面意思。这里我们许选择克隆仓库，然后在弹出界面选择一个仓库（下图右半，前提是你的GitHub账户里有仓库），再修改本地保存位置，软件就会自动完成克隆。  

**克隆：** 意思就是将你该仓库的所有文件全部下载到本地，原模原样，文件名就是仓库名。

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/GitHub_Desktop使用指南/clone.png)

克隆完成后，你就可以使用其他软件修改本地仓库的文件了（推荐 Visual Studio Code ），而 Github Desktop 软件会实时检查本地文件更改状态，并列出详细信息。

## 同步更新

### 本地到线上

如果文件编辑结束，就需要更新线上仓库。

- 第一步：我们需要对本次所作的更改写一个简短**更改介绍**，**详细描述**可选填（这里的**更改介绍**必须填写，否则第二步无法进行）
- 第二步：点击 **Commit master** 按钮将更改提交到主分支（不一定是主分支，根据你所选的分支提交），**这里的提交并不是真正的提交到线上，而是一种确认。**
- 第三步：单击 **fetch origin** 按钮进行线上线下文档对比

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/GitHub_Desktop使用指南/003-change_file.png)

对比结束后，软件会显示 **Push origin** 按钮，单击就可将本地文件更新到线上仓库。

![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/GitHub_Desktop使用指南/004-push.png)

### 线上到本地

如果是本地文档没变，线上发生了更改，则直接进入上一节的**第三步**，直接对比文档。

之后软件会显示拉取按钮（即更新本地文件）。单击即完成本地文档更新。