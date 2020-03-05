---
title: "videojs 无法播放 rtmp 直播流，报错 FLASH: rtmpconnectfailure"
date: 2020-03-05T11:30:38+08:00
draft: true
toc: false
images:
tags: 
  - rtmp
---
## 解决办法

无房间名，请在 URL 地址尾部添加 `/` 符号。

有房间名，在上述基础上添加房间号码。

```js
// 无房间名（错误）
'rtmp://example.com/mylive'

// 无房间名（正确）
'rtmp://example.com/mylive/'

// 有房间名（正确）
'rtmp://example.com/mylive/666666'
```

## 分析记录

### RTMP URL 格式

问题原因在于 URL 格式错误，但是通过 VLC 拉流显示正常。

videojs 和 VLC 对 RTMP URL 的解释逻辑不一样？

[Nginx RTMP Module URL format](https://github.com/arut/nginx-rtmp-module#rtmp-url-format):
```
rtmp://rtmp.example.com/app[/name]
```

我对这串 URL 的理解是
```
app => 程序名（斗鱼、BILI……）

name => 房间号（666666、888888）
```

进行推流测试时，我只填写了应用名称，房间名为空
```
rtmp://example.com/mylive
```

按理说应该不用添加尾部 `/` ……

---
### 翻阅 videojs-flash 源码

[处理 URL 字符串的函数](https://github.com/videojs/videojs-flash/blob/3141dc33f7d91be72afba26066dd06376519cccd/src/rtmp.js#L52)

截取部分代码如下
```js
// Look for the normal URL separator we expect, '&'.
// If found, we split the URL into two pieces around the
// first '&'.
let connEnd = src.search(/&(?![\w-]+=)/);
let streamBegin;

if (connEnd !== -1) {
  streamBegin = connEnd + 1;
} else {
  // If there's not a '&', we use the last '/' as the delimiter.
  connEnd = streamBegin = src.lastIndexOf('/') + 1;
  if (connEnd === 0) {
    // really, there's not a '/'?
    connEnd = streamBegin = src.length;
  }
}
```

URL 分为 `connection` 和 `stream` 两部分。

`&` 之后均为 `stream` 信息，如若不存在 `&` 符号，则以 最后一个  `/` 为分隔符。

那么，之前出现的问题就可以理解为：videojs 从 URL 中提取了错误的 `connection` 和 `stream` 。

我们按照该部分代码的逻辑，对 URL 进行转换提取，得到如下结果
```js
// input
URL:'rtmp://example/mylive'

// output
connection: "rtmp://example/"
stream: "mylive"

// input
URL:'rtmp://example/mylive/'

// output
connection: "rtmp://example/mylive/"
stream: ""
```

头脑有些混乱……

URL 结构为 `{connection}{stream}`

我们通过指定的 `connection` 信息连接到指定服务器，再通过 `stream` 提供的信息（房间号等）进入指定直播间。

`stream` 信息使用 `&` 或者 `/` 附加在 `connection` 后面。

举个例子
```js
//进入 example.com/mylive 服务器上的 666666 号直播间

'rtmp://example.com/mylive/666666'

'rtmp://example.com/mylive&666666'
```