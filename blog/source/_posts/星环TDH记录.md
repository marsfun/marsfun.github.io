---
title: 星环TDH安装
date: 2018-03-12 12:10:00
categories: 
- 技术
- 研究
tag: [bigdata TDH 星环]
---
# 资料
 
1. 文档集合
- https://docs.transwarp.cn/5.1/
2. sdk使用样例
- http://support.transwarp.cn/t/sdk-maven-tdh-repository/546

# 安装

- 虚拟机也可以安装！！！
- 注意根据版本不同 需要对docker分区做不同的处理。（建立pv/vgs 不分区 或 做好 xfs 分区。看文档！！ ）
- 集群不能在界面上删除，比起cloudera 不方便。（找售后支持要脚本！！）

# 概念
- TDH 本身采用spark作为计算引擎，对外不提供命令操作接口
- 集群交互方式有两种
    - 采用THD的client客户端（从集群管理页面下载）
    - 进入datanode pod 执行hdfs 相关命令

- TDH pod按照容器的角色进行了细分 data/yarn/nodenode/等


# 体验
- 从apache或原生的发型版转来，会感觉束手束脚，TDH采取半开放的方式提供api操作，与之前的体验差别较大。
- 另一方面，由于基于k8s的缘故所有的客户端操作实际需要与容器打通，也带来与物理集群操作方面的不同体验。（虽然还没有试用客户端，但可以想象基于k8s后，TDH这种交互也是一种必然的方式）