---
layout: post
title: "bilibili投稿与视频剪辑-必剪"
index_img: https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/bilibili投稿与视频剪辑-必剪/图像_25.png
date: 2021-05-24
updated: 2021-05-24
hide: false
# sticky: 100 #置顶，数字越大越靠前
# banner_img: #/img/post_banner.jpg
# comment: false
categories: 03-瞎折腾
---

<!--more-->

## 视频录制

OBS 

- [OBS教程：六分钟学会直播与视频录制](https://www.bilibili.com/video/BV1kW411K7HA/?spm_id_from=333.788.videocard.4)
- [五分钟教你在OBS安装按键显示插件](https://www.bilibili.com/video/BV1vk4y1r7jU/?spm_id_from=333.788.videocard.5)
- [录屏时显示按键点击——KeyCastOW编译](https://www.bilibili.com/video/BV1MV411y7UZ?from=search&seid=11252422796430700713)
- [OBS 去除背景噪声 滤镜的使用 【JD煎蛋】](https://www.bilibili.com/video/BV1qW411L7pt)
- [OBS 高级音频设置-监听设置+音轨 【JD煎蛋】](https://www.bilibili.com/video/BV19W411P7CG?from=search&seid=1270254410331759847)
- [为什么你的直播这么卡？用实测告诉你OBS推流到底需要什么配置](https://www.bilibili.com/video/BV1R4411b78i/?spm_id_from=333.788.videocard.6)

## 视频录制

### 录音设置
1. 使用NVDIA的降噪软件，麦克风源选择我们的麦克风，开启噪音消除效果，强度最大
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/bilibili投稿与视频剪辑-必剪/图像_1.png)
2. win10 右下角声音设置，输入选择插入设备的，右侧打开声音控制面板
![图像 7](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/bilibili投稿与视频剪辑-必剪/图像_7.png)
3. 选择输入麦克风，打开属性，加强10dB（后面根据实际使用改为10dB更好）
![图像 8](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/bilibili投稿与视频剪辑-必剪/图像_8.png)
4. 视频录制软件，麦克风音源选择 NVDIA Broadcast。即可开启录制
![图像 9](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/bilibili投稿与视频剪辑-必剪/图像_9.png)


## 视频剪辑

blender

## 字幕制作

1. 语音识别与字幕生成
  [VideoSrt使用指南](https://www.yuque.com/viggo-t7cdi/videosrt/em4n10)
2. 字幕编辑
-  [使用Aegisub为视频快速添加字幕](https://www.bilibili.com/video/av97213505/)  
-  [提升制作字幕的效率：Aegisub 的使用指南及技巧](https://sspai.com/post/47557)

 3. 字幕合成
 哔哩哔哩投稿工具自动合成
 
## 视频上传
 
 模板：网页版上传界面 新建模板 桌面版的没法新建，但可以使用网页版新建的模板，节省上传设置
 
## 字幕识别与导出
 
 ### 适用最新版本（V2.0以上）
 V2.0 之后新版的必剪改了工程位置和工程文件格式，现在是json文件，以前是xml文件
 
 打开必剪，开始创作
 ![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/bilibili投稿与视频剪辑-必剪/1707182587145.png)
 
 将音频或视频文件拖入到 **导入素材** 框中，然后再从 **素材** 框中将音视频素材拖入到**编辑栏**中，注意导轨向左对齐。
 ![导入素材](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/bilibili投稿与视频剪辑-必剪/导入素材.gif)
 
 选择 文本 -> 识别字幕  -> 开始识别
 >注：如果音视频文件时常超过15分钟，是不能直接识别的（软件中间会有提示，如下图所示），我们需要先将其分割成一小段，并且每段时长不超过15分钟

 ![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/bilibili投稿与视频剪辑-必剪/1707183478516.png)

拖动编辑栏中的分割线到合适位置（上面有时间线），然后单击分割图标，音视频就会被分割成两端（已分割线为界），然后再分割，直到每段时长不超过15分钟
![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/bilibili投稿与视频剪辑-必剪/1707184906602.png)
完整过程如下
![分割素材](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/bilibili投稿与视频剪辑-必剪/分割素材.gif)


### 适用旧版本（V2.0以前）
工程文件格式是xml文件

