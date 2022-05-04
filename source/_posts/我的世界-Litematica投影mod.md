---
layout: post
title: "我的世界-Litematica投影mod"
index_img: https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/我的世界-Litematica投影mod/minecraft_logo_icon_168974.png
date: 2020-11-01
categories: 娱乐
---

litematica 是由作者 masa 发布的一款工具项模组，投影 mod 可以把其它地图中的任何结构（包括模组方块）的空间结构（又称该空间结构为：原理图）保存起来，然后投影到当前地图中形成3D虚像（全息影像），然后玩家就可以照着虚像建造一个一模一样的结构。  
单机存档和服务器同样适用，可以说是一个既方便又强大的模组。

- Litematica 还包括一个“材料列表”，帮助你查看当前建成当前结构所需的材料。同时还包括“信息HUD“，可以在游戏同时查看材料清单。  
- 还有一个原理图验证程序功能，可以使用它来扫描整个原理图区域并查找任何错误。这在技术/红石构建时尤其有用，因为微小的错误会破坏一切。  
- 还有一些仅创造模式才有的辅助功能，例如填充，替换，删除以及粘贴模式，可以直接将原理图“粘贴”到世界中，无需再自己建造。  

<!--more-->

**环境**
- Minecraft 正版，原版启动器
- 游戏版本 1.16.3 ，未安装任何 mod
- Windows 10 家庭版


## 下载安装

要使用 Litematica 投影 mod 我们需要先要做一些前置下载安装。

### 下载

1. 首先点击 [综合下载链接](https://pena2.dy.fi/tmp/minecraft/mods/client_mods/?mcver=All)，按照下图提示下载。
![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/我的世界-Litematica投影mod/Litematica-MaLiLib下载.png)

2. 下载[Fabric](https://fabricmc.net/use/) ，按下图提示下载。
![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/我的世界-Litematica投影mod/Fabric下载.png)

3. Java安装
如果你前两步下载的文件图标显示不是 Java 图标（见下图描述，是 Java 图标请跳过这一步），就需要再下载 [Java](https://java.com/zh-CN/download/)。  
至于安装，按照软件提示默认安装即可，可更改安装位置。  
![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/我的世界-Litematica投影mod/isJava.png)


### 安装

1. 我们选中前面下载好的 fabric 文件，右键单击，使用 Java 打开。开始执行 fabric 安装。
![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/我的世界-Litematica投影mod/fabric安装.png)
2. 在安装界面选择 游戏版本，其余保持默认即可，单击安装，等待安装完成。
![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/我的世界-Litematica投影mod/fabric安装2.png)
3. 在 Windows 左下角搜索栏中输入 `run`，打开该工具，并在其输入栏中输入 `%appdata%`,单击确定。
![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/我的世界-Litematica投影mod/appdata.png)
5. 然后我们会自动进入 `AppData` 下的 `Roaming` 文件夹，找到 `.minecraft` 文件夹，单击进入。  
在 `.minecraft` 文件夹内新建一个 `mods`文件夹，并将我们之前下载好的 `Litematica` 和 `MaLiLib` 文件复制到该文件夹内。
至此，所以安装结束。
![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/我的世界-Litematica投影mod/复制文件.png)

### 查看是否安装成功

1. 启动 Minecraft，在启动界面左下角，我们可以看到 fabric 字样（如果没有，在下拉框中找到并选中）。单击开始游戏即可。
![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/我的世界-Litematica投影mod/开始游戏.png)
2. 如果启动失败，说明你没有安装 Minecraft 游戏本体。我们需要添加一个游戏本体。单击 `配置` -> `新建`，并在新建配置界面选择带 fabric 字样的版本，其他保持默认，
![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/我的世界-Litematica投影mod/新建存档.png)
3. 在开始界面左下角我们可以看到游戏版本名带 fabric 后缀，可以进一步验证fabric启动成功。  
进入游戏，按下 Ｍ 键(输入法切换到英文)，就会打开  Litematica 投影mod设置界面，说明一切安装成功，可以开始游戏了。
![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/我的世界-Litematica投影mod/M.png)

## Litematica使用

原理图：已保存的地图中的空间结构（建筑物，地形等）文件为原理图
选区：选择地图中某块区域，例如想要将一栋房子保存为原理图，我们先要选中该房子的三维空间区域，把它圈起来。
投影：将保存好的原理图投射到当前地图中，形成 3D虚像（全息影像）

### 主界面

- ![enter description here](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/我的世界-Litematica投影mod/主界面.png)

- 原理图编辑 可以对加载过的原理图进行编辑（我们称已保存的投影文件为原理图，文件后缀为.litematic）
- 已加载原理图 可以查看已经加载（投影到当前地图世界）的原理图信息
- 加载原理图 可以将已经保存过的原理图文件加载到当前地图世界中
- 配置菜单 投影mod的设置菜单
- 原理图目录 可以查看已经保存过的原理图，它们其实保存在.minecraf/schematics目录下
- 选区编辑器 可以对即将要保存为原理图的空间结构选取范围进行编辑
- 选区目录 可以查看已经选中的选区信息，有的时候我们需要保存同一地图中的不同空间结构，会出现多个选区。
- 区域选择模式 分为简单模式和正常模式两种，觉得只要掌握简单模式就行了，正常模式在此不讲（感兴趣的同学可以了解）
- 任务管理器可以看到正在进行的任务，如同时进行选区和投影
- 原理图文件夹？？（实则没用过，感兴趣的同学可以了解）
- 工具模式 可以切换模式，目前总共9种模式，目前可以还通过小木棍(核心工具)进行切换模式,接下来会讲到






 [\[litematica\]投影模组——详细入门教程](https://www.mcbbs.net/thread-906442-1-1.html)
[Mine-Tech小教室 | 超方便建築投影模組 Litematica - 安裝以及簡易使用教學 1.13.2](https://www.youtube.com/watch?v=-SKru_EruIk)
[【Litematica】投影模组——详细入门教程](https://tieba.baidu.com/p/6230849633)
[【XeKr】叉尅教你快速掌握投影mod](https://www.bilibili.com/video/BV1DJ411X78m/?spm_id_from=333.788.videocard.7)
[一顿饭学会Litematica常规操作](https://www.bilibili.com/video/BV1yk4y1y7Cj)

[\[伐竹猫\]我的世界ReplayMod教程,你还在为延时摄影不会而担忧吗？](https://www.bilibili.com/video/BV1D54y197U2)
[Replay Mod Documentation](https://www.replaymod.com/docs.9af5629/)
## 参考链接

- MaLiLib[下载链接](https://www.curseforge.com/minecraft/mc-mods/malilib)
- Litematica[下载链接](https://www.curseforge.com/minecraft/mc-mods/litematica)
- Fabric API[下载链接](https://www.curseforge.com/minecraft/mc-mods/fabric-api/download/3097415)
- [YouTube-Litematica in Minecraft 1.16.3下载安装教程](https://www.youtube.com/watch?v=q1q33tiJNCw)


