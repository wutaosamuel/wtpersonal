# Proxmox 命令


记录使用过的Proxmox VE命令。

## 目录

1. [import disk img](#import-disk-img)

### Import disk img

将.img的镜像转换为虚拟机(VM)的磁盘。

- 这里的例子是转换openWRT镜像到虚拟机的磁盘。

```sh
qm importdisk [VM id] [image path] [storage id]
```

Example:

```sh
qm importdisk 100 ./openwrt.img local-lvm
```

h
iperf3 -c [ip address] -p 5201
```

