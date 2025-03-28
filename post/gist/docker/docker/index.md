# Docker IPV6 Setup


Allow docker to use IPV6 network.

This solution is not working in my network that parent router may have firewall that make my device unable to get connection automatically,
details checkout to [my ipv6 network issue](./ipv6.en.md)

## Check docker IPV6 is enabled

## Get ipv6 address range in host

```bash
ip -6 route show
```

## Enable IPV6 in docker

There are 2 ways to enable

### 1. Enable IPV6 in docker configuration

Edit docker configuration file `/etc/docker/daemon.json`.
Assume the ipv6 address range is `2400::/64`.

```bash
{
  "ipv6": true,
  "fixed-cidr-v6": "2400::/64",
  "ip6tables": true # default is true
}
```

#### Restart docker service

```bash
systemctl restart docker
```

### 2. Enable IPV6 in docker compose file

```yaml
networks:
   ip6net:
     enable_ipv6: true
     ipam:
       config:
         - subnet: 2400::/64
```

## Problem

- old docker version does not support IPV6
  - solution: upgrade
- It does not work
  - no solution yet

