---
title: minikube启动问题
date: 2018-03-09 11:10:00
categories: 
- 技术
- 问题解决
tag: [minikube]
---

# minikube 版本说明

- 当前2018-03 好用的版本为 0.24.0
- 0.24.1 0.25.0 存在bug，启动后healthz 检查失败 ，此外 以docker tag 更换镜像的方式无法 启动 google addons等容器。
- 一般情况下 minkube启动后，容器创建不成功"containercreating" 都是镜像在墙外拉不下来的缘故。

##  0.24.0版本 从aliyun 拉取 相应的镜像，以及tag 命令
---
用到的镜像
- gcr.io/google_containers/k8s-dns-dnsmasq-nanny-amd64:1.14.5
- gcr.io/google_containers/k8s-dns-kube-dns-amd64:1.14.5
- gcr.io/google_containers/k8s-dns-sidecar-amd64:1.14.5
- gcr.io/google_containers/kubernetes-dashboard-amd64:v1.6.3
- gcr.io/google_containers/pause-amd64:3.0
- gcr.io/google-containers/kube-addon-manager:v6.4-beta.2
- gcr.io/k8s-minikube/storage-provisioner:v1.8.0
<!-- more -->

```bash

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/storage-provisioner:v1.8.0 
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/k8s-dns-kube-dns-amd64:1.14.5
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/k8s-dns-sidecar-amd64:1.14.5
docker pull registry.cn-hangzhou.aliyuncs.com/google-containers/pause-amd64:3.0
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/k8s-dns-dnsmasq-nanny-amd64:1.14.5
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-addon-manager:v6.4-beta.2
docker pull registry.cn-hangzhou.aliyuncs.com/google-containers/kubernetes-dashboard-amd64:v1.6.3

docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/storage-provisioner:v1.8.0           gcr.io/k8s-minikube/storage-provisioner:v1.8.0
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/k8s-dns-kube-dns-amd64:1.14.5        gcr.io/google_containers/k8s-dns-kube-dns-amd64:1.14.5
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/k8s-dns-sidecar-amd64:1.14.5         gcr.io/google_containers/k8s-dns-sidecar-amd64:1.14.5
docker tag registry.cn-hangzhou.aliyuncs.com/google-containers/pause-amd64:3.0                      gcr.io/google_containers/pause-amd64:3.0
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/k8s-dns-dnsmasq-nanny-amd64:1.14.5   gcr.io/google_containers/k8s-dns-dnsmasq-nanny-amd64:1.14.5
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-addon-manager:v6.4-beta.2       gcr.io/google-containers/kube-addon-manager:v6.4-beta.2
docker tag registry.cn-hangzhou.aliyuncs.com/google-containers/kubernetes-dashboard-amd64:v1.6.3    gcr.io/google_containers/kubernetes-dashboard-amd64:v1.6.3


```

## 手工给minikube 生成cache
---

```bash
minikube cache add k8s.gcr.io/pause-amd64:3.0 
minikube cache add k8s.gcr.io/k8s-dns-dnsmasq-nanny-amd64:1.14.5
minikube cache add k8s.gcr.io/k8s-dns-kube-dns-amd64:1.14.5
minikube cache add k8s.gcr.io/k8s-dns-sidecar-amd64:1.14.5
minikube cache add gcr.io/k8s-minikube/storage-provisioner:v1.8.0
minikube cache add gcr.io/google-containers/kube-addon-manager:v6.5
minikube cache add k8s.gcr.io/kubernetes-dashboard-amd64:v1.8.1
minikube cache add k8s.gcr.io/kube-apiserver-amd64:v1.9.0
minikube cache add k8s.gcr.io/kube-scheduler-amd64:v1.9.0
minikube cache add k8s.gcr.io/kube-proxy-amd64:v1.9.0
minikube cache add k8s.gcr.io/kube-controller-manager-amd64:v1.9.0
minikube cache add k8s.gcr.io/etcd-amd64:3.0.17

```

### 附 minikube 0.25.0对应的镜像

```bash 

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/storage-provisioner:v1.8.0 
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/k8s-dns-kube-dns-amd64:1.14.5
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/k8s-dns-sidecar-amd64:1.14.5
docker pull registry.cn-hangzhou.aliyuncs.com/google-containers/pause-amd64:3.0
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/k8s-dns-dnsmasq-nanny-amd64:1.14.5
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-addon-manager:v6.5
docker pull registry.cn-hangzhou.aliyuncs.com/kube_containers/kubernetes-dashboard-amd64:v1.8.1

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-apiserver-amd64:v1.9.0
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-scheduler-amd64:v1.9.0
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-proxy-amd64:v1.9.0
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-controller-manager-amd64:v1.9.0
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/etcd-amd64:3.0.17

#docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/tiller:v2.8.1

docker tag registry.cn-hangzhou.aliyuncs.com/google-containers/pause-amd64:3.0 k8s.gcr.io/pause-amd64:3.0
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/k8s-dns-dnsmasq-nanny-amd64:1.14.5 k8s.gcr.io/k8s-dns-dnsmasq-nanny-amd64:1.14.5
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/k8s-dns-kube-dns-amd64:1.14.5 k8s.gcr.io/k8s-dns-kube-dns-amd64:1.14.5
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/k8s-dns-sidecar-amd64:1.14.5 k8s.gcr.io/k8s-dns-sidecar-amd64:1.14.5
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/storage-provisioner:v1.8.0 gcr.io/k8s-minikube/storage-provisioner:v1.8.0
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-addon-manager:v6.5 gcr.io/google-containers/kube-addon-manager:v6.5
docker tag registry.cn-hangzhou.aliyuncs.com/kube_containers/kubernetes-dashboard-amd64:v1.8.1 k8s.gcr.io/kubernetes-dashboard-amd64:v1.8.1

docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-apiserver-amd64:v1.9.0 k8s.gcr.io/kube-apiserver-amd64:v1.9.0
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-scheduler-amd64:v1.9.0 k8s.gcr.io/kube-scheduler-amd64:v1.9.0
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-proxy-amd64:v1.9.0 k8s.gcr.io/kube-proxy-amd64:v1.9.0
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-controller-manager-amd64:v1.9.0 k8s.gcr.io/kube-controller-manager-amd64:v1.9.0
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/etcd-amd64:3.0.17 k8s.gcr.io/etcd-amd64:3.0.17

minikube cache add k8s.gcr.io/pause-amd64:3.0 
minikube cache add k8s.gcr.io/k8s-dns-dnsmasq-nanny-amd64:1.14.5
minikube cache add k8s.gcr.io/k8s-dns-kube-dns-amd64:1.14.5
minikube cache add k8s.gcr.io/k8s-dns-sidecar-amd64:1.14.5
minikube cache add gcr.io/k8s-minikube/storage-provisioner:v1.8.0
minikube cache add gcr.io/google-containers/kube-addon-manager:v6.5
minikube cache add k8s.gcr.io/kubernetes-dashboard-amd64:v1.8.1
minikube cache add k8s.gcr.io/kube-apiserver-amd64:v1.9.0
minikube cache add k8s.gcr.io/kube-scheduler-amd64:v1.9.0
minikube cache add k8s.gcr.io/kube-proxy-amd64:v1.9.0
minikube cache add k8s.gcr.io/kube-controller-manager-amd64:v1.9.0
minikube cache add k8s.gcr.io/etcd-amd64:3.0.17




```