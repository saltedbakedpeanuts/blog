---
title: "逻辑比较判断时，null运算符放在左侧的好处"
date: 2020-03-30T15:51:27+08:00
draft: true
toc: false
images:
tags: 
  - java
---

看 [solo 源码](https://github.com/88250/solo/blob/ed33d2bf39517ab981f42204c14bc468574f780a/src/main/java/org/b3log/solo/Server.java#L145) 时，很好奇为什么在进行逻辑判断时，null 总是放在运算符左边。

在输入逻辑等符（==）号时，如果因为失误少打了一个 = 符号，那么原本的判断操作就会变成赋值操作，从而打乱了原有的业务逻辑。

```java
// 常规写法
if (param == null){}

// 常规写法下手误，少打个 = 号。此时编译器不会提示错误，程序会继续执行。但这违背了业务逻辑，因为它总是表示为 false 。
if (param = null){}


// 非常规写法。null 置放运算符左侧。
if (null == param){}

// 非常规写法下，手误少打个 = 号。此时编译器会报错，因为尝试将变量值赋值给 null 。
if (null = param){} 
```

## 参考
+ https://www.cnblogs.com/codingmengmeng/p/9985061.html
+ https://blog.csdn.net/xyc_csdn/article/details/66472191
+ https://bbs.csdn.net/topics/360168351