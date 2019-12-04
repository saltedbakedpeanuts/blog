---
title: "React App 中开启 Antd 的按需加载"
date: 2019-12-02T15:36:27+08:00
draft: true
toc: false
images:
tags: 
  - React
  - Antd
---

1. `npm run eject`
2. 编辑 `config/webpack.config.js` 将 `babelrc: false` 更改为 `true`
3. 编辑 `package.json`，添加如下内容

```javascript
"babel": {
    "presets": [
        "react-app"
    ],
        "plugins": [
            [
                "import",
                {
                    "libraryName": "antd",
                    "libraryDirectory": "es",
                    "style": "css"
                }
            ]
        ]
}
```

参考：

+ https://cloud.tencent.com/developer/article/1467366 
+ https://www.jianshu.com/p/a25ba1adeda2 