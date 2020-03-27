---
title: "Web文件索引服务"
date: 2020-03-27T12:51:59+08:00
draft: true
toc: false
images:
tags: 
  - webserver
---

搭建 Web 文件索引服务有两种思路：

（1）nginx + php + [h5ai](https://github.com/lrsjng/h5ai)

优点：样式美观，可以对图片预览查看，会展示文件拓展类型对应图表……

缺点：需要安装 PHP

（2）nginx + [ngx-fancyindex](https://github.com/aperezdc/ngx-fancyindex)

优点：安装软件少，只有 nginx 和 fancyindex 拓展模块

缺点：自行编译安装包