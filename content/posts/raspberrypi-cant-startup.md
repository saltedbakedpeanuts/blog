---
title: "树莓派4B无法正常启动"
date: 2020-03-26T10:49:36+08:00
draft: true
toc: false
images:
tags: 
  - raspberrypi
---

## 问题描述

更改树莓派配置文件后，树莓派无法正常启动。

当时执行的如下命令
```bash
sudo raspi-config
```

## 解决办法

（1）检查 config.txt 是否损坏。

拔下 SD 卡，使用读卡器在电脑上查看 config.txt 内容。

若提示文件已损坏，可在 Windows 磁盘列表中，右键选中 SD 卡查看属性，转到工具一栏，点击查错中的检查按钮，即可移除损坏的 config.txt 文件。

之后新建一个 config.txt 文件，写入相关配置信息后，启动树莓派即可。

默认配置文件代码：https://www.cnblogs.com/jikexianfeng/p/6108074.html


（2）检查外围设备是否故障

拔掉摄像头、 USB 键盘等设备后尝试重启。
