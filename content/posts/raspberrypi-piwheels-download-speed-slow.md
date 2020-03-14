---
title: "树莓派 PiWheels 仓库下载速度慢"
date: 2020-03-14T21:43:52+08:00
draft: true
toc: false
images:
tags: 
  - raspberrypi
---


已经将树莓派默认 pip 源换成了 Aliyun ，但是开发环境中大多数的安装包还是只有 PiWheels 上才有。

每次从 PiWheels 下载只有 10-20KB/s 。其服务器地址在国外，这是延迟高、被限速的主要原因。

上网搜索后，发现可以尝试用多线程工具下载。

因为如果限速是针对于单个连接而言，那么连接越多速度就会上去了。

Linux 下多线程下载工具有：

+ aria
+ axel

由于本地网络状况不好，所以我是在服务器上下载好后回传至本地，再上传至树莓派。

Server -> Local -> Raspberry


服务器端是 CentOS 系统。

以 axel 为例

通过默认的 yum 安装 axel 会因为版本过低而导致下载时出现 Too many redirects. 的错误。
所以需要下载源码本地构建安装较新版本。

https://github.com/axel-download-accelerator/axel/releases

```bash
# -n 连接数量
axel -n 20 https://www.piwheels.com/xxxxx
```

多线程下载速度可以达到 100-150KB/s ，虽然仍旧是慢，但总比 10KB/s 的龟速要好。