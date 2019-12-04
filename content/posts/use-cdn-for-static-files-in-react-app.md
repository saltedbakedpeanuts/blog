---
title: "React App 使用 CDN 加载静态资源文件"
date: 2019-12-04T14:37:42+08:00
draft: true
toc: false
images:
tags: 
  - React
---

React App 构建后，默认从主机相对路径（如果配置了 `homepage`）或者根目录 `/` 下加载静态文件，加载路径为`/relativepath/static/filename` 或者 `/static/filename` 。



打包后的单个 JavaScript 文件大小达到 3 MB ，导致网页加载巨慢无比。



在 Nginx 开启 gzip 后情况有略微改善，但还是加载慢。



考虑到服务器带宽及延时限制，可以将静态资源文件分离部署到其它节点（如：CDN，这里只是换到另一台网络较好的服务器）来优化访问速度。



接下来更改 React App 打包后的默认静态文件加载路径。



1. 项目根目录创建 `.env.production`

```
PUBLIC_URL=url
```

之后构建时，静态资源的默认加载地址便是 `PUBLIC_URL/static/xxx` 。

2. 把构建完毕的静态资源文件部署到相应服务器上



参考

+ https://create-react-app.dev/docs/advanced-configuration/ 
+  https://create-react-app.dev/docs/adding-custom-environment-variables/#adding-development-environment-variables-in-env 