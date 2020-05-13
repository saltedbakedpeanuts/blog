---
title: "基于树莓派4B的4G路由器"
date: 2020-05-11T23:03:50+08:00
draft: true
toc: false
images:
tags: 
  - raspberrypi
---
## 前言（废话）

公寓宽带质量太差，延迟高丢包高，唯一用处就是用来下载东西。移动数据网络访问多数网站时都能正常打开，但是屋内信号差。

手持小米8，很多时候4G上网都成问题，更有甚者——信号差到无法拨打电话……

相反，之前试用过荣耀9X，发现同等环境下不仅能上网，而且还挺流畅，在小米8打不开网页的情况下，荣耀9X下载App速度能达到2M/s。

2020年，5G正呈现铺天盖地气势，手机价格也有了大的跃进……我不是很情愿为一部手机支出一台笔记本电脑的费用。

之前手头有个为树莓派设计的带有华为4G模块的USB网卡，但是我都多数时间只是用来插电脑上进行移动网络上网。其实我更希望手机、平板、电脑都能同时使用这个移动网络上网。于是诞生了用树莓派做个4G路由的想法。

## 实现步骤

### 硬件准备

> 我并不是为了实现 4G 路由功能去买的相关配件，相反因为它们都处于闲置状态，所以萌生了这个 DIY 想法。

硬件设备列表

+ 树莓派4B - 250 RMB
+ Mcuzone 树莓派 4G 模块套餐（含华为 ME909s-821a 模块、天线、驱动板） - 260 RMB

Mcuzone 4G 套餐价格偏贵，实际上可以单独购买 ME909s-821a 模块（140 RMB）以及 4G 模块转接板（30 RMB）进行搭配使用。

### 网卡拨号

网卡拨号操作主要分为 3 种方式:

+ MBIM
+ NCM
+ PPP

首先将 USB 网卡插入树莓派后，通过 `lsusb` 检查设备是否被正常识别，输出结果种应当包含 `Huawei Technology...` 字段。

为了选择合适的拨号方式，我们需要先使用 `ls /dev | grep cdc-wdm*` 检查网卡是否以 MBIM 模式连接到系统。

如果输出结果出现 `cdc-wdm0` 类似字段，则选用 MBIM 方式进行拨号上网。

否则输入 `ls /dev | grep ttyUSB*` 检查网卡是否被识别成了虚拟串口模式。

如果出现了 `ttyUSB0 ttyUSB1 ttyUSB2 ttyUSB3 ttyUSB4` 字段，则表明网卡为虚拟串口模式，可采用 NCM 或者 PPP 方式进行拨号上网。

#### MBIM

首先安装依赖并新建配置文件
```
# 安装依赖
sudo apt install libmbim-utils -y

# 编辑配置文件
sudo vim /etc/mbim-network.conf
```

向配置文件写入内容
```
APN=internet
APN_USER=
APN_PASS=
APN_AUTH=
PROXY=yes
```

连接至网络
```
sudo mbim-network /dev/cdc-wdm0 start
```

> 试过了 2019-07 ~ 2020-02 版本的 Raspbian 系统，USB 网卡插上树莓派后均始终无法识别成 MBIM 虚拟网卡，只能看到 ttyUSB 规格设备。

#### NCM方式

切换拨号模式及连接至网络
```
# 切换拨号模式
sudo echo 'AT^NDISDUP=1,1' > /dev/ttyUSB2
# 获取 IP
sudo dhclient usb0 -v
```

检查网络接口情况，注意查看 `usb0` （一般是这个）接口信息
```
ifconfig
```

#### PPP方式
> 脚本文件地址👇
> 
> 链接：https://pan.baidu.com/s/1rLvY6MfnY9XpNyKg1IGHyw 提取码：coxd 

安装依赖
```
sudo apt install pppd -y
```

复制脚本文件至 pppd 下目录
```
sudo cp huawei-dial huawei-chat /etc/pppd/peers
```

连接至网络
```
sudo pppd call huawei-dial &
```

检查网络接口，注意查看 `ppp0` （一般是这个）接口信息
```
ifconfig
```

## 开启并设置热点

`wlan0` 口默认是用来连接 WiFI 的，它在请求连接时会使用 DHCP 方式从目标路由获取 IP 地址。现在恰恰相反，我们需要使用这个接口为无线客户终端提供访问入口，提供 IP 地址分配及其它服务，所以需要给它设置一个固定的 IP 地址。

打开树莓派默认 dhcp 配置文件
```
sudo vim /etc/dhcpcd.conf
```

在文件末尾添加如下内容
```
interface wlan0
    static ip_address=192.168.1.1/24
    nohook wpa_supplicant
```

默认情况下 2 个网络之间是不相通的。现在我们的情况是使用 `usb0` 接口连接至互联网，使用 `wlan0` 接口与众多客户端相连接。为了使得无线网络具备网络访问能力，我们需要将 `wlan0` 端口的数据转发至 `usb0` 接口发送出至互联网上。

新建一个系统配置文件
```
sudo vim /etc/sysctl.d/routed-ap.conf
```

添加如下内容
```
net.ipv4.ip_forward=1
```

配置防火墙的转发与伪装规则
```
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

防火墙规则持久化（否则每次重启后都会失效）
```
sudo DEBIAN_FRONTEND=noninteractive apt install -y netfilter-persistent iptables-persistent
sudo netfilter-persistent save
```

为无线热点配置 DHCP 及 DNS 服务
```
# 安装 dnsmasq
sudo apt install dnsmasq -y

# 备份默认配置文件
sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig

# 新建空的配置文件
sudo nano /etc/dnsmasq.conf
```

写入以下内容
```
interface=wlan0
dhcp-range=192.168.1.2,192.168.1.254,255.255.255.0,24h
domain=wlan
address=/gw.wlan/192.168.1.1
```

为确保频率使用不被限制住
```
sudo rfkill unblock wlan
```

配置无线热点信息
```
sudo vim /etc/hostapd/hostapd.conf
```

写入以下内容（自行更改 `ssid` 及 `wpa_passphrase`）
```
country_code=US
interface=wlan0
ssid=NameOfNetwork
hw_mode=g
channel=7
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=AardvarkBadgerHedgehog
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```

hw_mode可选值

+ a = IEEE 802.11a(5GHz)
+ b = IEEE 802.11b(2.4GHz)
+ g = IEEE 802.11g(2.4GHz)
+ ad = IEEE 802.11ad(80GHz)

重启完成配置
```
sudo reboot
```

使用其它设备搜索无线网络进行连接测试

> 上述主要内容均摘抄至网络，只是稍微添加了点自己的理解。

## 参考资料

+ http://www.mcuzone.com/forum/forum.php?mod=viewthread&tid=34042&extra=page%3D2
+ http://www.mcuzone.com/forum/forum.php?mod=viewthread&tid=34040&extra=page%3D2
+ https://www.raspberrypi.org/documentation/configuration/wireless/access-point-routed.md

## 细节理解

不理解 `systemctl unmask hostapd` 这行命令？

https://blog.csdn.net/stpice/article/details/104569146/