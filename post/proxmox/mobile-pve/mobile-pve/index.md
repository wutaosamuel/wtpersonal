# All-in-one router in proxmox for rental place


- background
- software structure

## Change source

### Debian source

### Proxmox community source

## OpenWRT setup

## Internal network interface for Login

The official Proxmox provides a enterprise source,
but we do not need to subscription a enterprise license in home use.
We only need the free and well-supported community source.
By the way, we get a limitation download speed from international connect
(1Mbps approximately) in home use, even I bought 300Mbps download speed plan.
Thus, need to change system software sources as well.
It will need [Ali Yun source](https://developer.aliyun.com/mirror/)
and[USTC source](http://mirrors.ustc.edu.cn/)。

## Debian Source

The Proxmox is based on Debian, thus, we need to change debian sources.
Comment all contents of **/etc/apt/sources.list** file
and added following contents (need root authority):

```bash
deb http://mirrors.aliyun.com/debian/ buster main non-free contrib
deb-src http://mirrors.aliyun.com/debian/ buster main non-free contrib
deb http://mirrors.aliyun.com/debian-security buster/updates main
deb-src http://mirrors.aliyun.com/debian-security buster/updates main
deb http://mirrors.aliyun.com/debian/ buster-updates main non-free contrib
deb-src http://mirrors.aliyun.com/debian/ buster-updates main non-free contrib
deb http://mirrors.aliyun.com/debian/ buster-backports main non-free contrib
deb-src http://mirrors.aliyun.com/debian/ buster-backports main non-free contrib
```

## Proxmox community source

First, delete Proxmox enterprise sources:

```bash
rm -rf /etc/apt/sources.list.d/pve-enterprise.list
```

Download private key:

```bash
wget http://download.proxmox.com/debian/proxmox-ve-release-6.x.gpg -O /etc/apt/trusted.gpg.d/proxmox-ve-release-6.x.gpg
```

I used USTC sources, So, add USTC Proxmox sources into it

```bash
echo "deb https://mirrors.ustc.edu.cn/proxmox/debian/pve buster pve-no-subscription" > /etc/apt/sources.list.d/pve-no-subscription.list
```

Done!

