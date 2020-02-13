---
title: "Hugo文章中图片路径问题"
date: 2020-02-11T20:33:49+08:00
draft: true
toc: false
images:
tags: 
  - hugo
---
## 问题描述

+ 在 Typora 中编辑文章插入图片能够显示，而发布后网页中的图片不能正常显示（路径错误）。

+ 使用站点根目录（`/`）引用图片可以正常加载显示，但是无法在 Typora 编辑器中显示图片。

## 解决办法

在文章中使用相对路径引用图片时，开启 uglyurls 模式。

编辑 `config.toml` ，插入一行 `uglyurls = true` 即可 。

## 分析记录

Hugo 有个很蛋疼的问题就是：文章中引用图片的路径。

情况是这样的，我平常都用 Typora 编辑 Markdown 内容，习惯在 `posts/images` 下存放所有文章的图片，然后使用相对路径进行引用，此时文章和 `images` 处于同一目录下，故使用 `./images/xxx.png` 引用图片没问题。 但是发布时文章的路径结构就变成了 `/posts/xxx/index.html`，图片的路径依然是 `/posts/images` 。这时浏览网页出现找不到相关图片的错误。

我在网络上寻找解决方案，但是都大同小异，无法从根本上解决我的问题。

将图片统一放置在 hugo site 根目录的 `static` 或者 `content` 目录下，然后在文章中直接用 `/` 路径引用

```
图片存放路径
/static/images
or
/content/images

图片引用路径
![](/xxx.png)
```

虽然这样做在网页中能够正常显示图片。但是在 Typora  里编辑文章的时候，是无法根据这个相对路径找到图片的。

为了能在 Typora 中正常显示图片，肯定只能用相对文章的图片路径。

我将图片放置在文章同级目录下的 `images` 文件夹下。

而问题在于 hugo 根据文章生成网页时，文章的路径会从 `/posts-name.md` 变成 `/posts-name/index.html`，这就导致图片与文章的相对路径发生改变，从而造成图片加载失败。

只要发布后的文章与图片相对路径不变就没问题。

Hugo 默认使用 Pretty Urls 模式生成网页，此时都是以目录形式的路径访问内容，eg: `/posts/how-to-use-git` 。此外还有 Ugly Urls ，也就是用传统的 `/posts/how-to-use-git.html` 路径去访问内容。

Pretty Urls 模式会为网页额外创建一层目录，以此达到美化效果。

这里启用 Ugly Urls 模式即可解决问题。

## 参考

+ https://gohugo.io/content-management/urls/#ugly-urls
+ https://github.com/coderzh/gohugo.org/issues/15#issuecomment-470374514
+ [https://creaink.github.io/post/Devtools/Hugo/Hugo-intro.html#%E5%AD%98%E5%9C%A8%E7%9A%84%E9%97%AE%E9%A2%98](https://creaink.github.io/post/Devtools/Hugo/Hugo-intro.html#存在的问题)