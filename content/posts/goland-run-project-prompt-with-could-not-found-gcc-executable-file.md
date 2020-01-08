---
title: "GoLand 运行项目提示无法找到 gcc 可执行文件"
date: 2020-01-07T19:44:15+08:00
draft: true
toc: false
images:
tags: 
  - GoLand
---

## 问题描述

在 Go 项目中使用了最新的 mongo-go-driver 后，在 GoLand 中 Run 项目会提示：

```bash
running gcc failed: exec: "gcc": executable file not found in %PATH%
```

更新后的 mongo-go-driver 居然要用到 gcc ？

## 解决思路

### 安装 GCC Complier

mingw-w64 是 Windows 下的 gcc 编译器。

下载地址：http://www.mingw-w64.org/doku.php/download/mingw-builds

### 配置环境变量

安装完成后，在系统环境变量 PATH 中添加 mingw-w64 下 bin 目录。

打开 PowerShell ，输入 `gcc --version` 检查环境变量是否配置成功。

---

### 如果问题仍然存在……

> 1. 回到 GoLand，再次尝试 Run 项目，仍旧提示无法在 PATH 中找到 gcc 命令。
> 2. 打开 GoLand 中的 Terminal ，输入 gcc 命令，提示找不到该命令。
> 3. 使用 PowerShell 和 Git Bash 都能顺利 Build 项目。

#### 修改 GoLand 的 Run/Debug Configuration 

在 Enviroment 中添加 User enviroment variables 。

新增变量 PATH ，值填写为 mingw-w64 下的 bin 目录。

尝试 Run 项目，此时应该能够启动成功。