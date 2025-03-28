# 类unix系统命令


纪录一些可能有用的，不经常使用的命令。

## 目录

1. [iperf3 - 测试网络速度](#iperf3)
2. [qemu - 转换磁盘映像](#qemu-转换磁盘映像)
3. [base64 - 加密和解密](#base64-加密和解密)
4. [smartctl - 查看ssd健康](#macos-查看ssd健康)

## iperf3

测试设备间网络速度。
先开启服务端 iperf/iperf3 服务，
再用客户端 iperf/iperf3 测速。
windows 下为 iperf3.exe 应用。

### 服务端开启

```bash
iperf3 -s -p 5201
```

### 客户端开启

```bash
iperf3 -c [ip address] -p 5201
```

## Qemu 转换磁盘映像

```bash
qemu-img convert -f [格式] -O [输出的格式] vm-114-disk-0.qcow2 vm-114-disk-0.raw
```

比如:

```bash
# 转换 qcow2 成 raw 磁盘映像
qemu-img convert -f qcow2 -O raw vm-114-disk-0.qcow2 vm-114-disk-0.raw
```

## base64 加密和解密

加密:

```bash
echo 'YourText' | base64
```

生成伪随机数的base64

```bash
head -c32 /dev/urandom | base64
```

解密:

```bash
echo 'WW91clRleHQK' | base64 --decode
```

## macos 查看ssd健康

```bash
smartctl -a [device id]
```

