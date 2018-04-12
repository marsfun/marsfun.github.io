---
title: minikube dns解析不正确 v0.25.2 
date: 2018-04-02 19:10:00
categories: 
- 技术
- 问题解决
tag: [minikube]
---

* minikube 下 docker pull 遇到如下问题，本质是无法docker hub。可以在启动minikube时 设置 register-mirror 解决.
```bash
$ docker pull busybox
Using default tag: latest
latest: Pulling from library/busybox
d070b8ef96fc: Pulling fs layer
error pulling image configuration: Get https://dseasb33srnrn.cloudfront.net/registry-v2/docker/registry/v2/blobs/sha256/f6/f6e427c148a766d2d6c117d67359a0aa7d133b5bc05830a7ff6e8b64ff6b1d1d/data?Expires=1522710550&Signature=SVHVIzKYGemU72iOB1q-XVUtQ0ik7sVLmntzXseV2ih-RpN88Bg4J0E4QkSndDIFlhCODS2A8fFVaAxTEVrzIkfe6ihcT7i2lkT9KAsvj2JHFC7cOBckBHZQB-RncaTwnK2ygWRUqXqDcsSjXUuzXeWPXxtYLrakl7n2uR3ENXE_&Key-Pair-Id=APKAJECH5M7VWIS5YZ6Q: net/http: TLS handshake timeout
$ exit
logout
```bash

* 解决

minikube start --vm-driver=xhyve --registry-mirror=https://registry.docker-cn.com -v 7


** 另外 minikube v0.25.2 已经可以正确下载localkube 所需的 image镜像.