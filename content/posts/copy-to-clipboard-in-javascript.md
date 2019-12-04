---
title: "JavaScript 复制内容到剪切板"
date: 2019-12-02T15:18:51+08:00
draft: true
toc: false
images:
tags: 
  - JavaScript
---

1. 浏览器 Clipboard API

> 仅限在安全环境下使用，如 HTTPS

```javascript
// 返回 Promise
navigator.clipboard.writeText("something").then().catch();
```

参考：https://developer.mozilla.org/en-US/docs/Web/API/Clipboard 

2. Clipboard Copy 依赖包

Github：https://github.com/feross/clipboard-copy

```javascript
// 返回 Promise
const copy = require("cilpboard-copy");
copy("something").then().catch();
```
