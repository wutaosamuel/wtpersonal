# Proxmox 命令


记录使用过的 Proxmox VE 命令。

## 目录

1. [转换 img 磁盘](#转换-img-磁盘)
2. [添加/重新扫描未显示的磁盘映像](#重新扫描未显示的磁盘映像)
3. [直通 sata 盘](#直通-sata-盘)
4. [删除 PVE 集群节点](#删除-pve-集群节点)
5. [重新安装 ceph](#重新安装-ceph)
6. [固定内核版本](#固定内核版本)
7. [强制刪除 snapshot](#强制刪除-snapshot)
8. [解锁虚拟机](#解锁虚拟机)
9. [解锁 lxc 容器](#解锁-lxc-容器)

## 转换 img 磁盘

将.img 的镜像转换为虚拟机(VM)的磁盘。

- 这里的例子是转换 openWRT 镜像到虚拟机的磁盘。

```sh
qm importdisk [VM id] [image path] [storage id]
```

比如:

```sh
qm importdisk 100 ./openwrt.img local-lvm
```

## 重新扫描未显示的磁盘映像

一下命令会自动扫描虚拟机中所有磁盘, 并且会自动把未显示的磁盘添加到虚拟机配置文件中

```bash
qm rescan --vmid [VM id]
```

比如:

```bash
qm rescan --vmid 100
```

## 直通 sata 盘

```bash
# 显示所有 sata drivers
ls -l /dev/disk/by-id/

# add sata to virtual machine, where sataX is sata0, sata1 ...
# 添加sata盘到虚拟机, 其中sataX可以是 sata0, sata1 ...
qm set 100 -sataX /dev/disk/by-id/xxxxx
# 例子:
# 不要使用带part1, part2 ... 尾缀的, 比如 /dev/disk/by-id/xxxxx-part1
qm set 100 -sata0 /dev/disk/by-id/xxxxx
```

## 删除 PVE 集群节点

```bash
# 显示所有节点以及其信息
pvecm nodes
# 删除节点
pvecm delnode [NodeName]
# 显示所有节点的配置文件夹
ls -l /etc/pve/nodes/
# 删除或者移动到别的位置
mv /etc/pve/nodes/[NodeName] /root/[NodeName]
```

比如:

```bash
# 删除 pve-node3 这个节点
pvecm nodes
pvecm delnode pve-node3
ls -l /etc/pve/nodes/
mv /etc/pve/nodes/pve-node3 /root/pve-node3
```

## 重新安装 ceph

```bash
rm -rf /etc/systemd/system/ceph*
killall -9 ceph-mon ceph-mgr ceph-mds
rm -rf /var/lib/ceph/mon/  /var/lib/ceph/mgr/  /var/lib/ceph/mds/
pveceph purge
apt purge ceph-mon ceph-osd ceph-mgr ceph-mds
rm /etc/init.d/ceph
for i in $(apt search ceph | grep installed | awk -F/ '{print $1}'); do apt reinstall $i; done
dpkg-reconfigure ceph-base
dpkg-reconfigure ceph-mds
dpkg-reconfigure ceph-common
dpkg-reconfigure ceph-fuse
for i in $(apt search ceph | grep installed | awk -F/ '{print $1}'); do apt reinstall $i; done
pveceph install
```

## 固定内核版本

最新的内核版本可能不支持老的硬件, 需要固定老版本的内核

```bash
proxmox-boot-tool kernel pin [KernelVersion]
```

比如:

```bash
proxmox-boot-tool kernel pin 5.13.19-6-pve
```

## 强制刪除 Snapshot

```bash
qm delsnapshot [VMID] [SnapshotName] --force
```

比如:

```bash
qm delsnapshot 100 update00 --force
```

## 解锁虚拟机

```bash
qm unlock [vmid]
```

Examples:

```bash
qm unlock 100
```

## 解锁 lxc 容器

```bash
lct unlock [lxc-id]
```

Examples:

```bash
lct unlock 100
```

