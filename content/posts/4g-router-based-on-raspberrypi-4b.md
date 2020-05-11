---
title: "基于树莓派4B的4G路由器"
date: 2020-05-11T23:03:50+08:00
draft: true
toc: false
images:
tags: 
  - raspberrypi
---
## 序

公寓宽带质量太差，延迟高丢包高，唯一用处就是用来下载东西。移动数据网络访问多数网站时都能正常打开，但是屋内信号差。

手持小米8，很多时候4G上网都成问题，更有甚者——信号差到无法拨打电话……

相反，之前试用过荣耀9X，发现同等环境下不仅能上网，而且还挺流畅，在小米8打不开网页的情况下，荣耀9X下载App速度能达到2M/s。

2020年，5G正呈现铺天盖地气势，手机价格也有了大的跃进……我不是很情愿为一部手机支出一台笔记本电脑的费用。

之前手头有个为树莓派设计的带有华为4G模块的USB网卡，但是我都多数时间只是用来插电脑上进行移动网络上网。其实我更希望手机、平板、电脑都能同时使用这个移动网络上网。于是诞生了用树莓派做个4G路由的想法。

## 4G USB 网卡拨号

### PPP方式

> 需准备好 1> huawei-dial 2> huawei-chat 脚本文件

```
sudo pppd call huawei-dial &
```

http://www.mcuzone.com/forum/forum.php?mod=viewthread&tid=34040&extra=page%3D2

### MBIM方式

> 试过了 2019-07 ~ 2020-02 版本的 Raspbian 系统，USB 网卡插上树莓派后均始终无法识别成 MBIM 虚拟网卡，只能看到 ttyUSB 规格设备

http://www.mcuzone.com/forum/forum.php?mod=viewthread&tid=34042&extra=page%3D2

### NCM方式

```
# 切换拨号模式
sudo echo 'AT^NDISDUP=1,1' > /dev/ttyUSB2
# 获取 IP
sudo dhclient usb0 -v
```

## 将树莓派设置成路由无线热点

https://www.raspberrypi.org/documentation/configuration/wireless/access-point-routed.md

不理解 `systemctl unmask hostapd` 这行命令

unmask 与 disable 区别
https://blog.csdn.net/stpice/article/details/104569146/