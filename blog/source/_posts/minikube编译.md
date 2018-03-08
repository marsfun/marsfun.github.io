### 由于GFW的缘故，minikube 使用很不方便，可以通过修改minikube源码中的镜像地址，重新编译生成可执行文件。
---
注意事项

1. 编译环境使用linux，  macos下编译有问题。

2. 找个批量ide进行替换，参考以下对应的替换字符串

    -  gcr.io/k8s-minikube/ 

        替换为  registry.cn-hangzhou.aliyuncs.com/google_containers/
    - k8s.gcr.io/

        替换为  registry.cn-hangzhou.aliyuncs.com/google_containers/

    - gcr.io/google_containers/

        替换为  registry.cn-hangzhou.aliyuncs.com/google_containers/

    - gcr.io/google-containers/

        替换为  registry.cn-hangzhou.aliyuncs.com/google_containers/


3. make cross