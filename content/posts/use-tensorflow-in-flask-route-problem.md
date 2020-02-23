---
title: "Flask中使用TensorFlow出现ValueError: Tensor is not an element of this graph"
date: 2020-02-23T12:39:26+08:00
draft: true
toc: false
images:
tags: 
  - tensorflow
  - flask
---

## 问题描述

在 tensorflow 中训练好模型后，准备用 flask 搭建 web 服务提供模型预测功能。

代码结构如下：
```python
model = load_model()

@app.route('/upload', methods=['post'])
def upload():
	model.predict()
```

运行时提示：
```
ValueError: Tensor is not an element of this graph
```

## 解决办法

1）在全局范围内使用 Graph、Model

```python
graph = tf.get_default_graph()
model = load_model()

@app.route('/upload', methods=['post'])
def upload():
	with: graph.as_default_graph（）
	model.predict()
```

2）使用 tensorflow 2.0.0 以上版本 （简单方便）

## 分析记录

> 以下均为瞎猜

模型在程序初始阶段载入，它应该创建了一个图。

路由方法中，每次收到请求然后调用模型预测方法，此时它找不到先前创建的图，所以再次创建了一个图。

两个图互无关联，导致错误抛出。

使用模型时，在范围内将图指定某个图即可解决问题。



TensorFlow 2.0 版本后引入了 Eager Mode ，虽然我也不知道这是个啥，不过好像时改变了 Graph 处理相关的概念。



待补充修正。

## 参考资料

+ https://stackoverflow.com/questions/56872145/how-to-share-a-tensorflow-keras-model-with-a-flask-route-function