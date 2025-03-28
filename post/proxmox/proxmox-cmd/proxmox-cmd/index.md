# Proxmox Commands


Record usd Proxmox VE commands.

## Contents

1. [Import disk img](#import-disk-img)
2. [Add/Rescan undisplayed disk](#rescan-undisplayed-disk)
3. [Passthrough sata drivers](#passthrough-sata-drivers)
4. [Delete PVE cluster](#delete-pve-cluster)
5. [Reinstall ceph](#reinstall-ceph)
6. [Pin older kernel](#pin-older-kernel)
7. [Delete snapshot forcedly](#delete-snapshot-forcedly)
8. [Unlock virtual machine](#unlock-virtual-machine)
9. [Unlock lxc container](#unlock-lxc-container)

## Import Disk Image

Transfer .img disc image file to Virtual Machine(vm) disk.

- The example is to transfer openWRT image file to VM disk.

```bash
qm importdisk [VM id] [image path] [storage id]
```

Example:

```bash
qm importdisk 100 ./openwrt.img local-lvm
```

## Rescan Undisplayed Disk

It would be rescan all disk which belongs to a specific virtual machine. If find undisplayed disk, it'll add to virtual machine's configuration

```bash
qm rescan --vmid [VM id]
```

Example:

```bash
qm rescan --vmid 100
```

## Passthrough SATA Drivers

```bash
# list all sata drivers
ls -l /dev/disk/by-id/

# add sata to virtual machine, where sataX is sata0, sata1 ...
qm set 100 -sataX /dev/disk/by-id/xxxxx
# Example:
# DO NOT add disk with part1, part2 ..., like /dev/disk/by-id/xxxxx-part1
qm set 100 -sata0 /dev/disk/by-id/xxxxx
```

## Delete PVE Cluster

```bash
# list nodes and information
pvecm nodes
# delete the node
pvecm delnode [NodeName]
# list all nodes' configuration directory
ls -l /etc/pve/nodes/
# delete or move to another location
mv /etc/pve/nodes/[NodeName] /root/[NodeName]
```

Examples:

```bash
# remove this pve-node3 cluster
pvecm nodes
pvecm delnode pve-node3
ls -l /etc/pve/nodes/
mv /etc/pve/nodes/pve-node3 /root/pve-node3
```

## Reinstall ceph

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

## Pin Older Kernel

Latest kernel may not be supported to a old hardware, requires fix the kernel version

```bash
proxmox-boot-tool kernel pin [KernelVersion]
```

Examples:

```bash
proxmox-boot-tool kernel pin 5.13.19-6-pve
```

## Delete Snapshot Forcedly

```bash
qm delsnapshot [VMID] [SnapshotName] --force
```

Examples:

```bash
qm delsnapshot 100 update00 --force
```

## Unlock Virtual Machine

```bash
qm unlock [vmid]
```

Examples:

```bash
qm unlock 100
```

## Unlock LXC Container

```bash
lct unlock [lxc-id]
```

Examples:

```bash
lct unlock 100
```

