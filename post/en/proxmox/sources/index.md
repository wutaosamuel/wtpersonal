# Proxmox changes Sources


The official Proxmox provides a enterprise source,
but we do not need to subscription a enterprise license in home use.
We only need the free and well-supported community source.
By the way, we get a limitation download speed from international connect
(1Mbps approximately) in home use, even I bought 300Mbps download speed plan.
Thus, need to change system software sources as well.
It could use [Tsinghua mirror](https://mirrors.tuna.tsinghua.edu.cn/), [Ali Yun mirror](https://developer.aliyun.com/mirror/)
and[USTC mirror](http://mirrors.ustc.edu.cn/)。

## Debian Source

The Proxmox 8 is based on Debian 12 (bookworm), thus, we need to change debian 12 sources.
Comment all contents of **/etc/apt/sources.list** file
and added following contents (need root authority):

```bash
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware

# debian-security update
deb https://mirrors.tuna.tsinghua.edu.cn/debian-security bookworm-security main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security bookworm-security main contrib non-free non-free-firmware
```

## Proxmox community source

First, delete Proxmox enterprise sources:

```bash
rm -rf /etc/apt/sources.list.d/pve-enterprise.list
```

I used tsinghua mirror, So, add USTC Proxmox sources into it

```bash
echo "deb https://mirrors.tuna.tsinghua.edu.cn/proxmox/debian/pve bookworm pve-no-subscription" > /etc/apt/sources.list.d/pve-no-subscription.list
```

## CT Templates source

```bash
cp /usr/share/perl5/PVE/APLInfo.pm /usr/share/perl5/PVE/APLInfo.pm_back

```

## Proxmox software install

Install vim, iperf3 and net-tools

```bash
apt install vim net-tools iperf3
```

