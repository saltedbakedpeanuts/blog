---
title: "TensorFlow安装错误"
date: 2020-02-01T12:57:26+08:00
draft: true
toc: false
images:
tags: 
  - tensorflow
---


安装 TensorFlow 2.1.0 后，尝试运行程序：

```python
import tensorflow as tf
print(tf.__version__)
```

然后出现如下错误信息：
```
Traceback (most recent call last):
  File "C:\Users\xiexi\AppData\Local\Programs\Python\Python37\lib\site-packages\tensorflow_core\python\pywrap_tensorflow.py", line 58, in <module>
    from tensorflow.python.pywrap_tensorflow_internal import *
  File "C:\Users\xiexi\AppData\Local\Programs\Python\Python37\lib\site-packages\tensorflow_core\python\pywrap_tensorflow_internal.py", line 28, in <module>
    _pywrap_tensorflow_internal = swig_import_helper()
  File "C:\Users\xiexi\AppData\Local\Programs\Python\Python37\lib\site-packages\tensorflow_core\python\pywrap_tensorflow_internal.py", line 24, in swig_import_helper
    _mod = imp.load_module('_pywrap_tensorflow_internal', fp, pathname, description)
  File "C:\Users\xiexi\AppData\Local\Programs\Python\Python37\lib\imp.py", line 242, in load_module
    return load_dynamic(name, filename, file)
  File "C:\Users\xiexi\AppData\Local\Programs\Python\Python37\lib\imp.py", line 342, in load_dynamic
    return _load(spec)
ImportError: DLL load failed: 找不到指定的模块。
```

大致意思是 `_pywrap_tensorflow_internal` 模块导入 DLL 失败。

**解决办法：**

降级至 2.0.0 版本。

```bash
pip install tensorflow==2.0.0
```

参考：

+ https://www.tensorflow.org/install/errors
+ https://github.com/tensorflow/tensorflow/issues/22794#issuecomment-572606630