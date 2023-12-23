---
title: RC水下无人机项目
index_img:  https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/乐鑫ESP32_S2教程_基于ESP-IDF_v4.4/ESP32_S2教程封面-合集.png
date: 2023-12-23 19:23
updated: 2023-12-23 23:23
hide: false
# sticky: 100 #置顶，数字越大越靠前
# banner_img: #/img/post_banner.jpg
# comment: false
categories: 00-项目
---

<!--more-->


[如何DIY一个遥控潜艇？](https://www.zhihu.com/question/22127975)
[弥补童年的遗憾:遥控潜艇模型制作](https://www.bilibili.com/video/av17018072/)


[无人潜航器总体方案通过专家评审，产业化在即切入千亿市场](http://pg.jrj.com.cn/acc/Res/CN_RES/STOCK/2016/1/3/30b18633-fc61-4187-937e-51c359522034.pdf)
## 前期调研

### 网站
[5imx社区](http://bbs.5imx.com/forum.php?mod=forumdisplay&fid=700&filter=lastpost&orderby=lastpost)
[rovmaker](http://rovmaker.cn/c/rov)
[subcommittee.](https://subcommittee.com/forum/showthread.php?32640-433mhz-links-and-info-(Tim-s-Regatta-Seminar)&highlight=remote)
[方舟模型](http://www.arkmodel.com/index.php?cPath=21)
[模型吧](https://tieba.baidu.com/p/1734805169?pn=2)
[模型类新闻吧](https://tieba.baidu.com/f?kw=%E6%A8%A1%E5%9E%8B%E7%B1%BB%E6%96%B0%E9%97%BB&ie=utf-8&tp=0)
[船模吧](http://c.tieba.baidu.com/f?kw=%E8%88%B9%E6%A8%A1&ie=utf-8&tp=0)
[rc船吧](http://jump2.bdimg.com/f?kw=rc%E8%88%B9&ie=utf-8&tp=0)
[openLRSng](https://github.com/openLRSng/openLRSngWiki/wiki/Hardware-Guide)
[bluerobotics](https://bluerobotics.com/learn/bluerov2-assembly/)

### 视频参考

[rc submarine solidworks](https://www.youtube.com/watch?v=PGICQRAMXIA)
[Mazu Sensor Board : Assembly instructions](https://www.youtube.com/watch?v=tpPX1rwJyAw&feature=emb_logo)
[part2: Assembling an ROV today! BlueROV mkII](https://www.youtube.com/watch?v=uCO2BKlrRLk)
[ROV MAKER](https://www.youtube.com/channel/UCSzkO2WNCYh4BSL6FSEijzw/videos?view=0&sort=dd&flow=grid)

### 国外网站制作参考


[Engel's Akula II   Build log](http://www.cliffhangershideout.com/Akula/Akula-log.html)

 [1. 潜艇紧急上浮时角度是否可以大于30度？](https://bbs.tiexue.net/post2_8297233_1.html)

### 店铺

[水下机器人控制系统 电子舱体 ardusub 密封舱 ROV 竞赛](https://item.taobao.com/item.htm?spm=2013.1.w4023-23150242545.7.50211954kR4MnF&id=626813080975)


主要参考：  
[5iMX社区›遥控航海模型-【技术专栏】›潜艇模型讨论区](http://bbs.5imx.com/forum.php?mod=forumdisplay&fid=700)

[凯旋号潜水艇-各种功能](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1241194&extra=page%3D7%26filter%3Dauthor%26orderby%3Ddateline) 

[关于电磁阀和水泵气泵](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1065379&extra=page%3D10%26filter%3Dauthor%26orderby%3Ddateline)


[潜艇改造：配件选择参考](http://bbs.5imx.com/forum.php?mod=viewthread&tid=789531&extra=page%3D14%26filter%3Dauthor%26orderby%3Ddateline&page=1)

[做一艘能潜水的攻击潜艇，顺便改一下原来做的喇叭牌海狼小潜艇](http://bbs.5imx.com/forum.php?mod=viewthread&tid=837768&extra=&page=9)

### 下潜

[无刷终极版亚特兰蒂斯号开始之制作了](http://bbs.5imx.com/forum.php?mod=viewthread&tid=900532&extra=page%3D12%26filter%3Dauthor%26orderby%3Ddateline&page=3)

[水泵沉浮密封舱制作的一些经验，，希望对想要做的朋友一些提示](http://bbs.5imx.com/forum.php?mod=viewthread&tid=891492)

[通过控制进水排水量来实现潜艇在水中的深度](http://bbs.5imx.com/forum.php?mod=viewthread&tid=989320&extra=page%3D11%26filter%3Dauthor%26orderby%3Ddateline)

[水囊用什么做好那，冲水不均匀，不会导致侧偏么](http://bbs.5imx.com/forum.php?mod=viewthread&tid=890015&extra=page%3D12%26filter%3Dauthor%26orderby%3Ddateline)

[关于让潜艇更逼真的上浮和下潜的想法，气泵水泵二合一，不知是否能增加上浮下潜的速度](http://bbs.5imx.com/forum.php?mod=viewthread&tid=688391&extra=page%3D16%26filter%3Dauthor%26orderby%3Ddateline)
#### 冲浅

[1.2米台风级潜艇制作，留作纪念](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1189638&extra=&page=4)

#### 中继模式

[水底实时图像和录像的设备有搞头没](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1132575&extra=page%3D9%26filter%3Dauthor%26orderby%3Ddateline&page=2)

#### 化学上浮

[能不能用小苏打（碳酸氢钠）加醋（醋酸）混合，产生气体，控制沉浮？](http://bbs.5imx.com/forum.php?mod=viewthread&tid=578276&extra=page%3D18%26filter%3Dauthor%26orderby%3Ddateline&_dsign=64678c66)

#### 唧筒、针筒

>适应于内部空间小，浮力变化范围小的，否则压差过大

- 唧筒拉动吸水与排水
- 缺点：会对密封舱造成极大的内压变化（吸水内压增大 或者 排水内压减小 二选其一）

该方案是唧筒电机负责唧筒运动，下潜时抽气，气体排入腔内，水泵注水
上浮时，唧筒排水，从腔内稀奇，水泵排水
 水泵貌似多余啊？
[水下fpv 开工](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1442015&extra=page%3D3%26filter%3Dauthor%26orderby%3Ddateline&page=2)

[为做台风做的前期准备工作](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1237828&extra=page%3D7%26filter%3Dauthor%26orderby%3Ddateline&page=2)

[找到合适齿轮，搞一套唧筒](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1175854&extra=page%3D8%26filter%3Dauthor%26orderby%3Ddateline)

[台风潜艇3D图](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1080528&extra=page%3D10%26filter%3Dauthor%26orderby%3Ddateline&page=1)

[U-Boat Type VIIC](http://bbs.5imx.com/forum.php?mod=viewthread&tid=951089&extra=page%3D11%26filter%3Dauthor%26orderby%3Ddateline&page=3)

[活塞泵II号机完工](http://bbs.5imx.com/forum.php?mod=viewthread&tid=894490&extra=page%3D12%26filter%3Dauthor%26orderby%3Ddateline) 

[【向潜艇迷推荐】不得不说这是唯一的最佳的潜艇沉浮方式](http://bbs.5imx.com/forum.php?mod=viewthread&tid=404364&extra=page%3D22%26filter%3Dauthor%26orderby%3Ddateline&_dsign=c319aa85)
#### 水袋（唧筒、针筒）+水泵

>适应于内部空间小，浮力变化范围小的，否则压差过大

水袋在仓体内
下潜时水泵向水袋注水，舱体自动增加下沉
上浮时排除水即可
缺点：会对密封舱造成极大的内压变化（注水时水袋膨胀挤压内部空间，内压增大 或者 排水时水带压缩，内压减小）。容易造成漏水。

水带可用唧筒（针筒）代替
![enter description here](https://LonlyPan.github.io/images/Posts/2020-10-13-遥控潜艇模型制作/1610037616639.png)

- [第一艘 1：144 基洛级潜水艇](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1508356&extra=page%3D2%26filter%3Dauthor%26orderby%3Ddateline&page=2)
- [入模第一发：从这枚小鱼雷开始](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1440768&extra=page%3D3%26filter%3Dauthor%26orderby%3Ddateline)

**2**
最先使用水带方案，后面又改为了 气瓶+气袋方案
[最后一艘潜艇](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1435184&extra=page%3D4%26filter%3Dauthor%26orderby%3Ddateline&page=5)

**3**
详细介绍了唧筒的工作方式
[我的静改动之海狼](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1432493&extra=page%3D4%26filter%3Dauthor%26orderby%3Ddateline&page=5)

[RC1/200基洛。慢慢做，现在问题就是橡胶圈，没找到合适的。会有点漏水](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1428611&extra=page%3D5%26filter%3Dauthor%26orderby%3Ddateline&page=3)
[雷虎潜艇-RC潜艇资料，可以看看](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1417972&extra=page%3D5%26filter%3Dauthor%26orderby%3Ddateline)
[首做鲣鱼级核潜艇](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1327565&extra=page%3D5%26filter%3Dauthor%26orderby%3Ddateline&page=2)
[039宋级潜艇升级改造](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1323363&extra=page%3D5%26filter%3Dauthor%26orderby%3Ddateline&page=2)

**4**
后期改为 内部空气+气袋方案，见下文
[罗马湖舰队1/100台风级潜艇建造中](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1255282&extra=page%3D7%26filter%3Dauthor%26orderby%3Ddateline) 

[渝海军工场:U-ⅦC](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1061279&extra=page%3D10%26filter%3Dauthor%26orderby%3Ddateline&page=3) 

[PVC管外壳 + 水泵沉浮潜艇模型下水 + 详细制作过程+航行视频 ！留贴以作纪念](http://bbs.5imx.com/forum.php?mod=viewthread&tid=888635&extra=page%3D1&page=14)

[新手做的1：144海狼级密封舱](http://bbs.5imx.com/forum.php?mod=viewthread&tid=848850&extra=page%3D13%26filter%3Dauthor%26orderby%3Ddateline&page=7)


[打印 上一主题 下一主题再改海狼 水泵沉浮 前水平舵可控](http://bbs.5imx.com/forum.php?mod=viewthread&tid=838133&extra=&page=4)

[威骏 CB35104 1/35 二战德国海军XXIII型海防潜艇 2014开始制作 已下水](http://bbs.5imx.com/forum.php?mod=viewthread&tid=773440&extra=page%3D15%26filter%3Dauthor%26orderby%3Ddateline&page=3)

[请高手帮我看看我这个设计图可行不可行，谢谢了](http://bbs.5imx.com/forum.php?mod=viewthread&tid=718197&extra=page%3D16%26filter%3Dauthor%26orderby%3Ddateline&page=2)

[新手一个月做海狼潜艇，水泵沉浮，13年3月小改](http://bbs.5imx.com/forum.php?mod=viewthread&tid=671131&extra=page%3D16%26filter%3Dauthor%26orderby%3Ddateline&page=5)

[速度来围观！ 顶级遥控！1/100 台风级弹道dao dan核潜艇 （ 剧毒！慎入！）](http://bbs.5imx.com/forum.php?mod=viewthread&tid=668453&extra=page%3D17%26filter%3Dauthor%26orderby%3Ddateline&page=7)

[【向潜艇迷推荐】不得不说这是唯一的最佳的潜艇沉浮方式](http://bbs.5imx.com/viewthread.php?tid=404364&amp&_dsign=c319aa85)

[改号手1:144海浪 双水泵可差动沉浮控制](http://bbs.5imx.com/forum.php?mod=viewthread&tid=525467&extra=page%3D20%26filter%3Dauthor%26orderby%3Ddateline&_dsign=bb4f0cf6)
#### 气泵+气瓶+气袋

**1**
气泵向气瓶内注入高压空气
气袋接触水
上浮时气瓶注入气袋，产生浮力
下潜时释放气袋中的气体或者将气体重新压缩至气瓶，气袋的浮力消失，下潜

另一个是瓶里一个大气压，上浮时抽真空
空瓶子里有空气的前提下调整好潜艇状态处于下潜状态，上浮时用泵把空气吹进覆膜袋子里，瓶子外形不变（应该是真空），袋子充满气，进而上浮。 
- [1/144 武汉级 33g飞航式导弹潜艇-静潜及火箭发射制作进行时](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1534840&extra=&page=1)
[潜艇沉浮示意图及配置讲解](http://bbs.5imx.com/forum.php?mod=viewthread&tid=750887&extra=&page=1)
**2**
气袋可用唧筒（针筒）代替、

**3**
又名：
循环气体静潜系统
- ![enter description here](https://LonlyPan.github.io/images/Posts/2020-10-13-遥控潜艇模型制作/1610036069063.png)
 - [海底两万里—鹦鹉螺号潜艇制作进行时](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1526747&extra=page%3D2%26filter%3Dauthor%26orderby%3Ddateline)
  
 **4**
 该处采用了唧筒，但效果不佳，作者最后换成了气袋方案，仔细对比图片原有的唧筒方案应该是：
 - 唧筒后不还有似乎还有某个装置，这里猜测是水泵
 - 唧筒和气瓶之间连接简单，并未看到气泵，电磁阀等装置
 推测如下：
 下潜时，唧筒后部的水泵放唧筒注水，压缩内部气体至气瓶，唧筒浮力消失，下潜
 上浮时，水泵排水，压力回到唧筒（或者只靠气体压力回弹），此时唧筒为腔体具有浮力

-  [1/100基洛潜艇制作中](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1526746&extra=page%3D2%26filter%3Dauthor%26orderby%3Ddateline&page=1) 

**5**
该方案直接吸取内部空气压缩气袋，但空气量减少，无法用于大的模型。
[压载舱](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1438844&extra=page%3D3%26filter%3Dauthor%26orderby%3Ddateline&page=1)
[罗马湖舰队1/100台风级潜艇建造中](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1255282&extra=page%3D7%26filter%3Dauthor%26orderby%3Ddateline) 
[遥控潜艇，伊19](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1237735&extra=page%3D7%26filter%3Dauthor%26orderby%3Ddateline)


[潜艇比例控制水柜](http://bbs.5imx.com/forum.php?mod=viewthread&tid=818556&extra=page%3D13%26filter%3Dauthor%26orderby%3Ddateline&page=1)

[](http://bbs.5imx.com/forum.php?mod=viewthread&tid=784694&extra=page%3D14%26filter%3Dauthor%26orderby%3Ddateline&page=6)



[潜艇沉浮示意图及配置讲解](http://bbs.5imx.com/forum.php?mod=viewthread&tid=750887&extra=page%3D15%26filter%3Dauthor%26orderby%3Ddateline)

[1:100台风开工-有鱼雷](http://bbs.5imx.com/forum.php?mod=viewthread&tid=797774&extra=page%3D14%26filter%3Dauthor%26orderby%3Ddateline&page=6)

[超好看的视频新手改的号手基洛下水了那个速度](http://bbs.5imx.com/forum.php?mod=viewthread&tid=535779&extra=page%3D19%26filter%3Dauthor%26orderby%3Ddateline&_dsign=3195baa7)

[纯菜鸟打算改装海狼级潜艇自己画了个布局图](http://bbs.5imx.com/forum.php?mod=viewthread&tid=527714&extra=page%3D19%26filter%3Dauthor%26orderby%3Ddateline&_dsign=f4a3f5e7) 

[近来潜艇板块里很热闹呀](http://bbs.5imx.com/forum.php?mod=viewthread&tid=398633&extra=page%3D22%26filter%3Dauthor%26orderby%3Ddateline&_dsign=69c31d19)

[手把手教你改基洛 潜航潜航](http://bbs.5imx.com/forum.php?mod=viewthread&tid=398480&extra=page%3D22%26filter%3Dauthor%26orderby%3Ddateline&page=2)
[再谈潜艇最佳上浮方法！压缩气体方法！欢迎跟贴^_^！](http://bbs.5imx.com/forum.php?mod=viewthread&tid=480525&extra=page%3D21%26filter%3Dauthor%26orderby%3Ddateline&page=1&_dsign=ce96a14a)
##### 参考链接


#### 最仿真

**1**
类似于 气泵+气瓶+气袋。
区别在于气袋变为水袋
- 压缩空气注入排水上浮
- 释放空气水流入下潜
缺点：浪费气体

用压缩空气，电磁阀控制充气排水，放气下沉。但结构相对复杂成本增加。
[1.4米VIIC开工慢做，双无刷，双桨推进](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1490245&extra=&page=2)
[气瓶静潜测试视频](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1445123&extra=page%3D3%26filter%3Dauthor%26orderby%3Ddateline) 
**2**
气瓶可改成充气，上浮后，通过气泵从水面上往里充气存储，可实现无限使用。

[新方法，使用压缩气体上浮潜艇](http://bbs.5imx.com/forum.php?mod=viewthread&tid=495783&extra=page%3D20%26filter%3Dauthor%26orderby%3Ddateline&_dsign=840c3c60)
### 动力

告诉你一个简单的无刷电机防水方法：用厚厚的黄油涂抹在定子与转子上，轴上不要上卡簧。每次运行回来后取下外转子，查看转子内部与定子上的黄油量，不够了就补上一些。
用这种简易方法可以延长无刷电机的寿命。

无刷电机防水，可以长期浸泡，有刷碳刷不能长期。电机防水需要更换轴承，或者定期对轴承进行保养。轴承普遍用轴承钢会生锈

[小白请教无刷空心杯电机是不是也能直接泡水？](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1102145&extra=page%3D9%26filter%3Dauthor%26orderby%3Ddateline)

[无刷马达防锈，各位大神有啥高招啊？](http://bbs.5imx.com/forum.php?mod=viewthread&tid=981612&extra=page%3D11%26filter%3Dauthor%26orderby%3Ddateline)
### 防水

[饼干的密封罐](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1243311&extra=page%3D7%26filter%3Dauthor%26orderby%3Ddateline)

建议 潜艇内部 放一个 高压二氧化碳
气瓶，下潜之后 调节 气瓶的 出气量
始终 使得 潜艇内部 压力 高于水压，


你好，我想问一下你的这个导弹在水下时是怎么密封的？
自己做个圆把尾部顶上，因为头是密封的，里面有气，不会进水。


### 信号传输



1B/s=8bps（b/s）、1KB/s=8Kbps（Kb/s）、1MB/s=8Mbps（Mb/s）。  
3.5寸屏 像素 480\*320 一幅画面大小 480\*320\*2 Byte（使用imagelcd生成bin文件 字节，RGB565 一个像素16位） = 307200B = 307200/1024 KB = 300KB  
在传输速度为 1Kbps 下，传输一副图片需要 300KB\*8 = 2400s  
9.6Kbps下 300KB\*8/9.6 = 250s 再将图片压缩 10 倍，时间 = 25s  

以上计算基于先将图片转为二进制格式再传输，如果直接传输图片本身，一幅 480\*320 像素 jpg格式图片大小约为10-20KB，则压缩 10倍，9.6kbps下传输时间 = 20/10/9.6 = 0.21s  
基于该计算方式，理论上使用 lora 传输图片可行，而计划只要 10-20s内传输一幅图片即可。  

[lora通信的最大可靠速率是多少?可以传送视频吗?](https://www.icxbk.com/ask/detail/19044.html)
[是否可以使用基于LPWAN的物联网设备传输照片？：6个步骤 2021](https://cn.mustdothis.com/65459-Is-It-Possible-to-Transfer-Photos-Using-LPWAN-base-12#menu-2)

[发明专利申请说明书CN201911241926.9.pdf](http://www10.drugfuture.com/pdfview/generic/web/viewer.html?file=/cnpat/package/%E5%8F%91%E6%98%8E%E4%B8%93%E5%88%A9%E7%94%B3%E8%AF%B7%E8%AF%B4%E6%98%8E%E4%B9%A6CN201911241926.9.pdf)

[水底实时图像和录像的设备有搞头没](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1132575&extra=page%3D9%26filter%3Dauthor%26orderby%3Ddateline&page=2)

低频引力波进行视频通信

水里超短波不好使，微波更不好使，传说中波或者长波好使，只是天线要得长了点


对于水听换呢器 我有一定了解 它是压电陶瓷 通入一定的脉冲电流后就会产生振动 换能器实际是指水下超声波换能器（超声波喇叭）在水下不同于空气 电波基本没有有效的通信能力 细心的话你可以观察到无线电的频率都是以G为单位的 这样高的频率在水下的衰减度很惊人 而超声波只有2万HZ 水下只适用于压力波 声波就属压力波 所以在水下广泛使用 利用超声波通信完全可行 也不想想的那么难 我们无线电传送的数字信号 就是1和0 2个状态的 声波也可以甚至更多 一般做过单片机的人 都应该能做的 如果超声波有自己的信号源的话 应该可以直接连数据发送出去 接收也一样 只是先前用的是天线 这里用的是喇叭 当然还要根据具体的发射板的电路来定 稍加改动就差不多

[又声波通信的知识-用2.4的遥控器遥控潜艇距离能有多远？](http://bbs.5imx.com/forum.php?mod=viewthread&tid=874310)
#### 载波通信

#### 图像压缩

[干货分享 |  那些年你在STM32上被困住的难题，今天我们给你总结](http://www.emakerzone.com/test_comment_info/165)  
[STM32实现图像JPEG压缩编码，源码，全球首发](https://www.amobbs.com/thread-3859491-1-1.html)  
https://github.com/HuffieWang/STM32F4-JPEG    
[\[分享\] 基于STM32F4 的图像压缩技术研究](https://www.stmcu.org.cn/module/forum/thread-602627-1-1.html)  
[PNG 图片压缩原理解析](https://www.wandouip.com/t5i228063/)  
[腾讯是如何大幅降低带宽和网络流量的(图片压缩篇)](http://www.52im.net/thread-1559-1-1.html)

[潜艇航拍的问题](http://bbs.5imx.com/forum.php?mod=viewthread&tid=245192&_dsign=d3bd4a2c)
#### SSTV水下照相传输


航模的图传不能在水下传输，而FPV就不可能了。
可以用低频FM遥控潜水器，那低频传输图片信号因该也可以吧，成组的图片传输就能组成视频了。
宇宙空间站也用它SSTV给地面上的业余爱好者发送照片（只要有VHF手持机和有解码软件的电脑就能在空间站飞过的特定时间收到）。它可以一个个像素传播，图像要求越清晰，速度越慢。常用频率为3.528MHz、7.033MHz、14.230MHz±6KHz、18.160MHz、21.340MHz±6KHz、24.980MHz、28.680MHz、50.300MHz。
网上有大神用144.5MHz做出拍摄发送机（http://www.chuangkoo.com/project/32）。不知道可不可能用3.528MHz做出来，能不能用于水下有时延地缓慢实时全损传送图片。（本人不会单片机，会的可以试试）

### 舱体

- 方舟的密封舱

### 壳体

潜艇3D打印图纸分享
度盘链接：链接：https://pan.baidu.com/s/1kJlh8kpTuxl164XPwMGj2g
提取码：m6y2
链接：https://pan.baidu.com/s/1u4z8pQlxPjnkJqG2sw7tgg
提取码：c2cx
鹦鹉螺号潜艇
链接：https://pan.baidu.com/s/1UVm8urQ3vWbUYK-BWKmT0w
提取码：nwhn

### 国内卖家
无锡冬星模型

### [再造机敏-廉价静潜密封舱方案](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1565696&extra=page%3D1%26filter%3Dauthor%26orderby%3Ddateline)

密封舱，采用的是钓鱼的调漂桶。直径50mm。两端带有双密封圈的堵头。密封效果非常好。
原装桨基本没推力。所以换装了三江模型出的22mm7叶桨。

9g舵机做好防水（电路板部分灌满了708胶水，齿轮部分灌满润滑油）直接粘在上层船体上。
| 品类  |  价格 |
|---|---|
| 调漂桶  | 14包邮  |
| 齿轮水泵  | 3.66  |
| 9g舵机  | 7  |
| 电调  |  16x3  |
| 推进和侧推用的潜水泵  | 3.65x2  |
| 针管  | 1.5  |
| 防水开关  |  6 |
| 708胶水一管  |  2.6  |
| 胶管一米   | 2  |
| 电线若干。2mm摇臂一个  |  8元 |
|22mm七叶螺旋桨一个   | 28元  |

### [1/144比例，台风级核潜艇](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1561296&extra=page%3D1%26filter%3Dauthor%26orderby%3Ddateline)

无刷电机驱动，直接浸水（别款潜艇）
动力，两个395电机，胶封+顺滑油。直驱，两个28mm七叶桨。好像36mm的也放得进去
舵机胶封
磁控开关

### 遥控

[无名飞控 开源飞控 开源遥控器 STM32大功率NRF24L01多功能遥控器](https://item.taobao.com/item.htm?spm=a1z02.1.2016030118.d2016038.a5vX3Y&id=626446649474&scm=1007.10157.81291.100200300000000&pvid=3cfd6c1c-e816-4af4-adc6-e246731e93df)
[无名创新多功能开源遥控器用户手册](https://github.com/wustyuyi/HGS_HP/tree/master/HGS_HP_V1)
[DIY】开源遥控器X-RC](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1560377)
[开源自制的6通道航模遥控器(一) 超简单不超过100行代码](https://blog.csdn.net/weixin_42268054/article/details/105536264)
[Loli3](https://gitee.com/wooddoor/Loli3)

雷虎的是 75Mhz 最大下潜 10米（机械极限）
遥控距离 50米
速度3.5km、h
马达320Kv
电池5000mah
时间50min


据我所知，2.4g的0米
72mhz的1-2米
35mhz的3-5米
19mhz的10米

据了解，频率越低穿透力越好，信号传输距离越远。


35M或27M

6ex 72M 3米水深
一般由使用环境和电波波长，发射功率决定。波长越长穿透越好。

35mhz据网上传闻水下5米仍然有效

fm的比例控说实话对模拟电路技术要求很高，而且自己做了很容易频率不准。
现在很多单片机用2.4模块做，简单方便。


接收端和发射端都使用 SX1278，功率不同。  
采用 SX1278 方案，433MHz，水下测试1.5米正常。接收端：
![enter description here](https://LonlyPan.github.io/images/Posts/2020-10-13-遥控潜艇模型制作/1609779168146.png)
可将发射功率增大至 1W（再高有法律限制）增加深度（实际待测）。  **大功率发射器：**
![enter description here](https://LonlyPan.github.io/images/Posts/2020-10-13-遥控潜艇模型制作/1609779098754.png)

[本人潜艇小白，想请问大家的遥控潜艇都是用2.4G遥控吗](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1051654&extra=page%3D10%26filter%3Dauthor%26orderby%3Ddateline)

[【给潜艇装上PCM】Futaba 29MHz 多通道 遥控设备](http://bbs.5imx.com/viewthread.php?tid=403618&extra=page%3D1&_dsign=c460f703)

[打算制作自动深度仪和安全控制,有兴趣的朋友进](http://bbs.5imx.com/forum.php?mod=viewthread&tid=410171&extra=page%3D22%26filter%3Dauthor%26orderby%3Ddateline&_dsign=c0359e8e)

跟我在另一个帖子里预测的一模一样，越是现在的高端陀螺仪，采用了PID算法，效果越是不明显，因为PID是基于直升机尾翼的高反应速度而设计的，用在潜艇的水平控制器上不行，水下阻尼效果强，系统没有那么快的反应速度，而且陀螺仪是以操纵信号结束后的位置为中立点，而不是以水平位置为中立点，所以他的作用充其量就是缓解而不是保持
高端陀螺肯定是不行的，就算用futaba的结果也是一样，楼主不必试了

[新手制作红鲨潜艇-](http://bbs.5imx.com/forum.php?mod=viewthread&tid=524335&extra=page%3D20%26filter%3Dauthor%26orderby%3Ddateline&page=2) 
#### 遥控潜艇版实物图
初版：  
![enter description here](https://LonlyPan.github.io/images/Posts/2020-10-13-遥控潜艇模型制作/1609779325401.png)
改版，手机APP遥控
[开源！用手机开船是一种什么样的体验？](https://www.bilibili.com/video/BV1cV411S7nA)
![enter description here](https://markdown.xiaoshujiang.com/img/spinner.gif "[[[1609779374911]]]" )

#### 参考链接
[112通道船模遥控器？！魔方计划第四代作品发布！](https://www.bilibili.com/video/BV1Ht411j7pj)
[魔方计划！船模专用一体化控制器的前世今生](https://www.bilibili.com/video/av47573954)
[魔方二代开源地址](https://github.com/linzi0928/ModelShip_Controller-CUBE)

### 关于启动摇摆问题的讨论

http://bbs.5imx.com/forum.php?mod=viewthread&tid=1293524

### 鱼雷
[发射吧，鱼雷！](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1164699&extra=page%3D9%26filter%3Dauthor%26orderby%3Ddateline)
[1/35海豹潜艇鱼雷两连射](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1491602&extra=page%3D2%26filter%3Dauthor%26orderby%3Ddateline)
[如何制作一枚可以发射的电动鱼雷](https://www.bilibili.com/video/BV1s741187pQ/?spm_id_from=333.788.videocard.3)
[可发射鱼雷模型制作教程电容供电，YouTube转载 ユニオンテック频道](https://www.bilibili.com/video/BV1ZK4y147f6/?spm_id_from=trigger_reload)
[电子点火，水面发射-罗马湖舰队1/100台风级潜艇建造中](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1255282&extra=page%3D7%26filter%3Dauthor%26orderby%3Ddateline) 

[利用电磁炮原理可以垂直发射吧](http://bbs.5imx.com/forum.php?mod=viewthread&tid=984696&extra=page%3D11%26filter%3Dauthor%26orderby%3Ddateline)

[讨论潜艇模型如何在水下发射dao dan，我的构想](http://bbs.5imx.com/forum.php?mod=viewthread&tid=698490&extra=page%3D16%26filter%3Dauthor%26orderby%3Ddateline)
[关于潜艇发射dao dan的一点个人想法](http://bbs.5imx.com/forum.php?mod=viewthread&tid=481325&extra=page%3D21%26filter%3Dauthor%26orderby%3Ddateline&_dsign=32e10dd2)

[模型dao dan水下发射构想](http://bbs.5imx.com/forum.php?mod=viewthread&tid=476339&extra=page%3D21%26filter%3Dauthor%26orderby%3Ddateline&_dsign=24dbfbc3)

### 仿生鱼

[想建个仿生鱼潜水器讨论组](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1101521&extra=page%3D9%26filter%3Dauthor%26orderby%3Ddateline&page=2)
[简易机器鱼](http://bbs.5imx.com/forum.php?mod=viewthread&tid=928650&extra=page%3D12%26filter%3Dauthor%26orderby%3Ddateline)

## 制作-方案验证

### 水下超声波测距

[超声波测距仪原理图+PCB源文件+程序](https://www.cirmall.com/circuit/3873)

[超声波测距原理与制作](https://zhuanlan.zhihu.com/p/38453131)

[一体化超声波测距电路图](http://blog.sina.com.cn/s/blog_68541adc01017ujg.html)

[超声波测距论文(含原理图、程序)](https://wenku.baidu.com/view/8b83de27dd36a32d73758147.html)

[超声波测距电路](https://wenku.baidu.com/view/6f595d6f58fafab069dc024c.html)

### 加速度

![enter description here](https://LonlyPan.github.io/images/Posts/2020-10-13-遥控潜艇模型制作/1611278720894.png)

![enter description here](https://LonlyPan.github.io/images/Posts/2020-10-13-遥控潜艇模型制作/1611278748637.png)

### 磁力传感器

[Ultimate Sensor Fusion Solution - MPU9250](https://www.tindie.com/products/onehorse/ultimate-sensor-fusion-solution-mpu9250/)
[BNO055 VS 恩智浦 FXOS8700 + FXAS21002](https://forums.adafruit.com/viewtopic.php?f=8&t=140702)
[Guides for product: Adafruit Precision NXP 9-DOF Breakout Board](https://learn.adafruit.com/products/3463/guides)
[Overview](https://learn.adafruit.com/comparing-gyroscope-datasheets)
[BNO055 and MPU9250 "Roll" indication is correct in static position, wrong when moving.](https://github.com/kriswiner/BNO055/issues/11) 
[Next generation sensor platform ... MPU9250 vs. LSM9DS0](https://groups.google.com/g/diyrovers/c/TZJIvRdDrls/m/xOc2VxlyU_QJ)
[9 DoF Motion Sensor Bakeoff](https://github.com/kriswiner/MPU6050/wiki/9-DoF-Motion-Sensor-Bakeoff)
[LSM9DS1 vs. MPU-9250 vs. BMX055](https://github.com/kriswiner/MPU6050/issues/6)

### 开源控制器

- [Autopilot](https://ardupilot.org/copter/index.html)：开源固件
 [ArduPilot-Github](https://github.com/ArduPilot) 
- [Pixhawk](https://docs.px4.io/v1.9.0/en/)：软硬件标准，平行于Autopilot
[PX4 Autopilot User Guide](https://docs.px4.io/master/en/)
DS-012 Pixhawk Autopilot v6X Standard
- Holybro：无人机

#### 开源
[智能卡存储单元EEPROM，Flash和FRAM之间的性能比较](https://blog.csdn.net/badboy2008/article/details/6221787)
[FRAM 铁电存储器](https://www.cnblogs.com/yzl050819/p/4612984.html)
##### CUAV X7
[X7 Autopilot官网](http://doc.cuav.net/flight-controller/x7/en/x7.html)
[X7 Pro autopilot官网](http://doc.cuav.net/flight-controller/x7/en/x7-pro.html)
[CUAV X7](https://ardupilot.org/copter/docs/common-cuav-x7-overview.html) 
[开源GitHub](https://github.com/cuav/hardware/tree/master/X7_Autopilot)
**技术指标**
 - Processor
	 - 32-bit STM32H743 main processor
	 - 480Mhz / 1MB RAM / 2MB Flash
 - Sensors
	 - X7 : InvenSense ICM20689 加速度计 / 陀螺仪
	 - X7 Pro 用高 G 、高精度 InvenSense ADIS16470 加速度计/ 陀螺仪取代上述 IMU ，
	 - InvenSense ICM20649 加速度计 / 陀螺
	 - Bosch BMI088 加速度计/陀螺仪
	 - 2 MS5611 气压计
	 - RM3100 工业级磁力计

##### Nora Autopilot

[Nora Autopilot官网](http://doc.cuav.net/flight-controller/x7/en/nora.html)
[CUAV Nora Autopilot for PIX and APM 销售介绍](https://store.cuav.net/shop/nora/)
[CUAV Nora](https://ardupilot.org/copter/docs/common-cuav-nora-overview.html)   

诺拉是一种先进的自动驾驶仪，由CUAV独立设计。是CUAV的集成版，基本和CUAV X7一样，做成了一个板子。
[Nora Autopilot](https://ardupilot.org/copter/docs/common-cuav-nora-overview.html)

**技术指标**

 - Processor
	 - 32-bit STM32H743 main processor
	 - 480Mhz / 1MB RAM / 2MB Flash
 - Sensors
	 - InvenSense ICM20689 加速度计 / 陀螺仪
	 - InvenSense ICM20649 加速度计 / 陀螺仪
	 - Bosch BMI088 加速度计/陀螺仪
	 - 2 MS5611 气压计
	 - RM3100 工业级磁力计

#### 闭源

##### Holybro Durandal
[Holybro Durandal](https://ardupilot.org/copter/docs/common-durandal-overview.html)

**技术指标**

 - Processor
	 - 32-bit STM32H743 main processor
	 - 400Mhz / 1MB RAM / 2MB Flash
	 - 32-bit co-processor
 - Sensors
	 - InvenSense ICM20689 加速度计 / 陀螺仪
	 - Bosch BMI088 加速度计/陀螺仪
	 - MS5611 气压计
	 - IST8310 磁力计

##### Pixracer Pro
[Pixracer Pro](https://ardupilot.org/copter/docs/common-pixracer-pro.html)

**技术指标**

 - Processor:
	 - MCU - STM32H743IIK6
	 - 2MB flash allows full features of ArduPilot to be flashed
	 - 256KB FRAM - FM25V02-G
 - Sensors
	 -  加速度计 / 陀螺仪: Invensense ICM-20948 / Gyro / Mag (? KHz)
	 -  加速度计 / 陀螺仪: Invensense ICM-20602 Accel / Gyro (? KHz)
	 -  加速度计 / 陀螺仪: Bosch BMI088 Accel / Gyro (? KHz)
	 - 气压计: DSP310

### 开源遥控器2

[模型遥控器制式说明](https://cloud.tencent.com/developer/article/1144561)

#### 萝莉遥控

采用STC单片机，代码臃肿繁琐，落后。不采用。
- [wooddoor/Loli3](https://github.com/wooddoor/Loli3)
- [DIY萝丽航模遥控器制作！二代和三代PCB和固件分享！3代8通接收机，2代12通接收机，同时输出PPM和舵机信号PCB和固件分享！！！](https://www.geediy.com/post/21.html)
- [\[开源教程\] 转【我爱萝丽爱萝丽】震撼发布！第三代航模遥控器 DIY教程](https://www.moz8.com/thread-142710-1-1.html)
- [【参赛】DIY八通道开源萝莉航模遥控、接收(集成FPV数传)](http://www.crystalradio.cn/thread-1598574-1-1.html)

#### J20航模遥控器

[J20RC](https://github.com/J20RC)
[建军节献礼！J20航模遥控器开源项目系列教程（一）制作教程 | 基础版1.0发布，从0到1](https://www.bilibili.com/read/cv6986420)

#### X-RC

[【DIY】开源遥控器X-RC](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1560377)

#### X-CTRL
[X-CTRL](https://github.com/FASTSHIFT/X-CTRL)
[\[X-CTRL\] STM32开源航模遥控器](https://www.bilibili.com/video/BV1J5411W7zT/?spm_id_from=333.788.recommend_more_video.1)

#### OPEN-TX

[github-opentx](https://github.com/opentx/opentx)
[OpenTX遙控系統之開發環境搭建](https://www.itread01.com/content/1545420791.html)
[一步一步从0开始打造OpenTX一体控](http://bbs.5imx.com/forum.php?mod=viewthread&tid=1431181&extra=&page=1)
[OpenTx - Key Concepts](https://rc-soar.com/opentx/basics/index.htm)
[FrSky Taranis - OpenTX data flow diagrams](https://www.rcgroups.com/forums/thumbgallery.php?t=2609603&do=threadgallery&type=files&group=none&starter=no)
[bedienungsanleitungen OpenTxV2.1x und CompanionV2.1x X9E X9D 9XR-Pro 9XR Th9x](extension://bfdogplmndidlpjfhoijckpakkdjkkil/pdf/viewer.html?file=https%3A%2F%2Fwww.der-schweighofer.at%2Fpublic%2Ffiles%2Foriginal%2F1742%2F174210.pdf)


#### Multiprotocol TX Module

[DIY Multiprotocol TX Module](https://www.rcgroups.com/forums/showthread.php?2165676-DIY-Multiprotocol-TX-Module)


