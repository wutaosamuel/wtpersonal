# Proxmox 换源


由于国内连接国外官方源速度不开，所以需要更换软件源。
而且Proxmox官方源需要订阅，但是家庭使用只要免费的社区源，
也需要更换Proxmox的软件源。
这里会用到[阿里云源](https://developer.aliyun.com/mirror/)
和[中科大源](http://mirrors.ustc.edu.cn/)。

## Debian 换源

因为Proxmox是基于Debian，所以需要更换debian国内源。
注释掉所有 **/etc/apt/sources.list** 文件的内容，并添加(需要root权限):

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

## Proxmox 社区源

首先，删除Proxmox企业源：

```bash
rm -rf /etc/apt/sources.list.d/pve-enterprise.list
```

下载密钥:

```bash
wget http://download.proxmox.com/debian/proxmox-ve-release-6.x.gpg -O /etc/apt/trusted.gpg.d/proxmox-ve-release-6.x.gpg
```

添加中科大源:

```bash
echo "deb https://mirrors.ustc.edu.cn/proxmox/debian/pve buster pve-no-subscription" > /etc/apt/sources.list.d/pve-no-subscription.list
```

完成！

