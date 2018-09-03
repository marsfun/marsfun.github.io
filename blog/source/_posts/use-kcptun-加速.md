---
title: use kcptun 加速
date: 2018-04-12 21:17:19
categories: 
- 技术
- Helper
tag: [KcpTun VPN 加速 youtube]
---

# 总体思路
KcpTun 区别于bbr 等其他单边加速的方案，它需要同时安装服务端和客户端来使用。
```bash
服务端shadowsocks                                 客户端shadowsocks
     |                                                  |
     |------> 服务端KcpTun            客户端KcpTun------->|
                  |                       |
                  |---------------------->|
```
# 使用脚本安装KcpTun 服务端
https://github.com/kuoruan/shell-scripts/blob/master/kcptun/kcptun.sh

# 客户端使用shadowsocksX-NG 启用kcptun即可。

## 配置参数注意：
### 服务端 rcvwnd sndwnd 与客户端的这两个设置建议相同，即 server-rvcwnd == client-sndwnd （未验证，安装时用了默认值）
### 基本参数的理解

mode选择对速度的影响效果 fast3 > [fast2] > fast > normal > default  
速度越快，越费流量

---
## tuning
可以简单计算一下 是否达到了自己adsl的带宽上限。
```python
带宽计算公式：
在不丢包的情况下，有最大-rcvwnd 个数据包在网络上正在向你传输，以平均数据包大小avgsize计算，在任意时刻，有：

network_cap = rcvwnd*avgsize

数据流向你，这个值再除以ping值(rtt)，等于最大带宽使用量。

max_bandwidth = network_cap/rtt = rcvwnd*avgsize/rtt
<!-- more -->
举例，设rcvwnd = 1024, avgsize = 1KB, rtt = 400ms，则：

max_bandwidth = 1024 * 1KB / 400ms = 2.5MB/s ~= 25Mbps

（注：以上计算不包括前向纠错的数据量）

前向纠错是最大带宽量的一个固定比例增加：

max_bandwidth_fec = max_bandwidth*(datashard+parityshard)/datashard

举例，设datashard = 10 , partiyshard = 3，则：

max_bandwidth_fec = max_bandwidth * (10 + 3) /10 = 1.3*max_bandwidth ＝ 1.3 * 25Mbps = 32.5Mbps
```
ps：用ping 命令查rtt
```bash
--- XXXXXXXX ping statistics ---
3 packets transmitted, 3 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 262.543/41.140/427.469/67.552 ms

```
