---
title: "网课倍速播放"
date: 2020-03-03T10:31:24+08:00
draft: true
toc: false
images:
tags: 
  - web
  - 网课倍速播放
---

```js
// 方法 1（默认通用）
// 
// 根据播放器在 HTML 中的默认标签名查找元素
document.getElementsByTagName('video')[0].playbackRate=1.25

// 方法 2
// 
// 根据 ID 查找元素
//
// 以 Chrome 为例
// 按下 Ctrl + Shift + C
// 点击页面中的视频播放器
// 控制台中会自动选定该元素
// 查看该元素的 ID 填入下方命令中
document.getElementById('example_video_1_html5_api').playbackRate=1.25
```


参考：

+ https://blog.csdn.net/weixin_44205848/article/details/87929234

PlaybackRate 文档介绍：

+ https://developer.mozilla.org/zh-CN/docs/Web/Guide/Audio_and_video_delivery/WebAudio_playbackRate_explained