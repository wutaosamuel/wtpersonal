# OpenWRT


OpenWRT

## 目录

1. [子域名及多级域名的 IP 通配](#子域名及多级域名的-IP-通配)

## 子域名及多级域名的 IP 通配

### 介绍

IP 可以在 **网络 -> DHCP/DNS -> 静态地址** 固定.
默认的本地域名是根据主机名来的.
例子: **ubuntuNginx.lan** 是 **ubuntuNginx** 机器的本地域名.

然而, 如果我想所有多级域名都指向同一 IP 地址, 我必须在 **网络 -> 主机映射** 中手动添加每个子域名.

### 添加通配

LUCI 界面没办法添加通配, 只能在 ssh 或者命令行中操作

在这个文件 **/etc/dnsmasq.conf** 添加下面这行

```bash
# '.' 是必须的
address=/.[域名]/[ip]
```

例子:

```bash
# '.' 是必须的
address=/.ubuntunginx.lan/192.168.0.2
```

