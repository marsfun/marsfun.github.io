---
title: spark-on-kubernetes_serviceaccount_error
date: 2018-04-26 16:05:52
categories: 
- 技术
- 问题解决
tag: [kubernetes]
---

使用spark-2.3.0 版本，在k8s 1.9 上测试。

```bash
bin/spark-submit \
    --master k8s://https://10.0.1.4:6443 \
    --deploy-mode cluster \
    --name spark-pi \
    --class org.apache.spark.examples.SparkPi \
    --conf spark.executor.instances=5 \
    --conf spark.kubernetes.container.image=10.0.1.4:5000/spark:v2.3.0 \
    --conf spark.kubernetes.authenticate.driver.serviceAccountName=spark \
    local:///opt/spark/examples/jars/spark-examples_2.11-2.3.0.jar

    --jars local:///opt/spark/examples/jars/spark-examples_2.11-2.3.0.jar \
```

 is forbidden: User "system:serviceaccount:default:default" cannot get pods in the namespace "default"
解决:
1）
```bash
 kubectl create serviceaccount spark
 kubectl create clusterrolebinding spark-role --clusterrole=edit --serviceaccount=default:spark --namespace=default
 ```
2） 
命令参数增加  --conf spark.kubernetes.authenticate.driver.serviceAccountName=spark 
 