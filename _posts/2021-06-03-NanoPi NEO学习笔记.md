---
layout: post
show_title: "2021-06-03-NanoPi NEO学习笔记"
title: "2021-06-03-NanoPi NEO学习笔记"
date: 2021-06-03
last_modified_at: 2021-06-03
categories: c&c++
---

[NanoPi NEO/zh](https://wiki.friendlyarm.com/wiki/index.php/NanoPi_NEO/zh)






# 基础配置
[1.【NanoPi NEO教程】烧写系统镜像](https://www.dreamtoptech.com/archives/39)
[NanoPi-NEO——初体验](https://blog.csdn.net/lu_embedded/article/details/70676835)
[Nanopi Neo Core上网记（日积月累计划）](https://blog.csdn.net/watershade2010/article/details/113404521)

[nanopi--基础配置](https://blog.csdn.net/qq_30932167/article/details/81507544)
[NanoPi-NEO——系统配置工具npi-config](https://luhuadong.blog.csdn.net/article/details/70834346)
## 系统安装

## 串口连接

## 网络连接

修改ip后，无法使用putty连接，原因未知，暂且使用随机地址，不做改动

1. 使用 ifconfig 查看IP地址
 ```
pi@NanoPi-NEO:~$ ifconfig
eth0      Link encap:Ethernet  HWaddr 02:81:2a:c1:96:86
          inet addr:192.168.1.199  Bcast:192.168.1.255  Mask:255.255.255.0
          inet6 addr: fe80::5d34:7291:2ab9:4908/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:253 errors:0 dropped:0 overruns:0 frame:0
          TX packets:45 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:26058 (26.0 KB)  TX bytes:4177 (4.1 KB)
          Interrupt:40

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:259 errors:0 dropped:0 overruns:0 frame:0
          TX packets:259 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:19424 (19.4 KB)  TX bytes:19424 (19.4 KB)
```
上述 `192.168.1.199`就是IP地址，每个人的ip地址可能会不同，已自己的为准。

再输入命令：
```
sudo nmcli connection modify 'Wired connection 1' connection.autoconnect yes ipv4.method manual ipv4.address 192.168.1.199/24 ipv4.gateway 192.168.1.255 ipv4.dns 192.168.1.255
```
上述中的 `192.168.1`这三个数一定要和你刚查到的ip地址前三位一致，后面的 `199` 、 `255` 自己随意设定。但要保证 `199` 这里的数字不能和你电脑上已有的ip地址冲突，所以越接近255越好。
再输入命令：
```
sudo reboot
```
重启系统，完成配置。

- [Use NetworkManager to configure network settings/zh](https://wiki.friendlyarm.com/wiki/index.php/Use_NetworkManager_to_configure_network_settings/zh)
[2.【NanoPi NEO教程】网线登录 putty 登录配置方法](https://www.dreamtoptech.com/archives/42)
## OLED驱动

记得开全局翻墙，不然github无法下载

[NanoHat OLED/zh](https://wiki.friendlyarm.com/wiki/index.php/NanoPi_NEO/zh)

<!--more-->
