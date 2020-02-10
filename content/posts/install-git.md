---
title: "安装Git"
date: 2020-02-08T13:43:18+08:00
draft: false
toc: false
images:
tags: 
  - git
---

## Yum源安装


```bash
yum install -y git
```

但是这样安装软件的版本比较老……

查看 yum 源中 git 版本：

```bash
yum list | grep git

git.x86_64                                1.8.3.1-21.el7_7               updates
...
```

> 截至 2020年2月8日，此时 git 最新版本号为 2.25.0。

## 下载源码压缩包安装

Git 各版本压缩包索引：https://mirrors.edge.kernel.org/pub/software/scm/git/


```bash
# 以最新版（2.25.0）为例
wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.25.0.tar.xz
tar xvf git-2.25.0.tar.xz -C /home
cd /home/git-2.25.0

# 安装依赖，免得 make 过程中频繁出错中断
yum install -y openssl-devel libcurl-devel expat-devel

# 构建后执行安装
# 默认安装到 ~/ 目录
make
make install
```