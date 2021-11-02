# Proxmox Commands


Record usd Proxmox VE commands.

## 目录

1. [import disk img](#import-disk-img)

### Import disk img

Transfer .img disc image file to Virtual Machine(vm) disk.

- The example is to transfer openWRT image file to VM disk.

```sh
qm importdisk [VM id] [image path] [storage id]
```

Example:

```sh
qm importdisk 100 ./openwrt.img local-lvm
```

