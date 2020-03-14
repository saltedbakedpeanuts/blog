---
title: "树莓派安装TensorFlow"
date: 2020-03-14T22:16:31+08:00
draft: true
toc: false
images:
tags: 
  - raspberrypi
  - tensorflow
---

树莓派通过 pip 安装的 TensorFlow 版本最高只支持到 1.14 。 

想安装 2.0 版本的 TF，只有自行手动编译构建或者找到别人已经编译构建好的安装包进行安装。

直接用别人编译好的就行，省时省力。

2.0 => https://github.com/lhelontra/tensorflow-on-arm/releases

最新版本 => https://tensorflow.google.cn/install/pip#%E8%BD%AF%E4%BB%B6%E5%8C%85%E4%BD%8D%E7%BD%AE

下载完成之后执行 `pip install xxx.whl` 即可完成安装。