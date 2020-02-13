---
title: "PyCharm智能补全"
date: 2020-02-10T11:57:28+08:00
draft: true
toc: false
images:
tags: 
  - Python
  - PyCharm
---

## 问题描述

PyCharm 中引用第三方库时，智能提示/补全经常会失效。

## 解决办法

Python 是动态类型语言，其变量类型只有在运行时才得到确定。IDE 在代码编写阶段无法推断出三方库中的变量类型。

显示指定变量类型可以帮助编译器找到相关类型定义的方法。不同于静态语言，Python 引入的类型注解只是为了方便三方工具（IDE等）的使用，比如：它并不会真的限制函数接收参数的类型。

```python
import tensorflow as tf
from typing import Tuple


# 指定函数输入参数类型
def f(a: str, b: list):
    return


# 指定函数返回值类型


# 单个返回值类型
def f() -> list:
    return []


# 多个返回值类型
def f() -> Tuple[str, list]:
    return '', []


# 类型注解
model = tf.keras.models.load_model()  # type: tf.keras.Model
```

## 参考

+ https://stackoverflow.com/questions/40181344/how-to-annotate-types-of-multiple-return-values
+ https://stackoverflow.com/questions/33945261/how-to-specify-multiple-return-types-using-type-hints
+ https://blog.jetbrains.com/pycharm/2015/11/python-3-5-type-hinting-in-pycharm-5/