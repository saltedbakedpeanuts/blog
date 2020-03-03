---
title: "Golang float32 存入 MongoDB 后值会改变的问题"
date: 2020-03-03T11:47:56+08:00
draft: true
toc: false
images:
tags: 
  - golang
  - mongodb
---

在 Golang 中定义的数据结构大致如下，然后向 MongoDB 写入数据：

```golang
type Data struct {
  weight float32
}

d := &Data{weight: 200.75}

*mongo.Collection.InsertOne(..., d)
```

从 MongoDB 查询这条数据时，结果却类似下面这样：

```json
{
  weight: 200.74999999999...
}
```

Debug 发现，Golang 中 `float32` 的值显示正常，MongoDB 中数据记录的值异常，推断是写入 MongoDB 的途中发生了类型转换。

MongoDB 中只有 Double（64位）、Decimal（128位） 两种浮点类型，其中浮点类型默认为 Double 。

从 Golang 向 MongoDB 写入数据时发生了类型转换： `float32 -> float64` ，这导致了精度问题。

在 Golang 中使用与 MongoDB 相同大小的浮点数即可，即 `float64` 。