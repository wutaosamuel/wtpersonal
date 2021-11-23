# 类unix系统命令


纪录一些可能有用的，不经常使用的命令。

## 目录

1. [iperf3 - 测试网络速度](#iperf3)

## iperf3

测试设备间网络速度。
先开启服务端iperf/iperf3服务，
再用客户端iperf/iperf3测速。
windows下为iperf3.exe应用。

### 服务端开启

```bash
iperf3 -s -p 5201
```

### 客户端开启

```bash
iperf3 -c [ip address] -p 5201
```

