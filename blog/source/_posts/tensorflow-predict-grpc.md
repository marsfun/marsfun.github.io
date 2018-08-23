---
title: tensorflow-predict-grpc
date: 2018-08-23 18:39:12
categories: 
- 技术
- tensorflow
tag: [grpc,机器学习]
---


# client通过 grpc 调用tensorflow-serving


* 注意点 1- 返回数据的解析
```python
scores = r.outputs['scores'].float_val
lables = r.outputs['labels'].string_val
```
    *   

* 注意点 2- 异常捕获
        
    * code = UNAVAILABLE 时，一般多是请求的地址有误，如端口错误，signature 错误等.
```python
try:
    r = classify(path + filename, server, port, timeout)
except grpc.RpcError as err:
    status = 'error'
    if err.code() == grpc.StatusCode.UNAVAILABLE:
        msg = 'connect failed.'
except Exception as err:
    status = 'error'
    msg = 'grpc error: '+err.code()
```
      