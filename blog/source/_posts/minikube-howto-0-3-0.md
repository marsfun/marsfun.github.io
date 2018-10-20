---
title: minikube 代理设置 以及问题解决[0.3.0]
date: 2018-10-20 21:42:33
categories: 
- 技术
- 问题解决
tag: [minikube]
---

# 1. 设置 minikube 使用本机代理，实现对google镜像拉取
将本机的代理，暴露给minikube，让docker容器内可以访问本机的代理。由于shadowsocks默认代理的ip为127.0.0.1，需要改下ip地址才能让容器内的应用识别。

### 1） 设置shadowsocks
* 将HTTP代理监听的IP 由 localhost/127.0.0.1 改为 0.0.0.0
* 在本地网卡的lo0接口上，给127.0.0.1 增加别名,命令如下：
```bash
sudo ifconfig lo0 alias 111.111.111.111

```

### 2） minikube 启动命令加入参数
```bash
minikube start --vm-driver hyperkit --docker-env http_proxy=http://111.111.111.111:1087 --docker-env https_proxy=http://111.111.111.111:1087 -v 9
```


# 2. 解决 minikube在系统重启或者 minikube stop后，再次minikube start时，无法启动的问题。

* 问题现象
```bash
→ minikube start -v 9
...

&{{{<nil> 0 [] [] []} docker [0x140f940] 0x140f910  [] 0s} 192.168.65.78 22 <nil> <nil>}
About to run SSH command:
cat /etc/os-release
Error dialing TCP: dial tcp 192.168.65.78:22: connect: operation timed out
Error dialing TCP: dial tcp 192.168.65.78:22: connect: operation timed out
Error dialing TCP: dial tcp 192.168.65.78:22: connect: operation timed out

```

* 解决方法
```bash
rm ~/.minikube/machines/minikube/hyperkit.pid

```